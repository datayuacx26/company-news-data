---
schema_version: "1.0.0"
document_id: "137ce639efd3c01d26bbc923c72502ac4456fed44ac8dd1661796e7816363a65"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/e2b-session-limit"
published_at: "2026-08-19T16:29:14+00:00"
first_seen_at: "2026-08-19T18:44:07.874563+00:00"
fetched_at: "2026-08-19T18:44:10.590664+00:00"
content_hash: "sha256:4460964a33e53c1e74d12d5ce343535c96afe9803fccf226b550570bb22570e2"
---

# The E2B session limit (and what happens to agent state)

Your coding agent is deep into a multi-hour refactor across dozens of files. It has an indexed codebase, installed dependencies, running language servers, and a half-finished migration. If the sandbox is left on the default kill behavior, E2B terminates it when the session limit arrives. Everything not saved outside the sandbox is gone, and the task restarts from zero.


E2B enforces a[24-hour maximum continuous session](https://e2b.dev/docs/sandbox) on the Pro plan. The Hobby plan limit is one hour. A sandbox that hits that session limit without a pause configuration is killed. E2B does offer pause and resume. That resets the session limit clock and preserves full sandbox state. But the session limit still forces architectural decisions on any team running agent workflows that span hours or days.


**TL;DR:**


- **Hard session caps:** E2B enforces a 24-hour maximum on Pro and one hour on Hobby. The default behavior kills the sandbox when the limit arrives.
- **State destruction on kill:** Filesystem, installed dependencies, in-memory data, and running processes are all lost under the default timeout configuration.
- **Pause resets the clock:** Configuring auto-pause preserves full sandbox state and resets the session limit, but requires orchestration planning and reconnection logic.
- **Workaround options:** Teams can build pause/resume timers, implement application-level checkpointing, or move to a platform without session limits.
- **Short tasks unaffected:** Code interpreters and single-dataset jobs finish in minutes. The limit shapes architecture for multi-hour coding agents and persistent environments.


## **How E2B's session timeout works**


Every E2B sandbox has a configurable timeout, and the sandbox terminates when it expires. The default is[300 seconds](https://e2b.dev/docs/sdk-reference/python-sdk/v2.27.0/sandbox_async) . That default makes explicit timeout configuration necessary before long-running agents reach the plan limit. Teams set it through` timeoutMs` in the JavaScript SDK or` timeout` in Python.


The maximum is plan-dependent, with the longer session limit reserved for Pro. The Pro plan costs[$150 per month](https://e2b.dev/pricing) . That price identifies the tier with the 24-hour limit, but it does not remove the need to design around the session limit. Teams can extend a running sandbox with` setTimeout` (JavaScript) or` set_timeout` (Python). The new value counts[from the moment of the call](https://e2b.dev/docs/api-reference/sandboxes/set-sandbox-timeout) , not from sandbox creation.


The Pro session limit is a hard limit on continuous execution. No documented` setTimeout` call pushes a sandbox past its plan limit. For longer sessions, use pause and resume. E2B's persistence docs state that[pausing resets the runtime window](https://e2b.dev/docs/sandbox/persistence) . It also saves both filesystem and memory state. Paused sandboxes are kept indefinitely, and[billing stops immediately](https://e2b.dev/docs/billing) once a sandbox pauses, is killed, or times out.


Default behavior decides what the timeout actually does. The` onTimeout` option defaults to` kill` . An unconfigured sandbox that reaches its timeout is terminated. E2B's docs define the killed state as terminal: all resources are released, and no recovery path exists. The docs describe no shutdown signal to the agent before termination, so treat unsaved work as lost.


Teams can instead set` onTimeout: 'pause'` at creation. That snapshots the sandbox rather than killing it. The session limit is a documented platform constraint. But its consequences for long-running agent workflows deserve planning before you commit.


## **What happens to agent state when the limit hits**


Preparation determines what reaching the session limit costs. A team that configured auto-pause loses nothing beyond a reconnection cycle. The same is true for a team that built pause logic into its orchestration. A team relying on the default` kill` behavior loses every category of in-progress state at once. Filesystem state and in-memory state carry different recovery costs, so they're worth examining separately.


### **Filesystem and installed dependencies**


When E2B kills a sandbox at the session limit, the filesystem is destroyed with it. Cloned repositories, node_modules directories, Python virtual environments, generated files, and intermediate outputs all disappear.


Recovery means rebuilding the repository environment and regenerating everything the agent produced. For a coding agent on a large monorepo, that setup accumulated over real working time. A single` npm install` or` pip install` on a heavy dependency tree can run for minutes. That setup finishes before the agent does any work. For a data analysis agent that loaded a large dataset, the reload alone can stretch to minutes.


Pause avoids all of this. Per E2B's persistence docs, a full pause saves the entire filesystem in the snapshot. Resume restores every file. A filesystem-only pause option also exists (` keepMemory: false` in JavaScript,` keep_memory=False` in Python). It saves disk state but[cold-boots on resume](https://e2b.dev/docs/api-reference/sandboxes/pause-sandbox) and discards memory and processes. That cold boot reboots the sandbox from its disk, so files on disk (including installed packages saved to the filesystem) survive but running services do not, and the start command is not re-run.


Either way, the snapshot must exist before the timeout fires under a` kill` configuration. Filesystem state is the most expensive category to rebuild. Rebuilding it means repeating accumulated setup work rather than restoring a single recoverable artifact.


### **In-memory state and running processes**


Running processes die with the sandbox. Language servers, file watchers, background indexers, and long computations are all killed. In-memory data structures and caches are killed too. A full pause preserves them. E2B's persistence docs state that running processes, loaded variables, and data are restored on resume. The mechanics have measurable costs.


The persistence docs give rough timing:[pausing takes four seconds per one GiB of RAM](https://e2b.dev/docs/sandbox/persistence) , and resuming takes about one second. At that documented rate, a large-memory sandbox can take long enough to require scheduling margin. That pause duration is why orchestration should leave margin before the deadline.


Open network connections don't survive the cycle. Services inside the sandbox stay reachable after resume, but clients must reconnect. A reconnection cycle means re-opening external sessions and connection pools. Those connections time out on the remote side while the sandbox is paused.


That has a direct architectural consequence for agents that depend on long-running background processes. They need an orchestration layer that verifies those processes are healthy after resume. It also has to re-establish external connections. In-memory state is harder to recover than filesystem state because it includes ephemeral computation results. A half-built code index or partially reduced dataset may only be reproducible by re-running the work that created it.


## **Architectural patterns for working within the session limit**


Teams building on E2B can manage the session limit with pause/resume plus checkpoints, or move to a platform without a session limit. Each session-limit workaround carries a different implementation cost, and they aren't mutually exclusive. Each workaround has a different failure mode.


### **Build proactive pause/resume into the orchestration layer**


Set` onTimeout: 'pause'` at sandbox creation so the timeout snapshots the sandbox instead of killing it. Pair that with an orchestration timer that pauses well before the session limit. That leaves a safety margin for the snapshot to complete. Size that margin from the pause cost above: a large-memory sandbox needs tens of seconds of headroom.


[Auto-resume](https://www.e2b.dev/docs/sandbox/auto-resume) then restores the sandbox when HTTP traffic or an SDK operation arrives. It restarts with a minimum five-minute timeout. If a resumed sandbox times out again, it auto-pauses again.


The complexity lands in the orchestration layer. Every workflow must reconnect clients and verify process health before continuing from the correct step. Operations that can't be cleanly interrupted need special handling. A mid-write database transaction or in-flight API request may re-execute after resume. Those steps must be idempotent or moved outside the pause boundary.


An idempotency audit walks each tool call and asks whether re-running it duplicates a side effect. Examples: a second payment or a duplicate row. Start by auditing which of your agent's operations produce external side effects. Those define where a pause can safely land.


### **Implement application-level checkpointing**


Instead of trusting the platform snapshot, the agent periodically serializes its own state to external storage. That storage can be a database or object storage. Persistent volumes work too. If the sandbox dies unexpectedly, the agent recovers from the last checkpoint rather than from zero. Checkpoint frequently, and the agent loses only the progress made since the last durable save.


Application-level checkpointing requires the most engineering. The agent must serialize its task progress, in-memory context, next-step plan, and files. A checkpoint schema records the conversation history, including completed tool invocations and the pointer to the next step. That lets a fresh sandbox replay from that record.


Orchestration frameworks ship primitives for this. LangGraph's[checkpointer layer](https://docs.langchain.com/oss/python/langgraph/persistence) persists thread-scoped state to durable backends. Durable execution engines like[Temporal](https://docs.temporal.io/workflow-execution) recover by replaying an append-only event history. LangGraph's` InMemorySaver` loses everything on process restart, so production deployments need a Postgres or SQLite backend.


Application-level recovery[misses OS-side effects](https://arxiv.org/html/2604.28138) such as filesystem modifications and spawned processes. Pair checkpoints with a repeatable environment rebuild. Checkpointing protects against session limits and unexpected crashes alike. That makes it the defensive choice for teams needing resilience beyond platform snapshots.


### **Evaluate platforms without session time limits**


For teams whose agents routinely exceed E2B's session limit, a platform that never enforces one removes the E2B-specific constraint.[Blaxel Sandboxes](https://blaxel.ai/blog/codesandbox-alternatives) can remain in standby indefinitely. Idle sandboxes transition to standby after[15 seconds of network inactivity](https://docs.blaxel.ai/Sandboxes/Overview) . They resume in under 25ms with filesystem, memory, and running processes preserved.


With no session limit to engineer around, the E2B-specific orchestration timer can disappear from the codebase. Teams can reserve application-level checkpointing for crash recovery, durable persistence, or other resilience requirements. The models solve different problems. E2B's pause exists to reset a session limit. Standby on a perpetual sandbox platform is a cost mechanism, since there's no session limit to reset.


Switching platforms requires SDK/template migration and billing-model changes. Ongoing engineering cycles have their own cost. That cost compounds when teams maintain session-limit workarounds on every workflow. The accumulated cost can exceed a one-time migration. Compare maintenance and incident-recovery hours against the one-time migration effort.


## **When the session limit does and doesn't matter**


Most E2B workloads never touch the session limit. A code interpreter that runs a script or an analysis agent that processes a CSV finishes in minutes; small tool-calling jobs usually do too. For those tasks the session limit is irrelevant, and E2B's strengths carry real weight.


The platform is[open source](https://github.com/e2b-dev/E2B) under Apache 2.0 and isolates workloads with Firecracker microVMs. It also loads template snapshots in[about 80ms](https://e2b.dev/docs/template/how-it-works) with processes already running. That speed makes E2B a strong fit for short-lived agent tasks.


Long-running[coding agents](https://blaxel.ai/blog/ai-observability) are the first workload to hit it. Rakuten used Claude Opus 4 to code autonomously for[close to seven hours](https://www.technologyreview.com/2025/05/22/1117338/anthropics-new-hybrid-ai-model-can-work-on-tasks-autonomously-for-hours-at-a-time/) on a complicated open-source project. Anthropic tracked its[99.9th-percentile agent turn duration](https://www.anthropic.com/news/measuring-agent-autonomy) between[October 2025 and January 2026](https://www.anthropic.com/news/measuring-agent-autonomy) . It nearly doubled, from under 25 minutes to over 45 minutes.


Data analysis agents that load large datasets and iterate over hours can hit the session limit too. Persistent agent environments expected to stay available across sessions without explicit pause management face the same constraint.


Task horizons are also stretching. McKinsey reports the length of tasks AI can reliably complete has[doubled roughly every seven months since 2019](https://www.mckinsey.com/capabilities/people-and-organizational-performance/our-insights/the-agentic-organization-contours-of-the-next-paradigm-for-the-ai-era) . The pace has accelerated since then. A workload that fits inside the current session limit today may not fit next year. Which choice fits depends on how long the agents actually run.


E2B's developer experience makes it a strong platform for agents that finish in minutes. Firecracker isolation and an open-source model strengthen that fit. For hour- or day-long runs, the session limit shapes every design decision.


## **How to decide whether E2B's session limit affects your architecture**


Ignoring the session limit produces ongoing engineering investment rather than a one-time surprise. That investment includes pause timers, idempotency audits, checkpoint schemas, and workaround maintenance. It compounds with every agent workflow the team ships. For workloads that complete inside the window, E2B's developer experience makes it a strong platform. Firecracker isolation and an open-source model strengthen that fit.


Teams that need sessions beyond E2B's maximum continuous session without pause/resume plumbing can run them on[Blaxel Sandboxes](https://blaxel.ai/sandbox) . Blaxel is best suited for[coding and codegen agents](https://blaxel.ai/blog/llm-coding-benchmarks) , with a secondary fit for long-running analysis and persistent agent environments.


For shared context and artifacts across sandboxes or sessions,[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) provides a distributed filesystem. For data that must outlive a sandbox entirely,[Volumes](https://docs.blaxel.ai/Sandboxes/Volumes) provide persistent block storage that survives sandbox destruction and recreation.


Talk to the team at[blaxel.ai/contact](https://blaxel.ai/contact) or start building at[app.blaxel.ai](http://app.blaxel.ai/) .


Skip the session limit engineering


No session cap, no pause timers, no idempotency audits. Sandboxes stay in standby indefinitely and resume in under 25ms.


[Start building free](https://app.blaxel.ai/)


## **FAQ**


#### **What is E2B's maximum session duration?**


E2B's maximum session duration depends on plan: Pro has the longer continuous-session limit, while Hobby has the shorter one. Under the default configuration, reaching the session limit terminates the sandbox. Pausing before timeout resets the session limit clock and preserves sandbox state as a snapshot, so teams running long jobs should configure that behavior deliberately.


#### **What happens to agent state when an E2B sandbox times out?**


Under the default onTimeout: 'kill' setting, the sandbox enters a terminal state and all resources are released. Cloned repositories, installed dependencies, generated files, in-memory data, and running processes are destroyed. Recovery requires starting over unless the sandbox was configured to pause on timeout or the agent saved durable checkpoints outside the sandbox.


#### **How do you work around E2B's session limit?**


Common workarounds include managing E2B with pause/resume orchestration and serializing application-level checkpoints to external storage. Teams that keep running past the session limit can adopt a[platform without one](https://www.blaxel.ai/products) . Proactive pause is the lowest-friction option for existing E2B users, while checkpointing gives stronger resilience at higher engineering cost and protects against crashes beyond planned timeouts.


#### **Does E2B's session limit matter for short-lived agent tasks?**


No. Code interpreter runs and single-dataset analysis jobs typically finish in minutes, far inside the session limit; tool-calling tasks usually do too. The limit matters for coding agents working multi-hour refactors. It can also affect analysis agents iterating over large datasets for hours and persistent agent environments expected to stay available without explicit pause management or external checkpointing.
