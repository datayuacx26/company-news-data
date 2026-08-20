---
schema_version: "1.0.0"
document_id: "9ec3a6fdca09d7ed9c1149d914e52a9d52cdf6f18cc4d103483c285a27c9646a"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/multi-agent-systems"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-10T16:04:57.697539+00:00"
fetched_at: "2026-08-10T16:04:59.642666+00:00"
content_hash: "sha256:0af1530c3081ffd559ba811ade456382b23d521b1f11677059ed294095ab2975"
---

# Multi-agent systems: architectures, frameworks, and real-world applications

You have a request that needs flight data, hotel pricing, calendar coordination, and a personalized itinerary. A single AI agent can attempt all four, but it will lose coherence across tasks, struggle to recover when one step fails, and eventually hit context limits.


Multi-agent systems (MAS) solve this by creating specialized agents with scoped, specific tasks and building a communication network between them.


You are working with a pattern rooted in distributed AI and robotics research from the 1990s. Large language models made the same approach practical for production software. A[2025 survey from LangChain](https://www.langchain.com/state-of-agent-engineering) found that 51% of organizations run agents in production, powering coding assistants, agentic AI research workflows, and enterprise automation.


This guide covers how these systems work, the architectural patterns behind them, the frameworks you can use to build them, and the strategies that keep them reliable.


## What is a multi-agent system?


A multi-agent system is a computational system composed of multiple intelligent agents that interact to solve complex problems with many steps. You can think of each agent as an autonomous worker with its own goals, memory, and decision-making logic.


In modern LLM development, multi-agent systems are composed of agents that divide labor, share results, and check each other’s work.


In production, this usually means pairing autonomous, tool-using agents with a coordination layer that decides how work gets split up and how partial results get reassembled into a final answer. The sections below cover the properties that define these agents and how their coordination can produce useful, and occasionally unpredictable, emergent behavior.


## Core characteristics of agents


Your AI agents in these systems share several defining properties that separate them from simple function calls or microservices.


-


**Autonomy:** each agent plans, reasons, and acts independently without waiting for centralized instructions at every step


-


**Local views: no single agent holds the full picture of the system’s state, which forces distributed coordination and intentional knowledge sharing — except in a supervisor structure, where the lead agent holds the full context by design**


-


**Decentralization:** the system avoids relying on one controlling node, though some architectures introduce a routing layer for task allocation


-


**Tool access:** agents call external tools, APIs, databases, and other agents to accomplish their assigned work


### Self-organization, self-direction, and emergent behavior


When your AI agents share knowledge using agreed-upon communication protocols, they can converge on solutions through negotiation and feedback loops. This emergent coordination is both a strength and a risk. It enables adaptive problem-solving, but it also means your system can produce unexpected outcomes that are difficult to trace without proper tracing and logging.


In practice, this often takes the form of proposal-and-critique rounds: one agent suggests an approach, others flag gaps or conflicts, and the group converges after a few iterations rather than following a fixed script. The more autonomy you give agents to reach that convergence themselves, the harder it becomes to predict the exact path they will take to a solution, even when the final output is correct. This dynamic shows up in decentralized, handoff, and council-style systems, where no single agent holds the full picture; it looks different in a supervisor pattern, where one agent keeps context for the whole task and there typically isn’t this kind of back-and-forth between peers.


## Single agent vs. multi-agent systems


You need to understand where single-agent systems break down before committing to the added complexity of a distributed agent architecture.


### Where single agents fall short


Your single AI agent handles one thread of execution and one context window. Three practical limitations emerge quickly in production.


-


**Less reliability for complex tasks:** Increased scope and responsibility makes it less likely a single agent can reliably perform all steps of a task


-


**Context window saturation:** long documents, multi-turn conversations, and complex instructions push a single agent past its effective window, degrading output quality


-


**Hallucination compounding:** without other agents to verify outputs, hallucinated facts propagate unchecked through downstream steps


-


**More challenging auditing:** having one agent handling everything makes it harder to identify bottlenecks and track down bugs


-


**Sequential bottleneck:** a single agent’s reasoning loop still runs one step at a time, even when it fans out parallel tool calls within a step, so latency compounds as the number of sequential reasoning turns grows with task complexity


### When multi-agent systems are the right call


You should reach for this architecture when your task spans genuinely different kinds of work, when one agent would need more context or tools than it can reasonably hold, when parts of the task can run in parallel, or when different stages need different prompts, models, or guardrails. If one agent checks flight availability while another checks hotel pricing and a third checks calendar conflicts, you gain real parallel processing, cross-verification, and fault isolation, because none of those lookups depend on the others finishing first. The default is still one agent: add more only when the added structure clearly improves quality, speed, or reliability.


The tradeoff is coordination overhead: more AI agents means more communication protocols, more potential failure points, and more infrastructure to monitor.


## Advantages of multi-agent systems


Distributing work across multiple AI agents instead of overloading a single one gives you several concrete benefits.


### Flexibility and adaptability


Your system can add, remove, or swap agents without rewriting the entire pipeline. If a new data source becomes available, you add an agent that knows how to query it. If a capability becomes obsolete, you remove that agent.


This modularity makes these architectures easier to evolve than monolithic single-agent setups. It also lets you iterate on individual agents without risking regressions elsewhere in the system.


### Scalability


Your agents can run in parallel, processing multiple components simultaneously. This horizontal scalability means your system’s throughput grows with the number of agents you deploy, not with the size of a single model’s capacity. For workloads with high concurrency requirements, adding agents is often more cost-effective than scaling up one model.


### Domain specialization and greater performance


Your agents perform better when each focuses on a narrow domain. A coding agent that only writes TypeScript outperforms a generalist agent that also handles legal research and customer support. The system as a whole will tend to outperform a single-agent system because each specialized agent produces higher-quality outputs on the specific task it owns.


## Architectures of multi-agent systems


How you organize your agents determines how they discover each other, share information, and recover from failures. The table below summarizes four patterns Mastra uses to describe multi-agent systems, which map more directly onto how these systems actually get built in production than the academic centralized/decentralized taxonomy does.


**Pattern** **Coordination model** **Resilience** **Best for**


Supervisor agents One lead agent stays in control for the full task and decides when and what to delegate Medium — the supervisor is a bottleneck, but it keeps full context on the task Tasks where one agent needs to hold context across the whole run and make ongoing judgment calls


Handoffs One agent transfers control to another, which takes full ownership of the next part Depends on whichever agent currently holds control; a failure is contained to that agent Conversational systems where a specialist should own a turn completely once picked


Workflows The execution path is defined in code: steps, branches, loops, and parallel blocks High — deterministic paths are easy to retry, resume, and reason about Multi-step processes where the sequence of work is known ahead of time


Council Multiple agents work the same problem independently, then their outputs are compared or synthesized High — no single agent’s mistake dominates the final result Tasks that benefit from independent takes, like research synthesis or review


*Orchestrator routing workflow: a lead agent inspects each incoming task and routes it to the appropriate specialized agent.*


### Supervisor agents


You designate a lead agent that stays in control for the full task: it holds the context, decides when and what to delegate, and assembles the final result. Communication is simple because every specialist reports back to the same supervisor. The tradeoff is that the supervisor is a bottleneck — if it goes down or gets overloaded, the task stalls. You can extend this pattern with more layers, a supervisor of supervisors, for very large task decompositions; that extra layering is essentially what a hierarchical structure adds on top of the same centralized idea. This pattern works well when a task benefits from one agent holding full context and making ongoing judgment calls.


### Handoffs


In a handoff pattern, one agent transfers control to another, and the agent receiving control takes full ownership of the next part of the task instead of reporting back up to a coordinator. This suits conversational systems where a specialist should own a turn completely once picked, such as routing a support conversation from a triage agent to a billing specialist. Failure is contained to whichever agent currently holds control, but there is no shared global state, so agents need clear rules for when a handoff should happen.


### Workflows


Some tasks do not need agents to decide the execution path at all. You define it directly in code as steps, branches, loops, and parallel blocks, and agents fill in the reasoning at each step. This gives you a deterministic, retryable path that is easy to reason about and debug, at the cost of flexibility: the workflow only handles the branches you have explicitly designed for.


### Council


In a council pattern, you ask multiple agents to work the same problem independently, then compare or synthesize their outputs. This works well for tasks that benefit from diverse independent takes, like research synthesis or review, since no single agent’s mistake dominates the final result. The tradeoff is cost: you are running the same problem through multiple agents and then paying for a synthesis step on top.


## How LLM-based agent teams work


In a supervisor-style setup, your system follows a repeatable cycle; handoff, workflow, and council patterns skip or reorder several of these steps. Here is how the supervisor flow breaks down step by step:


1.


**Receive the task:** the orchestrator agent accepts an incoming request from a user or upstream system.


2.


**Decompose the task:** the orchestrator breaks the request into discrete subtasks based on domain boundaries.


3.


**Assign subtasks to specialized agents:** each component is routed to the AI agent best equipped to handle it, based on explicit routing logic or agent negotiation.


4.


**Agents reason and act:** each agent uses its large language model, tools, and memory to process its assignment, producing intermediate outputs.


5.


**Assemble results:** the orchestrator collects outputs from all agents, resolves conflicts, and produces a unified response.


### Roles, instructions, and specialization across agents


You define each agent with a system prompt that constrains its role, a set of tools it can access, and specific instructions for how it should reason.


A research agent might have access to web search and document retrieval tools, while a coding agent gets a code interpreter and file system access. This specialization keeps each agent’s prompt engineering focused and its working memory uncluttered.


### Inter-agent communication and coordination protocols


Your agents need a structured way to pass messages, share intermediate results, and signal completion or failure. Getting this exchange right is one of the harder engineering problems in a MAS.


This typically happens through shared memory stores, message queues, or direct function calls between agents. For cross-boundary delegation — handing work to an agent owned by another team, vendor, or runtime — the Agent2Agent (A2A) protocol is emerging as the practical standard: it exposes an agent as a discoverable agent card plus a JSON-RPC execution endpoint, so a parent agent can delegate a task to a remote agent without both sides sharing a framework or language. Academic multi-agent research has its own formal standards, like Agent Communication Language (ACL), but most production systems reach for A2A or simpler patterns instead.


Those patterns include JSON message passing, shared state objects, or workflow graphs that define the execution order. MCP (Model Context Protocol) is emerging as another option for exposing tools and resources across agent boundaries.


### Decision-making and task routing


Your orchestrator, if your architecture uses one, decides which sub-agent handles each component. It inspects the incoming request, breaks it into parts, and routes each part to the right specialist. Handoff and council patterns route work differently, without one central agent making that call. In more advanced setups, agents can negotiate among themselves to claim tasks, but most production systems use explicit routing logic to keep behavior predictable. The routing strategy you choose directly affects latency and resilience.


## Agent structures and behaviors


You can classify agent behaviors along two axes: how agents are organized and how they decide what to do.


### Team-based and coalition structures


Your agents can operate as persistent teams with fixed roles or as temporary coalitions that form around specific tasks. Teams are stable: the same set of AI agents works together across many requests. Coalitions are dynamic: agents join and leave based on the current workload. Teams give you predictability, while coalitions give you flexibility. Most production systems start with teams and introduce coalition-style patterns only when workloads vary significantly.


### Reactive vs. deliberative agent behaviors


You choose between two behavior modes for each agent. Reactive agents respond to stimuli without maintaining an internal model of the world, following simple condition-action rules. Deliberative agents maintain beliefs, form plans, and reason about future states before acting.


Most model-based agents are deliberative by default because the underlying model itself performs multi-step reasoning. You can constrain an agent to be more reactive by limiting its system prompt and disabling planning tools, which reduces latency at the cost of flexibility.


## Multi-agent frameworks


You have several multi-agent frameworks to choose from, each designed for building multi agent ai systems with different tradeoffs around language support, abstraction level, and production readiness. The table below compares the most widely adopted options.


**Framework** **Language** **Coordination model** **Best for**


CrewAI Python Role-based crew delegation Rapid prototyping of agent topologies


AutoGen Python Conversational topologies (now part of Microsoft Agent Framework) Research and human-in-the-loop experiments


OpenAI Agents SDK Python, TypeScript Explicit handoffs and guardrails between agents Teams building natively on OpenAI’s model ecosystem


LangGraph Python (TypeScript port growing) Graph-based state machines Fine-grained control over cyclic workflows


TypeScript workflow framework TypeScript Workflow graphs with code-level orchestration TypeScript teams building for production


### CrewAI


CrewAI lets you define a crew of AI agents, each with a role, a goal, and a backstory. You assign tasks to the crew and let the framework handle delegation and sequencing. It focuses on ease of setup and is popular for prototyping agentic AI systems quickly.


It is Python-based and integrates with common tool adapters for function calling. The tradeoff is less granular control over execution compared to graph-based frameworks like LangGraph. It works well when you want to validate an agent topology before investing in lower-level orchestration.


### AutoGen and other notable frameworks


AutoGen, originally from Microsoft Research, pioneered conversational agent topologies with human-in-the-loop patterns and flexible turn-taking; Microsoft has since folded its capabilities into the broader Microsoft Agent Framework. It remains a reasonable reference point for research, experimentation, and prototyping conversational agent pipelines.


You will also find newer entrants worth a look: Google’s Agent Development Kit (ADK) and the OpenAI Agents SDK, both aimed at teams who want first-party orchestration tied to their model provider’s ecosystem, and LlamaIndex Workflows, which focuses on event-driven orchestration for RAG-heavy pipelines.


### LangGraph


LangGraph extends LangChain to support cyclic agent workflows. You define your agents as nodes in a graph and connect them with edges that represent message-passing or conditional routing. LangGraph gives you fine-grained control over execution order, branching logic, and checkpointing.


It is Python-first, with a growing TypeScript port. If you need precise control over state machines and are already in the Python toolchain, LangGraph is a strong fit, though expect steeper initial setup than higher-level frameworks.


## Building multi-agent systems with Mastra


When your team works in TypeScript, you need a framework that handles agent definition, inter-agent communication, and workflow orchestration without requiring a separate Python layer.[Mastra](https://mastra.ai/ai-agent-framework) is an open-source TypeScript framework that gives you agents, workflows, memory, and observability in one package.


You define each agent with a system prompt, a model provider (routed through support for 90+ providers), and a set of tools. Workflows let you chain agent steps with` .then()` and` .branch()` , so your orchestrator logic lives in code, not in prompt engineering. A minimal workflow definition looks like this:


```text
import   {   Agent,   Workflow   }   from   "  @mastra/core  "  ;
const   researcher   =   new   Agent  ({
name:   "  researcher  "  ,
model:   "  gpt-4.1  "  ,
instructions:   "  Find and summarize relevant sources for the given topic.  "  ,
tools: [webSearchTool, docRetrieverTool],
});
const   writer   =   new   Agent  ({
name:   "  writer  "  ,
model:   "  gpt-4.1  "  ,
instructions:   "  Draft a structured report from the research summary.  "  ,
});
const   researchWorkflow   =   new   Workflow  ({   name:   "  research-pipeline  "   })
.  then  (researcher)
.  then  (writer);
```


MCP server support means your agents can expose tools and resources to other agents or external systems.


*Studio interface: inspect and manage your agents from a local development view that shows each agent’s tools, status, and workflow connections.*


Because it is TypeScript-only, teams working primarily in Python will need a different framework.


[Build your first agent workflow with Mastra.](https://mastra.ai/docs/workflows/overview)


## Challenges and limitations of multi-agent systems


You should plan for these failure modes before deploying to production.


### Coordination complexity


Getting agents to cooperate reliably is your biggest engineering challenge. Every inter-agent handoff is a potential failure point. Messages can be malformed, agents can misinterpret instructions, and routing logic can send work to the wrong specialist. The coordination overhead grows with the number of agents, so keep your agent count as low as your task decomposition allows.


### Unpredictable and emergent behavior


Your decentralized agents can produce conflicting outputs or enter infinite loops when their reasoning diverges. Emergent behavior is useful when it leads to creative solutions, but dangerous when it causes cascading errors. You need guardrails, timeouts, and evaluation checks at each handoff point to catch unexpected behavior early.


### Agent malfunctions and fault tolerance


Your agents share the same foundation models, which means a model-level bug or degradation affects every agent simultaneously. Build resilience into your architecture: retry logic, fallback models, and circuit breakers that isolate a failing agent before it corrupts the rest of the system.


Providers occasionally degrade in ways that affect latency, output quality, or both, so test your fallback paths regularly.


## Observability, debugging, and evaluation


You cannot debug a distributed agent system by reading logs from individual nodes. You need end-to-end tracing that shows how a request flows through every agent, tool call, and decision point.


### Tracing inter-agent calls and task graphs


Your trace should capture every span in the workflow: which agent received the task, what tools it called, how long each call took, and what tokens flowed in and out. Without this visibility, you are guessing at the root cause when something breaks. Tracking token usage across every call also helps you control costs before they spiral.


*Agent trace hierarchy: each agent call, tool invocation, and model completion appears as a span with timing data, token counts, and parent-child relationships.*


### Evals and guardrails for agent team outputs


Your evaluation strategy needs to cover both individual agent outputs and the assembled final result. Run LLM-as-a-judge scoring on each agent’s output to catch hallucinations and off-topic responses before they propagate.


Add classification evals that verify tool-calling accuracy and response format compliance. Built-in evals, tracing, and observability let you score agent outputs, inspect traces, and catch regressions without stitching together separate tools.


### Human-in-the-loop oversight and intervention


You should route high-stakes outputs like financial calculations, medical summaries, or legal analyses through an approval step before they proceed. Design your workflow graph with explicit approval nodes that block execution until a human confirms the output. This is especially important in agentic AI pipelines where errors can compound across multiple agent handoffs.


## Real-world applications of multi-agent systems


You will find these architectures in production across several domains where task complexity exceeds what a single agent can handle.


### Research automation and document processing


Your research workflow might involve one agent searching academic databases, another extracting key findings from papers, and a third synthesizing those findings into a structured report using RAG to ground its output in source material.[GPT-Researcher](https://github.com/assafelovic/gpt-researcher) uses this pattern with planner and execution agents that divide research questions, retrieve relevant sources via RAG pipelines, and assemble final reports.


### Software engineering and coding agents


Your coding pipeline can use separate AI agents for planning, code generation, code review, and testing. Each agent focuses on one phase of the software development lifecycle. This pattern reduces the chance that a single agent introduces a bug and then fails to catch it during its own review.


### Customer service, operations, and enterprise workflows


Your customer service system can route tickets to specialized agents: one for billing questions, one for technical support, and one for account management. Each agent accesses only the tools and data relevant to its domain.


Enterprise agentic workflows in supply chain management, healthcare coordination, and defense systems use similar patterns to distribute decision-making across specialized autonomous systems.


### Robotics and physical-world coordination


Your coordination patterns have deep roots in robotics, where coordination across autonomous systems predates the large language model era. Warehouse robots from companies like Amazon coordinate pick-and-route tasks across hundreds of units operating on a shared factory floor.


Drone swarms use decentralized architectures to divide surveillance or delivery zones without a central controller. Autonomous vehicle platoons negotiate speed, spacing, and lane changes through peer-to-peer protocols that mirror the decentralized network patterns described above.


These robotics applications share the same coordination challenges as LLM-based MAS: timing sensitivity, resilience under real-time constraints, and the need for robust inter-agent messaging.


## Wrapping up


Multi-agent systems give you specialization and fault isolation that single-agent systems cannot match, but they demand careful architectural planning and end-to-end tracing.


Start with the simplest agent topology that solves your problem, add agents only when a clear boundary justifies the coordination cost, and instrument every inter-agent handoff from day one. If your team builds in TypeScript,[Mastra](https://mastra.ai/ai-agent-framework) gives you agents, workflows, and end-to-end tracing in one framework.
