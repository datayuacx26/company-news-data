---
schema_version: "1.0.0"
document_id: "ca79772bce3e3695d32ef4ba86972c9cdb960a27338a702c35306f26043e7bf7"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/why-current-approaches-fail/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:aa13f4e5f2556b750863e2b016f87349fe417532f5b8ebf80eeed710320cad4b"
---

# Why Current Approaches to Context Fail and What Replaces Them

# Why Current Approaches to Context Fail and What Replaces Them


RAG and runtime discovery fail at scale. Learn why code context requires compilation, not retrieval, and how to build context as infrastructure.


Jan 30, 2026 — 15 min read


Daniel


Co-founder


Share on XShare on LinkedInShare on Facebook


## Code Is Different


Unlike natural language that is ambiguous and open to interpretation, source code is rigorously structured and unambiguous. Every symbol has a precise definition within the syntax and semantics of the language. Every relationship is explicit and traceable. This is a huge opportunity.


If you watch an experienced engineer approaches an unfamiliar codebase, they don’t read files sequentially, but follow import chains, trace call graphs, and build understanding incrementally. Who calls what, where the boundaries are, and what the bigger picture is. They compile an understanding.


The question is: why don’t we build systems that do the same? Why don’t we pre-compute the critical context that agents need exhaustively, deterministically, and in a structured manner so that they can work with the same comprehensive understanding a senior engineer builds up?


## Two Broken Paradigms


Today’s approaches to code context fall into two camps. Both fail at scale.


The first camp relies entirely on runtime discovery. Agents explore codebases on-the-fly using grep, file search, and code navigation utilities. This sounds flexible, but it’s fundamentally vulnerable to problems that require exhaustiveness. Furthermore, these “ad hoc walk” approaches fall off a cliff at larger scales.


Consider: “build an architecture diagram for this codebase.” This isn’t a search problem, it’s a synthesis problem. You need to see everything, understand what matters, and distill it. An agent doing random walk discovery through a million-line codebase will never achieve exhaustive coverage. It might find the main entry points. It will miss the critical edge case handler buried in a utility module. It will not perform consistently at the largest scales.


Runtime discovery also burns context window tokens on the discovery process itself. Every search, every file read, every dead end consumes the finite attention budget that should be reserved for actually solving the problem. By the time the agent finds what it needs, it may have exhausted the capacity to use that information effectively. This is especially true in long workflows. There will be major improvements to agentic memory systems in the future, but high signal-to-noise ratio (SNR) will always be very important.


The second camp pre-computes something, but the wrong thing. Naive RAG-style approaches chunk code into text fragments, embed them as vectors, and retrieve “similar” chunks at query time. This does work upfront, but only a minimal amount, and tends to erase the structure inherent to code.


When you flatten a codebase into embedding vectors, you lose hierarchy inherent to the file system of the codebase. Furthermore, RAG typically only chunks and embeds. It doesn’t derive new views, distillations, or conceptual insights or the kind of higher-level understanding that senior engineers build over time.


There is no rigorous accounting for symbol relationships: function A calls function B calls function C, with a constraint inherited from interface D. Chunked separately, retrieval might return A and C but miss B and D entirely. The transitive dependencies that govern real system behavior are invisible.


A typical RAG query retrieves top-k results based on similarity. But the authentication handler in your payment service might look lexically similar to the one in your admin console while being semantically incompatible. The embedding captures vocabulary, not meaning and broader context. The top-k paradigm also implies a needle-in-haystack problem and is poorly suited to help solve the exhaustiveness problems we’ve alluded to.


At 10,000 lines, the right chunk(s) probably appears in your top-10. At a million lines, the embedding space is so crowded that structurally similar but semantically wrong code floods the results, and there is no higher level or exhaustive context to help. And regardless of source size, the complexity of the task itself can demand a lot of disparate context not well captured by a top-k search.


Neither pure runtime discovery nor naive pre-computation can guarantee the completeness and rigor that engineering work requires.


## Context Collapse


Every engineering team knows this nightmare: your senior engineer who actually knows how everything works gives two weeks notice. The person who understands why that authentication module is wired the way it is. Who knows which services depend on which. Who can trace a bug through six layers of abstraction.


What you discover in the scramble to document their knowledge: you can’t write down a mental model. The person leaving didn’t just memorize files—they compiled a complete representation of how your system actually works. The dependencies, side effects, implicit contracts, the things that are fragile and why.


