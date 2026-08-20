---
schema_version: "1.0.0"
document_id: "919088f600ab93a23979c3d724d2654d77b75b38a06ee96c0ba30ef87d082cc2"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-4bb0a7224006"
canonical_url: "https://www.mezmo.com/newsroom/introducing-aura-building-an-open-agentic-harness-for-production-ai"
published_at: null
first_seen_at: "2026-07-22T04:13:37.000074+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:a0d2a7287d7975513c8e0d62c7f78b1b257ff8763582cf105cf366d4a4b1d01d"
---

# Introducing AURA: Building an Open Agentic harness for production AI

In this blog, Mezmo's Henry Andrews discusses why AI agents need production infrastructure, and introduces AURA, Mezmo's open-source agentic harness, offering declarative composition, multi-agent orchestration, and deep observability.


Over the past decade, platform engineering has reshaped how organizations build and operate software.


Instead of managing infrastructure directly, teams created platforms that coordinate complexity behind the scenes. Kubernetes standardized orchestration. CI/CD pipelines automated delivery. Observability platforms made distributed systems understandable.


Each of these innovations introduced a new layer in the stack, one that allowed engineers to move faster because the platform handled coordination underneath.


**AI has now entered that stack.** And with it comes an opportunity to build the next platform layer.


Large language models have unlocked new ways for software to analyze systems, synthesize insights across telemetry, and automate operational workflows. But moving AI out of the lab and into the cluster exposes a massive gap in maturity. Custom Python scripts wrapping LLM APIs are not production infrastructure. If AI agents are going to investigate incidents, correlate signals, and execute diagnostic queries, they need to run on a harness engineered for reliability, standard interfaces, and multi-step reasoning.


At Mezmo, we believe the next generation of platforms requires purpose-built infrastructure for composing and orchestrating AI. That belief led us to build and open-source[AURA](https://www.mezmo.com/aura) **.**


## The System of Context


To support operational workflows, AI systems need more than prompts and models. They need a way to dynamically understand how data, tools, and workflows relate to each other as an investigation unfolds. We refer to this architectural pattern as a System of Context.


A System of Context provides the intelligence layer that allows AI to reason about operational environments tying together telemetry signals, operational runbooks, and infrastructure APIs. When this contextual information is accessible, AI can move beyond answering questions and begin participating meaningfully in operational workflows.


But a concept needs an engine. AURA is the open-source runtime and orchestration harness built to make this concept a reality.


## AURA: The engine behind production AI


AURA is an open-source, Rust-based agent harness. It tackles the immediate engineering hurdles that platform teams face when moving AI into real-world environments.


Rather than acting as a black-box service, AURA uses declarative TOML configuration to define complete agent workflows, model provider, system prompts, MCP tools, RAG pipelines, and orchestration topology, in files that can be version-controlled, reviewed, and deployed alongside the rest of your platform.


Here is what AURA actually implements under the hood:


### 1. Declarative agent composition


Platform engineers shouldn't be writing complex application logic just to wire a prompt to a tool. AURA allows you to define an entire agentic workflow in a single config.toml file. Your AI configurations can now be managed with the same version-control and review workflows as your Kubernetes manifests.


### 2. Multi-agent orchestration & execution persistence


Simple LLM calls fail at complex tasks. AURA will have a robust multi-agent orchestration module that utilizes a DAG (Directed Acyclic Graph) executor. It supports dependency-aware parallel wave execution, quality evaluation loops, and iterative re-planning.


To ensure this complex reasoning isn't a black box, AURA features Execution Persistence. While workflows are scoped to the lifecycle of the request, AURA writes detailed execution artifacts like plans, prompts, responses, and tool call records, to disk per iteration. This provides deep, post-hoc observability into exactly how your orchestration solved a problem.


### 3. A Drop-In, OpenAI-Compatible API


Integrating AI into existing internal developer portals or ChatOps UIs is historically painful. AURA solves this by exposing a standard /v1/chat/completions endpoint with SSE (Server-Sent Events) streaming. You can point your existing integrations (like LibreChat or OpenWebUI) directly at AURA.


Behind the scenes, AURA handles multi-provider routing, allowing you to seamlessly swap between OpenAI, Anthropic, AWS Bedrock, Gemini, or local Ollama models simply by updating your TOML configuration.


### 4. Open tool integration via MCP & seamless interoperability


AI becomes significantly more useful when it can interact with real systems. AURA features deep, first-class integration with the Model Context Protocol (MCP), supporting HTTP streamable, SSE, and STDIO transports.


More importantly, AURA acts as a universal translator between your operational tools and your foundational models. Models like OpenAI that enforce strict schema requirements demand specific formatting for tool capabilities. AURA performs automatic schema sanitization translating standard MCP tool definitions at discovery time into the exact formats these models require.


This eliminates the need to write custom glue code or translation layers. You can plug MCP-compliant tools directly into these LLMs and it simply works. AURA also manages the messy realities of production request lifecycles, safely handling graceful shutdowns, disconnect detection, and MCP cancellation propagation so runaway processes don't consume cluster resources.


### 5. Deep observability & custom streaming


Standard LLM streaming isn't enough for complex, multi-step workflows. AURA enriches its SSE stream with custom events like aura.tool_requested, aura.tool_complete, and aura.orchestrator.plan_created. This allows your front-end applications to build rich UIs that show users exactly what the agent is thinking and doing in real time.


Furthermore, AURA ships with native OpenTelemetry integration (via an otel feature flag) and an OpenInference exporter. It automatically generates rich spans for agent streams, LLM calls, and tool executions, allowing you to trace AURA’s decisions in tools like Arize Phoenix or your existing APM.


## AURA in action: Incident response


To understand how this comes together, imagine a Kubernetes CPU spike alert triggering an investigation workflow in your developer portal.


Instead of routing to a basic LLM, the request hits AURA's OpenAI-compatible endpoint. AURA’s DAG orchestrator takes over. It delegates to a "Metrics Worker" (assuming an MCP server exists to connect it to Datadog) and a "Logs Worker" (using an MCP-connected Elastic instance).


The workers execute in parallel, querying the systems and returning their findings. An evaluation loop verifies the context, realizes it needs to check recent deployments, and dynamically re-plans to query GitHub. Finally, it synthesizes the root cause and streams the result to your defined destinations (PagerDuty, Slack, etc) with every step, reasoning event, and tool call fully traced via OpenTelemetry.


## Building the next platform layer


AI is quickly becoming part of the modern software stack. As that happens, platform teams have an opportunity to shape how these systems operate. By deploying a harness that explicitly manages standard APIs, declarative configuration, and advanced orchestration, you can transform AI from an experimental prototype into a dependable platform service.


[AURA](https://www.mezmo.com/aura) represents our first step toward that vision. By open-sourcing, we hope to provide a reliable, well-engineered foundation that the platform engineering community can adopt, stress-test, and evolve.


It is time to move past isolated scripts and fragile API wrappers. If AI agents are going to become real participants in our systems, they need a harness designed with production realities in mind.


‍
