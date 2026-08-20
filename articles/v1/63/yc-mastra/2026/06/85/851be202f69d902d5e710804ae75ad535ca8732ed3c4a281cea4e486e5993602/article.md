---
schema_version: "1.0.0"
document_id: "851be202f69d902d5e710804ae75ad535ca8732ed3c4a281cea4e486e5993602"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/ai-agent-framework"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:57:46.286490+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:8f898399124c7b389eccdcd1d10c780eff6881d68f29ec13f6b321b36fdf5996"
---

# AI agent framework: a practical guide to choosing and using the right one

Your next AI project probably won't start from a raw LLM API call. You'll reach for an AI agent framework, and the one you pick will shape everything from how you define tools to how you debug production failures six months later. According to a, 80% of enterprise applications now embed at least one AI agent, up from 33% just two years ago, which means your framework decision carries real architectural weight.


This guide breaks down what an AI agent framework actually provides, how the major options compare on language support and orchestration models, and what implementation patterns matter once you start building.


## What is an AI agent framework?


An AI agent framework is a software layer that sits between your application code and the LLM providers, giving you pre-built abstractions for the components agents need: tool calling, memory, orchestration, and structured output. Instead of stitching together raw HTTP calls to OpenAI or Anthropic, you work with higher-level primitives that handle the plumbing.


### How frameworks differ from raw LLM APIs


If you build directly with a provider SDK, the pattern looks like this: construct a messages array, send it to a completions endpoint, parse the response, and manage state yourself. That works fine for single-turn tasks like summarizing a document or extracting structured data from a resume.


Agents are different. They call tools in a loop to achieve a goal, accumulating context across turns. You need to manage conversation history, handle tool call results flowing back into the prompt, deal with token limits, and decide when the agent should stop. A framework gives you these primitives so you aren't reinventing them on every project.


### Core components: agents, tools, memory, and orchestration


Before you evaluate individual AI agent frameworks, you'll want to understand the four building blocks most of them share. The APIs look different across frameworks, but these concepts appear everywhere.


Component What it does Example


Agents The core loop that receives a goal, reasons about next steps, selects tools, and generates responses A support agent that triages tickets, queries a knowledge base, and drafts replies


Tools Functions the agent can invoke, each with a typed schema describing inputs, outputs, and when to use it searchOrdersByCustomerId, sendEmail, queryDatabase


Memory Mechanisms for retaining context across turns and sessions, from simple message history to vector-based semantic recall Working memory for user preferences, semantic recall for past interactions


Orchestration The control flow connecting agents, tools, and workflows through chaining, graph-based branching, or multi-agent supervision A supervisor agent routing tasks to specialized subagents


## Factors to consider when choosing an AI agent framework


Your framework choice constrains your architecture, so it's worth being deliberate. The right pick depends less on feature counts and more on how well the framework fits your actual workload.


### Task and workflow complexity


You should start by mapping out what your agent actually needs to do. A single agent handling classification or Q&A has different framework requirements than a multi-agent system with branching workflows, human approvals, and parallel execution paths.


For straightforward chains, most frameworks work. For stateful, multi-step workflows with conditional logic, you need a framework with explicit workflow primitives, not just prompt chaining.


### Integration needs and tooling fit


Your agents will need to connect with external services: calendars, CRMs, databases, search APIs. Evaluate how your framework handles tool integration. Does it support MCP (Model Context Protocol) for standardized tool access? Does it provide client abstractions so you aren't reimplementing specs for every third-party service?


As of 2026, model context protocol is the established standard for connecting agents to third-party tools via mcp servers. A framework with first-class MCP support saves you significant integration work.


### Customizability and scalability


Your framework choice shapes how much you can customize as requirements evolve. Some frameworks optimize for getting started quickly but constrain you when needs get specific. Others offer low-level control at the cost of more boilerplate.


Consider how the framework handles model routing. Can you swap providers with a one-line change, or are you locked into a specific SDK? As our book[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) puts it, the progression is "make it work, make it right, make it fast and cheap." Start with larger models, then route to cheaper ones for structured extraction tasks that need less reasoning.


### Data privacy and security


Your agents will process sensitive data. Evaluate where data flows, whether the framework supports on-premise deployment, and how it handles authentication and authorization. Agents are more powerful than pre-LLM data access patterns, so you may need more granular permissions than your current stack provides.


