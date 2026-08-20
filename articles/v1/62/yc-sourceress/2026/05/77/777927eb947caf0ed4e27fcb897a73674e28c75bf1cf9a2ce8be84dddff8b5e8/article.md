---
schema_version: "1.0.0"
document_id: "777927eb947caf0ed4e27fcb897a73674e28c75bf1cf9a2ce8be84dddff8b5e8"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/every-mngr-plugin-explained"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:662210f2818f99405fae5eb75dbed4693757015cd8ce50103fac27e324fe091a"
---

# Every mngr plugin, explained

` mngr` is an open-source CLI for running and managing parallel AI coding agents. The core experience handles the agent lifecycle, so things like creating, starting, stopping, and destroying agents.


You can go from one local agent to a nightly fleet of 200. Only if` mngr` knows how to talk to your cloud, your scheduler, and your editor. That's what plugins are for.


` mngr` on its own is generic. It handles the agent lifecycle (create, start, stop, destroy) and plugins teach it the rest: where to run agents, how to schedule them, how to sync files, how to chain them into real workflows.


These things are useful on their own, but figuring out how to wire them together into real workflows (scheduling nightly runs, syncing files from a remote agent, watching a dashboard of 20 parallel sessions) takes extra overhead. That’s where the plugin system comes in. Our plugins add specific workflows, integrations, and execution environments on top of that core experience.


This post walks through every plugin that ships with` mngr` today, what it does, and when you'd reach for it.


## How mngr plugins work


There are two categories:


1. **Provider plugins** define WHERE agents execute (your laptop, Modal, a VPS).
2. **Workflow plugins** add capabilities on top of the agent lifecycle (file access, scheduling, notifications, pair sync).


A quick map of what is available today:


Plugin What it does


mngr_claude Adds the claude, code-guardian, and fixme-fairy agent types. Required for any Claude-based agent.


mngr_modal Per-second cloud sandboxes for short bursts of parallel work. Shuts down when idle, you stop paying.


mngr_vultr Always-on VPS via Docker. Flat monthly rate, ideal for persistent monitoring agents.


mngr_file Read and write files on agents that are already shut down. Pull results without keeping compute warm.


mngr_kanpan A terminal Kanban board for many agents at once. Imbue's daily driver.


mngr_schedule Cron syntax for agent tasks. Nightly test runs, weekly tech debt sweeps, anything recurring.


mngr_pair Real-time file sync (unison) between you and a remote agent. Pair-programming with a Claude in the cloud.


mngr_tmr One agent per pytest test, run in parallel. Speed plus isolated auto-fixing on failure.


mngr_wait Blocks until an agent hits a target state. The join step when scripting parallel agents.


mngr_recursive Lets an agent spawn sub-agents. Turn a lead agent into a mid-level manager.


mngr_ttyd A web terminal for each agent. Check on (or unstick) an agent from any browser, even your phone.


mngr_notifications Desktop alerts when an agent transitions to WAITING. So long-running agents don't sit blocked overnight.


mngr_tutor Interactive in-terminal tutorial that auto-detects your progress. Start here on day one.


Now for a plugin-by-plugin breakdown. There are some real gems in here I know you’ll like.


## Provider plugins


### [mngr_claude](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_claude)


Registers three agent types into mngr:` claude` ,` code-guardian` , and` fixme-fairy` . All three use Claude Code under the hood.


` claude` is the base agent type. It launches a Claude Code session inside a tmux window, handles headless setup, and manages the full lifecycle so Claude can run autonomously without manual intervention. Every time you run` mngr create my-agent claude` , this is what executes.


` code-guardian` is an automated consistency auditor. It reads your codebase docs and style guides, scans the code for inconsistencies (like naming mismatches and pattern violations), writes a structured report to` _tasks/inconsistencies/` , and opens a PR.


**` fixme-fairy`** picks a random` FIXME` comment from your codebase, weighted by priority. It reads the surrounding context, attempts a fix, runs the tests, and opens a PR if it succeeds. If it fails, it increments an` attempts` counter on the FIXME and adds a note about what went wrong. Most developers run it on a schedule to chip away at tech debt automatically.


