---
schema_version: "1.0.0"
document_id: "3fe9918c1b2fbc651b4999fc1482b2e74a01d400376ebddbee6d8e801daec94e"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/product/persistent-volumes-vs-vm-snapshots-ai-agents"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T20:54:28.432730+00:00"
fetched_at: "2026-07-28T21:23:24.617893+00:00"
content_hash: "sha256:f888595c4c9534db63fca95c7435feaba889c2d8b9da8b308979f9156c2acbf4"
---

# Persistent Volumes vs VM Snapshots for AI Agents

AI agents do not need a bigger hard drive. They need a workspace they can trust.


That sounds like a storage problem, so teams reach for persistent volumes, mounted buckets, synced folders, and custom file APIs. Those are useful tools. They keep bytes around. They give agents somewhere to write intermediate files. They make it possible to stop a sandbox and later find the same directory tree.


But persistent storage is not the same thing as agent memory, and it is not the same thing as a resumable computer.


The current AI filesystem debate is really about that boundary. Microsoft Research's["Don't Let AI Agents YOLO Your Files"](https://www.microsoft.com/en-us/research/publication/dont-let-ai-agents-yolo-your-files-shifting-information-and-control-to-filesystems-for-agent-safety-and-autonomy/) paper studied public reports of agent filesystem misuse and found recurring failures around corrupted data, deleted files, leaked secrets, weak control, and weak visibility into filesystem effects. Projects like[FILESYSTEM.md](https://filesystem.md/) argue for a standard place to tell agents how to interact with a project's files. Letta's[MemFS](https://docs.letta.com/letta-code/memfs) stores agent memory as Markdown files in a local Git repository. Cloudflare's[Sandbox file docs](https://developers.cloudflare.com/sandbox/guides/manage-files/) show file operations and syncing inside sandbox filesystems, and E2B documents[isolated sandbox filesystems](https://e2b.dev/docs/filesystem) for agent code execution.


All of that is directionally right: agents need files, and the file layer needs product semantics.


The mistake is stopping there. A serious agent product does not merely need persistent files. It needs a runtime state model: files, processes, terminals, ports, installed packages, browser profiles, local databases, caches, background services, and a way to branch or roll back the whole working environment.


That is where persistent volumes and VM snapshots diverge.


## What a persistent volume is good at


A persistent volume answers one important question: "Will these bytes still be here later?"


That is valuable. If an agent downloads a dataset, builds a dependency cache, writes generated artifacts, or stores a working tree between turns, throwing all of that away after every command is wasteful. Persistent volumes reduce rework and make the agent feel less amnesic.


They are a good fit for:


- project directories
- downloaded inputs
- package caches
- generated artifacts
- logs that should outlive a process
- files that need to be copied into or out of a sandbox
- durable state for tools that are happy to restart


For short code execution, this may be enough. The agent writes a script, runs it, reads stdout, stores a result, and exits. A persistent filesystem gives the next run a place to pick up files from the previous run.


The problem starts when the product treats the volume as the whole agent state.


A volume usually does not know whether the dev server is still running. It does not know which terminal had a failing migration half typed into it. It does not preserve a process's memory. It does not prove that port 3000 still serves the app the agent claims it fixed. It does not tell you whether a file watcher, queue worker, database, browser, or REPL is alive.


Those facts live in the machine, not just on the disk.


## What to look out for with agent filesystems


The first warning sign is **disk-only continuity** .


The product says the agent is resumable because the filesystem persists. Then the agent comes back and has to rediscover everything: current directory, shell environment, running services, open logs, database state, browser session, and why the last command hung. Some of that can be reconstructed from files. Some cannot.


The second warning sign is **unclear truth** .


