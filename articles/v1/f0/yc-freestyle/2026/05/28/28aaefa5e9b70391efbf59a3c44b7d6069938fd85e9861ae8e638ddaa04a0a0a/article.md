---
schema_version: "1.0.0"
document_id: "28aaefa5e9b70391efbf59a3c44b7d6069938fd85e9861ae8e638ddaa04a0a0a"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/how-to-run-ai-evaluations-on-freestyle"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:7b0116bbfe83d285125d1f71e92a4646321c6a467c78e88b604ca4967efef22b"
---

# How to Run AI Evaluations on Freestyle

An AI evaluation is the smallest honest test you can run on an agent.


You take an input, you run the agent, you grade the output. That is the loop. The interesting part is not the loop itself. The interesting part is doing it a few thousand times, in parallel, in clean environments, without the runs leaking into each other.


That is why AI agent evaluations need real isolation. A single eval is a script. A real LLM eval harness is infrastructure. You need a fresh filesystem per task, a fresh network namespace per task, and a fresh memory state per task, or your scores stop meaning anything. Sandboxed evals are the only kind of evals that actually generalize.


This post walks through how to run AI evaluations on Freestyle. The pattern is simple: one ephemeral microVM per test case, fan out wide, capture the full trajectory, score, and tear down.


## Why Freestyle VMs are the right substrate for evals