You choose which agent type you want at creation time:


```text
sh
```


Without this plugin installed, mngr cannot create or manage Claude-based agents.


### [mngr_modal](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_modal)


Runs agents in[Modal](https://modal.com/) cloud sandboxes. Modal is a cloud platform that spins up containers on demand. You request a box with specific CPU, memory, and optionally GPU resources. When the agent finishes and goes idle, mngr shuts the sandbox down and you stop paying.


```text
sh
```


This is the primary way mngr handles remote agents. Running agents remotely matters for two reasons. First, parallelism. Running 10 agents on your laptop simultaneously will tank performance. Remote agents each get their own machine. Second, isolation. A remote sandbox lets you run` --dangerously-skip-permissions` without risking your local filesystem. The agent can do whatever it needs inside the container.


Modal is best for short-lived, “bursts” of work. For example, things like parallel code reviews, running a batch of tests across agents, and one-off refactors. When running remote agents, you pay per-second that the sandbox is running, so the economics favor tasks that finish and shut down.


### [mngr_vultr](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_vultr)


This plugin runs agents on[Vultr](https://www.vultr.com/) VPS instances inside Docker containers. A VPS (Virtual Private Server) is a rented machine that lives in a data center that you connect to over the internet. Unlike Modal, a VPS is always on. You pay a flat monthly rate (e.g. around $8/month for a basic instance) regardless of whether the agent is busy or idle. The benefit is that there is no startup delay and no state lost between runs.


We recommend that you use a VPS when you want a persistent, always-on agent. For example, when you want a long-running monitoring agent that watches your repo. Or an agent that picks up scheduled tasks around the clock. The main tradeoff versus Modal is that you pay the flat rate even when nothing is happening.


## Workflow plugins


### [mngr_file](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_file)


Pull results from an agent that's already shut down. With` mngr_file` , agent state lives on a persistent remote volume that stays accessible independently of the running container. You can read, write, and list files even when nothing is running.


```text
sh
```


This is the foundation of cost-efficient workflows. Your agent runs a task, produces output, shuts down so you stop paying for compute. You pull the results later without restarting anything.


### [mngr_kanpan](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_kanpan)


Running this gives you a terminal-based interactive dashboard. It shows all your agents in a Kanban-style board with their current states, aggregated with data from git branches, GitHub PRs, and CI status. You can bind keyboard shortcuts to take actions directly on agents from the board.


This is what the team at Imbue uses day-to-day. You can launch it with` mngr kanpan` .


### [mngr_schedule](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_schedule)


Mngr_schedule is a cron-based scheduling workflow for agent tasks. You define a schedule using standard cron syntax, a command to run, and optionally a remote provider.


```text
sh
```


For example, imagine that your team has a test suite that takes 20 minutes. With mngr_schedule, you can set up a scheduled agent that runs every night at 2am. It pulls the latest` main` , runs the full suite, and opens a GitHub issue if anything fails. You come in the next morning to either a clean run or a detailed failure report, and nobody had to remember to manually run the tests.


Another example could be scheduling a weekly` fixme-fairy` agent that picks off one FIXME per week from your codebase and opens a PR. This way, tech debt gets addressed without anyone assigning tickets.


### [mngr_pair](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_pair)


This plugin introduces real-time bidirectional file sync between your local machine and a remote agent using[unison](https://github.com/bcpierce00/unison) . As the agent edits files, you see the changes locally. As you edit locally, the agent sees your changes.


```text
sh
```


Think of it as pair programming where your partner is an AI running in the cloud. With this, you can kick off a remote Claude agent to refactor a module, run` mngr pair` , and watch its changes appear in your local editor in real time. If you spot it going down the wrong path, you edit a file locally and the change syncs back immediately. With this workflow, you stay in the loop without needing to SSH into anything.


### [mngr_tmr](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_tmr) (Test Map-Reduce)


mngr_tmr collects tests via pytest, launches one agent per test to run them in parallel, polls for completion, and generates an HTML report. If a test fails, the agent can attempt to fix it. Successful fixes get pulled into local branches and optionally merged by an integrator agent.


The obvious benefit is speed. Imagine running 200 tests across 200 agents simultaneously instead of sequentially in one process.


But the bigger benefit is isolation for automatic fixing. When each test runs in its own agent, that agent can focus entirely on understanding one failure, reading the relevant code, attempting a fix, re-running the test, and opening a PR. If you put all 200 tests in one agent, a single failure blocks the whole run, and the agent has to context-switch between unrelated failures.


NOTE: This plugin is currently focused on pytest, but this is still being made more generic by the engineering team behind mngr.


### [mngr_wait](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_wait)


Blocks execution until an agent or host reaches a target state (DONE, WAITING, STOPPED, etc.).


```text
sh
```


For example, imagine a shell script that kicks off three agents in parallel, then runs` mngr wait agent-1 DONE && mngr wait agent-2 DONE && mngr wait agent-3 DONE` to block until all three finish. Then it pulls their output files with` mngr file get` and merges the results. Without` mngr_wait` , you'd be polling in a loop.


### [mngr_recursive](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_recursive)


By default, remote agents are sandboxed from launching other agents. Why? Mostly because you don't want an agent to fork-bomb your cloud account by spawning more agents endlessly. This plugin deliberately lifts that restriction by injecting the mngr installation, config, and API keys into the remote environment.


The result: an agent can call` mngr create` to spawn sub-agents. Essentially, your first agent acts as a mid-level manager.


So imagine that you have a "lead" agent that reads a PRD, breaks it into subtasks, and spawns one agent per subtask (one for API work, one for frontend development, one for writing tests). Each sub-agent works independently.


Of course, use this carefully. There is no built-in spending cap if an agent decides to spawn 50 children.


### [mngr_ttyd](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_ttyd)


This plugin runs a[ttyd](https://github.com/tsl0922/ttyd) web terminal server alongside each agent, giving you browser-based access to the agent's terminal.


When you create an agent, this plugin automatically adds an extra tmux window that launches a ttyd process on a random port. ttyd takes a terminal session and serves it as a web page. The plugin detects the assigned port and registers it as a service so mngr's forwarding server can proxy the terminal.


It also supports custom commands. If you place shell scripts in` $MNGR_AGENT_STATE_DIR/commands/ttyd/` , they get registered as separate URL endpoints. One URL for the plain terminal, another to trigger a specific action.


If you pair this with Cloudflare Tunnel or ngrok, you can get a live terminal view of your agent from any browser, including your phone.


### [mngr_notifications](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_notifications)


Sends desktop notifications when an agent transitions from RUNNING to WAITING, which means the agent finished its current task and needs your input. On macOS, clicking the notification opens a terminal tab connected to that agent.


This plugin requires` alerter` on macOS (` brew install vjeantet/tap/alerter` ) or` notify-send` on Linux.


### [mngr_tutor](https://github.com/imbue-ai/mngr/tree/main/libs/mngr_tutor)


An interactive tutorial for learning mngr. Run` mngr tutor` in a separate terminal, follow the instructions, and it auto-detects when you've completed each step by monitoring agent state, filesystem changes, and tmux sessions. No manual confirmation needed.


This tutorial covers the basics, like creating agents, sending messages, stopping, restarting, and destroying. This is a great starting point if you've never used mngr before.


## Where to start


Here are a few starting paths for you, depending on your work:


**Solo developers:** Install` mngr_claude` and` mngr_kanpan` . Create a Claude agent, open the Kanban dashboard, and get a feel for the lifecycle.


**Teams running parallel workloads:** Set up` mngr_modal` for remote agents and` mngr_schedule` for recurring tasks. Use a nightly test runner or a weekly` fixme-fairy` agent.


**Power users building multi-agent systems:** Combine` mngr_recursive` with` mngr_pair` and` mngr_wait` . Let a lead agent orchestrate sub-agents, sync their work to your local editor in real time, and script the merging of results when everything finishes.


Get started here:[github.com/imbue-ai/mngr](https://github.com/imbue-ai/mngr) .


Have questions or built something we should know about? Open a[Discussion](https://github.com/imbue-ai/mngr/discussions) on the repo and our team will jump on it.