Simon Willison's "lethal trifecta" describes a specific risk: when an agent combines access to private data, exposure to untrusted content, and external communication ability, an attacker can use prompt injection to exfiltrate data. Your framework should make it straightforward to constrain these vectors.


### Ease of use vs. control trade-offs


You'll hit a tension between frameworks that abstract away complexity and those that expose it. No-code platforms let non-engineers build simple agents fast, but they introduce platform lock-in and limit architectural decisions. Code-first frameworks give you full control but require more upfront investment.


Match the framework to your team's skill level and your project's long-term needs. If your team writes TypeScript daily, a TypeScript-native framework will feel more natural than a Python wrapper with TypeScript bindings bolted on. Similarly, if your team works primarily in JavaScript, look for frameworks with first-class JS support rather than porting a Python-first tool. And if you're building generative AI applications (tools that produce text, code, or structured content as their primary output) the framework's output handling and streaming support matter as much as its orchestration model.


## Popular AI agent frameworks compared


You'll encounter several established frameworks when evaluating your options. Each makes different architectural trade-offs, and none is universally best. The sections below cover how the major ones differ and where each fits.


Framework Primary language Best for Orchestration model MCP support


Mastra TypeScript Full-stack TS agent development Graph-based workflows Native


LangChain Python, JS Modular LLM chains, RAG Sequential chaining Community


LangGraph Python, JS Stateful multi-agent workflows Graph (nodes + edges) Community


AutoGen Python Multi-agent conversation Async messaging Community


CrewAI Python Role-based team collaboration Sequential / hierarchical Community


LlamaIndex Python, TS Data-heavy RAG + agents Event-driven Community


Semantic Kernel C#, Python, Java Enterprise integration Plugin + process Native


### Mastra


