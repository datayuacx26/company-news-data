---
schema_version: "1.0.0"
document_id: "fc457e3b11eebe92d8b3ab96968ed184678a65bcd673fb184610db86cf917e2e"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/observe-mcp-server-cli-ai-agents-telemetry"
published_at: "2026-08-13T16:48:55+00:00"
first_seen_at: "2026-08-13T20:12:20.338083+00:00"
fetched_at: "2026-08-13T20:12:21.962674+00:00"
content_hash: "sha256:2ffe1a9336cb0b5d2a2aa2fdd18a45dcb3d83eb62b40b6b40798209a0786da42"
---

# Connect AI Agents to Telemetry with Observe MCP & CLI

Agents have a sophisticated understanding of your operational data. Coding assistants can now investigate errors before an engineer opens a ticket, and AI SREs can correlate failures across services without being paged. In this ever-evolving landscape, humans may no longer be the primary readers of your telemetry data.


This places new demands on observability platforms because observability must work for both humans and agents. Telemetry needs to be readily accessible, not just through a UI, but through interfaces agents can call and automate: APIs, model context protocol (MCP) and command line interface (CLI).


To help ensure observability works for both the humans and the machines interacting with data, we are proud to announce the general availability of the **redesigned Observe by Snowflake MCP server and a new Observe CLI that has full parity with the MCP** (general availability is rolling out now across all clusters; eu-2 and ca-1 will follow shortly). This means every operation the MCP server exposes to agents is also available as a CLI command.


These two tools are built for the growing community of engineers who want to connect their own AI agents to Observe, or simply bypass the existing UI to interact with Observe’s platform. Engineers are using the MCP server and CLI to build custom alert-triage agents that automatically query Observe when an alert fires and pull production telemetry for debugging context, and copilots that work alongside humans during incident investigations — along with other capabilities.


With the redesigned MCP server and new CLI, agents that need to reason about production can now query Observe directly, enhancing speed and efficiency.


[Learn how Observe by Snowflake turns telemetry into insights — fast](https://www.snowflake.com/en/product/observe/)


## Agent-compatible CLI access to the entire Observe platform


The previous Observe CLI covered only a narrow slice of what the platform could do, as most functionality was inaccessible from the terminal. The new, agent-compatible CLI provides programmatic access to Observe’s capabilities from the command line, with the same surface available through the MCP server and in the UI. It works from agent environments like Claude Code as well as interactive terminal sessions.


Some workflows can run autonomously in the background, handling routine tasks without an engineer present. Others are designed for interactive use, requiring an engineer to guide the investigation directly. The new CLI is a programmatic interface that helps our users compose, automate and extend their observability workflows.


### Prebuilt skills for common observability workflows


Both the CLI and MCP server ship with a library of skills: structured workflows built based on how Observe engineers solve common observability tasks. Agents and engineers can use these skills directly to investigate incidents, trace failures, validate changes, find n+1 issues and even detect outliers. The skills work immediately without additional setup after configuration, which means both humans and machines can hit the ground running.


## A rearchitected MCP, built for cost efficiency and agent access


Prior to rebuilding the MCP server, we spoke with the engineering teams that are already connecting their agents to Observe. Their feedback shaped what we built:


-


**Removing the LLM intermediary makes the MCP server more cost-efficient to operate.** Without it, agents connect directly to Observe’s APIs, reducing latency and overhead for every call.


-


**Several teams had built their own workarounds because there was no supported programmatic access.** The rebuilt server covers what those teams needed.


-


**Engineering teams had specific use cases in mind,** including updating dashboards after an investigation, fetching chart images for automated reports and querying APM errors directly. We expanded our platform to support them.


The most significant change in the new MCP server is economic. The original architecture had a custom LLM harness built in. It received a user query, handled the reasoning and returned an answer. With advances in foundation models, an Observe LLM middleman is no longer necessary. The new architecture drops it, removing the need to pass on that previous cost.


## Technical dive: MCP server now gives agents direct access to Observe


**In the old MCP Server,** agents had to send queries to a single endpoint, where Observe’s LLM interpreted the question and returned an answer. This meant agents didn’t have direct visibility into Observe’s data structure and didn’t have permission to explore data sets, which was expensive for both Observe and our customers.


**In the new MCP Server,** agents connect to the Observe platform and have access to the same APIs that power Observe’s frontend. This means agents can query Observe’s context graph directly, determine which data sets are most valuable to query (enabling more accurate answers), and write efficient[OPAL](https://docs.observeinc.com/docs/what-is-opal) , allowing them to run precise observability queries against any telemetry data.


Through the MCP server, agents have access to the same capabilities available in the Observe UI: APM service maps, OpenTelemetry data collection setup in Observe, and active alert listing.


Together, these capabilities give agents direct access to what’s in Observe, without the latency or cost of the old LLM intermediary.


## Get started with the MCP server


To get started with the Observe MCP server, log into your tenant, go to "Manage account" → "MCP server details", and follow the instructions for your agent of choice. The demo below walks through the full setup and a live investigation workflow.


## Making observability data accessible to every tool in your stack


At Observe by Snowflake, we believe that observability is fundamentally a data problem, and we are building to be at the forefront of this shift in how observability is consumed. The new Observe MCP server and CLI access enable us to make observability data directly accessible to the AI agents and developer workflows that teams already rely on.


Learn more and get started with[Observe by Snowflake](https://www.snowflake.com/en/product/observe/) .
