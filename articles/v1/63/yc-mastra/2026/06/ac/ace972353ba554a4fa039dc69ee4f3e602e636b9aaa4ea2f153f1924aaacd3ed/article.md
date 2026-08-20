---
schema_version: "1.0.0"
document_id: "ace972353ba554a4fa039dc69ee4f3e602e636b9aaa4ea2f153f1924aaacd3ed"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/agent-memory"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:57:46.286490+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:af475bbabce0c35f983774694f0b9bf94b3197de0b42161de1eb4a7a182e9d49"
---

# Agent memory: types, techniques, and implementation guide

Your AI agent answered the same onboarding question for the third time today, because it forgot the user answered it yesterday. Stateless LLM calls can parse text, generate drafts, and extract data, but they cannot remember. Agent memory is the set of systems that let an agent store, retrieve, and act on information across turns, sessions, and tasks.


This guide covers what agent memory is, the types you need to know, techniques for implementing each one, and how to test that your memory layer actually works in production.


## Why agent memory matters


Your agent's usefulness scales directly with how much relevant context it can bring to each interaction. Without memory, every conversation starts cold. With it, agents accumulate institutional knowledge, learn user preferences, and avoid repeating mistakes.


### The gap between stateless LLMs and persistent agents


You can think of a raw LLM call as a contractor you hire for a single job. It shows up, does the work, and leaves with no record of the engagement. An agent with memory is more like an employee: it maintains context, has a role, and builds on past experience. Simon Willison's widely cited definition captures this well: an agent calls tools in a loop to achieve a goal. Memory is what lets that loop carry forward.


