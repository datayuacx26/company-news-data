---
schema_version: "1.0.0"
document_id: "d22af79ed172c8d47c55e9e5d40d751c8c75c432eef803e0bc5eb066b63afaa5"
company_key: "yc-hyperspell"
company: "Hyperspell"
source_id: "yc-hyperspell-news-import-13fe3511aa44"
canonical_url: "https://www.hyperspell.com/blog/multi-agent-memory"
published_at: null
first_seen_at: "2026-08-09T23:13:54.426753+00:00"
fetched_at: "2026-08-09T23:13:55.393599+00:00"
content_hash: "sha256:d5e5b7c3c63e99c41fb0e458f9429bfd26e0bf8476883c977b0585e13d30b8d3"
---

# Multi-Agent Context and Memory: Who Remembers What

Three agents are working your sales pipeline. The research agent finds that a prospect mentioned a competitor in last week's email. The analysis agent compares pricing. The writing agent drafts the follow-up.


Here's the question no one asks until it's too late: does the writing agent know about the competitor mention?


Single-agent memory is straightforward. One context window, one memory store, one read/write cycle. But multi-agent systems multiply every coordination challenge. Whose memory is canonical? Who can see it? Who can update it?


This isn't a theoretical concern.[Research from Cemri et al.](https://arxiv.org/abs/2503.13657) found that inter-agent misalignment accounts for 36.9% of all failures in multi-agent systems. Not communication failures. State misalignment: agents operating on inconsistent views of shared state, causing duplicated work, divergent assumptions, and cascading errors.


The patterns for handling this are becoming clearer. Here's what we've learned at Hyperspell building memory infrastructure that serves multi-agent architectures.


## The coordination problem


A single agent reads from and writes to one memory store. Add a second agent and you've introduced three new questions: Can Agent B see what Agent A wrote? Can both write to the same memory? What happens when they disagree?


