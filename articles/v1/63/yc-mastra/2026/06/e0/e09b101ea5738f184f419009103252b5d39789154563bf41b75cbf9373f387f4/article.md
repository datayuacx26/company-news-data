---
schema_version: "1.0.0"
document_id: "e09b101ea5738f184f419009103252b5d39789154563bf41b75cbf9373f387f4"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/ai-agent-workflows"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:57:46.286490+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:b336556a3a4b1f8cd652ea1bbb62e4cc7515e816acd190ca395ff95e87cbf6e4"
---

# AI agent workflows: a complete guide for developers

**Meta title: AI Agent Workflows: A Complete Guide for Developers**


**Meta description** : Learn how AI agent workflows work, common patterns like routing and parallelization, and how to build, evaluate, and monitor agentic workflows in production.


**Author:** Aron Schuhmann


**Reviewer:** Sam Bhagwat


# AI agent workflows: a complete guide for developers


Your AI application probably started as a single model call. A prompt goes in, a response comes out, and you parse the result. That works for simple transformations, but the moment you need multi-step reasoning, tool calls, branching logic, or human approvals, you need something more structured. You need an AI agent workflow.


This guide covers how[agentic workflows](https://mastra.ai/articles/agentic-workflows) function under the hood, the patterns you'll use most often, how to decide between deterministic workflows and autonomous agents, and how to build, evaluate, and secure the whole system in production.


## What is an AI agent workflow?


An AI agent workflow is a sequence of tasks carried out by one or more AI agents that use LLMs, memory, tools, and decision logic to reach a defined outcome. Unlike a single prompt-and-response interaction, the workflow coordinates multiple steps where each step can involve model calls, tool execution, conditional branching, or human review.


The key distinction is adaptiveness. Your workflow defines the goal and constraints. The agent reasons about which step to take next based on intermediate results, rather than following a fully hardcoded script.


## How agentic workflows differ from traditional automation


Traditional automation systems like RPA follow rigid if-then rules. You predefine every branch, every condition, and every output format. That works for predictable, repetitive tasks with stable inputs. Most process automation tools operate this way, they excel at known procedures but break when inputs deviate from expectations.


Agentic workflows replace static decision trees with LLM-driven reasoning. The agent interprets context, selects tools, evaluates intermediate results, and adjusts its plan. When an API returns unexpected data or a classification falls into an ambiguous category, the agent adapts rather than failing silently or escalating immediately.


The tradeoff is predictability. Traditional automation gives you deterministic outputs for deterministic inputs. Agentic workflows give you flexibility at the cost of non-determinism, which means you need observability and evals to maintain quality.


## Workflows vs. AI agents vs. AI workflows


These three terms describe different levels of autonomy, and confusing them leads to architectural mistakes.


Workflow AI agent AI agent workflow


**What it is** A structured sequence of steps with predefined rules and explicit conditions A goal-driven system that uses an LLM to decide its own actions, select tools, and iterate until the task is complete A workflow where AI agents handle one or more steps, choosing their own actions within defined boundaries


**Autonomy level** None, every branch and condition is hardcoded High, the agent reasons about what to do next at each step Mixed, deterministic scaffolding with agent reasoning at specific steps


**Typical example** CI/CD pipeline, Zapier automation Research assistant that plans its own search and synthesis steps Customer support system that routes deterministically, then uses an agent to draft responses


**Best for** Predictable, repeatable tasks with stable inputs Open-ended tasks where the number of steps is hard to predict Most production systems, where you want structure around the parts you can predict and flexibility where you can't


In practice, you'll use all three. The question is always where the agent adds value and where deterministic code is safer and cheaper.


## How AI agent workflows work


Your AI agent workflow operates as a loop. The agent perceives its environment, reasons about what to do, acts using tools or model calls, and evaluates the result before deciding on the next step. Understanding each phase helps you design workflows that are both capable and debuggable.


### Perception and reasoning loop


At every step, the agent assembles its context window from the system prompt, conversation history, tool results, and any retrieved knowledge. It then reasons about the current state: what has been accomplished, what remains, and what tools are available.


This is where context engineering matters. Too little context and the agent lacks information to make good decisions. Too much, and you risk what practitioners call context distraction or context rot, where model performance degrades because the signal is buried in noise.[Chroma's research](https://research.trychroma.com/context-rot) on this phenomenon tested 18 frontier models and found that every one degraded as input length grew, well before hitting its advertised context limit.


### Planning, tool use, and action execution


Once the agent decides on a next step, it calls a tool, generates structured output, or produces a response. Tools are functions the agent can invoke: fetching data from an API, querying a database, running a calculation, or writing to a file.


Effective tool design is the most impactful part of building an AI agent workflow. Think like an analyst: break your problem into clear, reusable operations and write each as a tool. Give each tool a clear name, a detailed description, and a specific input/output schema.


### Feedback, memory, and iteration


After executing an action, the agent evaluates the result. Did the tool return valid data? Does the output match expectations? If something failed, the agent feeds the error back into its context and tries again.


Memory gives the agent continuity across steps and sessions. Working memory stores long-term user characteristics. Semantic recall uses embeddings and vector search to retrieve relevant past data. Observational memory compresses sessions into structured observations, keeping context manageable as conversations grow. These feedback loops are what separate a well-designed AI agent workflow from a brittle one-shot pipeline.


## Key components of an AI agent workflow


Every AI agent workflow shares a common set of building blocks. Your choices here determine the workflow's capability, cost, and reliability.


### LLMs and specialized models


The LLM provides the reasoning layer. You'll typically start with a frontier model from OpenAI, Anthropic, or Google to maximize quality, then optimize for cost and latency once the workflow is stable. Larger models produce higher-quality reasoning but cost more and run slower. The principle is simple: make it work, make it right, make it fast and cheap, in that order.


Reasoning models allocate internal compute before responding. Most state-of-the-art models now support this, with controls to dial reasoning effort up or down depending on task complexity. For simpler extraction tasks, you can often route to a smaller, faster model without losing accuracy.


### Tools, APIs, and external integrations


Tools connect your AI agents to the real world. Without them, the agent can only generate text. With them, it can query databases, update CRM records, send emails, search the web, or execute code.


Best practices for tool design:


-


Use semantic naming that matches the tool's function (searchCustomerOrders, not doStuff).


-


Provide descriptions that explain both what the tool does and when to call it.


-


Define specific input/output schemas with validation.


-


Keep each tool focused on a single operation.


### Memory systems


Your agent's memory architecture determines how well it maintains context across steps and sessions. Three approaches cover most use cases:


-


**Working memory:** persistent facts about the user or environment, updated over time.


-


**Semantic recall:** vector-based retrieval (RAG) to find relevant past content.


-


**Observational memory:** compressed session summaries that keep context lean as token counts grow.


As context accumulates, you'll need compression strategies. Remove oldest messages, filter verbose tool-call results, or run a summarization step when context hits a threshold. Building custom memory processors lets you control exactly what the agent remembers and what it discards.


### Multi-agent orchestration layer


Complex tasks often require multiple specialized AI agents working together. Think of it as organizational design: you group related tasks into roles, assign each role to an agent, and define how they communicate.


A supervisor agent can break work into subtasks, delegate to specialized workers, and verify the combined result. The most straightforward implementation wraps each subagent as a tool that the supervisor can call. Designs are fractal: a hierarchy is just a supervisor of supervisors.


One important caveat about parallelization: subagents working in isolation can produce conflicting outputs. When tasks have dependencies or shared context, a single-threaded linear agent often produces better results than parallel execution.


## Common agentic workflow patterns


Your choice of pattern determines how your AI agent workflow handles complexity, concurrency, and control. Most production systems use a combination of these.


### Prompt chaining


Prompt chaining breaks a complex task into sequential LLM calls where each step's output feeds into the next. You decompose the problem so the model does one thing at a time, keeping each step's input and output visible in tracing.


This is the simplest agentic pattern and the one you should reach for first. It works well for content generation pipelines, multi-stage document processing, and any task where the order of operations is known in advance.


### Routing


Routing classifies an incoming request and directs it to a specialized handler. A customer support system might route billing questions to one agent, technical issues to another, and general inquiries to a third. The same algorithms that power intent classification in natural language processing apply here: the router assigns each input to the handler most likely to resolve it correctly.


This pattern lets you optimize each handler independently. You can use a smaller, cheaper model for simple queries and reserve frontier models for complex reasoning. It also keeps each agent's tool set focused, which reduces the chance of tool-selection errors.


### Parallelization


Parallelization runs multiple independent operations simultaneously. If you need to check a medical record for twelve symptoms, twelve parallel LLM calls, each checking for one symptom, may produce more accurate results than a single call trying to evaluate all of them at once.


The key constraint is independence. Parallel branches should not depend on each other's results. When they do, you risk conflicting outputs that are difficult to reconcile downstream.


### Supervisor agent with worker agents


A supervisor receives a goal, decomposes it into subtasks, delegates each subtask to a specialized worker agent, and validates the combined output. This mirrors how engineering teams work: a tech lead breaks a project into tasks and assigns them to developers with relevant expertise.


Each worker agent gets a focused tool set and instructions tuned for its specific role. The supervisor handles coordination, error handling, and final assembly. Careful state management at the supervisor level is what keeps multi-step workflows from producing inconsistent outputs when a worker step fails partway through.


### Human-in-the-loop orchestration


Some tasks require human judgment at critical points. An AI agent workflow can pause mid-execution, present a draft or decision point to a human reviewer, incorporate feedback, and resume.


Three common patterns for human involvement:


-


**In-the-loop:** the agent pauses for human input before proceeding to the next step.


-


**Draft review:** the agent completes its work, then presents the output for human approval before delivery.


-


**Deferred execution:** the agent collects feedback asynchronously without blocking its own progress.


Deferred execution aligns best with real-world team dynamics because humans don't want to babysit agents. But in any human-in-the-loop architecture, humans become the bottleneck. Design around that constraint.


### Autonomous closed-loop workflow


In a fully autonomous workflow, the agent runs the entire process from start to finish without human intervention. It monitors inputs, takes action, evaluates results, and triggers the next cycle.


This pattern delivers the highest throughput but demands robust guardrails, evals, and monitoring. Reserve it for tasks where the cost of errors is low or where downstream validation catches mistakes before they reach users.


## Choose between a workflow and an agent


You'll encounter this question on every project: should this step be a deterministic workflow or an autonomous agent? The answer depends on how predictable the task is and how much flexibility you need.


### When deterministic workflows are the right call


If your task has clear requirements, stable inputs, and a known sequence of operations, use a deterministic workflow. You get predictability, easier debugging, and lower cost. Workflows are ideal for data transformations, validation pipelines, and any process where you can enumerate every possible path.


### When autonomous agents are warranted


Use agents when the task is open-ended, the number of steps is hard to predict, or the system needs to adapt to novel inputs. Research tasks, complex customer support, and multi-document analysis all benefit from agent-driven reasoning.


The tradeoff is cost and unpredictability. Agents burn tokens in loops. They can take unexpected paths. You need evals and observability to keep them reliable.


### Hybrid approaches


Most production systems combine both. A deterministic workflow handles routing, validation, and data transformation. Agents handle the reasoning-heavy steps where flexibility matters. The workflow provides structure. The agent provides intelligence.


This hybrid model lets you keep costs low on predictable steps while using agent reasoning only where it adds clear value.


## How to build an AI agent workflow


Building an AI agent workflow follows a process similar to designing any complex system. You define the outcome, understand the current process, identify where agents add value, choose your tools, build, and iterate.


1.


**Define the outcome and success criteria.** Start with what the workflow should accomplish and how you'll measure success. Clear goals constrain the agent's behavior and guide every design decision downstream.


2.


**Map the tasks in the existing human workflow.** Write out every step a human currently takes to complete the task. Identify which steps are repetitive, which require judgment, and where the process slows down. This map becomes your blueprint.


3.


**Decide where agents add the most value.** Not every step needs an agent. Use agents for steps that involve reasoning, data synthesis, or multi-step coordination. Keep deterministic code for validation, data transformation, and routing.


4.


**Choose a framework or platform.** You need a framework that supports model routing, tool integration, workflow orchestration, and observability. Evaluate based on your deployment targets, language preferences, and the specific patterns you plan to use.


5.


**Build and wire the workflow.** Implement each step, connect tools, define branching logic, and configure memory. Compose steps so input and output at each step are meaningful and visible in tracing. Limit each step to one model call.


6.


**Test, monitor, and optimize.** Review accuracy, latency, and edge cases. Adjust prompts, tools, and memory settings. Run evals against a test dataset before deploying, then add online evals against live traffic once in production.


## Building AI agent workflows on Mastra


[Mastra](https://mastra.ai/) gives you agents, workflows, memory, evals, and observability in one framework, with a built-in model router (AI SDK providers are supported too, if you want them). The open-source framework provides graph-based workflows and model routing across hundreds of models across dozens of providers through a single interface.


Chain steps sequentially with .then(), run steps concurrently with .parallel(), and route between paths with .branch(), where each branch pairs a condition function with the step to run. Call .commit() to finalize the workflow. Suspend and resume support lets workflows pause for human approvals or external events, with state persisted to durable storage. Built-in tracing surfaces every step's inputs, outputs, and latency in Mastra Studio during development.


Mastra's memory system supports working memory for persistent user facts, semantic recall backed by vector search for retrieving relevant past interactions, and observational memory that compresses session history to keep context lean. You configure memory per agent, choosing which processors (like TokenLimiter or ToolCallFilter) to apply as context grows.


For evaluation, Mastra's built-in eval framework lets you run LLM-as-judge scoring, tool-calling verification, and task completion checks against your workflows before deploying. Traces flow into Mastra Studio during local development, giving you a visual map of every step, its inputs, outputs, and latency.


The framework deploys to Vercel, Netlify, Cloudflare, and standalone Node servers, and ships native MCP support so your workflows can expose tools as MCP servers or consume third-party MCP tools without custom integration code.


[Build your first TypeScript agent workflow on Mastra.](https://mastra.ai/)


## Real-world use cases and examples


Understanding where AI agent workflows create value helps you prioritize what to build first. The following categories represent the most common production deployments.


### Marketing and customer experience


Your marketing team can use agentic workflows to generate personalized outreach, segment audiences based on behavioral data, and orchestrate multi-channel campaigns. An agent handles the reasoning-heavy work of analyzing customer signals and selecting message themes, while the workflow manages sequencing and delivery. Generative AI makes this practical at scale: the agent produces personalized variants, while the workflow enforces send schedules and compliance rules.


### E-commerce and retail


Agentic workflows in retail monitor inventory levels, adjust pricing based on demand signals, and generate personalized product recommendations. The agent interprets real-time data analytics and makes decisions. The workflow ensures those decisions execute reliably across systems.[Walmart's](https://tech.walmart.com/content/walmart-global-tech/en_us/blog/post/inside-the-ai-network-orchestrating-walmarts-fastest-holiday-deliveries-yet.html) fulfillment network is a real-world example of this pattern at scale: specialized agents continuously make micro-decisions about routing, driver assignment, and order timing, recalibrating as conditions like weather and demand shift.


### Financial services


Financial institutions use AI agent workflows for fraud detection, compliance document review, and risk assessment. An agent can scan transactions against historical patterns, flag anomalies, and generate explanations for human reviewers. The workflow enforces audit trails and approval gates required by regulators. AI governance policies determine what actions the agent can take autonomously versus what requires escalation.[Klarna's](https://www.klarna.com/international/press/klarna-ai-assistant-handles-two-thirds-of-customer-service-chats-in-its-first-month/) support assistant illustrates the escalation pattern in practice: it resolves the bulk of routine inquiries on its own and routes ambiguous or regulated cases to a human reviewer.


### Healthcare and science


In healthcare, AI agents help process diagnostic data, surface relevant research, and assist clinicians with documentation. Machine learning models provide the underlying pattern recognition, trained on large clinical datasets, while the workflow coordinates handoffs between the model, the codebase, and human reviewers. Careful access controls and audit trails are essential: the agent should never be the final decision-maker for clinical outcomes.


### Internal operations and knowledge work


Knowledge workers spend significant time on research, summarization, and report generation. Agentic workflows automate the data gathering and synthesis steps while keeping humans in the loop for judgment calls. An agent researches a topic across multiple sources, compiles findings, and presents a draft for review. Pharma company[ProQR's case study with Causaly](https://www.causaly.com/blog/proqr-target-identification-case-study) shows this pattern in research: an agent reviewed biomedical literature and surfaced connections between studies, helping the team reach its target identification goal a quarter early at five times the productivity of manual PubMed review.


## Evaluate and measure workflow performance


You can't improve what you don't measure, and[AI workflows](https://mastra.ai/articles/ai-workflows) are harder to measure than traditional software because outputs are non-deterministic. Build your evaluation strategy around these dimensions.


### Accuracy and reliability


Your primary concern is whether the agent produces correct, trustworthy outputs. LLM-as-judge evals let you use a second model to score outputs against a rubric. For structured tasks, compare agent outputs to known-good answers. Build eval datasets that mix hand-curated examples, synthetic data, and production logs.


### Task completion rate and autonomy score


Track how often the workflow reaches its intended outcome without human intervention. A high task completion rate indicates stable performance. The ratio of autonomous completions to human escalations shows you how much the agent can handle independently.


### Latency, throughput, and cost per run


Measure how quickly the workflow runs, how many concurrent runs it supports, and what each run costs in tokens and compute. Agents burn tokens in loops, so a workflow that works at low volume might become prohibitively expensive at scale. Monitor cost per run from day one.


### ROI impact across teams


Connect workflow metrics to business outcomes. Time saved, error reduction, faster response times, and revenue contribution all matter more than technical metrics alone. Cross-reference your engineering evals with domain-specific success metrics to ensure the workflow is solving the right problem.


## Observe, monitor, and debug agent workflows


Your AI agent workflow can return a 200 OK and still be wrong. Non-deterministic models mean you need observability at every layer.


### Tracing and logging agent runs


A trace is a tree of spans that captures every step in your workflow. The standard format is OpenTelemetry (OTel). Good tracing shows you how long each step took, the exact JSON flowing into and out of each model call, and metadata like status and latency.


Tracing is your primary debugging tool. When an agent produces a bad output, you need to see which step went wrong, what context the model had, and what tool results influenced its decision. Without traces, debugging agentic AI is essentially guesswork.


### Evals and guardrails for output quality


Evals provide quantifiable metrics for non-deterministic outputs. Key eval types include:


-


**LLM-as-judge:** a second model scores the output against a rubric.


-


**Tool calling evals:** verify the agent called the right tools in the right order.


-


**Multi-turn evals:** run the agent through full conversations and grade context retention.


-


**Task completion evals:** the most important: did it finish the job?


Run offline evals against a fixed dataset before deploys to catch regressions. Add online evals against production traffic to catch real-world issues that synthetic data misses.


### Explainability and human escalation


When your workflow flags an issue or escalates to a human, the reviewer needs to understand why. Full traces with collapsible detail views let domain experts see the agent's reasoning without wading through raw JSON. Mastra's tracing in Mastra Studio shows each step's inputs, outputs, and decision path at localhost:4111 during development, giving you this visibility from the start.


## Security and governance considerations


Agents are more powerful than traditional API consumers, which means your security model needs to be more rigorous. An agent that browses the web, reads uploaded documents, and takes actions on behalf of users creates a large attack surface.


### Data privacy and access control


Define two permission layers: what resources the agent can access, and which users can invoke the agent. Security through obscurity fails when a user can ask the agent to retrieve information hidden across your systems. Enforce granular access controls at the tool level, not just at the agent level.


### Guardrails and responsible agent actions


Input guardrails sanitize incoming messages before the LLM processes them. They protect against prompt injection, jailbreaking, PII exposure, and off-topic queries. Output guardrails screen generated content for data leakage, hallucination, and toxicity before it reaches users.


Be aware of what security researcher Simon Willison calls the "lethal trifecta": an agent that combines access to private data, exposure to untrusted content, and the ability to communicate externally. Removing any one leg of this triangle neutralizes the exploit. The easiest to remove is usually the exfiltration vector, constraining the agent so untrusted input cannot trigger outbound actions with negative side effects.


### Audit trails and AI governance


Log every action the agent takes, every tool it calls, and every decision it makes. These logs serve as your audit trail for compliance reviews and incident investigations. In regulated industries, you'll need to demonstrate that the agent operated within defined boundaries for every transaction it processed. AI governance frameworks typically require this kind of action-level accountability, separate from the model-level policies your LLM provider enforces.


## Challenges and limitations of AI agent workflows


Building with agents involves real tradeoffs. Understanding these limitations upfront helps you design systems that handle them gracefully.


### Integration complexity across systems


Connecting agents to multiple enterprise systems with different APIs, auth flows, and data formats creates significant integration overhead. Each integration is a potential failure point. Standardize on protocols like MCP where possible, and treat third-party integrations as first-class concerns in your architecture.


### Data quality issues affecting agent decisions


Your agent is only as good as the data it receives. Incomplete, outdated, or inconsistent data produces unreliable outputs regardless of how sophisticated your model or workflow is. Add validation steps at data boundaries and monitor input quality alongside output quality.


### Risks of over-automation and the "mega-agent" trap


A common mistake is building one massive agent that handles every possible task. As you add tools and responsibilities, the agent's error rate climbs. The likelihood of choosing the wrong tool increases with the number of available tools.


The better approach, as described in our[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) book, is to start with one focused agent that solves one problem well. When users ask for more, build a new agent or split the existing one. If you have multiple agents, add routing logic. You discover the right architecture by iterating, not by designing a mega-agent upfront.


### Managing multi-agent orchestration at scale


Multiple agents must coordinate task order, hand off work correctly, and avoid conflicting actions. Without clear role definitions and monitoring, the system can produce inconsistent results or deadlock. Share context deliberately between subagents so downstream agents understand not just what was done, but why.


## Model Context Protocol and its role in agentic workflows


As your AI agent workflow integrates with more external services, the cost of maintaining custom integrations grows quickly. Model Context Protocol (MCP) addresses this by standardizing how agents discover and call tools.


### How MCP standardizes tool and resource access


MCP is an open protocol for connecting AI agents to tools, models, and each other. It works like a universal adapter: if your tool speaks MCP, it can plug into any MCP-compatible agent. Servers wrap sets of tools and communicate with clients over stdio (locally) or Streamable HTTP (for remote servers). Clients query servers to discover available tools, then request execution and receive results.


MCP hit critical mass in early 2025 and became the functional default after OpenAI and Google announced support. Vendors like Stripe ship MCP servers for their API functionality. Independent developers build MCP servers for browser automation, error tracking, and more. LangChain and LangGraph also support MCP-compatible tools, which means switching from LangChain to an MCP-native framework like Mastra doesn't require rebuilding your tool integrations from scratch.


### Exposing workflows as MCP servers


If you're building tools for other agents, ship an MCP server. This lets any MCP-compatible agent discover and use your tools without custom integration code. Mastra ships MCP server and client abstractions so you don't have to reimplement the protocol spec yourself. You can expose your agents, tools, and workflows as MCP resources that other systems consume directly.


Treat third-party MCP servers with the same trust level as any third-party API. Validate inputs, enforce access controls, and monitor usage.


## AI-adjacent capabilities: a quick reference


AI agent workflows are the most prominent application of LLMs in production today, but they sit within a broader landscape of artificial intelligence techniques. You'll encounter these terms in architecture discussions, vendor documentation, and conversations with ML engineers.


Term What it is Relevance to agentic workflows


**Deep learning** The neural network architecture underlying modern LLMs Helps you distinguish a trained model artifact from the agent system built on top of it


**Machine learning** The broader category covering deep learning, classical methods, and reinforcement learning Relevant when your routing or classification logic uses a trained ML model rather than an LLM


**Natural language processing (NLP)** Techniques for intent classification, entity extraction, and semantic similarity NLP tasks surface naturally as agent tools, especially in routing and input parsing steps


**Generative AI** AI systems that produce text, images, or code as output The category your LLM falls into; useful shorthand when talking to non-technical stakeholders


**Computer vision** Models that interpret images and video Can be called as a tool within an agentic workflow, for example to describe an image before the agent reasons about it


**Facial recognition** A computer vision subfield for identifying individuals from images Occasionally relevant in identity verification workflows; subject to strict regulatory constraints


**Autonomous vehicles** Systems that combine perception, planning, and action in physical environments A useful reference point for understanding agentic decision loops at high reliability requirements


**Artificial general intelligence (AGI)** A theoretical system that generalizes reliably across arbitrary domains without task-specific prompting Not achievable with current LLMs; design your workflow around what models can do today, not future AGI capabilities


Designing AI agent workflows well comes down to matching autonomy to task predictability, keeping tool surfaces focused, and building the observability infrastructure to catch regressions before users do. Start with the simplest pattern that meets your requirements, instrument everything, and expand agent autonomy only when your evals give you confidence. If you're working in TypeScript and want a framework that covers the full stack,[Mastra](https://mastra.ai/) provides agents, workflows, evals, and tracing in a single open-source package.
