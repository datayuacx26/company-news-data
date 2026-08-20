---
schema_version: "1.0.0"
document_id: "ecd1f8cf152151a04527afe32ed4370c56ea1b532ce897592c48e5d06d93c097"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/langgraph"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-10T16:04:57.697539+00:00"
fetched_at: "2026-08-10T16:04:59.642666+00:00"
content_hash: "sha256:10f3a558115e7c234da32ee720a8ec766fe3413ede838cec67116d995ec5671a"
---

# LangGraph: the complete guide to stateful AI agent orchestration

Your AI agent needs to call a model, check the result, decide whether to retry or escalate, invoke two tools in parallel, and then wait for human approval. A linear chain can’t express that, which is why teams keep running into it as adoption grows. According to the[2025 State of Agent Engineering survey](https://www.langchain.com/state-of-agent-engineering) , 57.3% of organizations building agents with large language models now have those agents running in production, up from 51% the year before.


LangGraph is a low-level orchestration framework built by the LangChain team specifically for stateful, multi-step AI agents. The LangGraph framework gives you explicit control over how state flows through cyclical graphs, how nodes branch on conditions, and how execution persists across failures and restarts.


This guide covers what LangGraph is, how its core components work, when to choose it over alternative approaches, and how it compares to other methods for building agents in production.


## What is LangGraph?


LangGraph is an open-source framework that gives you graph-based orchestration for stateful AI agents. It extends the LangChain toolchain by replacing linear chains with a directed graph model that supports cycles, conditional branching, and persistent state.


You define your logic as a set of nodes (functions that do work) and edges (transitions that decide what runs next), then compile the agent graph into a runnable LLM application.


The framework compiles that graph into a state machine that an LLM application can run and resume across steps, rather than a script that executes once and exits. The sections below cover the primitives that make up a graph and how LangGraph tracks state as execution moves between nodes.


## Core concepts: graphs, nodes, and edges


Your application is built from three primitives. Nodes are Python or JavaScript functions that receive the current state, perform an operation (call an LLM, invoke a tool, transform data), and return an updated state.


Edges connect nodes and define the transition logic. Fixed edges always route from node A to node B. Conditional edges evaluate a function against the current state and route to different nodes based on the result.


This structure allows cyclical graphs, which is the key difference from directed acyclic graphs (DAGs). A DAG flows in one direction. A graph built with LangGraph can loop back, letting your LLM agent retry a failed tool call, re-evaluate its plan, or refine its output iteratively.


Unlike NetworkX, which handles static graph computation, these graphs execute stateful logic step-by-step through an agent runtime.


## State management and how context is tracked


You define your state as a typed schema, typically using Python’s` TypedDict` . Every node reads from and writes to this shared state object. As the graph executes, the state accumulates the full history of what has happened: messages exchanged, tool_calls made, intermediate results, and decisions taken.


This centralized approach means you always have a single source of truth for your agent’s current context. It also makes debugging straightforward, because you can inspect the state at any point in execution to understand exactly what the agent knew and decided.


This matters most across multi-turn interactions or long-running workflows, where you need the graph to pick up exactly where it left off rather than reconstruct context from scratch. Checkpointers persist that state between steps, which is what makes suspend-and-resume patterns like human approval or waiting on an external system possible.


## Why a chain-based approach is not enough for complex agents


Your first prototype probably worked fine. A prompt goes in, a model responds, maybe a retriever fetches some context first. But the moment you need your agent to loop, branch, or coordinate multiple sub-tasks, the linear chain model starts fighting you.


### Limitations of linear chain-based workflows


You hit these limits quickly when your agent needs retries or parallel branches. A chain architecture processes steps sequentially, where each step receives the output of the previous one and passes its own output forward.


This works well for retrieval-augmented generation pipelines, chatbot prompt chaining, and simple LLM applications. But it cannot natively express patterns like retrying a step based on output quality, branching into parallel sub-tasks, or routing to different logic paths based on intermediate results.


When developers try to force these patterns into chains, the result is deeply nested callbacks and custom routing logic that becomes hard to read, test, and maintain. The control flow is implicit rather than explicit.


### Where graph-based orchestration fills the gap


Graph-based orchestration makes your control flow visible and explicit. You declare each step as a node and each transition as an edge, including conditions that branch on runtime state. Cycles let your agent revisit earlier steps when needed. The graph itself becomes a readable map of your agent’s decision-making process.


This is why LangGraph exists as a separate framework rather than an extension of the chain model. The underlying execution model is fundamentally different: graphs with cycles versus linear sequences.


## Key LangGraph features and components


Your application is assembled from a small set of composable primitives. Understanding each one gives you full control over how your agent reasons, acts, and recovers. The building blocks you will use most often are:


-


StateGraph with a typed state schema


-


Nodes that perform work and return state updates


-


Conditional edges that route based on runtime results


-


Checkpointers that persist state between runs


-


Interrupt points for human-in-the-loop review


*Routing workflow illustrating how an orchestrator agent directs tasks through conditional edges to specialized nodes.*


### StateGraph and typed state schemas


You start by defining a` StateGraph` and a typed state schema. The schema is a` TypedDict` that declares every field your agent needs to track: the conversation messages, any tool results, flags for human approval, intermediate computations.


The StateGraph then uses this schema to validate transitions and ensure that every node reads and writes the correct fields.


```text
from   langgraph  .  graph   import   StateGraph
from   typing   import   TypedDict  ,   Annotated
class   AgentState  (  TypedDict  ):
messages  :   Annotated  [  list  ,   "  append  "  ]
tool_results  :   list
needs_review  :   bool
graph   =   StateGraph  (  AgentState  )
```


By declaring your state as a` TypedDict` , you get both runtime validation and IDE autocompletion across every node in the workflow.


### Nodes and conditional edges


You add nodes to the graph as named functions. Each node receives the current` AgentState` and returns a partial state update. You then connect nodes with edges. A fixed edge always transitions from one node to the next. A conditional edge evaluates a routing function and sends execution to different nodes based on the result.


```text
def   should_continue  (state:   AgentState  )   ->   str  :
last_message   =   state  [  "  messages  "  ][  -  1  ]
if   last_message  .  tool_calls  :
return   "  tools  "
return   "  end  "
graph  .  add_conditional_edges  (  "  agent  "  ,   should_continue  ,   {
"  tools  "  :   "  tool_node  "  ,
"  end  "  :   END
})
```


This pattern is how you implement loops, retries, and branching without custom callback logic.


### Checkpointers and persistence layers


Your agent can persist its state between executions using checkpointers. A checkpointer serializes the graph’s state after each node execution and stores it in a backend (in-memory, SQLite, PostgreSQL, or a custom store).


If your agent crashes, you resume from the last checkpoint rather than starting over. This makes the framework suitable for durable execution, where long-running LLM agents need fault tolerance.


### Human-in-the-loop support and interrupt or resume


You can insert interrupt points in your graph where execution pauses and waits for human input. This is essential for workflows that require approval, review, or manual data entry before proceeding.


The state is persisted at the interrupt point, and execution resumes exactly where it left off once the human responds. This human-in-the-loop pattern is a first-class feature, not a workaround.


## LangGraph compared to chain-based workflows


Your choice between a chain-based approach and LangGraph depends on how complex your agent’s control flow needs to be. They share components (model interfaces, tool definitions, retrievers), but they differ in architecture and execution model.


**Dimension** **Chain-based approach** **LangGraph**


Execution model Sequential chains Cyclical graphs with state


Control flow Implicit (callbacks, nested chains) Explicit (nodes, edges, conditions)


State handling Optional memory modules Built-in typed state schemas


Loops and retries Manual implementation Native via graph cycles


Human review Limited First-class interrupt/resume


Persistence External add-ons Built-in checkpointers


Best fit RAG pipelines, chatbot chains Multi-step agents, complex workflows


### Architecture comparison


You can see the difference most clearly in how each approach handles control flow. Chain-based pipelines are composed with LCEL (LangChain Expression Language), which chains components using pipe operators. The output of one step feeds directly into the next. LangChain’s chain model processes those steps sequentially, so retries and branching require custom workarounds.


LangGraph replaces this with a` StateGraph` where nodes read from and write to a shared state object, and conditions control transitions explicitly.


### When to use a chain-based approach


You should stick with chains when your workflow is genuinely sequential: retrieve documents, prompt a model, parse the output, return the result. If every request is independent and you don’t need to loop or branch, the simpler model is faster to build and easier to maintain. Straightforward chatbot pipelines and single-turn retrieval tasks are its strength.


### When to choose LangGraph


You should reach for LangGraph when your agent needs cycles (retry loops, reflection steps), conditional branching (route to different tools based on intent), persistent state across turns, or human review checkpoints. If you need multiagent workflows where sub-agents coordinate through shared state, subgraph composition makes that possible within a single multi-agent system.


## Building stateful agents with Mastra as an alternative


Your agent orchestration needs might not require a Python-centric approach. If you build in TypeScript,[Mastra](https://mastra.ai/ai-agent-framework) offers a different path to the same destination: stateful agents with explicit workflow control, observability, and production-grade persistence.


It is an open-source TypeScript framework (Apache 2.0) that provides agents, workflows, memory, and observability in a single package. Its workflow engine supports step-based graphs with` .then()` for sequential steps and` .branch()` for conditional routing, giving you the same graph-based control flow but in TypeScript. Model routing supports over 90 providers through one interface, built on Vercel’s AI SDK.


*The Studio’s visual interface for inspecting agent configurations, workflow graphs, and run traces during development.*


Every run produces a trace: a tree of spans showing model calls, tool invocations, latency, and token usage. You inspect these traces in the Studio during development and export them to OpenTelemetry-compatible backends in production.


[Build your first stateful TypeScript agent on Mastra.](https://mastra.ai/ai-agent-framework)


## LangGraph use cases


Your implementation will depend on the specific problem you’re solving. The framework is flexible enough to handle a wide range of agent architectures across different domains. The LangGraph documentation covers additional examples and reference patterns for each category below. A typical build path looks like this:


1.


Define the typed state your agent must track


2.


Add nodes for reasoning, tools, and human review


3.


Wire conditional edges for retries and escalation


4.


Attach a checkpointer and run evaluation suites before shipping


### Multi-step research and retrieval agents


You can build research agents that plan a query strategy, execute multiple searches using tools like Tavily, evaluate the quality of retrieved documents, and loop back to refine queries when initial results are insufficient.


For example, a research agent might call Tavily’s search API, parse the results into its state, and use routing logic to send execution back to the search node when relevance scores fall below a threshold.


This plan-search-evaluate cycle is a natural fit for cyclical graphs, where the “evaluate” node can route back to the “search” node based on quality criteria. Tavily is one of the most commonly integrated search tools in the LangGraph community because it returns structured results that map cleanly to tool_calls responses.


### Customer support and escalation workflows


You can model a support chatbot that classifies incoming tickets, attempts an automated resolution, and escalates to a human agent when confidence is low. Routing conditions handle the logic, and interrupt points let a support rep review and approve the proposed response before it goes to the customer.


### Coding assistants and iterative feedback loops


You can build coding agents that generate code, run tests, analyze failures, and iterate on fixes. Each cycle through the graph refines the output. The state tracks the current code, test results, and error history, so each iteration has full context about what has already been tried.


### Data analysis pipelines with branching logic


You can create data analysis agents that branch based on the type of data received. Numerical data routes to statistical analysis nodes, text data routes to NLP nodes, and mixed data routes to a coordination node that fans out to both. Results merge back into a single state for final synthesis.


## How LangGraph scales


Your agents will eventually need to handle concurrent work, survive failures, and coordinate across multiple sub-agents. The framework provides primitives for each of these requirements.


*Development loop illustrating the iterative cycle that stateful agents follow when scaling: plan, execute, evaluate, and refine.*


### Parallelism and concurrent node execution


You can define fan-out patterns where multiple nodes execute in parallel. The graph waits for all parallel branches to complete before merging their results back into the shared state. This is useful for agents that need to query multiple data sources simultaneously or run several tool_calls at once.


### Durable execution and fault-tolerant state


Checkpointers give your agents durable execution. If a node fails or the process crashes, the agent resumes from its last checkpoint rather than restarting the entire workflow. Combined with retry logic on individual nodes, this makes your agents resilient enough for production workloads where uptime matters.


### Multi-agent subgraph composition


You can compose multiple graphs into a parent graph using subgraph nodes. Each subgraph operates with its own state and logic, and the parent graph coordinates between them. This pattern supports multi-agent systems where specialized agents handle different domains and a supervisor agent routes tasks between them.


## Adding memory and persistence to your agents


Your agents need memory to maintain context across turns and sessions. LangGraph provides two approaches that map to short-term and long-term memory strategies.


### Short-term vs long-term memory strategies


Your short-term memory lives in the graph’s state and persists within a single session or thread. It tracks the current conversation, recent tool results, and in-progress decisions.


Long-term memory uses external stores (databases, vector stores) to persist information across sessions. Your agent might store user preferences, past interaction summaries, or learned facts in a long-term store and retrieve them at the start of each new session.


**Strategy** **Scope** **Storage** **Typical use**


Short-term memory Single session or thread In-graph state Current conversation, recent tool_calls, in-progress decisions


Long-term memory Cross-session External database or vector store User preferences, interaction summaries, learned facts


### Connecting external stores and databases


You can connect your agent to external databases by defining nodes that read from and write to those stores. The state includes references to external data, and dedicated nodes handle the retrieval and persistence logic. This keeps your graph clean, because memory operations are just nodes like any other.


## LLM integration in LangGraph


Your nodes can call any LLM provider that the framework supports through its model interfaces. The model is just another component that a node invokes when it needs a completion.


### Supported model providers and how routing works


You get access to integrations that include OpenAI, Anthropic (Claude), Google (Gemini), and open-source models through Ollama and similar inference servers. You initialize a model with its API credentials and pass it to the nodes that need it.


You can use different models in different nodes, routing simple conversational requests to faster, cheaper models and complex reasoning to more capable ones. The ChatOpenAI class is the most common entry point for OpenAI models in these applications.


**Provider** **Class** **Common models** **Typical use**


OpenAI ChatOpenAI GPT-4, GPT-4o Reasoning nodes, tool-calling agents


Anthropic ChatAnthropic Claude 3.5 Sonnet Long-context analysis, code generation


Google ChatGoogleGenerativeAI Gemini Pro, Gemini Ultra Multimodal tasks, large context windows


Open source ChatOllama Llama 3, Mistral Local development, privacy-sensitive workloads


### Tool calling and function binding inside nodes


You bind tools to your LLM using` bind_tools` , which tells the model what functions are available and how to call them. When the model decides to use a tool, it returns a structured` tool_calls` response.


A ToolNode then executes the requested function, adds the result to the state, and routes execution back to the model node for the next reasoning step.


```text
from   langchain_openai   import   ChatOpenAI
from   langgraph  .  prebuilt   import   ToolNode
from   langchain_community  .  tools  .  tavily_search   import   TavilySearchResults
tavily_tool   =   TavilySearchResults  (max_results  =  3  )
tools   =   [  tavily_tool  ]
llm   =   ChatOpenAI  (model  =  "  gpt-4o  "  ).  bind_tools  (  tools  )
tool_node   =   ToolNode  (  tools  )
```


This pattern is how most LangGraph agents implement the ReAct (Reason + Act) loop. The ChatOpenAI model reasons about what to do, returns` tool_calls` in its response, and the ToolNode executes the requested function (such as a Tavily search).


The result flows back into the state for the next reasoning step. Each` tool_calls` response is logged, giving you a full audit trail of every tool invocation.


## Monitoring, debugging, and evaluating your agents


Your LLM agents will occasionally call the wrong tool, loop longer than expected, or produce incorrect output. Tracing and evaluation tooling give you the visibility to diagnose these issues quickly.


*Trace span hierarchy for a multi-step agent run, showing the parent-child relationship between model calls, tool_calls, and state transitions.*


### LangSmith tracing and run inspection


You can trace every step of your agent’s execution with LangSmith, which provides observability for LangGraph applications. Every node execution, model call, and tool invocation is recorded as a span with inputs, outputs, latency, and token counts.


You can inspect a full run to see exactly which path the graph took, what the state looked like at each step, and where errors occurred. Setting two environment variables enables automatic tracing and streaming of all your runs.


### LangGraph Studio: visual interface for workflow development


You can use LangGraph Studio, a desktop application that lets you visualize your graph’s structure, step through executions interactively, and debug issues without reading raw logs.


You can see which nodes fired, what routing conditions evaluated to, and how the state evolved over time. It is particularly useful during development when you are iterating on graph structure and routing logic.


### Evaluation patterns and regression testing for agent behavior


You should test your agents the way you test any non-deterministic system: with evaluation datasets and scoring functions. Define a set of input scenarios, run them through your agent graph, and score the outputs on criteria like correctness, completeness, and tool-use accuracy.


Running these evaluations on every code change catches regressions before they reach production. LangSmith integrates evaluation workflows, but you can also build custom evaluation pipelines using your existing testing infrastructure.


## The broader LangGraph tooling and future developments


Your investment in LangGraph connects you to a growing set of tools and deployment options beyond the core framework.


### LangGraph Cloud and deployment options


You can deploy your agents to production using LangGraph Cloud, which provides managed infrastructure for running them as API endpoints. It handles scaling, persistence, and streaming without requiring you to manage servers.


You can also self-host applications using LangServe, which wraps your compiled graph in a FastAPI server. For teams already on cloud infrastructure, this deployment flexibility means you can start managed and move to self-hosted (or vice versa) without rewriting your agent logic.


### Emerging patterns: reflection loops, plan-and-execute, and agentic RAG


You will find several LLM application patterns becoming standard in production. Reflection loops have an agent evaluate its own output and iterate until quality criteria are met. Plan-and-execute separates planning (generating a task list) from execution (running each task), with the planner able to revise the plan based on intermediate results.


Agentic RAG wraps retrieval-augmented generation in an agent loop that can reformulate queries, evaluate retrieved document relevance, and decide whether to search again or synthesize an answer. These patterns all rely on the framework’s core strength: the ability to express cycles and conditional transitions in a stateful graph.


## Wrapping up


LangGraph gives you the graph primitives to build LLM agents that loop, branch, persist state, and coordinate across sub-agents. If your team builds in TypeScript,[Mastra](https://mastra.ai/ai-agent-framework) provides the same stateful workflow control with built-in evals and observability.