Add a third agent and those questions compound.[Anthropic found](https://www.anthropic.com/engineering/built-multi-agent-research-system) that multi-agent systems use roughly 15x the tokens of typical chat interactions, largely due to coordination overhead and redundant retrieval.


The computer science community recognized this class of problem decades ago. CPUs solved it with cache coherence protocols. Recent research applies the same architectural lens to LLM agents, proposing a three-layer memory hierarchy:


-


**I/O layer** : information ingestion and emission (text, images, API calls)


-


**Cache layer** : fast, limited capacity for immediate reasoning (compressed context, KV caches)


-


**Memory layer** : large-capacity persistent storage (dialogue history, vector databases, document stores)


Multi-agent coordination touches all three layers. The cache layer is particularly tricky: there's no principled protocol yet for transforming and reusing one agent's cached results across other agents.


## Memory sharing patterns


Three patterns dominate production systems. Most teams start with one and evolve toward another.


Pattern


How it works


Best for


Watch out for


**Shared memory**


All agents read/write one pool


Small teams (2-3 agents), same task, low write frequency


Conflicts, noise, permission complexity


**Isolated memory**


Each agent has private memory


Independent parallel tasks


Duplication, missed context at handoffs


**Scoped sharing**


Private + shared tiers with promotion rules


Production systems, mixed trust levels


Implementation complexity, scope design decisions


### Shared memory (single pool)


All agents read and write to the same memory store. Vector database, document store, knowledge base: doesn't matter. Everyone sees everything.


This is the default in frameworks like CrewAI, where all agents in a crew share the crew's memory unless explicitly scoped otherwise.


The appeal is simplicity. One store, one source of truth, no synchronization headaches. But the problems surface fast: conflicts when two agents update the same fact simultaneously, noise when one agent's working notes pollute another's retrieval results, and permission complexity when agents have different trust levels.


Shared memory works best for small agent teams (two or three) working on the same task with low write frequency.


### Agent-specific memory (isolated)


Each agent has its own private memory. No shared state at all.


Google's Agent Development Kit takes this approach with its["Agents as Tools" pattern](https://developers.googleblog.com/architecting-efficient-context-aware-multi-agent-framework-for-production/) : specialized agents receive focused prompts with minimal context and no conversational history. Anthropic's multi-agent research system works similarly. Subagents receive task-specific instructions from the lead agent rather than full context.


The upside is clean boundaries. No conflicts, no noise, no permission issues. The downside is that agents miss each other's context entirely. Multiple agents discover the same fact independently. Handoffs lose information because there's no shared substrate.


Isolated memory works best for independent parallel tasks where agents genuinely don't need each other's findings.


### Scoped sharing (hybrid)


Some memory shared, some private. This is where most production systems end up.


The pattern we see most often defines explicit promotion rules: personal memory stays agent-isolated, task memory is visible to agents on the same task, and organizational memory is available to everyone. Scope becomes a first-class design choice, not an afterthought.


Recent academic work formalizes this as a two-tier architecture: private memory fragments visible only to their originating agent, plus shared fragments with asymmetric, time-evolving access controls.


Three scopes to define:


-


**Task scope** : intermediate work products, scratchpad notes, partial findings. Lives for one task, garbage collected after.


-


**Agent scope** : role-specific knowledge, learned preferences. Private to each agent, persists across tasks.


-


**Global scope** : shared facts, user preferences, organizational knowledge. Available to all agents always.


In plain English: most teams start with shared memory because it's simple, discover noise and conflict problems, and evolve toward scoped sharing. If you're building for production, start with scoped sharing and save yourself the migration.


## Consistency challenges


Even with the right sharing pattern, consistency is hard. Three failure modes show up repeatedly.


### Read-after-write visibility


Agent A writes a new fact. Agent B queries immediately. Does Agent B see it?


In shared vector stores, embedding and indexing add latency. The write isn't searchable until indexing completes. Most memory systems are eventually consistent, which means there's always a window where Agent B sees stale data.


For sequential workflows (research, then analysis, then writing), this rarely matters. Each agent finishes before the next starts. For parallel agents sharing a memory store, it's a real problem.


### Write conflicts


Two agents update the same memory simultaneously. What wins?


This is the most pressing open challenge in multi-agent memory. Emerging approaches borrow from distributed systems: CRDTs (conflict-free replicated data types) allow independent writes with guaranteed eventual consistency, and CQRS separates the write model from the read model. We covered the broader problem of conflicting data in our article on memory conflict resolution. The multi-agent case adds concurrency on top.


The practical reality: most teams use last-write-wins and accept occasional stale reads. It works until an agent overwrites a critical fact that another agent just discovered.


### Stale reads


Agent B has a cached or outdated view of memory. It doesn't see Agent A's recent updates, and acts on old information.


This is particularly dangerous in multi-step workflows. If Agent A discovers the prospect's budget doubled and Agent B drafts a proposal based on the old number, the output is wrong in a way that's hard to detect.


Mitigation starts with timestamp metadata on all memories. Query-time freshness filters help. Event-driven notifications when memory changes (like webhooks that fire on index completion) let agents react to updates rather than polling for them.


## Coordination patterns


Four patterns for how agents stay in sync on memory changes.


### Message passing


Agents explicitly communicate changes. Memory stays private; updates flow through messages.


This is AutoGen's approach: coordination happens through conversation history rather than a unified memory system. Agent A finishes, passes its findings to Agent B in a structured message.


Clear boundaries, manageable complexity. Works well for sequential handoffs where one agent's output is the next agent's input.


### Event-driven updates


Memory changes emit events. Interested agents subscribe and react.


This requires event infrastructure (pub/sub, webhooks) but decouples agents from each other. Agent A doesn't need to know which other agents care about its findings. It just writes to memory, and the infrastructure handles notification.


### Central coordinator


An orchestrator agent manages memory access. It serializes writes, broadcasts updates, and controls what each agent sees.


[Anthropic's multi-agent research system](https://www.anthropic.com/engineering/built-multi-agent-research-system) uses this pattern: the lead agent maintains overall research state while subagents report findings back. Google ADK's orchestrator controls context transmission with a configurable parameter that determines whether subagents receive full working context or just new prompts.


The simplest pattern to reason about, but a bottleneck at scale. The coordinator becomes a single point of failure and a throughput ceiling.


### Optimistic concurrency


Agents write freely. The system detects and resolves conflicts after the fact.


CrewAI takes a version of this approach internally, serializing database operations with a shared lock and retrying on conflict. It works when write frequency is low relative to read frequency, which describes most agent workflows.


## Designing memory boundaries


The hardest question isn't which pattern to use. It's what should be shared and what should stay private.


A useful progression: start with stateless retrieval, add episodic logging for multi-hour tasks, layer semantic indexing as knowledge repeats, then promote artifacts to shared memory with governance only as collaboration demands it.


The default should be private. Promote to shared with intent.


The cost of oversharing (noise, conflicts, permission complexity) is higher than the cost of under-sharing (occasional duplication, sometimes missed context). You can always share more later. Unsharing is harder.


In plain English: start by having agents pass structured outputs to each other explicitly. Only add shared memory when you can point to specific information loss at handoffs.


## When shared memory helps vs. hurts


**Shared memory helps when:**


-


Tasks are collaborative and build on each other's findings (research, analysis, writing pipeline)


-


Handoffs between agents need to preserve context that's hard to pass in a structured message


-


Long-running workflows need agents to see each other's progress in real time


**Shared memory hurts when:**


-


Tasks are independent and parallel (agents don't need each other's work)


-


Agents have different trust levels (mixing customer-facing and internal agents)


-


High-frequency updates cause contention (agents step on each other's writes)


Anthropic's findings support this: their multi-agent research system outperformed a single-agent baseline by 90.2% on their internal research eval. The improvement came from spreading reasoning across independent context windows, not from sharing everything. Subagents worked in isolation with focused tasks, and the lead agent synthesized.


## Building multi-agent memory systems


Start simple. Have agents pass structured outputs to each other. Agent A finishes, returns JSON with its findings, Agent B gets it as input.


Add shared memory incrementally based on real needs:


-


"Am I losing information at handoffs?" Add shared task memory.


-


"Are agents duplicating work?" Add shared semantic memory.


-


"Are agents acting on stale data?" Add event-driven notifications.


Instrument your handoffs. What percentage of shared writes actually get read by other agents? If it's low, you're adding complexity for nothing.


Where infrastructure helps: the hardest parts of multi-agent memory aren't the patterns themselves, but the plumbing underneath. Per-user memory isolation, metadata-based scoping, event notifications on memory changes. These are platform concerns.


Hyperspell's multi-tenant architecture already separates memory by user, which maps directly to agent-specific memory boundaries.[Custom metadata](https://docs.hyperspell.com/usage/metadata) enables tagging memories by agent role or task scope:


` from hyperspell import Hyperspell # Research agent writes findings tagged by task and agent role research_client = Hyperspell(api_key="API_KEY", user_id="user_123") research_client.memories.add( text="Prospect mentioned competitor Acme in March 15 email thread.", resource_id="research-finding-042", metadata={ "agent_role": "research", "task_id": "sales-followup-q1", "scope": "task" } ) # Writing agent queries only task-scoped memories writing_client = Hyperspell(api_key="API_KEY", user_id="user_123") response = writing_client.memories.search( query="competitor mentions for prospect", sources=\["vault"\], options={ "filter": { "task_id": "sales-followup-q1", "scope": "task" } } )`


[Webhooks](https://docs.hyperspell.com/usage/webhooks) provide the event-driven coordination primitive: your agents can subscribe to index-completed events and react when new memories become searchable.


Multi-agent memory is not a solved problem. The research is still catching up to what teams are building in production. But the patterns are becoming clear: scope by default, share with intent, and invest in the coordination layer early rather than bolting it on when things break.


## Frequently asked questions


**How is multi-agent memory different from single-agent memory?** Single-agent memory is a straightforward read/write cycle: one agent, one memory store. Multi-agent memory introduces coordination challenges: visibility (can one agent see another's writes?), consistency (what happens when two agents update the same fact?), and scoping (what should be shared vs. private?).[Cemri et al.](https://arxiv.org/abs/2503.13657) found that 36.9% of multi-agent failures trace to inter-agent misalignment, and[Anthropic's engineering team](https://www.anthropic.com/engineering/built-multi-agent-research-system) reports that multi-agent systems use roughly 15x more tokens than typical chats, largely because of coordination overhead.


**Should all agents share the same memory by default?** No. Default to private memory and promote to shared with intent. The cost of oversharing (noise from one agent's working notes polluting another's retrieval, write conflicts, permission complexity) is higher than the cost of occasional duplication. Start with structured message passing between agents and only introduce shared memory when you can point to specific information being lost at handoffs.


**What's the best coordination pattern for multi-agent memory?** It depends on your workflow. Sequential pipelines (research then writing) work well with message passing. Parallel agents with low write frequency suit optimistic concurrency. Systems that need real-time awareness of changes benefit from event-driven updates. The central coordinator pattern (used by Anthropic's research system) is the simplest to reason about but can become a bottleneck at scale. Most production systems combine patterns rather than relying on one.


**How do you handle write conflicts between agents?** Most teams use last-write-wins and accept the risk. For more robust approaches, CRDTs (conflict-free replicated data types) allow independent writes with guaranteed eventual consistency, while CQRS (command query responsibility segregation) separates write and read models. Write conflicts remain the most pressing open challenge in multi-agent memory. If your agents rarely write to the same memory simultaneously, last-write-wins is pragmatic. If they do, invest in conflict detection early.
