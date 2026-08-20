---
schema_version: "1.0.0"
document_id: "1cd5bbf38c6005200a816d61e0a9931673e98f49285b65e22417d88b70453d61"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/ai-agent-stack"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-13T12:39:25.041167+00:00"
fetched_at: "2026-08-13T12:39:27.012997+00:00"
content_hash: "sha256:6c41e0bed411ccf34460bfd562c0337e913abc3bc49ced9846f2fe10ddd6778a"
---

# The AI agent stack: layers, tools, and how to build in 2026

You pick a graph framework for a support chatbot, and three weeks later you have fourteen nodes, custom retry logic, and a checkpointer for a tool that fails once a week. A fifty-line script would have done the same job. That gap between what you built and what the problem needed is why the AI agent stack matters.


According to[LangChain’s State of Agent Engineering survey](https://www.langchain.com/state-of-agent-engineering) , 89% of teams running production agents have tracing, but only 52% have evals. That 37-point gap is where quality quietly dies.


This guide walks through each layer of the agent stack, what it does, what to actually use, and the tradeoffs you accept at every level.


## What the AI agent stack is and why it matters


You can think of the AI agent tech stack as the set of layers between a raw foundation model call and a production agent that works under real traffic. It is not the same thing as the LLM stack. A chatbot needs inference and maybe retrieval. An agent needs much more infrastructure around the model to behave predictably.


The reason the distinction matters is failure. When an agent breaks, it rarely breaks at the foundation model. It breaks when the reasoning loop stalls, when memory drops the context it needed, or when a vague tool schema triggers the wrong call. Mapping these layers tells you which one to fix.


### From LLMs to LLM agents


You already know the LLM as a text generator: send tokens in, get tokens out, no memory of the last call. That statelessness is fine for a one-shot completion and a real problem the moment you want an agent to track a task across steps.


The shift from LLM to agent happens when the foundation model starts emitting actions rather than just text. It decides to call a tool, observes the result, and loops until the task is done.


That think-act-observe cycle is the atomic unit of every agent. Everything else in the stack exists to make that loop run reliably. This is the core of agentic AI: systems that reason, act, and adapt across steps without a human triggering each one.


The combination of autonomous execution, tool use, and persistent state is what forced a new stack to emerge. You cannot get there with a single prompt and a chat completion endpoint.


### What makes the stack different from a traditional app architecture


Your traditional app stack assumes deterministic behavior. The same input produces the same output, correctness is binary, and a 200 response means the request worked. Agentic AI violates all three assumptions.


The output is probabilistic, correctness is semantic, and a hallucinated answer still returns a clean 200. This changes what each layer has to do.


State management becomes central because the agent carries conversation history, memories, and execution stage between calls. Tool execution becomes a security surface because the model chooses actions that your code runs. Monitoring stops being optional because you cannot eyeball whether an answer is right at scale.


## Layer 1: Foundation models and inference


You start here because the foundation model is the reasoning core of your agent. Everything above this layer either feeds context into the model or acts on what it produces. The good news is that this is the most stable and commoditized layer in the AI agent stack, with the smallest gap between a demo and production.


The foundation model landscape shifted more in tone than in structure. Reasoning models fold planning into a single call, so agents that once needed multi-step chains can now solve some problems in one pass. Open-weight models closed much of the quality gap, which means “always use the biggest closed foundation model” is no longer default advice.


### Closed-source API models


Your fastest path to a working agent is a closed API from a frontier lab. Providers like **OpenAI** , **Anthropic** , and **Gemini** ship strong tool-calling, mature tooling, and wide community coverage of the edge cases you will hit. You send a request, you get a response, and there is no infrastructure to manage.


The tradeoff is lock-in. Each foundation model reasons differently, so switching providers means returning prompts, adjusting for new failure modes, and rerunning your eval suite. Closed APIs are also stateless by design, which pushes all memory work up into higher layers.


### Open-weight models as a service


You can get most of the convenience of an API while running open-weight models like Llama or Qwen through a hosted provider. This keeps your infrastructure thin while giving you a model you can later self-host without rewriting everything around it.


The main advantage here is portability. Lock-in risk drops sharply because you can swap the model and keep the surrounding code. A common pattern is to prototype on a closed model, then deploy on an open-weight one once behavior is nailed down and cost matters more than convenience.


### Local and self-hosted model serving


You self-host when the numbers stop working. Two situations justify it: your call volume makes per-token API pricing untenable, or you need sub-100ms latency that an API round-trip cannot deliver. Data residency requirements are a third common driver.


Serving your own foundation model with an inference engine gives you full control over deployment and data, at the cost of running GPUs and the operational overhead that comes with them. For most teams starting out, this is premature. Reach for it when a specific constraint forces the move, not by default.


### Model routing across providers


You rarely want to hard-code a single model for an agent’s entire lifetime. Routing lets you send cheap calls to a fast model and hard reasoning tasks to a stronger one, or fall back to a second provider when the first fails. A clean routing abstraction reaches multiple providers through one interface, so you can switch models without rewriting call sites.


Routing also softens the lock-in problem. When your provider abstraction is clean, retesting on a new foundation model is a config change plus an eval run, not a rewrite.[Mastra’s AI Gateway](https://mastra.ai/ai-gateway) applies this pattern directly, routing agent calls across 600-plus models from 40-plus providers through one API key, with automatic failover if a provider goes down.


## Layer 2: Protocols and tools


You give an agent leverage on the world through tools. A tool is a function the model can choose to call: web search, code execution, a database query, an API request. The model decides which tool to use and with what arguments. Your code executes it. That separation is the single most important thing to understand about this layer.


This layer barely existed as a distinct category two years ago. Every framework had its own schema for defining tools. Standardization changed that, and the tools layer is now where a lot of the energy sits in the agent stack.


### Tool calling and external integrations


Your agent’s tool reliability comes down to schema quality. The model reads a tool’s description to decide whether to call it, and reads the parameter schema to decide what to pass. A vague description produces wrong calls. A well-typed schema with clear parameter docs produces reliable ones.


A few tool categories cover most production needs. Rather than treating them as scattered options, plan for these deliberately:


-


**Web search:** for current information the model was not trained on.


-


**Code execution:** for calculation, transformation, and data processing.


-


**File and API access:** for reading, writing, and connecting to external services.


-


**Browser automation:** for interacting with interfaces that expose no API.


### The Model Context Protocol (MCP)


You no longer have to write custom integration code for every tool. The Model Context Protocol, introduced by Anthropic, is a shared standard for how models connect to external tools and data sources. It has been adopted across major providers and moved into foundation-level governance, which makes it the safe default for wiring tools today.


The reason MCP matters for the AI agent stack is portability. Because it is an open standard, any MCP-compatible agent can use the servers you build, and your tools transfer across frameworks.[Mastra](https://mastra.ai/ai-agent-framework) supports MCP servers that expose agents, tools, and resources, so your integrations are not trapped inside one runtime.


Security is an open problem. Independent analysis of thousands of MCP servers found a large share vulnerable to path traversal and code injection. Treat every server as an attack surface and lock it down before it ships.


### Sandboxes and secure execution environments


You cannot let an agent run arbitrary tool code directly on your infrastructure. When an agent executes actions the model chose, especially code, you need a sandbox that limits what the process can touch. This is how coding agents run generated code safely inside a contained environment.


Isolated execution contains blast radius. If the model produces a destructive command or an infinite loop, the contained environment caps the damage and the cost. For any agent that executes code or shell commands, this layer is not optional.


## Layer 3: Memory and knowledge


You decide here what your agent remembers, how it retrieves that information, and when it forgets. This is the highest-complexity layer in the stack and the one where most teams get stuck. Two years ago, memory meant “pick a vector database and do RAG.” Now it is a first-class architectural concern with distinct tiers.


Bigger context windows did not remove the need for memory. They changed the tradeoff. The question is no longer how to fit everything in, but what you keep in-context on every call versus what you retrieve on demand.


### Short-term versus long-term memory


Your agent’s short-term memory is the context window itself: the current conversation, injected documents, and recent tool results. It is fast and needs no infrastructure, but it is session-bound. When the session ends, it is gone.


Long-term memory persists across sessions. In practice it splits into a few types. Episodic memory logs what happened and when, so the agent can answer “what did we work on last week?” Semantic memory holds external facts, which is where retrieval feeds in. Procedural memory encodes repeatable workflows, usually in a version-controlled instruction file.


The honest advice is to avoid overbuilding. Start with conversation history in Postgres and a structured system prompt. Add retrieval when history outgrows the context window. Add agentic memory management only when the agent genuinely needs to learn across sessions.


### Vector databases and retrieval-augmented generation (RAG)


Your model does not know your documents. It was not trained on your internal knowledge base or anything after its cutoff.[RAG](https://mastra.ai/blog/rag-tutorial) fixes this by converting documents into embeddings, storing them in a vector store, and retrieving only the most relevant chunks at query time through semantic search.


Your store choice depends on scale and existing infrastructure. The table below compares common options so you can shortlist before committing.


**Option** **Type** **Best for**


**Pinecone** Managed Shipping fast with zero infrastructure to run


**Weaviate** Open source, managed cloud Hybrid search combining vectors, keywords, and filters


**pgvector** Postgres extension Teams already running Postgres who want low-friction RAG


For many teams, pgvector is the pragmatic default. It adds vector search to a database you probably already run, and it handles millions of vectors with HNSW indexing before you need anything dedicated. Frameworks like **LlamaIndex** simplify the chunking and retrieval pipeline if you want higher-level abstractions over your store. Reach for a specialized option when your retrieval needs exceed what your relational database comfortably handles.


### Storage solutions: open and closed source


You will store more than vectors. Conversation history, agent state, and memory blocks all need durable homes. Postgres remains the workhorse here, and with the pgvector extension it can serve both relational state and semantic search from one system. Specialized memory stores like **Zep** handle long-term agent memory with built-in summarization, while caching layers like **Redis** speed up hot-path lookups for session state and recently used context.


Specialized memory infrastructure earns its place when in-context memory breaks down, for example when agents share state across instances or maintain continuity across provider switches. The tradeoff is portability. Your primary database travels easily, while dedicated memory services are harder to migrate away from later.


## Building across the AI agent stack with Mastra


You do not have to assemble every layer from separate vendors.[Mastra](https://mastra.ai/ai-agent-framework) is an open-source TypeScript framework, licensed Apache 2.0, that spans most of these layers in one place: agents, workflows, memory, a model router that reaches 90+ providers through one interface, observability, and tracing. It is built on Vercel’s AI SDK and extends it with the pieces production agents actually need.


*Mastra Studio brings agent definitions, workflow graphs, and run traces into a single view during local development.*


The workflow engine chains steps explicitly with .then() and .branch(), memory persists across sessions, and every run produces a trace you can inspect in Studio or export to your preferred backend. Mastra also supports MCP servers, so your tools stay portable across frameworks.


Mastra is TypeScript-only, so it is not the right fit if your team lives in Python, and it is a younger project than some long-established libraries. It deploys to Vercel, Netlify, Cloudflare, or a standalone Node server, and it is free to start with no seats or usage tiers.


[Build your first traced TypeScript agent on Mastra.](https://mastra.ai/docs)


## Layer 4: Frameworks and SDKs


You wire the model, tools, and control flow together at the framework layer. This is where the agent’s actual logic lives: what it does next, when it calls a tool, how it handles the result, and how the reasoning loop stays coherent across steps. It is also the layer with the highest lock-in risk, because orchestration code rarely ports cleanly between frameworks.


Two years ago there was effectively one framework. Now you choose between several camps: a provider’s built-in SDK, a graph-based orchestration framework, or raw code with thin wrappers. That choice is itself a design decision.


### What an orchestration framework handles


Your orchestration layer owns control flow and state, not the individual tool logic. It decides sequencing, manages retries and timeouts, tracks execution stage, and coordinates multiple steps into a coherent run. Tools sit below it. The model sits beside it. The framework is the connective tissue.


A useful line to draw: the tool layer answers “what actions are available,” while the orchestration framework answers “in what order, under what conditions, and with what state.” Keeping that boundary clean makes both layers easier to test and to swap.


### Workflows and multi-step orchestration


Your simplest agents are stateless tool callers that answer a question and stop. Real work often needs multi-step workflows where steps depend on each other, things fail mid-run, and a human sometimes has to approve before the agent acts. That is where explicit orchestration pays off.


*An orchestration workflow routes a task through discrete, inspectable steps rather than a single opaque model call.*


Multi-agent workflows introduce additional complexity: coordinating handoffs between agents, propagating context, and maintaining consistent guardrails across agent boundaries. These are distinct from single-agent pipelines and need their own orchestration patterns.


### Choosing a framework: API clarity and production readiness


Your framework choice is really a bet on migration cost and how much the abstraction helps versus fights you. The table below compares the three main approaches.


**Approach** **Examples** **Strengths** **Tradeoffs**


Provider SDK **OpenAI** SDK Fastest start, tight model integration Locks you to one model provider


Graph-based framework **LangGraph** , **CrewAI** , **AutoGen** Explicit state management, portable across models Steeper learning curve, heavier abstraction


Thin wrapper or raw code Custom code over MCP Full control, minimal dependencies You build every feature the framework provides


LangChain popularized the tool-schema pattern before MCP standardized it, and its tooling still offers the widest library of pre-built integrations. Graph-based frameworks like LangGraph add explicit state transitions for complex agentic AI workflows, while multi-agent systems like AutoGen and CrewAI handle agent-to-agent coordination.


The common mistake is picking too much framework. If your agent calls a model and a few tools, you do not need a heavy graph engine. Match the framework to the agent type, and remember that the Model Context Protocol is the one layer that transfers across all three camps.


## Layer 5: Evals, observability, and tracing


You measure whether your agent actually works at this layer, and it is where most production quality is won or lost. The reason is simple: LLMs fail silently. A hallucinated answer returns a clean 200, so traditional monitoring sees a successful request while your agent gives wrong answers for days.


Observability and evaluation together form the biggest gap between prototype and production. Most prototypes ship with zero evals, and you do not feel the pain until real users find the failures for you.


### Tracing and monitoring agent runs


Your first move is tracing. A trace follows every step of a run: each model call, tool invocation, retrieval query, and intermediate reasoning step, along with how long each took and what tokens flowed through. Without it, a multi-step failure is a black box.


*An agent trace renders each step as a span, so you can see exactly where a run went wrong and how long it took.*


[Mastra’s tracing](https://mastra.ai/ai-agent-observability) records model calls, tool runs, and workflow steps as spans with inputs, outputs, latency, and token usage, and it exports to **OpenTelemetry** -compatible backends. Structured tracing is what turns “the agent is acting weird” into a specific span you can inspect. Platforms like **LangSmith** and **Langfuse** cover tracing and eval for LangChain-compatible stacks, each with different strengths in visualization and production alerting.


The table below compares tracing and eval tooling across the stack so you can match capabilities to your framework and workflow.


**Tool** **Best for** **Framework tie-in**


**LangSmith** Deep tracing and dataset management for LangChain and LangGraph stacks LangChain tooling


**Langfuse** Open-source tracing, prompt management, and cost tracking Framework-agnostic


Mastra tracing Span-level observability for agents, workflows, and tool runs with OpenTelemetry export Mastra framework


### Evals, datasets, and testing agents


Your evals score outputs against metrics that matter: faithfulness to retrieved context, relevance to the question asked, and hallucination rate. Evaluation is converging on three tiers: fast checks on every pull request, nightly regression suites judged by an LLM, and continuous production monitoring that alerts on drift.


If you want to avoid silent regressions, build evals before you deploy, not after something breaks. As Sam Bhagwat argues in[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) , treating evaluation as infrastructure is what keeps both accuracy and token cost under control in production.


### Debugging failures in production


Your production failures usually trace to one bad step early in a run that doomed everything after it. If your eval only checks the final output, you will never learn why. Step-level traces plus scoring on intermediate steps are what make root-cause debugging possible.


Current tooling is strongest on single-turn and tool-calling evaluation. Multi-agent evaluation and long-horizon task assessment remain partly unsolved, so if your agent does either, expect to write some custom eval logic beyond what platforms offer today.


## Layer 6: Guardrails and safety


You stop your agent from doing things it should not at the guardrails layer, and it is the least mature part of the AI agent stack. Agent guardrails split from LLM guardrails once agents started calling tools, spending money, and taking real actions. Filtering the output is too late if the agent already sent the email. This matters more as agents become autonomous systems that act without human approval on every step.


Expect to spend unplanned engineering time here. There is no dominant framework and few established patterns, which means you write most of the policy code yourself.


### Prompt injection and input validation


Your agent’s inputs are a live attack surface. A malicious tool description or a crafted user message can hijack the reasoning loop and steer the agent toward actions you never intended. Prompt injection is not theoretical, and it gets worse as agents connect to more external sources.


Input validation is your first line of defense. Constrain what enters the context, treat tool descriptions from external servers with suspicion, and never assume that content pulled from a document or a webpage is safe just because it parsed.


### Output sanitization and policy enforcement


Your guardrails work best when they sit at the tool-execution layer, not the output layer. The pattern you converge on after learning the hard way is “guardrails before action”: authorize the tool call before it runs, enforce rate limits, and validate what the agent is about to do while it can still be stopped.


Enforcing policy in real time means tracking agent state as it acts. Multi-agent setups make this harder, since propagating guardrails across agent boundaries when one agent delegates to another is still an open problem.


## Layer 7: Deployment infrastructure


You ship the agent to real traffic at the deployment layer, and this is where a flawless demo turns into a maintenance problem. Deploying an agent is trickier than deploying an LLM because state and secure tool execution have to survive outside the script they were written in.


Most teams still deploy with their own server code, though managed agent infrastructure is maturing. The right choice depends on latency, cost, and how much operational control you want.


### Agent hosting and serving


Your agent should run as a service with a clean API, not a script trapped in a notebook. That means persisting tools and their dependencies, normalizing agent state into a database, and defining interactions through stable endpoints. Containerizing with[Docker](https://aws.amazon.com/docker/) gives you consistent behavior across environments and a clean path to any deployment target.


Serving shape matters too. A synchronous API works when the agent finishes in a couple of seconds. When a run involves multiple tool calls or long retrieval that takes 30 to 60 seconds, an async queue is the better fit: the client submits a job, gets an ID, and polls for the result.


### Serverless, standalone servers, and embedding in existing apps


Your deployment target should match how the agent is used. Serverless platforms suit spiky, low-maintenance workloads. A standalone server suits steady traffic and full control. Embedding the agent directly in an existing app suits teams that want the agent to live alongside their product code.


Beyond TypeScript runtimes, teams running Python services often deploy with **FastAPI** on[Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) or[AWS](https://medium.com/@saikiransarvepalli/understanding-aws-infrastructure-a-complete-2040101f1c0d) infrastructure. TypeScript agents have a similarly wide range of targets: Vercel, Netlify, Cloudflare, a standalone Hono server, or Node runtimes like Express and Fastify. The same agent can slot into most existing JavaScript infrastructure without a rewrite.


## Putting the stack together: a coding agent walkthrough


You can see the whole stack working at once in a coding agent, the most proven application of the AI agent tech stack. Tools like the leading code assistants exercise all seven layers under heavy real-world load, which makes them a useful reference build.


Walking through how each layer shows up in a real system is the fastest way to internalize what belongs where, and where the sharp edges are.


### Mapping each layer to a real build


Your coding agent touches every layer in sequence. At the inference layer it routes between foundation models depending on task difficulty. At the protocols layer, MCP servers connect it to the editor, terminal, filesystem, and Git. The memory layer uses codebase-aware retrieval so it pulls only the files relevant to the current edit.


At the framework layer, these systems use purpose-built orchestration rather than a generic engine. At the eval layer, acceptance rates feed continuous production evaluation. At the guardrails layer, sandboxed execution lets the agent run code without touching anything outside its container. Deployment ties it together into a service that serves constant traffic.


### Tradeoffs and common pitfalls


Your biggest risk is building like it is still an earlier era: reaching for a heavy framework before you know you need state, adding a dedicated store before outgrowing Postgres, or designing a multi-agent system before shipping one agent that works. A tool-calling chatbot and a multi-agent research system share almost no infrastructure.


The common failure pattern is predictable: no evals, output-only filtering, and a system prompt that grows until the context window chokes. Invest in the layer where your specific agent feels the demo-to-production gap most, rather than half-building all seven at once.


## Wrapping up


The model gets the attention, but the six layers beneath it decide whether your agent survives production. Map the layers your problem actually needs, invest first where the demo-to-production gap is widest, and add complexity only when something specific breaks. If you are building in TypeScript,[Mastra](https://mastra.ai/ai-agent-framework) gives you agents, workflows, memory, routing, and tracing across these layers in one framework, so you can measure quality from your first deployment.
