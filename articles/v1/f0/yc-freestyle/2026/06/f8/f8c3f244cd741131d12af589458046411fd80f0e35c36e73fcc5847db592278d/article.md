---
schema_version: "1.0.0"
document_id: "f8c3f244cd741131d12af589458046411fd80f0e35c36e73fcc5847db592278d"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/product/riza-alternative-ai-agents"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-21T23:31:13.198983+00:00"
fetched_at: "2026-07-28T21:23:24.617893+00:00"
content_hash: "sha256:c4172ad558095d24acd102b5e779789a3003a384ba96a4ce6962669813d2ace2"
---

# Riza Alternative for AI Agents

Riza is a clean product for a clean job: let an application execute model-generated code in a secure interpreter and get a result back.


That job matters. A lot of agents need to run a Python snippet, transform data, call a small function, or validate that generated code does what the model thinks it does. Riza's docs describe a Code Interpreter API where you send code, language, input data, files, arguments, and environment variables, then receive stdout, stderr, and an exit code. Its getting-started docs frame the default sandbox as WebAssembly-powered execution for Python, JavaScript, Ruby, and PHP.


That makes Riza a reasonable fit when the agent needs a tool.


The question is what happens when the tool becomes the workspace.


If your agent only needs to execute short snippets, evaluate Riza seriously. If your agent needs a long-running machine with terminals, services, ports, user-visible files, package managers, SSH, preview URLs, and recovery paths, the better Riza alternative is a real VM.[Freestyle VMs](https://www.freestyle.sh/products/vms) are the most powerful VMs for AI agents: hardware-virtualized machines that run real Linux, can run forever when idle timeout is disabled, and give your product API control over files, commands, lifecycle, terminals, networking, domains, and forks.


## The Riza alternative search


The useful way to compare Riza and Freestyle is not "which one can run code?" Both can.


The useful comparison is the shape of the product you are building.


Riza is request oriented. Its[How It Works docs](https://docs.riza.io/getting-started/how-it-works.md) describe a flow where your application makes a request to the Riza API with code and input data, Riza validates resource limits, loads the interpreter as WebAssembly, creates a module with stdin, arguments, environment variables, and limits, executes the code, and returns stdout, stderr, and an exit code.


That is a strong shape for code interpreter tasks:


- run a generated script
- evaluate a function call
- transform a file
- test a small code sample
- give an LLM a controlled computation tool
- collect stdout and stderr for the next model turn


It is not the same shape as a development environment. An agent workspace is not a single execution. It is a place where work accumulates: packages installed, files edited, services started, ports exposed, terminals left open, logs inspected, state preserved, and humans brought in when something gets stuck.


That is where a VM becomes the cleaner primitive.


## A code interpreter is not a computer


Riza's documented limits are reasonable for an interpreter product, but they also show the boundary clearly.


The[resource limits docs](https://docs.riza.io/interpreters/limits.md) list quotas including 1 MiB of input code, 10 MiB of stdin, 20 MiB of stdout, 10 MiB of stderr, 30 seconds of execution duration, and 128 MiB of memory. They also say output files are not yet supported, and that third-party packages are currently supported only for Python and JavaScript.


The[Python interpreter docs](https://docs.riza.io/interpreters/python.md) say default Python runs in a WASI environment, custom runtimes run in a microVM environment, and Python package additions require building a custom runtime. They also say the filesystem is read-only in all environments, network access is blocked by default, and direct network access in the WASI environment is not supported outside HTTP controls.


Those tradeoffs are not defects. They are the point of a constrained interpreter. The product gives generated code a narrow, safe execution box.


But serious agents usually do not stay inside the box. They install native dependencies. They run package managers. They write output artifacts. They start background processes. They need a database, a dev server, a browser, a queue worker, or a local API. They debug with normal Linux tools. They keep state across many turns. They recover from partial failure.


At that point, the "code execution" abstraction starts making your application responsible for reconstructing a computer around the agent.


## Agents need writable state


A useful agent usually needs more than input files and stdout.


It needs a working directory that changes over time. It needs caches, lockfiles, logs, generated artifacts, test fixtures, local databases, and intermediate attempts. It may need to leave a failing state intact so the next model turn, or a human reviewer, can inspect what happened.


Freestyle VMs expose file APIs, but the important part is not the API surface. The important part is that those files live on a real Linux machine where the agent can also use normal shell commands, package managers, editors, build tools, and services.


When the state is source code or reviewable user work, it should not only live in the runtime. A VM is where the agent runs the software.[Freestyle Git](https://www.freestyle.sh/products/git) is where the agent stores code, branches changes, produces diffs, and gives your product a reviewable history. The two products solve different parts of the same agent problem: live execution state and durable source-control state.


Riza is a good fit when each execution is the unit of work. Freestyle is a better fit when the workspace is the unit of work.


## Terminals are product state


The first sign that an interpreter is no longer enough is usually the terminal.


Agents do not only run complete scripts. They start a command, read partial output, send input, interrupt it, rerun it, leave a server running, open a REPL, run a debugger, tail logs, and come back later. Many real developer tools expect a terminal with prompts, line editing, job control, signals, and scrollback.


Freestyle's[PTY docs](https://www.freestyle.sh/docs/vms/pty) describe long-lived interactive shells inside a VM. A PTY session can be opened, detached, and reattached over WebSocket. Sessions survive client disconnects, VM suspends, and VM forks. They are backed by a real PTY, so REPLs, package managers, debuggers, editors, and terminal UIs behave like normal Linux programs.


That matters for product design. A terminal is not just command history. It is the live working surface of the agent. When the browser disconnects, the terminal should not disappear. When the VM is forked before a risky attempt, the terminal context should fork with it. When support needs to inspect a failed run, they should not have to reconstruct the session from sanitized logs.


## Services need ports, not just output


Interpreter APIs are great when the output is text, JSON, or a returned value.


Agent products often need a service.


An AI app builder needs to run a web app. A coding agent needs a dev server. A QA agent needs the app under test plus its dependencies. A data product might need Postgres, Redis, Jupyter, or a dashboard. A webhook agent might need a callback receiver. These are not one-shot scripts. They are processes listening on ports.


Freestyle's[domain docs](https://www.freestyle.sh/docs/vms/domains) describe mapping public HTTPS traffic from a hostname to a port inside a VM. The flow is normal infrastructure: verify a domain, point DNS at Freestyle, map the domain to a VM port, and run a service inside the VM that listens on that port. HTTPS is provisioned automatically.


That is the right shape for previews and agent-operated services. The agent can start the service, tail logs, edit files, restart processes, and keep the same machine available while a user opens the preview. The preview is not an artifact returned from an execution. It is a running service in the agent's workspace.


## SSH is the human escape hatch


Every serious agent product eventually needs a human to inspect the machine.


Logs are useful, but they are not enough. A developer may need to check processes, inspect files, run a repair command, open an editor, reproduce a bug, or understand why the agent is stuck. If the runtime is only an interpreter API, that support workflow has to be invented somewhere else.


Freestyle's[SSH docs](https://www.freestyle.sh/docs/vms/ssh) show SSH access through scoped identities and tokens. Your server can grant an identity access to a VM, mint a token, and connect through Freestyle's SSH proxy. The docs also show access as a specific Linux user, which lets your product separate users, agents, and support flows without handing a browser client your server-side API key.


That escape hatch matters because agents fail in ordinary operating-system ways. They leave files in weird places. They run out of disk. They start the wrong process. They break permissions. They need the same debugging surface a developer would want.


## Riza vs a real VM


The fair comparison is straightforward.


Choose Riza when you want a focused code interpreter API. It is a good fit for short generated scripts, function execution, controlled data transformation, LLM math or analysis tools, code generation evals, and workflows where stdout, stderr, and exit code are the right return values.


Choose Freestyle when the environment is becoming the product surface. That includes AI coding agents, app builders, browser-adjacent agents, notebook-like products that need services, long-running assistants, QA agents, internal automation tools, and any workflow where users and agents share a live machine over time.


Requirement Better fit


Run a short generated script Riza


Execute a model-written function Riza


Return stdout, stderr, and exit code Riza


Keep a writable Linux workspace alive VM


Run package managers and native tools VM


Preserve interactive terminals across reconnects VM


Run services and expose HTTPS ports VM


Let a human SSH into the same environment VM


Fork a live workspace before risky work VM


The boundary is not about whether Riza is useful. It is. The boundary is whether your agent needs a code execution tool or a computer.


If the product is still a tool call, a code interpreter API is elegant. If the product has become a workspace, give the agent the primitive that already matches the job: a real Linux VM with files, processes, ports, terminals, users, services, SSH, lifecycle controls, and state your product can manage directly.
