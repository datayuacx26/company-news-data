---
schema_version: "1.0.0"
document_id: "e8898ab1e166f23730ae6c4e6f491951af1e9d37ba06119c66d37ed86d306d09"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/how-to-do-reinforcement-learning-on-freestyle"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:b08afba9e4da12f27676b363c5b252febde3ab08ba117579fa11d2d8bb540cca"
---

# How to Do Reinforcement Learning on Freestyle

Agent RL is bottlenecked by environments, not gradients. The policy is an LLM. The action is a shell command or a tool call. The reward is something concrete: a test passes, a build goes green, a page renders. Every rollout needs its own clean Linux machine because every rollout will install packages, mutate files, and occasionally do something unhinged.


This post is about how to run reinforcement learning for AI agents on Freestyle. The pattern is small: cache a golden environment as a` VmSpec` , fan out N VMs from that snapshot, run the policy in each, collect the trajectory, kill the VM.


## Why Freestyle VMs


[Freestyle VMs](https://www.freestyle.sh/products/vms) are the most powerful VMs for AI agents, and that matters more for RL than for any other workload. Each VM is a real Linux machine with full root, SSH, systemd, users, groups, and configurable networking. A VM created from a cached snapshot starts in under 500ms, suspending preserves memory and CPU state and resumes in under 100ms, and` vm.fork({ count })` gives you N live copies of a running VM in one call. That is what an RL training step actually wants: a fast way to get many identical machines, and a cheap way to branch them.


## Installing Freestyle


Install the SDK:


```text
$   bun i freestyle
```


Set` FREESTYLE_API_KEY` in your environment and the SDK auto-detects it.


The runtime helpers ship as separate packages. Pick whichever your env needs:


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


## The golden environment


Build the environment once, as a` VmSpec` . The first` freestyle.vms.create` from the spec warms a snapshot; every subsequent create from the same spec starts from that cached layer.


```text
import { freestyle, VmSpec } from "freestyle";


const env = new VmSpec()
.workdir("/env")
.repo("org/rl-env", "/env")
.systemdService({
name: "deps",
mode: "oneshot",
exec: ["bash /opt/install.sh"],
});


```


Anything the policy needs at rollout time goes in here: the target repo, the test harness, datasets, browsers, MCP servers, anything you would otherwise reinstall on every step. Bake it in once so every rollout starts with it warm.


## Fan out


Each rollout is its own VM, started from the cached snapshot:


```text
async function rollout(seed: number) {
const { vm } = await freestyle.vms.create({
snapshot: env,
persistence: { type: "ephemeral", deleteEvent: "onSuspend" },
idleTimeoutSeconds: 60,
});


await vm.exec(`python rollout.py --seed ${seed}`);
const trace = await vm.exec("cat /env/trace.json");


await vm.kill();
return trace.stdout;
}


const traces = await Promise.all(
Array.from({ length: 64 }, (_, i) => rollout(i)),
);


```


The` ephemeral` persistence with` deleteEvent: "onSuspend"` means the VM deletes itself the moment it stops being talked to. Combined with the 60-second idle timeout, the rollout fleet stays self-cleaning even when a worker crashes mid-episode.


## Branching mid-episode


When you want to explore two actions from the same state, fork a running VM.` vm.fork({ count })` returns N live copies in a single call:


```text
const { forks } = await master.fork({ count: 2 });
const [branchA, branchB] = forks;


await Promise.all([
branchA.vm.exec("python step.py --action left"),
branchB.vm.exec("python step.py --action right"),
]);


```


Forking from an already-running VM is much cheaper than rebooting two fresh ones, because the parent's memory and disk state come along for free.


## Pause and resume


Some rollouts wait on a slow tool or a human label. Suspend the VM instead of holding the compute open. Suspended VMs are storage-only and resume in under 100ms:


```text
await vm.suspend();
// ...later
await vm.start();


```


When a state is worth keeping permanently, snapshot it. The snapshot is an immutable checkpoint you can create new VMs from later:


```text
const { snapshotId } = await vm.snapshot();


```


## Persistence and idle timeout


Two` freestyle.vms.create` options decide how rollout VMs are reused or recycled.


` persistence` picks one of three modes:


- ` sticky` : the **default** . The VM is kept around as a cache (priority 0–10, default 5). Right for the master golden-environment spec, so the next training step starts from a warm cached snapshot instead of reinstalling.
- ` ephemeral` : right for individual rollout VMs. Set` persistence: { type: "ephemeral", deleteEvent: "onSuspend" }` and each rollout deletes itself the instant the trajectory is shipped.
- ` persistent` : kept indefinitely until you delete it. Use only when you specifically want a long-lived VM you keep returning to.


` idleTimeoutSeconds` auto-suspends a VM after that many seconds of network inactivity (default **300s** ; pass` null` to disable). For RL fan-out, a tight` idleTimeoutSeconds` paired with ephemeral persistence keeps the fleet self-cleaning. The` vm.kill()` at the end of` rollout()` is the happy path; the timeout is the safety net.


## Questions and answers


### Q: How many parallel rollouts can I run?


The limit is your account's VM concurrency, not the platform. Each rollout starts from the cached snapshot in under 500ms, so the per-rollout boot cost rounds to nothing against the policy step. Talk to us if you need a higher cap.


### Q: Does Freestyle host the GPU side?


Freestyle is the env and rollout side: clean Linux VMs, snapshotted and forked. Run the learner and the policy server on your own GPU host (or a dedicated inference provider) and have rollout VMs call the policy over HTTP. The two halves scale independently, on the hardware each one actually needs.


### Q: How do I share updated weights with rollouts?


You usually do not. Run a centralized inference server that holds the current weights, and have every rollout VM hit it over HTTP. When the learner publishes new weights, reload the server. The next training step's rollouts pick up the new policy automatically.


### Q: Can I branch an episode to try two actions?


Yes.` vm.fork({ count: N })` returns N live copies of the running VM in one call. Each diverges from the exact same state, which is the cheap way to do tree search or counterfactual rollouts.


### Q: How do I checkpoint a long rollout?


Suspend the VM. It costs storage only and resumes in under 100ms. For a permanent restore point, call` vm.snapshot()` .


### Q: How is this different from running rollouts in containers?


Containers share a kernel; an agent that modifies sysctls, mounts filesystems, or fights with init either fails or contaminates the host. Freestyle VMs give each rollout its own machine with full root and real systemd, and` vm.snapshot()` and` vm.fork({ count })` are first-class operations.


## The short answer


Build the golden environment as a` VmSpec` . Cache it as a snapshot. Fan out N VMs per training step from that snapshot, run the policy inside each, collect the trajectory, kill the VM. Use` vm.fork({ count })` when you want to branch a running episode and` vm.suspend()` when you want to park one. That is the whole loop.
