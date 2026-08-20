---
schema_version: "1.0.0"
document_id: "f0b080ce76331fded7928dc8c592595965e083e61025cdc94283f11564fae493"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/how-to-build-a-code-interpreter-on-freestyle"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:287bc336a98bd994e0d99710becb3842a37af1db3924d0fb16c46ceba391359d"
---

# How to Build a Code Interpreter on Freestyle

Models do not do math. They do not manipulate state. They cannot read a file, hit an API, transform a CSV, run a test, sort a list, or check that the answer they just produced is actually correct. They generate text. Code is how that text becomes work.


A model that wants to add two large numbers writes Python. A model that wants to verify a logical claim writes a checker and runs it. A model that wants to build something runs` bun init` , edits files, runs the test suite, fixes what broke, and tries again. Anything that requires real computation, real state, or real verification routes through code. The tool calls that are not code are mostly thin wrappers around code somebody else wrote.


A code interpreter is the surface that lets the agent actually run that code. You decide what the surface looks like: which languages are available, which packages are preinstalled, what the filesystem contains, what the network can reach, what the agent is allowed to break. The agent emits code into that surface and gets back stdout, stderr, exit codes, and the files it produced.


The hard part is not running the code once. The hard part is running untrusted, model-generated code many times, in parallel, with a workspace that survives between turns, without giving the model anything that touches your production environment. This post walks through how to build a code interpreter for AI agents on top of[Freestyle VMs](https://www.freestyle.sh/products/vms) .


## What a code interpreter actually needs


A real code interpreter has to execute arbitrary code in a real language runtime, capture stdout and stderr along with any files produced, keep the workspace alive across calls in the same session, isolate every user from every other user, handle timeouts without hanging the agent loop, and allow network egress only when the agent should have it.


Notebook kernels do most of this on a laptop. Multi-tenant systems like ChatGPT's Code Interpreter do it for thousands of concurrent agents. They all converge on the same shape: each session is a small isolated VM with a process inside that the orchestrator talks to.


## Why Freestyle VMs


Freestyle VMs are the most powerful VMs for AI agents because they give the agent a full Linux environment with root and SSH, provision in under 500ms, resume from suspend in under 100ms, can be live-forked mid-execution into independent copies, and can be snapshotted as immutable layers that subsequent VMs boot from instantly.


Most "sandboxed code execution" products give you something narrower: a container with a restricted Python kernel, a fixed runtime, and limited control over the filesystem. That breaks the moment an agent wants to install a system package, run a long-lived service under systemd, or hold open a Python session with hundreds of objects in memory.


A microVM is the right granularity for a code interpreter. Small enough to give every session its own machine, strong enough to enforce hard isolation between tenants, and flexible enough to host any language runtime the agent might generate.


## Installing Freestyle


The SDK is a single package. Install it with your package manager of choice:


```text
$   bun i freestyle
```


Then set` FREESTYLE_API_KEY` in your environment. The SDK auto-detects it.


Runtime helpers ship as separate packages so you only pull in what you need:


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


## Spinning up a session


A session is, basically, a VM. When the agent opens a chat, your backend creates the VM. When the conversation ends, you suspend it, snapshot it, or stop it.


```text
import { freestyle } from "freestyle";
import { VmPython } from "@freestyle-sh/with-python";
import { VmNodeJs } from "@freestyle-sh/with-nodejs";


const { vm, vmId } = await freestyle.vms.create({
with: { python: new VmPython(), js: new VmNodeJs() },
workdir: "/workspace",
idleTimeoutSeconds: 600,
});


```


The` with` field attaches typed language runtimes so the agent does not have to install Python or Node on first use. Provisioning happens in under 500ms, which is fast enough to create per-conversation VMs without users feeling it.


## Persistence and idle timeout


Two options on` freestyle.vms.create` decide what happens to a session VM when the user walks away mid-conversation.


` persistence` picks one of three modes:


- ` sticky` : the **default** . The VM is kept around as a cache (priority 0–10, default 5). Lower priority and older VMs are evicted first, so treat sticky as fast-restart, not durable storage.
- ` ephemeral` : the VM is deleted on suspend or idle timeout. Use for one-shot interpreter calls you do not plan to revisit; no storage charge after teardown.
- ` persistent` : the VM is kept indefinitely until you delete it. Use sparingly; it counts against your storage quota.


` idleTimeoutSeconds` auto-suspends a VM after that many seconds of network inactivity (default **300s** ; pass` null` to disable). Suspend writes memory and CPU state to disk and stops the CPU/memory bill. Only storage is charged while suspended, and the next message resumes the session in under 100ms. For most chat-style code interpreters the defaults are correct: sticky persistence so warm sessions resume instantly, plus the 5-minute idle timeout so abandoned conversations stop billing on their own.


## Running code


The interpreter is a thin wrapper around` exec` and the per-language helpers.


```text
const result = await vm.python.runCode({
code: `
import pandas as pd
df = pd.read_csv("/workspace/sales.csv")
print(df.groupby("region")["revenue"].sum())
`,
});


console.log(result.stdout, result.stderr);


```


Take the model's tool call, route it to the right runtime helper, capture stdout and stderr, truncate gracefully if the output is huge, and pass the result back as a tool result. For shell-shaped tools,` vm.exec("...")` is the same idea without a language wrapper, and` vm.js.runCode({ code })` and` vm.bun.runCode({ code })` cover Node and Bun the same way.


## Keeping state between turns


The reason a notebook feels alive is that the workspace sticks around. If your agent writes` report.csv` in one turn, it expects that file to still be there three turns later when it asks to chart it.


Two clean ways to do this on Freestyle. The first is to keep one VM bound to the conversation and use` vm.suspend()` between turns. Suspend writes the full memory and CPU state to disk; you only pay storage costs while suspended, and the VM resumes in under 100ms when the next message arrives.


```text
await vm.suspend();
// ...later, on the next user message:
await vm.start();


```


The second is to call` vm.stop()` for graceful shutdown when you do not need to preserve in-memory state. The disk is preserved and the next` start()` is a cold boot. Use` vm.kill()` only when something is truly wedged. To rebuild the typed handle on a later request, restore it from` vmId` :


```text
const { vm } = await freestyle.vms.get({ vmId, spec });


```


For sticky in-memory variables, run a long-running language process inside the VM (a Python REPL or Jupyter kernel) under systemd and route` runCode` calls into it.


## Snapshots and forking for parallel runs


Agents often want to try several approaches at once: three SQL queries, two model configs, four data-cleaning strategies. The honest way is to fork the workspace, run each branch independently, and pick the best result.


Freestyle VMs can be snapshotted and live-forked mid-execution:


```text
const { snapshotId } = await vm.snapshot();


const { forks } = await vm.fork({ count: candidates.length });
const results = await Promise.all(
forks.map(async ({ vm: forked, vmId }, i) => {
const out = await forked.python.runCode({ code: candidates[i] });
return { code: candidates[i], out, vmId };
}),
);


```


` vm.snapshot()` captures memory, disk, and CPU state as an immutable layer.` vm.fork({ count })` returns N live copies of a running VM in a single call. Each one diverges independently, which is what makes mid-run branching cheap. Snapshots double as cached startup layers: pass one back into` freestyle.vms.create({ snapshot: spec })` and new sessions boot from that exact state. This is the primitive that makes fan-out evaluation useful, applied here to interactive sessions.


## Files, packages, network, timeouts


Use` vm.fs.writeTextFile("/workspace/input.csv", contents)` to seed inputs at session start, or pass` additionalFiles` into` vms.create` so the workspace is populated on first boot. Let the model write to` /workspace` for outputs you can stream back to your UI.


The agent will eventually want a package that is not preinstalled. Because the VM is a full Linux environment with root,` pip` ,` npm` , and` apt-get` all work. Preload common packages by baking them into a snapshot layer; let the agent install the long tail on demand.


Make network egress an explicit policy rather than something that leaks by default. Freestyle exposes configurable networking on the VM; if you only want internal tools reachable, expose them through the agent's tool layer and keep the VM's egress closed.


Wrap every` runCode` call in a timeout. If it expires, kill the offending process inside the VM with` vm.exec` and return the timeout as a tool result. Reserve` vm.kill()` for when the runtime is truly stuck.


Two users should never share a VM. Each Freestyle VM is an isolated microVM with its own kernel and filesystem; your job is to keep the session-to-VM mapping correct and to stop the VM when the session ends.


## Putting it together


A minimal code interpreter on Freestyle is roughly 200 lines: a session table mapping conversations to` vmId` s, a tool handler that takes` { language, code }` and routes it to the right` runCode` call, a timeout wrapper, suspend on idle, and a cleanup job that stops abandoned VMs. Everything else is product surface: streaming output, showing files the agent created, letting users branch the conversation (a` vm.fork()` under the hood).


The reason to build on[Freestyle VMs](https://www.freestyle.sh/products/vms) is that the primitives line up with what a code interpreter actually needs: real Linux with root, real isolation, sub-second provisioning, sub-100ms resume, snapshots, live forks, and typed language helpers. None of the "you cannot run that here" surprises that show up in restricted sandboxes.


## Questions and answers


### Q: Is it actually safe to run LLM-generated code in a Freestyle VM?


Each Freestyle VM is an isolated microVM with its own kernel and filesystem, the same isolation model used by other production sandboxed code execution platforms. The agent has root inside its VM but cannot reach other tenants, the host, or your backend. Treat the VM as untrusted (no mounted secrets, gate egress) and the answer is yes.


### Q: What languages can the code interpreter run?


Anything that runs on Linux. The agent can write Python today, a Bash pipeline next turn, a Rust crate after that. Freestyle ships first-class typed helpers for Python, Node.js, Deno, Bun, Ruby, Java, and PostgreSQL, and anything else installs with the system package manager.


### Q: How does state persist between turns of the same conversation?


Suspend the VM between turns with` vm.suspend()` and resume it with` vm.start()` when the next message arrives. Suspend preserves full memory and CPU state to disk, you only pay storage while suspended, and resume happens in under 100ms. For sticky in-memory variables, run a long-running language process under systemd inside the VM.


### Q: How do I run many candidate scripts in parallel?


Snapshot the configured VM with` vm.snapshot()` and call` vm.fork({ count: N })` to get N live copies in a single round trip. Each fork is an independent VM that starts from the same live state, so you can run different candidates and compare results. Same pattern is useful for evals and multi-attempt agents.


### Q: What does this cost compared to running my own sandbox?


You pay for VM time and storage rather than for managing a fleet of sandboxes yourself. Suspended VMs cost only storage, and snapshot layers let you trade storage for startup latency. See[pricing](https://www.freestyle.sh/pricing) ; the more relevant comparison is engineering time saved by not building microVM orchestration in-house.


### Q: How does this compare to E2B or Modal?


Both are real options for sandboxed code execution. E2B focuses on agent code interpreter use cases with a Python-first API. Modal is a serverless platform with strong autoscaling. Freestyle VMs sit on the "real Linux microVM with snapshots, live forks, suspend/resume, and typed language helpers" end of that space, which fits when the interpreter needs to do more than run a single Python cell.


### Q: How do I handle a script that hangs or runs forever?


Wrap every execution in a timeout. If it expires, kill the offending process inside the VM with` vm.exec` and return the timeout as a tool result so the model can react. Only call` vm.kill()` if the runtime is truly wedged; otherwise the next turn can reuse the same VM.