If the agent writes to a mounted volume, the user reviews a generated summary, the app previews from a running process, and the source branch lives somewhere else, which one is authoritative? This is how storage bugs become trust bugs. The VM disk can be the workbench, but reviewable source should still move through a repository. If the agent is changing code, pair the workspace with[Freestyle Git](https://www.freestyle.sh/products/git) or another Git system so branches, diffs, commits, and rollback are not hidden inside a mutable disk.


The third warning sign is **fake forkability** .


Copying a directory is not the same as forking the current work. A directory copy may capture files, but it misses live process state and often loses the context that made the moment valuable. For agents, branching matters most at decision points: before a risky refactor, before chasing a flaky failure, before trying three dependency upgrades, or before letting multiple workers explore alternatives.


The fourth warning sign is **files without a computer** .


Agents use boring operating-system conventions because those conventions work. They run` npm install` ,` pytest` ,` apt-get` ,` docker compose` ,` curl localhost` ,` tail -f` ,` systemctl status` , and` git diff` . The filesystem is one part of that interface. The process model, network stack, users, package manager, and terminal are the rest.


If your storage abstraction cannot support the rest, the agent will keep needing escape hatches.


## Comparing persistent volumes and VM snapshots


Persistent volumes and VM snapshots both preserve state, but they preserve different kinds of state.


Requirement Persistent volume VM snapshot or fork


Keep files between commands Strong fit Strong fit


Cache installed packages Good if tools can restart cleanly Better when the whole prepared machine should be reused


Resume a long-running terminal Not enough by itself Fits when terminal sessions are part of VM lifecycle


Branch before a risky action Usually means copying files Fork the working machine at the decision point


Preserve a running service for preview Not enough by itself Fits when ports and processes are part of the workspace


Recreate a known baseline Good for file trees Better for full environments with runtimes and services


Explain code changes to a human Needs Git or another review layer Still needs Git or another review layer


This is not an argument against persistent volumes. It is an argument against pretending that a volume is an agent runtime.


A persistent volume is the right primitive when the state you care about is files. A VM snapshot is the right primitive when the state you care about is a working environment.


That distinction becomes obvious in app builders, QA agents, browser agents, code interpreters that install native dependencies, and long-running coding agents. The useful state is not only` src/App.tsx` . It is also the installed Node version, the running dev server, the package manager cache, the local database, the process that is streaming logs, and the preview URL the user is looking at.


## Why VM snapshots fit agent truth


[Freestyle VMs](https://www.freestyle.sh/products/vms) are the most powerful VMs for AI agents: they are hardware-virtualized, run real Linux, can run forever when configured to stay running, and expose the machine through an API instead of a narrow code cell.


The[Freestyle VM docs](https://www.freestyle.sh/docs/vms) describe full Linux VMs that can execute commands with` vm.exec()` , read and write files through` vm.fs` , resize CPU, memory, and storage, stop and start while preserving disk, and route web traffic from a domain to a port inside the VM. The[lifecycle docs](https://www.freestyle.sh/docs/vms/lifecycle) make the state model explicit: a running VM accepts commands, SSH sessions, and network traffic; stopping preserves disk but not memory; forking creates a new VM from the current running state; and` idleTimeoutSeconds: null` is the option for workloads that should keep running until you stop or delete them.


That gives product builders a cleaner model than "save some files and hope the agent can reconstruct the rest."


Snapshot the prepared environment once. Boot agent workspaces from it. Let the agent install packages, run services, open terminals, edit files, and expose ports. Before a risky path, fork the current VM instead of copying a directory. If the path fails, delete that fork. If it works, keep the result and push the reviewable source changes through Git.


The[PTY docs](https://www.freestyle.sh/docs/vms/pty) are especially important for this comparison. A Freestyle PTY is a long-lived interactive shell inside the VM. It can be attached, detached, and reattached over WebSocket. Sessions survive client disconnects, VM suspends, and VM forks. That means an agent can keep a real terminal session as part of its workspace instead of reducing every interaction to a completed command and a log blob.


The[domain docs](https://www.freestyle.sh/docs/vms/domains) close another gap. A service running inside the VM can be mapped to a public HTTPS domain, including zero-setup` *.style.dev` preview domains. For generated apps and browser-driven workflows, that matters because "the files exist" is not the same claim as "the running app works."


## When a persistent volume is enough


Use persistent volumes when your agent workflow is file-first and process-light.


Examples:


- batch code execution where each run exits quickly
- document processing pipelines
- artifact staging
- cache reuse between short jobs
- data preparation tasks
- environments where restart cost is low and live state is irrelevant


In those cases, a persistent volume can be simpler and cheaper than preserving a whole machine. The agent does not need a live world; it needs a durable directory.


The fairness matters. Not every agent needs a VM snapshot. Many useful agents are just tool callers with a scratch directory and a few durable artifacts. Overbuilding the runtime can make the product harder to operate.


But if your agent starts acting like a developer, tester, operator, researcher, or app builder, disk-only persistence usually becomes the wrong abstraction. The agent is no longer just producing files. It is inhabiting an environment.


## The better storage question


The useful question is not "does the sandbox have persistent storage?"


The useful question is: "What state does the agent need to treat as true when it resumes?"


If the answer is only files, a persistent volume is probably enough. If the answer includes processes, terminals, ports, installed tools, local services, browser state, or parallel exploration, then you need a computer-shaped state primitive. That usually means a real VM with snapshots, forks, lifecycle controls, terminals, and domain routing.


Persistent volumes keep bytes. VM snapshots keep worlds.


Agent products need both at different layers, but they should not confuse them. Store artifacts in durable storage. Put reviewable source in Git. Run the agent inside a real Linux VM when the work depends on a live environment. That split gives the agent a filesystem without pretending the filesystem is the whole truth.