We call this context collapse: when the complexity of a software system exceeds what any current method can reliably surface. It’s the ceiling every engineering team hits when they try to use AI on real codebases. Here are three distinct ways context collapse can manifest:


**The “whiffing” problem.** Your agent fails to find critical source information needed to solve a task. Runtime discovery whiffs because it can’t guarantee exhaustiveness. RAG whiffs because semantic similarity doesn’t mean semantic relevance and scaling in general for source size and task complexity is poor. They both fall down at scale.


**The “task-dependent SNR” problem.** Context engineering isn’t just about getting relevant material in, it’s about keeping irrelevant material out. What counts as “signal” depends entirely on the task. The same code might be critical context for one task and hallucination-inducing noise for another.


**The “shape and abstraction level” problem.** Complex tasks decompose into stages requiring different context. Early planning needs high-level architecture. Detailed implementation needs specific code with all the gory details. Without the ability to shape context to each stage, agents drown in detail too early or lack specifics when they need them.


This is why AI tools work on demos and fail in production. The demo codebase is small enough that any approach works. Your production codebase is massive, interconnected, and layered with a decade of decisions.


But it’s 2026 and it’s not even about challenges replacing a critical bottleneck engineer. There’s a much bigger opportunity: AI agents can now make every engineer dramatically more productive and more independent. Junior engineers can work with senior-level context. New team members can be productive from day one. The institutional knowledge that used to exist only in a few heads can be available to everyone and every agent, all the time.


We’re headed to a world where there is little or no coding by hand anymore, only directing and managing coding agents. Those that take advantage of this watershed early will see major gains. But only if you have an effective context layer.


## Why Bigger Context Windows Don’t Solve This


Here’s an attractive counterargument: “Won’t LLM context windows just keep growing until this doesn’t matter?”


No. Even with unlimited context windows, you have the SNR problem. This goes back to fundamental information theory. SNR matters regardless of how advanced models get.


Dumping an entire codebase into context requires a model to simultaneously pick the signal from a vast array of noise and ascertain the relationships and higher level connections that matter within the signal subset. This is analogous to deconvolution in signal processing, a process well known for noise amplification (and therefore appropriate only with sufficient SNR) due to information theory fundamentals.


Imagine giving a new engineer the entire codebase printed out on their first day. That’s not helpful, it’s overwhelming. What they need is structured understanding: an architecture overview, an explanation of how the pieces fit together, guidance on where to look for what. They can then iterate over a small number of the right high SNR sub-problems as they assemble the context needed for the task and execute against it.


The same is true for models. You could give an LLM infinite context, and it would still benefit from receiving structured, high-signal information over unstructured dumps. And because agents excel at breaking down and executing tasks iteratively, if the structured information provides conceptual guidance as much as direct detailed context, even extremely complex tasks can be solved reliably.


At scale, SNR matters much more than raw capacity. The goal isn’t to fit more in. It’s to fit the right things in, structured the right way, for each specific task.


## Compilation: The Alternative


Effective context engineering requires ahead-of-time compilation optimized jointly with downstream runtime interfaces.


Think about what a compiler does. It builds a complete model of the program: parsing every symbol, resolving every dependency, constructing the full graph of relationships. It processes exhaustively, deterministically, and structurally.


The critical insight: we can build systems that compile understanding in the same way. Pre-process the entire codebase ahead of time. Build the graph of dependencies. Generate human language context at multiple levels of abstraction. Distill complex relationships into forms that are immediately useful. Keep it automatically up-to-date.


It starts with treating the codebase as the structured graphs that they are. At one level, we have a directed acyclic graph where every file and folder is a node that can be topologically traversed in compiler passes. At a deeper level, syntax trees built from static analysis where every symbol, function, import, and dependency is included with their relationships.


From these, we can derive different data structures, such as symbol tables and call graphs, that enable convenient views for different tasks. Then comes another hallmark of compiler architectures: multiple passes over these structures, each generating understanding at different levels of abstraction or distilling/optimizing content of particular kinds in parallel, culminating in whole-codebase synthesis.


Architecture documents, onboarding guides, service dependency maps: These exhaustive, synthesized artifacts are exactly what can’t be created on-the-fly. They require seeing everything, understanding what matters, iterating over it, and distilling it.


The key innovation is separating what can be pre-computed from what must be dynamic, then optimizing them together. Ahead of time, we parse and build a multi-pass synthesis. At runtime, agents have immediate access to this foundation. They can shape and augment it for specific tasks and drill into details or zoom out to architecture.


