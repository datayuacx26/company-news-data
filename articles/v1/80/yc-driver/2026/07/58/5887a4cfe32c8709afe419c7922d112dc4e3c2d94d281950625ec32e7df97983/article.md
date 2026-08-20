---
schema_version: "1.0.0"
document_id: "5887a4cfe32c8709afe419c7922d112dc4e3c2d94d281950625ec32e7df97983"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/compiling-context-delivering-signal/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:8293c00effc8d29aa567e3602a78159c6376d8e3b10c9d4f68778247505a1b4f"
---

# Compiling Context, Delivering Signal

# Compiling Context, Delivering Signal


Intelligence without the right signal cannot deliver. Daniel explains how Driver's compiler frontend, atomic guiding documents, and a focused runtime step, including gather_task_context, jointly optimize exhaustiveness and high signal-to-noise for AI agents at scale.


Apr 16, 2026 — 11 min read


Daniel


Co-founder


Share on XShare on LinkedInShare on Facebook


## How Driver Jointly Optimizes Compilation and Runtime Intelligence for Context


AI coding agents are ushering in the biggest transformation to software engineering in my career. There is a lot of activity in this new frontier including structured AI SDLCs, orchestration frameworks, and agentic workflows. It’s all exciting and rapidly evolving.


But beneath the excitement, there are foundational truths that will limit what’s possible unless we build the right solutions. Top of mind for us at Driver: intelligence *alone* does not equate to efficacy. Intelligence without the proper signal to operate on cannot deliver. In software development as in signal processing, if the right context is missing or swamped by irrelevant noise, no amount of reasoning can compensate.


For AI-driven software engineering at scale, context is the bottleneck and is not resolved by model capability or orchestration techniques. In this post, I’ll walk through the journey Driver has taken to solve this problem rigorously. It involves compiler architectures, runtime engines, and **resolving a fundamental tension between exhaustiveness and high signal-to-noise ratio (SNR)** . The story culminates with a deceptively simple MCP tool that is powering a lot of customer value.


## The Problem and Our Conviction


### How Context is Handled Today


The first and most widespread approach is **no explicit context solution** at all. An agent is dropped into a codebase with native tools and in every session must perform time-limited *ad hoc* discovery. For large codebases (consider an internal ten-million-line repository) this effectively fails at scale. The agent touches only a small fraction of source content, exhibits extreme path dependence, produces high variance from session to session, and requires the developer to manually point it in the right direction.


Another common approach is **retrieval augmented generation (RAG)** . These methods range from naive RAG with basic chunking and semantic top-k retrieval to more sophisticated methods like GraphRAG. RAG and semantic search are useful tools, but they are not sufficient for exhaustive code context management. They focus on chunking source material instead of building abstraction layers, often flattening structure and erasing conceptual information. They rely on semantic search via vector math rather than guidance through abstraction levels, and this is a fundamentally weaker form of semantic comparison than LLM reasoning or ontological content generation. And they are biased toward solving needle-in-haystack problems, not exhaustiveness problems. RAG is a sampling method, not a compilation method.


A final emerging approach is **continuously operating agents** which are agentic systems that follow code changes and development work, building and maintaining artifacts, documents, and memory that persist beyond any particular session. These can build up sophisticated and potentially exhaustive context over time. But they remain fundamentally *ad hoc* in how they accumulate understanding, without the structural guarantees of a compilation process.


The common thread: none of these approaches provide, or mechanistically *can* provide, statistical guarantees on delivering complete context at all scales. At their worst, they are plagued by *not knowing what they don’t know* . Important signal is ablated from the flow, and ablated signal poisons the entire process, even if that process is otherwise highly structured and effective. At their very best, they are still too *ad hoc* to guarantee consistent results at enterprise scale.


### The Future: Context as Infrastructure


Driver’s founding conviction is that context is an infrastructure problem, bound and defined by information theory. If you want guarantees and determinism, you need structure. In compiler architectures, we found both a solution and a framework to articulate the key building blocks.


The decision to commit to heavy upfront pre-computation is critical and liberating. Unconstrained by real-time latency, we can treat context generation as the exhaustive build step it is. The key features of a compiler are *all* needed: exhaustiveness (every line, every symbol), multiple passes (building up abstraction levels), and structured intermediate representations (IRs) critical to build up in abstraction and record context at each level of abstraction for completeness.


This conviction led to a three-step journey at Driver:


1. Build an exhaustive, symbol-complete compiler frontend.
2. Build higher-level context in the later stages of the compiler, including atomic codebase-wide guiding documents.
3. Build a runtime API and intelligence engine.


The analogies with traditional compiler and software execution run deep. There are things you can and should solve only at an ahead-of-time compilation phase (exhaustiveness) and others you can and should only solve with a runtime engine (task-specific high SNR). By jointly optimizing across both, we can build a rigorous and high-performing context layer applicable everywhere.


## Step One: Exhaustive Compiler Frontend