Without memory,[AI agents](https://mastra.ai/articles/ai-agents) at medium and high levels of autonomy simply cannot function. They can't retry failed tasks with lessons learned, manage multi-step plans, or personalize responses. The gap between a stateless LLM and a persistent agent is the gap between a search box and a colleague.


### How memory changes what agents can actually do


When you give your agent memory, you move it from reactive to adaptive. A support agent remembers that a specific user is on an enterprise plan and routes accordingly. A coding agent recalls that your team uses pnpm instead of npm and stops suggesting npm commands. A sales agent tracks which prospects responded to which messaging and adjusts its next outreach.


These are not hypothetical improvements. They are table-stakes behaviors that users expect from any tool they interact with more than once. Memory is the mechanism that makes those behaviors possible.


## What is agent memory


Agent memory is the system that determines what your agent remembers and how it retrieves that information when needed. It spans everything from the rolling message buffer in a single conversation to long-lived knowledge stores that persist across sessions, users, and deployments.


### Memory vs. raw context: when data becomes memory


Your agent's context window holds the tokens the model can see right now. That is not the same as memory. Raw context is ephemeral: once the window fills up or the session ends, it's gone. Data becomes memory when it is explicitly captured, stored, and made retrievable for future use.


This distinction matters for design. If you only rely on the context window, you are building a system that degrades as conversations grow longer. Context rot, where model quality drops as token counts climb, is a well-documented problem. Research from Google's Gemini team showed performance degrading at roughly 125K tokens, even with a 500K-token window. Memory systems exist to keep your context lean and relevant.


### The difference between LLM memory and agent memory


LLMs do not inherently remember anything. Each inference call is stateless. When people say ChatGPT "remembers" things, they are referring to an external memory system built around the model, not a capability of the model itself.


Agent memory is the broader architectural concept. It includes the LLM's context window but also encompasses message history persistence, summarization pipelines, vector databases, and any other mechanism that lets an agent access past information. The model provides the reasoning; your memory system provides the continuity.


## Types of agent memory


If you've read cognitive science literature on human memory, you'll recognize the taxonomy. The categories map to distinct engineering concerns, so understanding each one helps you decide what to build. The table below gives a quick reference before the full breakdown.


Memory type What it stores Typical implementation


Short-term / working Active conversation turns and pinned task context Message buffer in the context window


Long-term User preferences, historical decisions, accumulated knowledge Vector databases, relational DB, key-value store


Episodic Specific past events with timestamps and outcomes Structured event logs with temporal retrieval


Semantic General facts and knowledge independent of when they were learned Vector database with cosine similarity search


Procedural Action sequences, tool-calling patterns, workflow templates Runbooks, workflow definitions, or fine-tuned model weights


Shared Context accessible to multiple agents in a system Common data store with consistency guarantees


### Short-term memory


Short-term memory holds the information your agent is actively using right now. In practice, this is the message buffer: the recent conversation turns sitting in the context window. It is fast, immediately accessible, and limited.


Working memory also includes structured blocks of context that the agent keeps pinned for the current task. A user's name, the current project ID, or a set of instructions all qualify. The constraint is size: your context window is finite, and every token in working memory is a token not available for reasoning.


### Long-term memory


Long-term memory persists across sessions. It lives outside the context window in databases, vector databases, or key-value systems and gets pulled back in through retrieval. This is where user preferences, historical decisions, and accumulated knowledge reside.


Implementing long-term memory usually involves a write path (capturing and indexing memories) and a read path (searching and injecting them into context). RAG pipelines are the most common implementation pattern, but they are not the only one.


### Episodic memory


Episodic memory captures specific past experiences: what happened, when, and in what context. An agent with episodic memory can recall that a deployment failed on Tuesday because of a missing environment variable, and use that information to diagnose a similar failure today.


You typically implement episodic memory by logging events with timestamps and outcomes in a structured format. The retrieval query is often temporal: "What happened last time we deployed to staging?"


### Semantic memory


Semantic memory stores general knowledge and facts, independent of when or how they were learned. Think of it as your agent's knowledge base: product documentation, API specifications, domain rules, company policies. It differs from episodic memory in that it is not tied to a specific event.


Vector databases are the standard storage layer for semantic memory. You embed facts as vectors, index them, and retrieve by cosine similarity at query time.


### Procedural memory


Procedural memory encodes how to do things. In human cognition, this is the "muscle memory" that lets you type without looking at the keyboard. For agents, procedural memory manifests as learned action sequences, tool-calling patterns, and workflow templates.


You can implement procedural memory explicitly by giving agents access to runbooks, standard operating procedures, or workflow definitions. You can also implement it implicitly by fine-tuning a model on past successful task completions, though this is less common in production[agent systems](https://mastra.ai/articles/agent-systems) today. A coding agent that has internalized your team's commit conventions and test patterns is effectively using procedural memory every time it generates a pull request.


### Shared memory in multi-agent systems


When you run multiple AI agents, they often need to share context. A code-review bot should know what the coding agent already learned about your team's conventions. A support escalation agent should see the full history from the frontline agent.


Shared memory can be as simple as a common data store that multiple agents read from and write to. The engineering challenge is consistency: if Agent A updates a memory, Agent B needs to see the update without stale reads causing conflicting behavior. As the Principles of[Building AI Agents](https://mastra.ai/articles/how-to-build-ai-agents) framework notes, subagents that lack each other's context produce incompatible outputs. Context engineering across agent boundaries is as important as the memory architecture within a single agent.


## Short-term vs. long-term memory in practice


The theoretical distinction between short-term memory and long-term memory maps to concrete engineering decisions. Your choice of data structure, eviction policy, and retrieval mechanism all depend on which type you are implementing.


### Message buffers and recent-context windows


Your agent's message buffer is the simplest form of short-term memory. It holds the last N messages or the last T tokens of conversation history. When the buffer fills, you need an eviction strategy.


The most common approach is a rolling window: drop the oldest messages. A better approach is to summarize evicted messages before discarding them, preserving the key information in compressed form. A good rule of thumb is to evict roughly 70% of messages at each compaction, keeping enough recent context for continuity.


### Semantic caching for repeated queries


If your agent gets the same question repeatedly, you can cache the answer keyed to the query's embedding. This bridges a gap between short-term and long-term memory: common queries get fast, consistent responses without a round-trip to the model.


Semantic search over a cache works best for factual lookups and FAQ-style interactions. It breaks down for queries that depend on evolving state, so you need a cache-invalidation strategy tied to your memory writes.


### Archival and recall stores for persistent knowledge


Long-term memory lives in external stores. Vector databases handle semantic search. Relational databases handle structured lookups. Graph databases handle relationship traversal and knowledge graphs. Your choice depends on your query patterns.


For most agent applications, a vector database combined with metadata filtering covers 80% of use cases. You embed memories as vectors, attach metadata like timestamps and categories, and run hybrid queries that combine semantic similarity with structured filters.


## Techniques for implementing agent memory


You know the types. Now you need to build them. The techniques below represent the main implementation patterns, from lightweight to sophisticated.


### In-context memory blocks (core memory)


The simplest technique is to reserve a portion of your context window for structured memory blocks. Each block has a label, a description, and a value. One block might hold user preferences, another the agent's current task state, and a third the conversation summary so far.


The agent (or an external process) updates these blocks as new information arrives. Because they're pinned in the context window, retrieval latency is zero. The tradeoff is space: every token in a core memory block is a token you can't use for conversation or reasoning.


### Message eviction and summarization


When your conversation exceeds the context window, you need to compress. Two patterns dominate:


1.


Evict the oldest messages and prepend a running summary.


2.


Use recursive summarization: chunk evicted messages, summarize each chunk, then summarize the summaries.


Both approaches lose detail. The key is deciding which details matter. If specific decisions or errors need to be preserved, flag them as non-compressible and keep them in your memory blocks. Summarize everything else.


### External storage and retrieval (RAG-style recall)


For long-term memory, you store information outside the context window and retrieve it on demand. The standard pipeline is the same as retrieval augmented generation (RAG): embed the memory, index it in a vector database, and query at inference time.


The twist with agent memory, compared to document-oriented RAG, is that your data source is the agent's own interactions. You are not indexing static documents; you are indexing a growing, evolving stream of conversations, decisions, and observations. Your chunking, embedding, and indexing pipeline needs to handle this incremental, temporal data.


### Asynchronous and sleep-time memory processing


You do not have to process memories synchronously during the conversation. Sleep-time processing offloads memory management to background jobs that run between interactions.


During idle periods, a separate process (or a dedicated memory-management agent) can re-index memories, merge duplicates, update summaries, and promote important observations from short-term to long-term storage. This approach improves response latency during conversations because the agent is not spending tokens on memory housekeeping in real time. It also enables higher-quality memory consolidation, since the background process can use more compute without impacting the user experience.


### The operating-system analogy: MemGPT-style paging


The MemGPT research paper introduced a useful mental model: treat the context window like RAM and external storage like disk. The agent gets function calls to page data in and out, creating the illusion of unlimited memory within a fixed context window.


In this model, core memory (in-context blocks) is RAM, recall memory (searchable conversation history) is fast disk, and archival memory (vector-indexed knowledge) is slow disk. The agent decides what to page in based on the current query, and pages out stale information to make room. This operating-system analogy gives you a practical framework for reasoning about memory hierarchy in your own agent architecture.


## Memory engineering: designing the ingestion and retrieval pipelines


Your agent memory system is only as good as its ingestion and retrieval pipelines. A poorly designed write path fills your store with noise. A poorly designed read path retrieves irrelevant context and pollutes the model's reasoning.


### Ingestion pipeline: capturing, chunking, and indexing memories


Your ingestion pipeline transforms raw interactions into structured, searchable memories. A robust pipeline typically includes these stages:


1.


**Extraction:** identify facts, events, instructions, and tasks from conversation history.


2.


**Verification:** check extracted memories against the source transcript for accuracy.


3.


**Classification:** tag each memory by type (fact, event, instruction, task) and assign metadata like timestamps and topic keys.


4.


**Deduplication:** detect content-addressed duplicates and supersede outdated memories.


5.


**Embedding and indexing:** generate vector embeddings and write to your storage layer.


Idempotency matters. If the same conversation is ingested twice, you should get the same result. Content-addressed IDs (hashing the session ID, role, and content) are one way to ensure this.


### Retrieval pipeline: scoring, ranking, and injecting into context


When your agent needs a memory, the retrieval pipeline finds it. A single retrieval method rarely suffices. In practice, you want to run multiple retrieval channels in parallel and fuse the results.


Effective retrieval strategies include:


-


**Direct vector search:** embed the query and find semantically similar memories


-


**Full-text search:** handle keyword-precise queries where you know the exact term


-


**HyDE (hypothetical document embedding):** generate a hypothetical answer and search for memories similar to it, catching cases where the question and answer use different vocabulary


-


**Exact key lookup:** match on structured topic keys for known entities


-


**Raw message search:** search stored conversation history as a safety net


Reciprocal Rank Fusion (RRF) is a proven technique for combining results from multiple channels. Weight each channel by its signal strength, break ties by recency, and pass the top candidates to the model for synthesis.


### Application modes: assistant vs. workflow


Your memory architecture depends on how your agent runs. A conversational assistant has long-lived sessions with a single user and needs strong working memory plus personalization. A workflow agent executes deterministic multi-step tasks and needs episodic memory of past runs plus procedural memory of step sequences.


For assistants, prioritize message-buffer management, summarization, and user-specific memory blocks. For workflows, prioritize structured logging, task-state persistence, and cross-run recall. Many production agents combine both modes, so your memory architecture needs to support both read patterns.


## Evaluating memory quality: LongMemEval and beyond


Testing agent memory requires more than unit tests on individual retrieval calls. You need end-to-end evaluation that measures whether your agent actually uses stored context correctly across multi-turn interactions.


**LongMemEval** (longmemeval-s is the short version) is a benchmark designed specifically for this purpose. It tests whether agents can recall facts from earlier in a conversation, whether they correctly update beliefs when new information supersedes old, and whether they avoid hallucinating memories that were never stored. Running longmemeval against your memory layer gives you a quantified baseline and a regression test suite.


Beyond longmemeval, you should build your own eval datasets from production logs. Mine real conversations where your agent gave a wrong answer, trace back whether a memory miss or memory drift caused the error, and add those cases to your test suite. An eval suite that mixes benchmark cases with production-derived edge cases is far more predictive than either alone.


## How Mastra Handles Agent Memory


If you're building agents in TypeScript,[Mastra](https://mastra.ai/) gives you composable primitives for agent memory out of the box. Mastra's Agent class supports persistent memory with configurable processors: TokenLimiter prevents context-window overflow by removing the oldest messages, and ToolCallFilter strips verbose tool-call results to save tokens. You can extend the base MemoryProcessor class to implement custom logic like semantic deduplication or priority-based retention. Each processor runs in sequence, giving you fine-grained control over what enters the context window and what gets evicted.


Mastra's memory layer writes to PostgreSQL, Redis, or LibSQL through pluggable storage adapters. Conversation history, working memory blocks, and long-term knowledge all persist to whichever backend you configure, and the agent code stays identical across environments. For retrieval, Mastra's vector query tools let your agent pull semantically relevant memories at inference time, combining embedding similarity with metadata filters for precise recall. You can also run sleep-time memory processing as a background workflow, consolidating and re-indexing memories between user sessions.


Observability ties the system together. Every memory read, write, and eviction is captured as an OTel span, so you can trace exactly which memories were injected into context and whether the model used them. Mastra's tracing UI surfaces these operations alongside LLM calls and tool invocations in a single view, exportable to Langfuse, Datadog, or any OTel backend. The memory system integrates with Mastra's workflow engine and evals, so you can test recall accuracy and measure memory drift over time. Replit, PayPal, and SoftBank run Mastra's memory infrastructure in production environments.


[Build your first agent with memory on Mastra.](https://mastra.ai/)


## Frameworks and open-source options for agent memory


You don't have to build everything from scratch. Several frameworks offer memory abstractions that can accelerate your implementation.


### LangChain and LangGraph memory abstractions


If you're working in Python, LangChain provides memory classes that plug into its chain and agent abstractions. You get conversation buffer memory, summary memory, and entity memory out of the box. LangGraph extends this with graph-based state management, letting you model memory as nodes and edges in a stateful execution graph. This is useful when your agent's memory has complex dependencies between entities.


The tradeoff is coupling. LangChain's memory abstractions are tightly integrated with its chain API, which can make it harder to swap components or migrate to a different framework later.


### Letta / MemGPT


Letta (formerly MemGPT) is the reference implementation of the operating-system memory model described earlier. It gives agents function calls to manage their own memory across core, recall, and archival tiers. Every agent in Letta maintains a perpetual conversation thread with automatic persistence.


Letta's strength is its opinionated architecture: it handles eviction, summarization, and retrieval as first-class concerns. The limitation is flexibility. If your memory needs diverge from Letta's model, you may find yourself working against the framework rather than with it. For teams that want Letta's durability guarantees without committing to its full architecture, implementing the core/recall/archival tiering pattern yourself on top of a vector database gives you similar control.


### Other open-source offerings


The landscape is broad and moving fast. Mem0 focuses on user-level memory for personalization. Zep provides long-term memory with automatic summarization and entity extraction. Cloud providers are entering the space too: Cloudflare recently announced an agent memory service built on Durable Objects and Vectorize. If you're already on a specific cloud platform, check whether your provider offers a managed memory layer before building your own.


For knowledge graphs, tools like Neo4j and FalkorDB let you store memories as entities and relationships, which works well for agents that need to traverse connections between facts rather than just retrieve individual memories by similarity search.


## Testing, tracing, and debugging memory in agents


Your agent memory system will fail in ways that traditional software does not. A broken API returns an error code. A broken memory system returns a plausible but wrong answer, and you might not notice for weeks.


### What can go wrong: memory drift, retrieval misses, and injection errors


You should watch for three classes of failure:


-


**Memory drift:** accumulated summaries gradually shift away from the original facts, introducing subtle inaccuracies that compound over time


-


**Retrieval misses:** the retrieval pipeline fails to surface a relevant memory, so the agent acts as if it never learned that information


-


**Injection errors:** irrelevant or outdated memories get injected into context, confusing the model's reasoning


Each of these can look like a model failure from the outside. The agent gives a wrong answer, and your first instinct is to blame the LLM. Tracing infrastructure that logs memory reads and writes separately from model calls lets you isolate the real cause.


### Tracing memory reads and writes across agent turns


You need visibility into what memories were written, what was retrieved, and what was injected into context for each model call. This means instrumenting your memory infrastructure with spans that capture the query, the retrieved results, the scores, and the final context that reached the model.


Mastra surfaces these memory operations in its tracing UI, so you can inspect memory reads and writes alongside model inputs and outputs in a single trace view. OpenTelemetry is the standard format for this kind of observability. If your framework does not expose memory operations as spans, you'll need to instrument them yourself.


### Evals and datasets for validating memory quality


You should evaluate your memory system the same way you evaluate your agent's outputs: with structured evals and curated datasets. Build test cases that exercise specific memory behaviors:


-


Can the agent recall a fact stored three sessions ago?


-


Does it correctly supersede outdated information?


-


Does summarization preserve the critical details you flagged?


LLM-as-judge works well here. Give a judge model the original fact plus the agent's recall, and score on accuracy and completeness. Start with hand-curated test cases, then mine production logs for real-world edge cases. A mature eval suite mixes both. User feedback on recalled context is another signal: when users correct the agent on something it should have remembered, that's a retrieval miss worth capturing.


## Privacy, ownership, and security of agent memory


When your agent remembers things, you are storing user data. That creates obligations around privacy, security, and compliance that do not exist for stateless agentic systems.


### Where memory is stored and who controls it


Your memory architecture determines your data sovereignty posture. Memories stored in a managed third-party service live on infrastructure you do not control. Memories stored in your own database give you full control but require you to handle replication, backup, and access management.


Think about this early. Migrating a memory system after launch is significantly harder than choosing the right storage layer upfront. If you use a managed service, verify that you can export your data. Portability should be a hard requirement, not a nice-to-have.


### Prompt-injection risks through retrieved memories


Retrieved memories are injected into the model's context. If an attacker can influence what gets stored in your memory system, they can influence what your agent does. This is a prompt-injection vector that is easy to overlook.


Consider a customer support agent that stores user messages as memories. A malicious user sends a message designed to override the agent's system prompt. If that message gets stored and later retrieved for a different session, it can affect the agent's behavior for other users. Sanitize memories before storage, validate retrieved memories before injection, and treat your memory store with the same security posture as any other user-input pipeline.


### Data retention, deletion, and compliance considerations


You need clear policies for how long memories are retained and how they are deleted. GDPR, CCPA, and similar regulations give users the right to request deletion of their data. If your agent memory system cannot identify and delete all memories associated with a specific user, you have a compliance gap.


Design your memory schema with deletion in mind. Tag every memory with a user identifier. Build deletion endpoints that cascade through your vector database, your relational database, and any summarization caches. Test those endpoints regularly, because a deletion API that does not actually delete is worse than no API at all.


Agent memory is what separates a stateless LLM wrapper from a system that actually learns and adapts. Getting the architecture right, from message buffers to long-term vector stores, determines whether your agent compounds value over time or resets with every session.
