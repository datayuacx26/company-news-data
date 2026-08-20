---
schema_version: "1.0.0"
document_id: "ea1b83b792ce639e4953d05811b0d87d65b5fc84b8867f6883603ff3a9117973"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/opinion/ai-agent-storage-three-layers"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T20:54:28.432730+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:a02be36a4b7dc643e101bb11d5a1f4f9066c13125621d03514c442bb1a63757f"
---

# AI Agent Storage Needs Three Layers, Not One Filesystem

The argument about agent filesystems is too compressed.


Some people want a folder full of Markdown. Some want Git. Some want object storage. Some want a SQLite-backed virtual filesystem like[AgentFS](https://turso.tech/blog/agentfs) . Some want a standard project file like[FILESYSTEM.md](https://filesystem.md/) or[CLAUDE.md](https://code.claude.com/docs/en/memory) . Some want every tool call, file write, and piece of memory to land in one queryable store.


Those are all reasonable instincts. The mistake is expecting one filesystem-shaped thing to carry every storage responsibility for an AI agent.


Your agent does not need one magic filesystem. It needs three layers:


- a durable record for source, inputs, approvals, and final artifacts
- an active workspace where commands, files, processes, ports, and terminals live
- an evidence layer that explains what happened well enough for a human or another agent to trust the result


When those layers are separate, each can be honest. When they are collapsed into one folder, the product starts lying by accident.


## The filesystem debate is really three debates


The first debate is about **memory** .


Agents need instructions, preferences, task notes, summaries, and discovered facts. Claude Code's memory docs are a good example of the shape:` CLAUDE.md` files give persistent project instructions, while auto memory captures notes from corrections and preferences. The docs are explicit that these are context, not enforced configuration. That distinction matters. A memory file can guide the model, but it is not the same thing as permission, source control, process state, or a verified result.


The second debate is about **storage** .


Products need somewhere to put uploaded files, generated artifacts, logs, reports, datasets, model outputs, and backups. Object storage, databases, and repositories are good at this. AgentFS is interesting because it treats agent state as a filesystem abstraction backed by SQLite, with file-like paths, key-value state, and tool call audit in one database. That is a strong answer for portable, queryable state.


The third debate is about **work** .


This is the part storage posts often underweight. Agents do not only store data. They use computers. They install packages, run tests, start dev servers, keep REPLs alive, tail logs, modify files, hit local ports, open browsers, create temporary directories, cache dependencies, and recover from half-finished commands.


That active layer is not just a durable byte store. It is a working environment.


## What to look out for with one-filesystem designs


The first warning sign is a filesystem that claims to be memory, runtime, audit log, source control, permissions, and artifact store at once.


That might be a useful product internally. It is not a useful mental model for the agent or the user. A directory of notes should not pretend to prove that a test passed. A queryable database of tool calls should not pretend to be the running server. A copied worktree should not pretend to include the terminal session where the package manager is waiting for input.


The second warning sign is a storage model that only describes bytes at rest.


A persistent folder is better than a stateless command runner, but it still leaves major questions unanswered. Is the server still running? Did the database migration complete? Is the browser profile logged in? Which port serves the preview? Did the test watcher finish, hang, or keep streaming failures? Can another process attach to the same terminal and inspect the current screen?


For many agent products, those facts matter more than the final directory listing.


The third warning sign is treating the active workspace as the final record.


A workspace should be messy. Agents need room to try things, install dependencies, leave scratch files, generate failed attempts, and branch before risky work. That does not mean the workspace should become the permanent human-facing truth. For code, review should still happen through a branch, diff, commit, or pull request. Pair the runtime with a version control system when changes need durable review.


The fourth warning sign is hiding evidence inside chat.


If the agent says "the test passed," the product should be able to show more than a sentence. It should have the command, exit status, stdout/stderr, relevant files, preview URL, logs, or terminal session. Chat is a narrative. Evidence is the trail that lets a user trust or reject the narrative.


## Comparing the three layers for AI agents


The cleanest architecture is not "filesystem vs database vs Git vs object storage." It is assigning each layer a job.


Layer Best for Bad at


Durable record source code, user uploads, approved changes, final artifacts, long-term retention representing a live process or terminal


Active workspace running commands, editing files, installing packages, serving ports, keeping terminals alive being the final review artifact


Evidence layer logs, traces, stdout, screenshots, diffs, command history, preview URLs replacing the environment where work happened


This makes the storage decision less ideological.


Use a repository when the question is "what changed and can a human review it?" Use object storage when the question is "where should this artifact live after the job?" Use a database when the question is "how do we query state, metadata, or audit events?" Use a VM when the question is "where does the agent actually do the work?"


Those are different questions. Forcing one substrate to answer all of them is how agent platforms become fragile.


## The active layer should be a real computer


The active layer needs to be more than a directory or a command executor. It needs to be a real, durable computer—one that preserves its state between steps, survives client disconnects, and can be inspected while work is still in progress.


That means the runtime should hold its full memory across pauses. When the agent goes idle, the environment should hibernate with every running process, open file descriptor, in-flight server, and terminal session intact. When work resumes—triggered by a request, a webhook, or a schedule—nothing is rebuilt and nothing is re-executed. The environment that existed before the pause is the environment that continues. A VM does not reset between turns any more than a developer's laptop resets when they close their laptop lid.


This matters for cost too. "Run forever" does not mean "burn CPU continuously." Idle time is hibernation; active time is execution. The state is preserved across both.


The active layer also needs interactive terminals, not just command runners. A long-lived shell that can be attached, detached, and reattached—surviving disconnects, suspends, and even forks of the environment—is what lets an agent leave a package manager open, pause at a debugger prompt, keep a REPL warm, or watch a dev server stream logs without losing the thread. A command runner that exits after each invocation makes the filesystem carry state it was never designed to carry.


Services running inside the workspace need real URLs. If an agent builds a web app, starts a notebook, or runs a webhook receiver, the product should be able to expose that live service directly instead of reducing it to a downloaded artifact. URL routing that maps hostnames to ports inside the running environment closes the gap between "the file exists" and "the thing works."


Access control rounds out the model. The workspace should be operable by the agent, a human reviewer, and the end-user without requiring every party to hold a root credential. Scoped identities and delegated sessions let those actors share the same machine safely.


[Freestyle VMs](https://www.freestyle.sh/products/vms) are built around this model. They are hardware-virtualized Linux VMs that run indefinitely, hibernate with full memory state, support long-lived PTY sessions that survive disconnects and forks, expose services over HTTPS preview domains, and grant access through scoped identities and client sessions. They are designed specifically so the active workspace does not have to pretend that a directory listing is the whole truth.


## The durable layer should not be the active layer


There is a fair objection here: if the VM is where work happens, does the VM become the source of truth?


It should not.


The VM is the workbench. It is allowed to be temporary, forked, messy, and eventually deleted. Important state should graduate out of it.


Source code should move through Git. Approved artifacts should move to durable storage. Long-term metadata should move to a database. Logs and traces should move to whatever observability system your product uses. User-visible claims should point back to evidence. The active workspace is where the agent discovers and proves things; it is not necessarily where every proof should live forever.


This is why the "one filesystem" framing causes trouble. It makes teams choose between durability and usefulness. A database-backed agent filesystem may be great for audit and portability. A Git repo may be great for review. Object storage may be great for artifacts. A VM may be great for execution. The product should compose them instead of asking one layer to impersonate the others.


## A practical checklist


When evaluating agent storage, ask these questions in order:


- What state must survive deletion of the workspace?
- What state only matters while the agent is working?
- What evidence must be shown to a user, reviewer, or future agent?
- What must be branchable before a risky step?
- What must be inspectable while it is still running?
- What belongs in Git, object storage, a database, logs, or the VM?


If every answer points to the same filesystem, the design is probably too vague.


For a simple batch agent, one folder may be enough. Read input, write output, upload artifact, delete the environment. Do not overbuild that.


For coding agents, app builders, QA agents, browser agents, long-running research agents, and self-hosted tool agents, one folder is rarely enough. The agent needs a durable record, a real active workspace, and an evidence trail. The active workspace should be a real computer with files, processes, terminals, ports, users, and lifecycle—one that persists state across idle periods so the agent is never forced to fake continuity with a static snapshot.


That is the more useful conclusion from the filesystem drama. Files are a good interface. Storage is necessary. Memory matters. But the agent's work happens in a computer, and the storage architecture should admit that from the start.