Our earliest effort, and the foundation everything else builds on, was an exhaustive compiler frontend that consumes codebases of arbitrary size, shape, and implementation languages. The key is destructuring the problem of foundational comprehension of a codebase so that the difference between a small, simple codebase and a complex enterprise codebase is the *number* of rigorously constrained processing units, not the *kind* of processing. Key components:


- **The DAG pipeline** : Every codebase’s file tree admits immediate construction of a directed acyclic graph (DAG). Topological sort of this graph establishes highly constrained, granular tasks at the file and symbol level for exhaustive processing regardless of size or shape. The DAG also maintains rigorous 1:many relationships (e.g., one file to many symbols) that are easy to navigate in subsequent processing.
- **Static analysis for syntax trees and symbol tables** : Alongside the file tree DAG, we build a second graph from parsing the source code, just like a traditional compiler frontend. Using multi-language static analysis, we create detailed syntax trees complete with dependency and relationship mapping. The file tree DAG and syntax trees connect naturally at file boundaries, and we navigate through and between both graphs in subsequent compiler passes.
- **Multi-pass architecture** : The compiler progresses through: symbol extraction → symbol documentation → file-level technical documentation → per-folder higher-level documentation → codebase-wide synthesis (tagging, classification, and Deep Context Documents). Each pass builds on the previous, maintaining exhaustive understanding while constructing content at progressively higher abstraction levels. The total output is an exhaustive, well-organized set of IRs from the lowest symbol level to the highest codebase-wide concepts.
- **Highly structured output** : Concrete data structures are used at every level, starting from symbol content. This enforces critical constraints and maintains consistency across LLM content generation.
- **Always up-to-date** : We detect whenever a commit or PR lands on a tracked branch and trigger a scoped update flow. The structured processing that enables greenfield onboarding also enables surgically scoped updates where only the affected parts of the DAG are reprocessed. Driver’s pre-computed content stays current with your codebase automatically.


## Step Two: Atomic Guiding Documents


Something we learned building the transpiler, and which further deepens the relationship with compiler architecture, is that exhaustive structured data alone is nowhere near enough.


A symbol-complete and dependency-complete syntax tree with symbols explained in clear language is a major accomplishment to produce for arbitrary codebases. But a complete multi-language syntax tree, a high-quality knowledge graph, and the file structure of a codebase all share the same limitation at scale. If we stopped at the syntax tree, at some scales we’d be little better off than an agent dropped into a ten-million-line codebase and asked to get to work. The structure is too big, too complicated, and too partitioned to exhaustively consume. It’s not even clear how to effectively traverse a particular sub-graph in the limited time available. The core problem: you don’t know what you don’t know.


This is where higher-level synthesized documents become crucial. We call a critical set of codebase-wide documents produced by a dedicated later stage of the transpiler our “Deep Context Documents.” A dedicated engine performs exhaustive review of the codebase and all content emitted by earlier transpiler stages, synthesizing the result into single, self-contained documents. These documents identify and describe the major conceptual and architectural features of the codebase and, crucially, where to go for more detail and which components are significantly linked. The goal: given an arbitrary task, an agent can know all the concepts and places in the codebase that are relevant, to be followed up on in subsequent iterations.


These guidance documents must be *atomic* and designed to be consumed in their entirety. If an agent *can* read only partial content, it *will* sometimes choose to do so. Any selective hierarchy opens the door to the “whiffing problem.” An agent may reason, and quite plausibly, that it only needs sections X, Y, and Z. But at statistical scales, that reasoning will sometimes be wrong and important context will be missed, triggering signal poisoning and the possibility for arbitrary hallucination-level failures.


This can’t be overstated: if core signal is missing in a processing chain, it *cannot* be recovered by any algorithm or intelligence engine. This is analogous to inverse problems in applied mathematics. If a forward model attenuates high-frequency signals to zero, you cannot recover them by inverting the problem. You cannot divide by zero.


With these grounding deep context documents in place, the complex and detailed graphs from Step One become incredibly wieldable all of a sudden. After orientation via the guiding documents, an agent knows what details it needs and where they are. It can effectively use the structured graphs to navigate from code maps to files to symbols and their dependencies with confidence. This detailed navigation also benefits from iterative refinement, but it is fundamentally enabled by high-level grounding.


The realization: **guidance is distinctly important** and requires its own architectural commitment. Building top-level atomic guiding documents provided a major step change in performance of our context layer. Before consuming these, an agent is guessing. After, it can be the wizard it was born to be.


## Step Three: A Runtime Engine


With Steps One and Two complete, we had a sophisticated, structured, and agent-navigable set of context that closes over an entire codebase.


But how do we make this accessible to agents? Interface design is always critical, and the best information in the world, locked behind a bad interface, cannot be used to its full potential.


### The Core MCP Interface


The first development was building our Model Context Protocol (MCP) remote server. From the beginning, we built the guidance-into-detail dichotomy into the structure of our tool set.


The “Deep Context Tools” each fetch and present an atomic guiding document. Together, they cover: a technical architecture description, an onboarding guide, and a changelog compiled from the entire commit history. Agents are instructed to always read these first.


