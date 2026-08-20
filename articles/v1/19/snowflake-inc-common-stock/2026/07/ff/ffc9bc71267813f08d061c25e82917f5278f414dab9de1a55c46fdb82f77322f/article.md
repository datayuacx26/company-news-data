---
schema_version: "1.0.0"
document_id: "ffc9bc71267813f08d061c25e82917f5278f414dab9de1a55c46fdb82f77322f"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/snowflake-cortex-agents-enterprise-ai-scale"
published_at: "2026-07-21T16:00:00+00:00"
first_seen_at: "2026-07-24T01:53:21.113388+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:b69657ec6702a8c35de26863bbef8820e77d24468eb326bebe6e3bbb62b05329"
---

# What's New in Snowflake Cortex Agents for Enterprise AI

The first generation of enterprise AI focused on building agents. The next generation is about operating them at scale.


[Cortex Agents](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents) have always provided a managed runtime for building AI applications inside Snowflake. Over the past year, we've watched customers move from deploying their first agent to managing enterprise deployments. That shift has surfaced a new set of operational challenges around orchestration, execution and governance.


Today, we're expanding Cortex Agents with new capabilities that simplify the operational complexity of building, deploying, and operating enterprise AI agents at scale.


## At a glance


- **Build**


- **Coding Agent (in public preview soon):** Build a managed coding agent backed by the same runtime that powers CoCo and deploy it in any application.
- **Skills Package (in public preview soon):** Package a curated set of skills into a single object and reference them in your agent with a single URI
- **Agent Toolset (in public preview soon):** Reference another agent's tools in your current agent specification instead of rewriting and maintaining duplicate tool definitions.
- **Tool Search (in public preview soon):** Discover tools on demand instead of loading every definition upfront.


- **Run**


- **Async Agent API (generally available soon):** Run long jobs in the background without holding a connection open.
- **Code Execution Tool (in public preview soon):** Process data and generate polished outputs inside a sandboxed Python environment.
- **Interrupt and Resume (generally available soon):** Stop a running agent, correct course and pick up where it left off.
- **Partial Access (in public preview soon):** Serve users with different permission levels from a single managed agent.


- **Manage**


- **Versioning UI (in public preview):** Compare, roll back and promote agent configurations without editing YAML.


## Cortex Agents — a refresher


A Cortex Agent can reason over your enterprise data, invoke tools, execute code and take actions across connected systems — all within Snowflake's governance boundary. Every action is subject to the same access controls and audit trail that cover the rest of your data.


You define a Cortex Agent declaratively that includes its orchestration model, tools and instructions. Applications invoke it through a REST API, while Snowflake handles model routing, tool invocation and result assembly.


## The production challenge


Most deployments start with a single agent, a handful of tools and short tasks. That setup works fine for a proof of concept and maybe a small deployment.


Enterprise production at scale looks different: Workflows span multiple specialized agents, tool libraries grow to dozens of integrations, requests run for minutes rather than milliseconds and governance teams need visibility into exactly what each agent is authorized to do. These aren't problems that better prompting or a faster model will solve. They're operational problems, and they're what our new capabilities below are designed to address.


## Build your agent


#### Bring Snowflake CoCo's coding capabilities to any application


Snowflake CoCo is the first truly Data-native AI coding agent, built to speed time to value across modern data stack. Deeply aware of your enterprise's data, compute, governance, and operational semantics, CoCo turns complex data engineering, analytics, machine learning, and agent-building tasks into simple and informed conversations with high accuracy and trust.


With the Cortex Agents REST API, you can now build a managed agent that brings those same **coding capabilities (in public preview soon)** into your own applications and interfaces. Configure the agent with the CoCo toolset (including sandbox), scope its instructions and data grounding to your environment, and expose it over a standard REST endpoint. Any query that requires code generation, SQL execution, data transformation or pipeline automation is handled inside the sandboxed runtime — the same foundation that powers CoCo, now composable as part of your own agentic workflows.


For example, our engineering team built "Debug with CoCo," an internal tool that root-causes any Cortex Agent request, in under 20 min. The app itself is a few hundred lines long: it takes a request_id via UI, and makes a single Cortex Agent API call with a dedicated prompt. The managed API provisions a CoCo sandbox, mounts the logs as the agent's grounding, runs bash and SQL through the CoCo toolset, and streams an evidenced root-cause analysis back to the browser. No coding-agent runtime, and no data leaving Snowflake — just REST calls:


*Figure 1: "Debug with CoCo," an engineer tool built on the Managed Agent API with CoCo runtime.*


#### Give your agents a shared capability layer


As agent deployments grow, skill management becomes a bottleneck. Every new agent needs its own list of capabilities, every update needs to be applied across each one individually, and as skill libraries expand, agent specifications become harder to maintain and easier to get out of sync.


**Skills Package (in public preview soon)** lets you define a curated set of skills once and reference the entire collection in any agent specification with a single URI. When the package is updated, every agent that references it picks up the change automatically. Teams can build a shared capability layer — a standard set of data workflows, domain-specific tools, or approved integrations — and compose new agents on top of it without duplicating or maintaining separate copies.


#### Reuse tools across agents without rewriting them


