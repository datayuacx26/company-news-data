---
schema_version: "1.0.0"
document_id: "3fd417a7c92f24e7b5ed0773fb48c9d71b004643efc7270e6e369613522fbb25"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/how-to-deploy-long-running-agents-on-freestyle"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:f61afe69cde32babf212a12fc9f7acd6657301d8bdb82be823369800379d4525"
---

# How to Deploy Long-Running Agents on Freestyle

Most agent demos run for thirty seconds. A user types a prompt, a model picks tools, a sandbox spins up, the result is returned, the sandbox is destroyed. That is an ephemeral agent task. It is fine for one-shot generations, but it is the wrong model for almost everything interesting.


Real products need long-running agents. A coding agent that works on a project across weeks. A research agent that watches a data source for days. An app-builder that keeps a dev server warm between user edits. A workflow agent that wakes up on a webhook, does five minutes of work, and goes back to sleep until the next event.


Those agents need a runtime that survives. They need to keep their installed packages, their build cache, their working directory, their environment, their open processes, and their accumulated context. Spinning up a fresh sandbox per turn throws all of that away and pays the rebuild cost every time.


This post walks through how to deploy long-running agents on Freestyle, using[Freestyle VMs](https://www.freestyle.sh/products/vms) as persistent agent runtime infrastructure.


## Ephemeral tasks vs. long-running agents


An ephemeral agent task is a single function call. The agent boots, does work, returns a result, and goes away. State that should outlive the call has to be written somewhere else.


A long-running agent is a session. It might run for hours or days. It accumulates files in its working tree, packages in` node_modules` , models in a cache, secrets in environment variables, and partially built artifacts on disk. Between user messages it might be idle for minutes or weeks.


The split matters because the infrastructure pattern is different. Ephemeral tasks want fast cold starts and aggressive teardown. Long-running AI agents want a stateful host that can be paused cheaply, resumed instantly, snapshotted before risky operations, and forked when the agent wants to try two paths in parallel.


That is the gap Freestyle VMs are built for.


## Freestyle VMs are the most powerful VMs for AI agents


Freestyle VMs are the most powerful VMs for AI agents because they give an agent a full Linux machine that behaves like a process. Each VM has root, runs any binary, supports` systemd` services, multiple Linux users and groups, configurable networking, and SSH. VMs provision in under 500ms, resume from suspend in under 100ms, can be live-forked mid-execution into an exact copy of a running machine, and can be assembled from layered specs whose snapshot layers are cached so common base images do not get rebuilt.


Most "agent sandboxes" are containers with a write-protected filesystem and a ten-minute timeout. They are fine for` eval` style code execution. They cannot host a stateful microVM that an agent lives in for a week.


Freestyle VMs can. Full Linux root, sub-100ms resume, live forking, snapshot caching, systemd, layered specs, and SSH are what make them a viable agent runtime instead of a glorified scratch directory. That is the foundation everything below depends on.


## Installing Freestyle


Install the SDK:


```text
$   bun i freestyle
```


Set` FREESTYLE_API_KEY` in your environment and the SDK auto-detects it.


The runtime helpers ship as separate packages so you only pull in what your agent actually uses:


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


## The lifecycle of a long-running agent


A long-running agent on Freestyle moves through a small set of states: created, running, suspended, forked, snapshotted, terminated. The product code drives those transitions in response to user events.


A typical session starts by provisioning a VM with the language runtime the agent needs and a working directory it can own:


```text
import { freestyle, VmSpec } from "freestyle";
import { VmNodeJs } from "@freestyle-sh/with-nodejs";


const { vm, vmId } = await freestyle.vms.create({
with: { js: new VmNodeJs() },
workdir: "/agent",
idleTimeoutSeconds: 600, // auto-suspend after 10 minutes of inactivity
additionalFiles: {
"/agent/state.json": { content: "{}" },
},
});


```


The agent's tools get installed once. That cost is paid a single time and then captured by the VM disk:


```text
await vm.exec("git clone https://git.freestyle.sh/acme/project.git /agent/work");
await vm.exec({ command: "npm install", });
await vm.fs.writeTextFile("/agent/work/.env", process.env.AGENT_ENV ?? "");


```


For agents whose base environment is reused across sessions, define the shape with` VmSpec` and create from a cached snapshot of that spec. Layers in the spec are cached, so subsequent VMs skip the rebuild:


```text
const spec = new VmSpec()
.workdir("/agent")
.repo("acme/project", "/agent/work")
.systemdService({
name: "agent",
mode: "service",
exec: ["node /agent/work/run.js"],
workdir: "/agent/work",
});


const { vm: warm } = await freestyle.vms.create({ snapshot: spec });


```


Persist the` vmId` next to the user or session. To come back to the same machine on the next event, restore a typed handle and resume:


```text
const { vm } = await freestyle.vms.get({ vmId, spec });
await vm.start(); // resume from suspend in <100ms
await vm.exec("cd /agent/work && npm test");


```


## Persistence and idle timeout


Two` freestyle.vms.create` options decide what happens to the agent's home VM between bursts of activity.


` persistence` picks one of three modes:


- ` sticky` : the **default** . The VM is kept around as a cache (priority 0–10, default 5). Lower priority and older VMs are evicted first, so treat sticky as fast-restart, not durable storage.
- ` ephemeral` : the VM is deleted on suspend or idle timeout. Wrong default for a long-running agent; use only for the bookend tasks the agent fires off into other VMs.
- ` persistent` : the VM is kept indefinitely until you delete it. Right choice for agents that need to be the same machine in a week or a month, with the trade-off of indefinite storage charges.


` idleTimeoutSeconds` auto-suspends a VM after that many seconds of network inactivity (default **300s** ; pass` null` to disable). Suspend writes memory and CPU state to disk and stops the CPU/memory bill. Only storage is charged while suspended, and the next event resumes from exactly where the agent left off in under 100ms. The agent does not re-clone, re-install, or re-warm anything. Long-running agents typically pair` persistent` with a generous` idleTimeoutSeconds` , so the machine survives weeks of intermittent traffic without burning compute on the quiet days.


## Snapshot before risky steps. Fork to try alternatives.


Long-running agents do destructive things. They run migrations. They` rm -rf` directories. They upgrade dependencies. They merge branches. Any of those steps can leave the VM in a worse state than it started.


Snapshots are the obvious safety net.` vm.snapshot()` produces an immutable saved state you can recreate a VM from later. Snapshots are full machine state, not partial dumps:


```text
const { snapshotId } = await vm.snapshot();
const result = await vm.exec("npm run migrate");


if (result.exitCode !== 0) {
// Recreate from the pre-migration snapshot using the same spec.
const { vm: rolledBack } = await freestyle.vms.create({ snapshot: spec });
// ...point the session at rolledBack
}


```


Forking is the more interesting capability.` vm.fork({ count })` produces N live copies of a running VM in a single call. The agent can keep working on the original while the forked clones each try a different approach. Every copy starts from the same memory and disk state, then diverges:


```text
const { forks } = await vm.fork({ count: 2 });
const [tryRefactor, tryPatch] = forks;


await Promise.all([
tryRefactor.vm.exec("git checkout -b refactor && node tools/agent-run.js --strategy=rewrite"),
tryPatch.vm.exec("git checkout -b patch && node tools/agent-run.js --strategy=minimal"),
]);


// Compare results, keep the winner, kill the others.
await tryPatch.vm.kill();


```


That is how multi-strategy agent loops work without a fleet of pre-warmed sandboxes.


## Cost while suspended, surviving restarts, attaching Git


Suspended VMs only cost storage. CPU and memory are not billed while the VM is on disk. That is what makes long-running agent sessions economically reasonable: the agent can sit "online" for a month while only being charged for the kilobytes it occupies on disk between events. See[pricing](https://www.freestyle.sh/pricing) for the current numbers.


Surviving restarts is mostly about the` persistence` config field on` freestyle.vms.create` and the` vmId` you store. The VM ID is the durable handle. Store it next to the user or session record and the agent comes back to the same machine every time via` freestyle.vms.get({ vmId, spec })` or a lightweight` freestyle.vms.ref({ vmId, spec })` .


Working trees should usually live in[Freestyle Git](https://www.freestyle.sh/products/git) rather than only on the VM disk. The disk is fast local state. The Git repo is the source of truth. The agent clones the repo into the VM (or declares it via` gitRepos` /` VmSpec.repo()` ), branches per task, commits checkpoints, and pushes when work is reviewable. If the VM is ever lost, a fresh VM can clone from Git and the agent picks up from the last commit instead of from zero.


For agent products that need durable project state next to the machine,[Freestyle Git](https://www.freestyle.sh/products/git) sits next to VMs in the same control plane.


## Operating long-running agents


Long-running agents fail in long-running ways. A process leaks memory over a week. A background loop pegs the CPU. An agent gets stuck retrying the same tool call. None of that shows up in a thirty-second test.


For day-to-day operation, the CLI is the fastest way in:` npx freestyle vm list` ,` npx freestyle vm exec <vm-id> '<cmd>'` ,` npx freestyle vm create --snapshot <id> --ssh` to drop into a fresh VM from a known spec, and` npx freestyle vm delete <vm-id>` when you are done. For deeper inspection, every VM is reachable over SSH at` ssh <vm-id>@vm-ssh.freestyle.sh` , with token-scoped variants like` ssh <vm-id>:<token>@vm-ssh.freestyle.sh` and` ssh <vm-id>+<user>:<token>@vm-ssh.freestyle.sh` for scoping into a specific Linux user.


The idle timeout is the main defense against zombies. A VM that nobody is talking to suspends itself after` idleTimeoutSeconds` and stops billing for compute. For agents that should hard-stop after some bound, call` vm.stop()` for a graceful shutdown that preserves disk, or` vm.kill()` to force-terminate.


## Putting it together


A long-running agent on Freestyle is a stateful microVM with a persistent disk, an attached Git repo, a generous` idleTimeoutSeconds` , and a product that knows when to suspend, start, snapshot, fork, and stop it. The agent gets a real Linux machine that survives between turns. The product gets a small, well-defined lifecycle to manage:` create` ,` start` ,` exec` ,` suspend` ,` snapshot` ,` fork` ,` stop` ,` kill` .


Deploying long-running agents is mostly a matter of stopping the per-turn rebuild and treating the VM as the agent's home.[Freestyle VMs](https://www.freestyle.sh/products/vms) are designed for that. Persistent agent sessions, sub-100ms resume, live forks, cached snapshot layers, and full Linux are the primitives. The rest is product code.


## Questions and answers


### Q: What does it cost to keep a long-running agent suspended?


While suspended, a Freestyle VM only incurs storage cost for its disk and saved memory image. CPU and RAM are not billed. That is what makes a month-long agent session affordable: most of that month the VM is on disk, not running. Current rates live on the[pricing page](https://www.freestyle.sh/pricing) .


### Q: How long can a single agent session last?


There is no fixed maximum session length. A VM can be created once and resumed indefinitely as long as you hold its` vmId` . Long-running AI agents that span weeks or months are a normal use case, not an edge case. Set` idleTimeoutSeconds: null` if you do not want auto-suspend at all.


### Q: How do I get back to the same VM on the next user event?


Store the` vmId` returned from` freestyle.vms.create` alongside your user or session record. Restore a typed handle with` freestyle.vms.get({ vmId, spec })` , or grab a lightweight reference with` freestyle.vms.ref({ vmId, spec })` . Then call` vm.start()` to resume from suspend in under 100ms.


### Q: How does this compare to ephemeral runtimes like Lambda or Workers?


Ephemeral runtimes are great when each request is independent and short. They are a poor fit for agents that need to keep installed packages, build caches, open processes, or accumulated working state between calls. Freestyle VMs are designed for the opposite shape: long-lived, stateful, suspendable. You can absolutely call ephemeral functions from inside a long-running agent, but the agent itself wants a persistent home.


### Q: How do I run multiple agents on the same project without them stepping on each other?


Call` vm.fork({ count })` once with the number of attempts and branch the Git repo per task. Each forked VM in the returned` forks` array gets its own live copy of the running machine and its own working tree. Each branch keeps the agent's commits isolated until something is ready to merge. That is the same pattern that makes[Freestyle Git](https://www.freestyle.sh/products/git) useful for multi-agent systems, applied to the runtime layer.


### Q: Can the browser talk to a VM directly?


Yes, through token-scoped client sessions. The client SDK runs in the browser against tokens you mint on the server, and you can scope a client to a specific Linux user with` .user()` . By design, clients cannot create VMs; that stays on the server where the API key lives. The browser can exec, read, and write within whatever scope the token grants.


### Q: When should I stop or kill a VM instead of suspending it?


Suspend when the agent might be back. Call` vm.stop()` for a graceful shutdown that keeps the disk but releases memory, and` vm.kill()` when you need to force-terminate. Suspended VMs are cheap but not free, so terminate when the session is genuinely over: the project was deleted, the user churned, the task is complete and the result has been pushed to Git.