The navigation and detail tools fetch pre-computed content at much lower granularity: a rich “code map” queried hierarchically with a depth parameter, symbol-complete tech docs for any file or folder, the ability to fetch raw source code, and detailed development history at monthly granularity.


Together, these constitute our set of MCP primitives. They are read-only, instantaneously-returning tool calls with no on-the-fly generation. While compilation is expensive, it is a one-time cost relative to the runtime where the pre-computed output is available for continual cheap, fast, and consistent reuse. An agent follows a simple flow of deep context first, detail gathering second to ensure proper context is gathered.


### Solving the Exhaustiveness vs. High SNR Tension


This setup was a major leap forward. The transpiler output solves exhaustiveness well, and agents equipped with Driver’s MCP tools could reliably find what they needed across arbitrarily large codebases. But it revealed the next challenge:


- The discovery process pushes a lot of content into the parent agent’s context window, much of which isn’t relevant to the specific task at hand. We’ve solved exhaustiveness but not necessarily high SNR.
- Some agent rigs and models aren’t consistent in following the proper tool-calling flow.
- Even the best parent agent usually has a lot going on. Multiple goals, competing skills, and other tools can compromise a pure context-gathering process.
- Ideally, Driver would supply perfect, dense, high-SNR context for a particular task with one call at runtime. All the signal important to the task, nothing else.


We can’t pre-compute such context packages at compile time as there are infinitely many task definitions, only known at runtime. But what we *can* do is build a lightweight runtime agent that does exactly this.


This is where` gather_task_context` enters the picture. Presented as a single MCP tool, underneath the hood we built a headless sub-agent running in a dedicated sandbox. The sub-agent uses **the same MCP primitive tools** exposed to users, but in a controlled and focused environment. Its operation is straightforward: read all deep context documents first (orientation), navigate to specifics using the primitive tools (detail gathering), then synthesize into a dense, high-SNR output for the task at hand.


The elegance is that the runtime is conceptually simple: it’s a reader that follows a structured methodology against the transpiler’s pre-computed outputs. The sophistication lives predominantly in the compiler. By adding this focused runtime step, we resolve the otherwise challenging exhaustiveness-vs-SNR tradeoff. From the parent agent’s perspective: one tool call, task description in, dense high-SNR context out. Wash, rinse, repeat.


A significant bonus:` gather_task_context` folds in multi-codebase discovery without any extra effort. The primitive tools are indexed by codebase, so the runtime agent can operate against any set of codebases. This can look like a frontend and backend pair, interacting microservices, or any other scenarios where cross-codebase context is needed in the same flow. This enables more ambitious work and extends Driver’s value well beyond traditional IDE contexts.


## The Full Picture is Joint Optimization


` gather_task_context` was a keystone in this technical journey. Today, after plugging in your codebases, you can work in any MCP-compatible environment and` gather_task_context` will provide tailored, high-SNR context consistently regardless of task complexity or the number of codebases involved.


This comes from a compiler and runtime jointly optimized, each designed with the other in mind. What can only be done at compile time: exhaustiveness. What can only be done at runtime: task-specific SNR. This is directly analogous to the compile-time / runtime split in traditional software systems.


To recap the progression:


- **No Driver** : Agents explore with native tools. They touch only a tiny fraction of a codebase, exhibit high variance at statistical scales, and fail at enterprise size. Neither exhaustiveness nor high SNR is solved.
- **Driver before` gather_task_context`** (compiler + manual discovery): A huge improvement. Agents self-serve context with guided discovery flows using Driver’s MCP. But discovery can flood the parent agent’s context window and be disrupted by competing goals. Exhaustiveness is solved; task-specific high SNR is not.
- **Driver with` gather_task_context`** (compiler + runtime intelligence): One MCP call delivers exhaustive *and* high-SNR context for any task. Driver’s runtime agent handles discovery in a dedicated sandbox. The parent’s context window is used maximally efficiently, and multi-codebase discovery comes along effortlessly.


There is plenty more we have planned to improve our core context layer, but we’re convinced that a dedicated compiler system, runtime tools, and joint optimization across both is what it takes to really solve context.


## Context Compilers as Core Infrastructure


We’ve been building this system for two years using first principles drawn from information theory, signal processing, and compiler design. This isn’t incremental improvement on existing approaches. It’s a fundamentally different architecture.


We have high conviction that context compilers are the future. The information-theoretic argument is clear: agents on their own doing *ad hoc* runtime exploration, and RAG-based systems sampling from chunked source code, will never in principle be as statistically robust as a compiler approach. Sampling cannot provide the guarantees that compilation can. This is not a matter of better models or smarter retrieval, it’s a structural limitation.


Context is a deep and profound infrastructure challenge for the age of AI agents. It requires the same rigor we bring to compilers, databases, and operating systems. Codebases need dedicated compilation for context, the same way source code needs compilation for execution. We believe Driver is a leading instance of this new category of infrastructure, and we’re excited to see it grow, both in what we’re building and in the broader industry’s recognition of how context issues require rigorous solutions.
