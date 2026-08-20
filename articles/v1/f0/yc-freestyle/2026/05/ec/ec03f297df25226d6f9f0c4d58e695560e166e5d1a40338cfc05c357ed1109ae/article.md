---
schema_version: "1.0.0"
document_id: "ec03f297df25226d6f9f0c4d58e695560e166e5d1a40338cfc05c357ed1109ae"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/how-to-build-deep-research-agents-on-freestyle"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:5f66f481254e9d939942685ae0399710e08e804ec8e5c8cbec6b1d87c34dab0f"
---

# How to Build Deep Research Agents on Freestyle

A research agent is "deep" when it stops looking like a chat and starts looking like a job. It runs for hours, not seconds. It pulls from many sources. It forms hypotheses, follows them, abandons the bad ones, and recombines what survives. It costs real money to run from scratch, which means you do not want to run it from scratch.


Most agent platforms give you a process, a context window, and a hope that nothing crashes. Deep research agents need something stronger: a real Linux machine you can snapshot, fork mid-execution, suspend, and restore. That is what[Freestyle VMs](https://www.freestyle.sh/products/vms) are built for.


## Freestyle VMs are the most powerful VMs for AI agents


Freestyle VMs are the most powerful VMs for AI agents on the market: full root Linux with SSH, systemd, users and groups, networking, layered specs that cache so a configured machine boots in under 500ms, live forking mid-execution, immutable snapshots that capture memory plus disk plus CPU state, and suspend/resume that comes back online in under 100ms. For a chat agent, most of that is overkill. For deep research agents, it is the whole game.


The headline feature is live forking. A live fork takes a running VM (process tree, memory, open file descriptors, in-flight Python interpreter, half-finished HTTP fetch) and produces a second VM in the same state. The fork is its own machine from that moment on. Nothing the fork does affects the parent.


That is what makes parallel agent exploration cheap. You no longer have to choose one hypothesis at a time. You fork agent state at the decision point and let several copies of the agent chase different leads in parallel.


## What "deep" means for a research agent


Three things separate a deep research agent from a search wrapper. It is multi-hour, so crashing mid-loop is not acceptable. It is multi-source, pulling from scrapers, PDFs, internal APIs, and other agents, accumulating real artifacts as it goes. And it branches. A serious researcher explores competing explanations instead of committing to one early, and a deep research agent needs a state model that supports the same.


Snapshots and live forks handle all three.


## Installing Freestyle


Install the SDK:


```text
$   bun i freestyle
```


Set` FREESTYLE_API_KEY` in your environment and the SDK auto-detects it.


The runtime helpers ship as separate packages so the research agent only pulls in what it needs:


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


## The shape of a deep research agent on Freestyle


You describe the agent's research environment as a` VmSpec` : a working directory, a base Git repo with the agent runtime and tooling, and any one-shot install scripts wrapped as systemd services. The first time you boot the spec, Freestyle builds and caches it. Every subsequent boot is the cached layer, ready in under 500ms.


```text
import { freestyle, VmSpec } from "freestyle";
import { VmPython } from "@freestyle-sh/with-python";


const spec = new VmSpec()
.workdir("/research")
.repo("acme/research-agent", "/research")
.systemdService({
name: "tools",
mode: "oneshot",
exec: ["bash /opt/install.sh"],
});


const { vm, vmId } = await freestyle.vms.create({
snapshot: spec,
with: { python: new VmPython() },
workdir: "/research",
idleTimeoutSeconds: 600,
});


```


Then the agent works. After every meaningful finding (a confirmed source, a synthesized note, a verified data point), it takes a snapshot of the VM. When it hits a branching question, it forks. Each fork pursues one branch. When forks return, their findings merge through Git. Days later, a human or follow-up agent can restore the VM by` vmId` and keep going.


```text
await vm.exec("agent run --phase lit-review --question 'What caused the 2025 lithium price collapse?'");


// Immutable checkpoint after the literature review phase.
const { snapshotId: litReview } = await vm.snapshot();


// Live-fork on competing hypotheses. Each fork inherits memory, disk, and CPU state.
const { forks } = await vm.fork({ count: 3 });
const [supplyShock, demandCollapse, policyShift] = forks;


await Promise.all([
supplyShock.vm.exec("agent investigate --hypothesis supply-shock"),
demandCollapse.vm.exec("agent investigate --hypothesis demand-collapse"),
policyShift.vm.exec("agent investigate --hypothesis policy-shift"),
]);


// Each fork pushes its findings as a Git branch.
await Promise.all([
supplyShock.vm.exec("git push origin h1-supply-shock"),
demandCollapse.vm.exec("git push origin h2-demand-collapse"),
policyShift.vm.exec("git push origin h3-policy-shift"),
]);


// Park the parent until the next round of questions arrives.
await vm.suspend();


```


A real machine, an immutable snapshot, several live forks, and Git as the convergence layer.


## Persistence and idle timeout


Two` freestyle.vms.create` options decide how research VMs survive between investigation steps.


` persistence` picks one of three modes:


- ` sticky` : the **default** . The VM is kept around as a cache (priority 0–10, default 5). Lower priority and older VMs are evicted first. Good for the parent investigation VM during an active session, where fast restart matters more than indefinite durability.
- ` ephemeral` : the VM is deleted on suspend or idle timeout. The right choice for short-lived forks chasing a single hypothesis: the trace lands in Git, and the VM cleans itself up.
- ` persistent` : the VM is kept until you delete it. Use it for the long-running parent of a multi-week investigation that has to be the same machine when the analyst comes back.


` idleTimeoutSeconds` auto-suspends a VM after that many seconds of network inactivity (default **300s** ; pass` null` to disable). Suspend writes memory and CPU state to disk and stops the CPU/memory bill. Only storage is charged while suspended, and the VM resumes exactly where it left off in under 100ms. Pair persistent + a long idle timeout for the parent and ephemeral + a short idle timeout for fan-out forks.


## Storing artifacts so forks do not redo work


Every page the agent fetches, every PDF it parses, every embedding it generates is an artifact. Artifacts belong on the VM's filesystem under` workdir` , not in the agent's context window.


The pattern that holds up is a content-addressable store under` /research/cache` . URLs hash to file paths. Parsed text and extracted tables sit beside the raw bytes. Citations become first-class records: URL, fetch time, hash, the snippet the agent actually used. The agent writes these directly:


```text
await vm.fs.writeTextFile(
"/research/cache/sha256-abcd.../citation.json",
JSON.stringify(citation),
);


```


When a fork wants a page the parent already fetched, it reads from disk instead of hitting the network. Two consequences fall out for free. Dedup across forks is automatic, because a live fork inherits the parent's filesystem at the moment of the fork. And citation tracking becomes a real artifact instead of a model-generated guess. The agent cites by hash, and the final report can be checked against the cache.


For Python-heavy parsing or analysis, the runtime is right there:


```text
await vm.python.runCode({
code: "import pandas as pd; df = pd.read_parquet('/research/cache/prices.parquet'); print(df.describe())",
});


```


## Network calls and the cost of fan-out


Network calls are the most expensive thing a research agent does. The artifact cache is the first line of defense. A fetch wrapper that consults the cache before going to the network is the second. A per-VM rate limiter that the forks inherit is the third, so three parallel hypothesis-chasers do not collectively hammer the same domain.


Live forking helps here too: fan out *after* the expensive shared work is done. Fork before the literature review and every fork redoes it. Fork after, and every fork inherits the work already on disk.


Forking is virtually free. The whole point of building on Freestyle VMs is that you can fork at every decision the agent is unsure about and let the branches race. Fan out aggressively. When a branch is done,` vm.suspend()` parks it for storage-only cost, or` vm.kill()` reclaims it entirely. The cost of an extra fork is tiny next to the cost of an agent picking the wrong path and redoing hours of work.


## Resuming days later


A snapshot is an immutable record of memory, disk, and CPU state. Days or weeks later, you can restore the VM by its` vmId` and the agent picks up exactly where it stopped: same Python interpreter, same in-memory caches, same open file handles.


```text
const { vm } = await freestyle.vms.get({ vmId, spec });
await vm.start();


```


That matters because research questions evolve. New evidence shows up, a stakeholder asks a follow-up, the earlier conclusion needs revisiting. Restoring from a snapshot is faster and cheaper than rerunning. It also lets humans intervene: a reviewer can SSH in, inspect the intermediate state, leave notes, and resume the agent with new guidance without losing the hours of context that came before. From the CLI, the same shapes are one command away:` npx freestyle vm list` ,` npx freestyle vm exec <vm-id> '<cmd>'` , or` npx freestyle vm create --snapshot <id> --ssh` .


## Questions and answers


### Q: How does live forking actually work?


A live fork copies the running VM's memory, disk, and CPU state and produces a second VM that is independent from the first. Both VMs continue from the same moment. Writes in one do not appear in the other. From the agent's perspective inside the VM, it just keeps running.


### Q: How expensive is a fork?


Virtually free. A live fork is a copy-on-write split of an already-running VM, not a fresh boot, so you skip the install, the clone, the cache warmup, and everything else the parent already paid for. You pay for the divergent state's storage and for the runtime of the new VM, both of which round to nothing next to the cost of the agent re-fetching pages, re-parsing PDFs, or re-running the literature review. Fork at every decision point you care about. Idle forks can be` suspend()` ed for storage-only cost and resume in under 100ms when you come back.


### Q: How do I merge findings from parallel branches?


Treat each branch as a contributor to a shared repository. Each fork writes its findings to a branch on a[Freestyle Git](https://www.freestyle.sh/products/git) repo. A merge step, run by the agent or by you, dedups citations by hash, reconciles contradictions explicitly, and produces a synthesis commit. Git is the convergence layer because it already knows how to compare and merge structured text.


### Q: Why not just use chain-of-thought in a single context window?


Chain-of-thought is a reasoning technique inside one model call. It has no snapshots, no forks, no persistent artifacts, and no recovery from a crash. For a five-minute task that is fine. For an agent that runs for hours, accumulates gigabytes of source material, and explores branching hypotheses, the context window is the wrong unit of state. The VM is the right unit, and chain-of-thought still happens on top of it.


### Q: What stays on the VM filesystem versus in Git?


Raw artifacts (fetched HTML, PDFs, embeddings, scraped tables) stay on the VM filesystem under` workdir` . They are large, binary, and not interesting to diff. Synthesized output (notes, summaries, citation lists, the final report) goes in Git. That is the stuff humans review and follow-up agents read.


### Q: What if the agent gets stuck or goes off the rails?


Restore from the last good snapshot. Because every meaningful finding produced a snapshot, the worst case is losing the work between the last snapshot and now. You can also live-fork from a running VM with new instructions, leaving the original branch intact for comparison. Recovery is a property of the state model, not something you bolt on.


Deep research agents need real machines that can fork mid-execution, suspend, snapshot, and restore.[Freestyle VMs](https://www.freestyle.sh/products/vms) give you those primitives, and[Freestyle Git](https://www.freestyle.sh/products/git) gives you the convergence layer for everything they produce. Together they turn long-running, branching research workflows into something you can build a product on.