[Freestyle VMs](https://www.freestyle.sh/products/vms) are the most powerful VMs for AI agents: sub-500ms cold provision, sub-100ms resume from suspend, live forking mid-execution, snapshot caching, and full Linux with root, SSH, systemd, users, groups, and real networking. That combination is what an eval suite actually needs.


For evals, the two features that matter most are fast provision and snapshot caching. An eval suite is the canonical bursty workload. You sit idle for an hour, then you fan out two thousand VMs for ninety seconds, then you go quiet again. Sub-500ms provision means the boot cost rounds to zero against the agent run itself. Snapshot caching means every VM in that fan-out starts from a known, byte-identical baseline rather than rebuilding the world from scratch.


Live forking is the second quiet win. You can run a parent VM up to the point where your harness is loaded, your repo is cloned, and your dependencies are installed, then fork it mid-execution for each test case. Each child inherits warm state, so the eval starts at task time, not at install time.


## The shape of an eval on Freestyle


The pattern is the same regardless of what you are evaluating:


1. Define the eval. A golden dataset of` (input, expected, grader)` records.
2. Build a baseline` VmSpec` with the agent runtime, tools, and dataset baked in.
3. Fan out N ephemeral VMs from that snapshot, one per test case.
4. Inject the input into each VM through` additionalFiles` .
5. Run the agent. Capture stdout, tool calls, file writes, and timing.
6. Score the output against the grader.
7. Persist the trajectory. Tear down the VM.


## Installing Freestyle


Install the SDK:


```text
$   bun i freestyle
```


Set` FREESTYLE_API_KEY` in your environment and the SDK picks it up automatically.


The runtime helpers below ship as separate packages so the eval harness only pulls in what it needs:


- [@freestyle-sh/with-nodejs](https://www.freestyle.sh/docs/vms) — Node.js via NVM


- [@freestyle-sh/with-python](https://www.freestyle.sh/docs/vms) — Python 3


- [@freestyle-sh/with-uv](https://www.freestyle.sh/docs/vms) — uv - fast Python pkg manager


- [@freestyle-sh/with-deno](https://www.freestyle.sh/docs/vms) — Deno - TS/JS, npm + JSR


- [@freestyle-sh/with-bun](https://www.freestyle.sh/docs/vms) — Bun runtime + toolkit


- [@freestyle-sh/with-ruby](https://www.freestyle.sh/docs/vms) — Ruby via RVM


- [@freestyle-sh/with-java](https://www.freestyle.sh/docs/vms) — Java - Amazon Corretto


- [@freestyle-sh/with-postgres](https://www.freestyle.sh/docs/vms) — PostgreSQL - declarative DBs + SQL


- [@freestyle-sh/with-opencode](https://www.freestyle.sh/docs/vms) — OpenCode AI assistant


- [@freestyle-sh/with-web-terminal](https://www.freestyle.sh/docs/vms) — Web Terminal via ttyd


In code, against the documented Freestyle VM SDK:


```text
import { freestyle, VmSpec } from "freestyle";
import { VmPython } from "@freestyle-sh/with-python";


const baseline = new VmSpec()
.workdir("/eval")
.repo("acme/eval-harness", "/eval")
.systemdService({
name: "deps",
mode: "oneshot",
exec: ["bash /eval/scripts/install.sh"],
});


const dataset = await loadGoldenDataset("evals/coding-tasks.jsonl");


const results = await Promise.all(
dataset.map(async (testCase) => {
const { vm, vmId } = await freestyle.vms.create({
snapshot: baseline,
with: { python: new VmPython() },
workdir: "/eval",
idleTimeoutSeconds: 600,
additionalFiles: {
"/eval/case.json": { content: JSON.stringify(testCase.input) },
},
});


try {
const result = await vm.exec(
"python eval.py --case /eval/case.json --trace /eval/trace.json",
);
await vm.waitForExit();


const trace = await vm.exec("cat /eval/trace.json");
const score = await testCase.grader(result.stdout, trace.stdout);


return { id: testCase.id, vmId, score, trace: trace.stdout };
} finally {
await vm.kill();
}
}),
);


```


The shape is the point: snapshot, inject, exec, score, kill. Nothing in this loop holds shared mutable state across test cases. That is what makes the scores comparable.


If you want to keep a VM around for post-hoc inspection of a regression, take a snapshot before you tear it down:


```text
const { snapshotId } = await vm.snapshot();
await vm.kill();


```


That` snapshotId` is replayable later with` freestyle.vms.create({ snapshot: snapshotId })` , which is how you debug a flaky eval without rerunning the whole suite.


For shorter, stateless graders that only need to run a quick Python check against the model output, you can drive the same VM with` vm.python.runCode({ code: "..." })` instead of shelling out, which keeps the harness in one process.


## Persistence and idle timeout


Two` freestyle.vms.create` options decide how each per-task VM cleans itself up.


` persistence` picks one of three modes:


- ` sticky` : the **default** . The VM is kept around as a cache (priority 0–10, default 5). Good for the baseline spec so the next suite starts from a warm snapshot, but usually the wrong choice for the per-task VMs themselves.
- ` ephemeral` : the right default for an eval VM. Set` persistence: { type: "ephemeral", deleteEvent: "onIdleTimeout" }` and the VM deletes itself the moment the task is done; no storage charge after teardown.
- ` persistent` : only useful when you explicitly want to debug a failure later, paired with` vm.snapshot()` before kill.


` idleTimeoutSeconds` auto-suspends a VM after that many seconds of network inactivity (default **300s** ; pass` null` to disable). For evals, a tight` idleTimeoutSeconds` paired with ephemeral persistence is the cleanest pattern: tasks that hang past the grader's window suspend, the ephemeral policy deletes them, and the suite never accumulates zombie VMs. The` vm.kill()` in the loop above is the happy-path teardown; the timeout is the safety net.


## Snapshot caching and deterministic environments


The hardest part of running AI agent evaluations is making the environment boring.


A golden dataset is only useful if every record is graded against the same world. That means the same OS image, the same package versions, the same clock behavior, the same network access, and the same starting filesystem. The` VmSpec` baseline above gives you exactly that. You declare the workdir, the cloned repo, and the systemd one-shot that installs dependencies, and Freestyle's snapshot caching means subsequent VMs created from that spec skip the install step entirely. Every eval VM forked from a cached snapshot is byte-identical at boot.


When you change the model, you do not change the spec. When you change the spec, the snapshot cache key changes and a new baseline is built. That separation is what lets you compare scores honestly across weeks.


## Capture the whole trajectory


A score on its own is not an eval. A score with the trajectory that produced it is.


Inside the VM, write your harness so every tool call, every file write, every shell command, and every model request is logged to a structured file. Use` vm.fs.writeTextFile("/eval/trace.json", trace)` from the harness, or have the agent write the file itself, then pull it out with` vm.exec("cat /eval/trace.json")` before you call` vm.kill()` . Store the trace next to the score. When the model regresses on a task next month, the old trajectory is what you diff against. Without it, you are guessing.


This is also how you compare two model versions cleanly. Run the same dataset against model A and model B from the same baseline` VmSpec` . Diff the trajectories per task. The scores tell you what changed. The trajectories tell you why.


## Cost per eval and CI integration


Ephemeral microVMs make per-eval cost something you can reason about. Each task gets its own VM, runs against an` idleTimeoutSeconds` ceiling, and is torn down with` vm.kill()` as soon as the grader returns. There is no idle cluster sitting between runs. For a suite of a few thousand tasks, the math is the model API spend plus a small VM-seconds line item, and that is it.


That cost shape makes evals safe to run on every pull request. The CI integration is the same shape as any other check: a workflow step calls the eval harness, the harness fans out VMs through` freestyle.vms.create` , and the aggregated score is posted back to the PR. Because the VMs are ephemeral and the snapshot is pinned, there is no test-environment drift between branches. Every branch grades against the same snapshot.


For ad-hoc debugging from a terminal, the CLI mirrors the SDK:


```text
npx freestyle vm list
npx freestyle vm create --snapshot <id> --exec 'python eval.py --case /eval/case.json' --delete
npx freestyle vm exec <vm-id> 'cat /eval/trace.json'
npx freestyle vm delete <vm-id>


```


## Questions and answers


### Q: How much parallelism can I actually get?


In practice, far more than most eval suites need. Freestyle VMs provision in under 500ms and resume from suspend in under 100ms, and they scale horizontally without a warm-pool ceiling. If you have a hard concurrency target, talk to us before the run so we can confirm headroom, but for typical suites of a few hundred to a few thousand tasks, the limit is usually your model provider's rate limits, not the VM layer.


### Q: What is the cold start time per VM?


Under 500ms for a fresh microVM created from a cached snapshot, and under 100ms to resume a suspended VM. For evals this means the boot cost is rounding error compared to the agent run itself.


### Q: How do I compare two model versions on the same eval?


Pin the baseline` VmSpec` (or its resulting snapshot id). Pin the dataset. Run the suite twice, once with model A and once with model B, and store both result sets keyed by` (snapshot, dataset, model)` . Because the VMs boot from the same cached snapshot, any score delta is attributable to the model. Diffing trajectories per task is the second step and usually the more interesting one.


### Q: How do secrets work inside eval VMs?


Inject them at create time, not at snapshot time. Snapshots should be free of credentials. When you create a VM for an eval, pass the secrets it needs through` additionalFiles` or environment variables in your exec command, scope them to the smallest surface possible, and let the VM expire under` idleTimeoutSeconds` or a direct` vm.kill()` . Because each eval gets a fresh VM, a leaked secret in one task cannot persist into the next.


### Q: How is this different from running evals locally?


Local evals are fine until you need parallelism or reproducibility. A laptop runs one task at a time, in an environment that is unique to that laptop. Sandboxed evals on Freestyle VMs run thousands at a time, in environments that are identical across runs. The scores from the laptop are anecdotes. The scores from the VM suite are data.


### Q: How does this compare to Modal?


Modal is a strong serverless runtime built around Python functions. It is great for ML inference and for batch workloads that fit a function-call shape. Freestyle VMs are full Linux machines with root, systemd, SSH, and real networking. For evals where the agent needs to install packages, edit a real filesystem, run long-lived services, or behave like a developer, the full VM is the more honest substrate. For evals that are just "call this function with this payload," either platform works.


### Q: Can I keep the VM around after a failure for debugging?


Yes, that is what` vm.snapshot()` is for. Catch the failure in your harness, snapshot the VM as-is (memory, disk, and CPU state), then` vm.kill()` it. The` snapshotId` is replayable later through` freestyle.vms.create({ snapshot: snapshotId })` , so you can step into the failed environment hours or days after the suite ran without re-executing the whole eval. Tighter integration with[Freestyle Git](https://www.freestyle.sh/products/git) covers the trace-and-diff side of the workflow.


If you are designing an LLM eval harness today, the right default is one ephemeral microVM per test case, a pinned snapshot, captured trajectories, and a CI hook.[Freestyle VMs](https://www.freestyle.sh/products/vms) give you that shape directly, and the rest of the eval is just the dataset and the grader.