If your stack is TypeScript, you’ll soon notice that most AI agent frameworks are Python-first.[Mastra](https://mastra.ai/) gives you agents, workflows, memory, evals, and observability in one framework, with a built-in model router (AI SDK providers are supported too, if you want them). It is open source under the Apache 2.0 license and built specifically to close the gap between Python-dominated tooling and the TypeScript teams that need production-grade agent infrastructure.


Mastra's model router connects to hundreds of models across dozens of providers through a single interface, including OpenAI, Anthropic, Azure OpenAI, Google, and self-hosted models via Ollama. Swapping from one provider to another is a config change rather than a rewrite. The middleware layer handles concurrency, state management, and retry logic. Mastra also ships with native MCP support, LangSmith-compatible OpenTelemetry tracing, and a local development environment (Mastra Studio) for inspecting agent decisions before you deploy.


Mastra supports[agentic workflows](https://mastra.ai/articles/agentic-workflows) natively: you define steps with typed schemas, connect them with .then(), use .parallel() for fan-out execution, add conditional routing with .branch(), and call .commit() to finalize your workflow graph. If you need to build assistants, customer-facing agents, or internal automation tools in TypeScript, Mastra provides the full stack in a single package. You can also build coding agents or integrate with tools like Claude Code for developer-facing workflows where the agent needs to write, test, or execute code.


Teams that want to run open-source models or experiment with models from Hugging Face can do so through Ollama integration, giving you local model execution alongside Mastra's production-grade tooling. The evals framework lets you score agent outputs against rubrics in CI, so prompt changes that degrade accuracy fail the build before reaching production.


[Build your first TypeScript agent on Mastra.](https://mastra.ai/)


### LangChain


LangChain is an open-source framework with a modular architecture. You chain components together: prompts, LLM calls, output parsers, memory, and tools. It works well for straightforward agent patterns like Q&A with retrieval or document summarization. LangChain is Python-first with a JavaScript port available, and it remains one of the most widely used frameworks for building RAG pipelines and conversational AI pipelines.


The trade-off is that complex multi-step workflows can become hard to reason about as langchain chains grow. LangChain's strength is its wide range of integrations and its large community.


### LangGraph


LangGraph extends LangChain with a graph-based execution model. You define agent tasks as nodes and transitions as edges, which makes stateful, branching workflows more explicit. It supports cyclical and conditional flows, human-in-the-loop steps, and persistent state across interactions. LangSmith, LangChain's observability companion, integrates natively with LangGraph and gives you trace-level visibility into every step.


If you need fine-grained control over multi-agent orchestration in Python, LangGraph is a strong option. The graph model adds complexity, but it pays off for workflows with non-linear execution paths.


###


###


###


###


###


###


###


### AutoGen


AutoGen is Microsoft's open-source framework for multi-agent conversations and a core part of the broader microsoft agent framework ecosystem. Its core idea is that agents interact through asynchronous messaging, supporting both request-response and event-driven patterns. It provides a layered architecture: a Core programming framework, an AgentChat layer for conversational patterns, and Extensions for integration. AutoGen also connects with Azure OpenAI natively, making it a natural fit for teams already in the Microsoft cloud. Microsoft foundry and related Azure AI services can also be combined with AutoGen for enterprise deployments that require managed infrastructure and compliance guarantees.


AutoGen shines when your use case involves agents debating, collaborating, or handing off tasks dynamically. It's one of the more capable options for building multi-agent systems that need to coordinate complex, asynchronous workflows. The messaging model is flexible but requires careful design to keep agent conversations from spiraling.


### CrewAI


CrewAI uses a role-based metaphor: agents are "crew members" with defined roles, goals, and backstories. Tasks are distributed across the crew, and execution can be sequential or hierarchical with a manager agent overseeing delegation. CrewAI has grown rapidly since its launch and is now one of the most popular frameworks for teams that want to express multi-agent coordination in business-readable terms.


This model maps well to business workflows where you can describe the problem in terms of job roles. The natural language configuration makes crewAI approachable, though it can feel constraining for highly custom agent architectures.


### LlamaIndex


LlamaIndex started as a data ingestion and retrieval framework and has expanded into agent workflows. Its event-driven architecture means workflow steps communicate through events rather than explicit graph edges, enabling more dynamic transitions. LlamaIndex is one of the most mature frameworks for building agents that need to search, retrieve, and synthesize from large document corpora, and its integration ecosystem covers a wide range of vector databases and storage backends.


If your primary use case is RAG-heavy (with agents that maintain detailed knowledge from a large document set) llamaindex is worth evaluating. Its data connectors and indexing primitives are mature and its retrieval-focused architecture is a natural fit.


### Semantic Kernel


Semantic Kernel is Microsoft's SDK for integrating LLMs into enterprise applications. It combines traditional programming logic with AI capabilities through a plugin-based architecture. Its Agent Framework and Process Framework support multi-agent orchestration. Like AutoGen, semantic kernel connects with Azure OpenAI, and both tools are central to the microsoft agent framework strategy. Alongside AutoGen, Semantic Kernel represents Microsoft's two-pronged approach to the microsoft agent framework landscape: AutoGen for conversational and research-oriented multi-agent patterns, Semantic Kernel for structured enterprise integrations.


The enterprise focus shows in its design: strong typing, extensible connectors, and production-grade security. Semantic Kernel supports C#, Python, and Java, but not JavaScript or TypeScript, making it a better fit for .NET or JVM shops than for TypeScript teams.


### Google ADK


Google ADK (Agent Development Kit) is Google's framework for building and deploying AI agents. Like other framework options, google adk provides primitives for tool definition, orchestration, and memory. It integrates natively with Google's model ecosystem, including Gemini, and supports deployment to Google Cloud infrastructure. Teams building on Google Cloud or already using Vertex AI will find google adk reduces integration overhead significantly.


### OpenAI Agents SDK


The OpenAI agents sdk gives developers a lightweight way to build agents directly on top of OpenAI's models. It handles the tool-calling loop, handoffs between agents, and guardrails natively. For teams that are already committed to OpenAI and want minimal abstraction overhead, the OpenAI agents sdk is a practical starting point before investing in a more full-featured framework.


### Ollama


Ollama lets you run open-source language models locally, which changes the economics for teams that don't want to send data to cloud providers. You can use Ollama alongside most major frameworks via standard API compatibility, giving you a self-hosted inference option without abandoning your existing tooling.


### No-code and low-code options


If your team includes non-engineers who need to build agent workflows, tools like n8n and Langflow offer visual, drag-and-drop interfaces. n8n connects AI capabilities with app integrations through trigger-based workflows. Langflow provides a visual builder for multi-agent and RAG workflows.


These platforms trade control for speed. They're effective for prototyping and simpler automations, but they can hit limits when you need custom orchestration logic or granular performance tuning.


## Building agents in TypeScript: implementation patterns and workflow design


Once you've chosen your AI agent framework, the real work begins. The patterns below apply broadly, but the code examples reflect a TypeScript-native approach.


### Defining agents with typed instructions, models, and tools


You should start by whiteboarding your agent's capabilities before writing any code. List everything you want the agent to do, group related capabilities together, and figure out natural divisions. Then write each group as a focused agent with a clear role.


When you define an agent, give it a specific system prompt with a role, constraints, and avoidance instructions. Design your tools carefully: use semantic naming (e.g., getBooksByGenre instead of fetchData), provide detailed descriptions, and specify typed input/output schemas. The quality of your tool definitions matters more than the number of tools. Type safety at the schema level also reduces the risk of malformed tool calls reaching your downstream systems.


If your agent tries to handle too many tasks, its tool selection accuracy drops. Start with one problem, build that agent well, notice what users ask for next, and split or add agents as the workload demands it.


### Graph-based workflow orchestration: branching, parallelism, and human-in-the-loop


Your agents and workflows aren't mutually exclusive. Agents handle open-ended reasoning, while graph-based workflows provide deterministic scaffolding. You'll typically use both together.


Understanding a handful of core primitives will cover most workflow patterns you encounter:


-


**Chaining:** feed the output of one step into the next, where each step does exactly one thing


-


**Branching:** trigger multiple LLM calls on the same input in parallel, such as checking a medical record for twelve different symptoms simultaneously


-


**Conditions:** make decisions based on intermediate results, routing execution down different paths


-


**Suspend and resume:** pause a workflow for human approval or a slow external API, persisting state to a durable store so it survives server restarts. This is similar in principle to checkpointing in long-running compute jobs.


For human-in-the-loop patterns, you have three main approaches: context injection (the human provides additional information mid-execution), draft review (the human reviews output before delivery), and deferred tool execution (the human provides feedback asynchronously). Deferred execution often fits real-world workflows best because you don't need to babysit the agent in real time.


### Managing memory and retrieval across agent runs


Your agents accumulate tokens quickly. Tool results, conversation history, and[agent memory](https://mastra.ai/articles/agent-memory) all add up, and context management becomes a core engineering challenge.


Three established memory types address different needs:


-


**Working memory:** stores persistent user characteristics across sessions


-


**Semantic recall:** embeds text as vectors and uses similarity search via vector databases to surface relevant history


- **Observational memory:** compresses session interactions into structured observations with timestamps, using a separate observer agent to distill raw messages into persistent memory


State management is a related concern. When an agent run suspends (for a human approval or an external callback) the workflow state needs to be serialized and stored durably so the agent can resume exactly where it left off.


Sometimes expanding your context window isn't the right solution. You may want to deliberately prune context. Token limiters prevent errors by removing oldest messages. Tool call filters strip verbose tool interactions from memory. And prompt caching, now standard across most providers, reduces cost for frequently used system prompts and knowledge bases.


## Observability, telemetry, and multi-agent orchestration


Your agent can regress while still returning 200 OK. Traditional monitoring won't catch it. You need observability and evals purpose-built for non-deterministic systems.


A trace is a tree of spans, following the OpenTelemetry standard, that shows how long each step took, the exact JSON flowing into and out of LLM calls, and metadata like status and latency. Good telemetry answers two questions that are uniquely hard in AI applications: accuracy and token cost. Tools like LangSmith (for LangChain and LangGraph workflows) and[Mastra's built-in Studio](https://mastra.ai/studio) give you this visibility in local development before you've deployed anything.


For multi-agent orchestration at scale, you need middleware that manages concurrent agent runs without race conditions. Concurrent execution is common in production systems, with multiple agents processing requests in parallel, and without proper concurrency controls you risk corrupted state or duplicate tool calls.


## Guardrails: prompt injection protection and output validation


Your agents face security threats that traditional software doesn't. Input guardrails intercept incoming messages to catch prompt injection, jailbreak attempts, and PII before the LLM processes them. Output guardrails screen results for data leakage, hallucination, and policy violations before they reach the end user. Function calling behavior also needs scrutiny: agents that invoke functions with elevated permissions should require explicit scoping and logging of every call.


Models have gotten better at resisting direct jailbreaks, but prompt injection has grown more sophisticated as agents gain autonomy. Name your guardrails by what they do and treat them as a core part of your agent architecture, not an afterthought.


## Wrapping up


Your agent framework decision comes down to your team's language, your workflow's complexity, and how much control you need over routing, memory, and observability. Start with the simplest framework that handles your actual workload, instrument it from day one, and layer in evals before you go to production.
