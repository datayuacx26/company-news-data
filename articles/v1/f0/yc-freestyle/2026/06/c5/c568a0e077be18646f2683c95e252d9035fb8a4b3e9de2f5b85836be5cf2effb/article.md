---
schema_version: "1.0.0"
document_id: "c568a0e077be18646f2683c95e252d9035fb8a4b3e9de2f5b85836be5cf2effb"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/product/best-ai-sandbox-for-agent-clis"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T20:54:28.432730+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:8c5502994b36dd38270111aab30ae86244f0256f4fcb360a966c894858b425ce"
---

# The Best AI Sandbox for Agent CLIs

The next wave of coding agents is not just an API call.


It is a CLI.


Claude Code, GitHub Copilot's headless runtime, OpenClaw, local agent frameworks, project-specific scripts, and internal developer tools all assume the same thing: there is a real machine underneath them. They expect a shell, a home directory, environment variables, package managers, network access, files, processes, ports, signals, and a terminal that behaves like a terminal.


That is why the best AI sandbox for agent CLIs is not a narrow command runner. It is a real Linux VM.[Freestyle VMs](https://www.freestyle.sh/products/vms) are the most powerful VMs for AI agents: hardware-virtualized machines that run real Linux, can run forever when configured that way, and expose the runtime through APIs for commands, files, lifecycle, PTYs, ports, domains, private networking, snapshots, forks, and cleanup.


If your product runs an agent CLI, the sandbox is not just where code executes. It is where the agent lives.


## The agent CLI test


A simple code sandbox can run this:


```text
python script.py


```


An agent CLI usually does something closer to this:


```text
claude
copilot --headless --host 0.0.0.0 --port 4321
openclaw gateway --bind lan --port 18789
npm run dev
journalctl -u agent -f


```


Those commands are not equivalent to "evaluate this snippet and return stdout." They may prompt for input. They may keep a long-lived session open. They may use a TUI. They may start a server. They may require a token in a root-only environment file. They may need to bind a port, serve a browser UI, or accept a private network connection from an SDK client.


That is the agent CLI test: can your sandbox run the CLI the way its authors expect it to run on Linux?


If the answer is "only if you wrap it in our task abstraction," the product will inherit that abstraction forever. Every new CLI feature becomes a platform integration project. Every interactive edge case becomes a special case. Every background process becomes another thing your application has to model outside the machine.


The cleaner answer is to give the CLI a computer.


## Exec is useful, but it is not the whole interface


Freestyle VMs expose` vm.exec()` because a lot of agent work really is command-shaped. Install a package. Check a version. Write a config file. Run a test. Inspect a process. Build a project.


But agent CLIs routinely cross the boundary where` exec()` is too coarse.


A CLI may draw prompts, wait for approval, stream intermediate reasoning, open a REPL, use job control, react to` Ctrl-C` , or leave a process running while the user closes the browser. A buffered command response cannot represent that honestly. The interface has to support live input and live output.


That is what a PTY is for. Freestyle's[PTY docs](https://www.freestyle.sh/docs/vms/pty) describe long-lived interactive shell sessions inside a VM. They can be opened, detached, reattached over WebSocket, resized, signaled, listed, and closed. Sessions survive client disconnects, VM suspends, and VM forks. They are backed by a real pseudo-terminal, so prompts, line editing, job control, REPLs, debuggers, package managers, and terminal UIs behave like normal Linux software.


For an agent CLI, that changes the runtime from a command endpoint into a workspace. The CLI can keep running. The user can reconnect. The agent can send input later. A human can inspect the same terminal state instead of reading a reconstructed transcript.


## CLIs need services, not just shells


Many agent CLIs are not only shell programs. They are services.


Some run a headless server that an SDK client connects to. Some expose a web UI. Some run a local gateway with HTTP and WebSocket traffic. Some need an app server beside them because the agent is editing and previewing a project. Some need a language server, file watcher, database, or worker in the same environment.


That pushes the sandbox beyond "run a process." The product needs a service model.


On a Freestyle VM, the boring Linux answer works. Put long-lived agent runtimes under` systemd` , store secrets in root-only env files, bind services to` 0.0.0.0` when they need to accept traffic, and inspect them with` systemctl status` or` journalctl -u` . Freestyle's docs use that pattern for browser-based gateways, dev servers, Docker, databases, and agent CLIs.


This matters because` systemd` gives the agent runtime a name, restart behavior, boot behavior, logs, and an operational surface the agent already knows how to inspect. Your product does not have to invent a bespoke process supervisor for every CLI it supports.


## Ports are part of the CLI runtime


Once a CLI starts a server, the sandbox has to make network behavior explicit.


Freestyle[VM domains](https://www.freestyle.sh/docs/vms/domains) route public HTTPS traffic from a hostname to a port inside a VM. The docs show the normal shape: run a service inside the VM, make sure it listens on the mapped port, then create a domain mapping. For previews,` *.style.dev` domains can be mapped without custom DNS or verification.


That fits HTTP agent surfaces. A generated app can serve on one port. A gateway UI can serve on another. A notebook, dashboard, or preview can receive real browser traffic. The URL points at the process running in the same machine where the agent has files, logs, terminals, package caches, and local state.


Not every agent protocol is HTTP. Some runtimes use raw TCP or private SDK connections instead of a public HTTPS domain. That is where Freestyle[VPCs](https://www.freestyle.sh/docs/vms/network/vpcs) and[WireGuard VPNs](https://www.freestyle.sh/docs/vms/network/vpns) matter. VMs on the same VPC can talk over private IPs, and a developer machine can temporarily join that VPC over WireGuard.


The point is not that every port should be public. The point is that ports are real product surfaces. An agent CLI sandbox should be able to route public previews, keep private control channels private, and let the product choose which network path each runtime gets.


## State is more than the working tree


Agent CLIs accumulate state in more places than a repo.


They write config under` ~/.config` . They cache packages. They store auth state. They keep shells alive. They leave servers running. They write logs. They create temp files. They update lockfiles. They may hold a conversation or active session in memory. They may need a token available only to a supervised process, not to every command the agent runs.


That state is why a real VM is a better primitive than a disposable command sandbox. The workspace can be stopped, started, resized, forked, and deleted as one object. If a workload should stay alive until the product explicitly stops it, set` idleTimeoutSeconds` to` null` . If it should be reclaimed after inactivity, give it an idle timeout.


Freestyle's[VM lifecycle docs](https://www.freestyle.sh/docs/vms/lifecycle) are explicit about those states. A VM can run, stop while preserving disk, start again, resize, fork from the current running state, and be deleted when the workspace is finished.


That is exactly the shape of agent CLI work. Start from a prepared environment. Let the CLI install what it needs. Keep the session alive while the user is active. Suspend or stop when the product policy says to. Fork before a risky run. Delete after durable outputs have been saved.


When the CLI is editing source code, keep the long-term code history in[Freestyle Git](https://www.freestyle.sh/products/git) or another real repository system. Git is where branches, diffs, commits, and review belong. The VM is where the agent actually runs the tools, services, terminals, and previews that make those changes meaningful.


## Snapshots make agent CLIs practical


Installing an agent CLI on every session is usually wasteful.


A production agent platform often has a baseline: Node, Python, package managers, a browser, Docker, one or more agent CLIs, org-specific config, trusted certificates, helper scripts, and service units. Rebuilding that on every user session adds latency and increases the number of things that can fail before the agent does useful work.


Freestyle snapshots let you prepare that baseline once and boot new VMs from it. The docs show this pattern across runtime guides: install a runtime or service, verify it works, snapshot the VM, then create many sandboxes from the prepared image.


For agent CLIs, snapshots should be treated as product infrastructure. Bake the slow and stable pieces. Keep per-user secrets out of the image. Start services with session-specific credentials at VM creation time. Then the first user-visible action is not "wait while the platform installs the agent," but "give the agent a machine that already has the tools it needs."


## Forking is a CLI superpower


Agent CLIs are useful because they can take initiative. That also means they make risky choices.


One prompt might upgrade dependencies. Another might rewrite a migration. Another might change an authentication flow. A human reviewer may want to compare two approaches. The product may want multiple agents to try different fixes from the same reproduced bug.


Filesystem snapshots are useful, but the stronger primitive is forking the running environment. Freestyle VM forking creates a new VM from the current running state. The PTY docs add an important detail for CLI agents: non-exited PTY sessions are inherited by forks under the same session ID, seeded with the parent's at-fork screen, and then output diverges per child.


That means an agent can reach a decision point with dependencies installed, a server running, logs visible, and a terminal open. Then your product can fork the machine and let alternatives diverge without rebuilding the setup.


For CLI-based agents, that is a better mental model than "rerun the whole task in a fresh sandbox." It preserves the context that made the next decision possible.


## What to look for in an agent CLI sandbox


When evaluating a sandbox for agent CLIs, test the uncomfortable parts first.


Can it run a real interactive shell? Can it preserve a terminal across disconnects? Can it send input and signals after the process starts? Can it run a long-lived CLI under a service supervisor? Can it expose HTTP ports as real URLs? Can it keep private raw connections private? Can it install native packages? Can it store config and caches in a normal home directory? Can it fork from a running state? Can it run forever when the product needs it to?


Also test the operational path. Can a developer inspect the same machine the agent used? Can the product stop, start, resize, and delete the environment cleanly? Can code changes move into a real repo for review instead of living only inside a sandbox volume?


Those questions are not edge cases. They are the normal shape of CLI agent products.


## The bottom line


The best AI sandbox for agent CLIs is a real Linux VM.


Agent CLIs are built for computers. They expect terminals, files, processes, ports, users, auth state, package managers, service supervisors, private networks, and long-lived runtime context. If the sandbox only understands one-shot execution, your product has to recreate the missing computer around it.


Start with[Freestyle VMs](https://www.freestyle.sh/products/vms) when the agent CLI is part of the product experience. Use PTYs for interactive sessions, domains for HTTP surfaces, VPCs and VPNs for private control channels, snapshots for prepared baselines, forks for parallel attempts, and Git for code that needs review.