The pre-computed context makes sure they always have a complete and general understanding and tells them where to go for details; their runtime capabilities let them use this to iterate exactly as needed to complete concrete and immediate tasks.


When code changes, only the affected nodes and their dependents get recomputed. Content remains stable and always up-to-date with detected code changes.


This is what we have built at Driver: our transpiler that transpiles source code into human language context.


## Tradeoffs and Considerations


Compilation isn’t free. What does it cost and how does it relate to alternatives?


**Build time.** Creating compiled understanding takes time, minutes for small codebases and longer for multi-million-line systems (but typically linear with line count and measured in hours for a greenfield onboarding for the largest codebases). The fundamental tradeoff: spend time upfront to guarantee completeness vs. accept that runtime discovery will miss things. This cost is paid once, while the benefits compound across every subsequent query, every agent task, every developer interaction.


**Compute cost.** Pre-computing understanding for a large codebase requires significant processing. But it’s a one-time cost for any given code state, amortized across the many times that understanding will be used. Aside from inducing consistency where on-the-fly agent discovery cannot be, pre-computation obviates the wasteful need (and cost) for agents to reinvent the wheel across and between sessions. Compare this to the alternative: every developer and AI agent waste hours per week on manual (and often common) context engineering, or ship code that breaks because the agent missed a dependency.


**Freshness constraints.** Pre-computed understanding is a snapshot. When code changes, affected portions must be recomputed. If we can generate the context layer from scratch automatically, we can certainly keep it up-to-date automatically. And because we are so structured in the transpiler, we can surgically and deterministically only update context affected by the diff for a commit, PR merge, etc. For most changes, this is very fast, but it is not zero. A single file edit triggers updates to a handful of nodes. Analogous to incremental compilation, we don’t have to wait nearly as long as greenfield compilation for most updates.


There is also the choice of when to trigger an update. Incomplete local state in the midst of development is not reflected in our context layer today, as committing changes and pushing to a remote is required for updates. This is a limitation in some cases but coverage can be extended in the future.


**Token budgets.** Pre-compiled understanding helps by providing structured, dense, high-SNR content with more meaning in fewer tokens. But agents also work iteratively. They can build task-specific context from the compiled foundation, consulting details piecemeal as needed. Compilation enables agents to efficiently access exactly what they need, when they need it, without burning tokens on discovery. Spawning Driver-enabled context gathering in an independent sandbox (e.g., via sub-agents or generative MCP tool calls) can further optimize token usage for the parent agent.


Are the tradeoffs worth it? For teams with non-trivial codebases, absolutely. The cost of context collapse can be enormous in wasted time, in bugs shipped, distrust and reduced AI usage by developers. These are real negative outcomes and missed opportunity in unrealized potential. The temporal cost of compilation is generally felt one time up front and relatively fast thereafter and the compute cost is amortized over downstream use that would otherwise reinvent the wheel each time.


## Context as Infrastructure


Context collapse isn’t one company’s problem. It’s an industry problem. And solving it requires recognizing that context is infrastructure.


Every engineering organization hits the moment when the codebase outgrew what any individual could hold in their head. Then teams focused on documentation to compensate, the documentation went stale, and they then tried wikis, internal tools, mandatory README updates. None of it scaled.


AI agents promised to change this. Give them the codebase, point them at a task, watch them work. Except they hit the same walls human engineers hit. The context window fills with fragments, discovery misses critical dependencies, and the agents produce plausible code that breaks in production.


The industry’s response has been to treat this as a prompt engineering problem, or a retrieval problem, or a model capability problem. It’s none of those. It’s an infrastructure problem.


Consider the precedents. Observability became infrastructure when teams realized they couldn’t ship complex distributed systems without structured telemetry. CI/CD became infrastructure when teams realized manual testing and deployment didn’t scale. Context follows the same pattern.


You can’t ask engineers to manually maintain documentation, trace dependencies, or orient every AI agent. You can’t ask your AI agents to exhaustively build and maintain context in runtime interactions. You need infrastructure that compiles understanding systematically, keeps it fresh automatically, and serves it consistently to everything that needs it, humans and agents alike.


Not a tool. Not a feature. Infrastructure.


The era of ad hoc context engineering is ending. The era of compiled understanding is beginning. Context collapse was the ceiling. Compilation jointly optimized with runtime agent capabilities is what comes next.