Enterprise agent deployments often start with a well-defined agent — one with a carefully curated set of tools, tested integrations and tuned configurations. When a second agent needs some of those same tools, the current approach is to duplicate the tool definitions: copy the spec, maintain two copies and keep them in sync whenever something changes.


**Agent Toolset (in public preview soon)** lets you reference another agent's tools directly in your current agent's specification. A single line points to the source agent, and all its tools are available immediately — no redefinition required. When the source agent's tools are updated, every agent referencing that toolset inherits the change automatically. Teams can define a canonical set of tools once — a shared data access layer, a standard set of search integrations, a common MCP configuration — and compose new agents on top of it without duplicating or diverging from the original.


#### Optimize context building for your agents


A large enterprise deployment with connections to GitHub, Slack, Jira, Salesforce and internal MCP servers can easily accumulate dozens of tools. Before answering a single question, an agent may spend tens of thousands of tokens loading tool definitions into context.


**Tool Search (in public preview soon)** will address this through progressive disclosure. Instead of loading every tool definition upfront, the agent will search its tool library at inference time and will load only the definitions relevant to the current task. At scale, this will be more than a performance optimization — it can improve accuracy by reducing context window pressure and will enable agents to operate across tool ecosystems that would otherwise be impractical to load all at once.


## Run your agent


#### Running long tasks shouldn't block your app


Some tasks take minutes, not milliseconds. Reconciling monthly financial data, generating a comprehensive research report, processing a batch of customer contracts — these aren't jobs where you can hold an HTTP connection open.


The **Async Agent API (generally available soon)** lets agents run these tasks in the background. Callers receive a run_id immediately and retrieve responses when the task is complete.


The agent runs to completion — or pauses if it requires user input — and the result is available when ready. Long-running agent workflows can now execute entirely within the managed runtime, without requiring custom queuing or orchestration infrastructure.


#### From raw data to polished output, securely, in one agent run


The **Code Execution Tool (in public preview soon)** gives Cortex Agents access to a secure, sandboxed Python environment where they can process data, run calculations, generate visualizations and produce polished outputs — PDFs, PowerPoint presentations, charts — without leaving Snowflake.


The sandbox runs Python 3.12 and includes numpy and pandas by default. Additional packages can be retrieved from PyPI through the Artifact Repository. External network access can be scoped through named integrations. *Each execution runs in an isolated environment. Agent generated code cannot access your data without explicit permissions.* This allows an agent to complete an entire workflow — from analyzing a data set to producing a formatted PDF report — within a single run.


#### Pause, correct and continue your workflow


You might ask an agent to analyze two years of financial data and then realize you actually need five. Or you notice it heading down an unhelpful path and want to redirect it before it finishes.


**Interrupt and Resume (generally available soon)** gives builders a first-class API for human steering. Stop the agent, send a correction, and it picks up from where it left off.


The agent pauses, stores its work and waits for the next instruction, eliminating the need to build custom state management or recovery logic around interrupted workflows.


#### One managed agent, multiple access levels


In most organizations, different users have different permissions. Sales teams may be able to access customer health metrics, while finance teams can also view billing information. Supporting these differences often requires building and maintaining multiple versions of the same agent.


With **Partial Access (in public preview soon)** , the same managed agent can serve requests across different permission levels. When a request arrives, the agent evaluates which tools the user is authorized to use and fulfills the request within that scope. Controls are enforced at the Snowflake layer — consistent with the RBAC already governing the rest of your data. In practice, a single agent can serve both employees and managers, or analysts and executives, from the same knowledge base while enforcing different access boundaries for each.


## Manage and update your agents


#### A visual snapshot of every agent configuration


Agent configurations evolve over time: Prompts get tuned, tools get added, business logic changes. Without structure, teams accumulate undocumented drift between what's running in production and what's being tested.


The **Versioning UI (in public preview)** gives teams a visual history of agent configurations — compare versions side by side, roll back to a previous state and promote a tested candidate to production without editing YAML by hand. A LIVE version stays in production while teams iterate safely on candidate versions before release. For regulated industries, this creates a clear chain of custody for every agent configuration — what changed, when and who approved it.


## Built for enterprise scale


The capabilities we launch today extend the managed platform that Cortex Agents already provides. Orchestration, execution and operational controls help teams scale from their first production deployment to organizationwide adoption.


This has been the main focus for us as Snowflake delivers managed agents: a platform that simplifies the operational complexity, so builders can spend less time maintaining infrastructure and more time building AI applications. As enterprise AI deployments continue to mature, the work shifts from building agents to operating them. That's the next phase Cortex Agents is built to support.


## Get started


- Try the Code Execution Tool[using this sandbox configuration and package setup](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents/code-execution)
- Build your first multi-agent workflow[using this declarative orchestration guide with working examples](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents/multi-agent)
- [Watch the Snowflake Summit 2026 demos](https://www.snowflake.com/summit/) to see these capabilities running live


*Forward-looking statement: This blog post contains forward-looking statements, including statements about products, features and capabilities that are under development, not yet generally available or otherwise not yet available. These forward-looking statements are subject to risks and uncertainties that could cause actual results to differ materially. Please refer to Snowflake's SEC filings for a more detailed description of the risks that could cause actual results to differ.*
