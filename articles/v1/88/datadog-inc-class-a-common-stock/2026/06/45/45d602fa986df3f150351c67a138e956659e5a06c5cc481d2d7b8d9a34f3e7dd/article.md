---
schema_version: "1.0.0"
document_id: "45d602fa986df3f150351c67a138e956659e5a06c5cc481d2d7b8d9a34f3e7dd"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/give-your-ai-agents-live-datadog-access-from-the-command-line/"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:b4610777b3f0944db17d3c880bcbe5b81f853d2834d110bf4122c4ed7dda444a"
---

# Give your AI agents live Datadog access from the command line

Cody Lee


SeGe Jung


Sumedha Mehta


Eddie Cai


Technical Content Writer


AI agents are becoming a standard part of how engineers write, deploy, and troubleshoot software. Getting observability data into those workflows, securely and without manual intervention, remains the harder problem.


[Pup CLI](https://github.com/DataDog/pup) gives shell-style agents, automation pipelines, and scripting workflows secure, OAuth-scoped access to 33+ Datadog product domains through a single binary with 200+ commands. Agents can discover commands dynamically, reason over structured JSON or YAML output, and use built-in skills and runbooks without managing long-lived API keys. Because agents pull only the commands they need via` pup agent schema` rather than loading a full tool definition library upfront, working context is more token efficient. The CLI pairs with the[Datadog MCP Server](https://docs.datadoghq.com/bits_ai/mcp_server/) , which covers conversational, chat-style workflows in IDEs and assistants.


In this post, you’ll learn how to:


-Give shell-style agents a CLI built for Datadog


-Access the Datadog platform from one binary


-Authenticate once with OAuth


-Add Datadog skills and runbooks to your agent


-Connect any AI client to Datadog locally


## Give shell-style agents a CLI built for Datadog


AI agents interact with systems differently depending on where they run. Chat-style assistants inside IDEs need curated, conversational interfaces with confirmation gates and constrained actions. Terminal-native agents, CI/CD workflows, and automation harnesses need composable command-line access that fits naturally into shell workflows.


Modern coding agents are already fluent in shell environments. Models trained on large volumes of terminal interactions understand workflows built around commands such as` git` ,` kubectl,`` jq` , and Unix pipes. A well-designed CLI extends an agent’s operational capabilities without additional configuration. Agents can invoke commands, chain outputs, and interpret structured responses using patterns they already know.


Pup CLI builds on that model with a single, consistent Rust binary across Claude Code sessions, Cursor workflows, CI runners, runbooks, and custom agent harnesses. Regardless of where an agent runs, it interacts with the same Datadog backend through the same interface and authentication flow.


## Access the Datadog platform from one binary


Pup CLI gives agents a single interface for the Datadog platform, from[metrics](https://docs.datadoghq.com/metrics/) and[logs](https://docs.datadoghq.com/logs/) to[Incident Management](https://docs.datadoghq.com/incident_response/incident_management/) ,[Real User Monitoring](https://www.datadoghq.com/product/real-user-monitoring/) ,[Application Performance Monitoring](https://www.datadoghq.com/product/apm/) ,[Cloud SIEM](https://www.datadoghq.com/product/cloud-siem/) ,[Cloud Cost Management](https://www.datadoghq.com/product/cloud-cost-management/) , and more without switching tools or managing separate integrations.


The CLI is designed to be discoverable for both humans and agents. Engineers can use standard help commands to explore the available functionality, while agents can retrieve the complete command schema dynamically in machine-readable form.


Terminal window


```text
1   pup     --help    2   pup     agent     schema
```


The` pup agent schema` command returns the complete command tree as structured JSON, allowing agents to inspect available operations only when they need them. Dynamic schema retrieval reduces the amount of tool metadata that needs to remain in an agent’s working context, leaving more room for reasoning and planning.


Pup CLI returns structured JSON or YAML output by default, making it easy for agents to parse and chain results into additional workflows. Commands integrate naturally with standard shell tooling such as` jq` ,` grep` , or custom automation scripts.


For example, an agent investigating a production issue could search logs for recent checkout service failures:


Terminal window


```text
1   pup     logs     search     --query=  "status:error service:checkout"     --from=  "1h"     \    2        |   jq     '.data[].attributes.message'
```


An agent could then pivot into monitor investigations:


Terminal window


```text
1   pup     monitors     search     --tags=  "team:api-platform"    2   pup     monitors     get     <id>
```


Or retrieve active incidents directly from Datadog:


Terminal window


```text
1   pup     incidents     list     --status=active     \    2        |   jq     '.data[].attributes.title'
```


Beyond core observability workflows, Pup CLI also covers a broad set of the[Datadog API](https://docs.datadoghq.com/api/latest/?tab=java) surface. Niche admin operations, preview surfaces, and CRUD support for platform configuration objects are reachable through the same binary and authentication layer, without additional setup.


## Authenticate once with OAuth


Many teams rely on long-lived API and application keys spread across shell environments, CI pipelines, Terraform configurations, and internal documentation. Pup CLI addresses this with an OAuth 2.0 + PKCE authentication flow.


Instead of distributing static API keys across multiple environments, you authenticate once through:


Terminal window


```text
1   pup     auth     login
```


Tokens are scoped, revocable, and aware of multi-organization environments. If a device is lost or access needs to be revoked, you can invalidate the associated credentials without rotating every script or automation workflow tied to a static key.


Pup CLI inherits[Datadog role-based access control](https://docs.datadoghq.com/account_management/rbac/?tab=datadogapplication) . Agents can only view or act on the resources their authenticated user is already authorized to access. This helps you preserve existing access boundaries while extending Datadog into AI-assisted workflows, including for teams with HIPAA requirements.


A single authenticated session can support multiple local agent workflows, including Claude Code, Cursor, OpenCode, custom agents, and ad hoc scripts. You no longer need to provision and manage separate API keys for every tool or environment.


Managing API keys across CI pipelines, shell environments, and agent tools adds friction when you adopt AI-assisted operations. Pup CLI helps reduce that friction by consolidating access through a single OAuth-scoped interface, using a centralized audit trail and your existing Datadog permissions.


## Add Datadog skills and runbooks to your agent


Pup CLI ships with built-in skills and subagents that give agents reusable operational workflows without manual prompt crafting.


For example, you can install bundled skills into Claude Code:


Terminal window


```text
1   pup     skills     install     claude-code
```


This command installs skills into` .claude/skills/` and native subagents into` .claude/agents/` .


Cursor workflows are similarly supported:


Terminal window


```text
1   pup     skills     install     cursor
```


You can also browse and install individual skills:


Terminal window


```text
1   pup     skills     list    2   pup     skills     install     claude     --name=dd-monitors
```


Pup CLI integrates directly with the Claude Code plugin marketplace as well:


Terminal window


```text
1   /plugin     marketplace     add     datadog/pup
```


The bundled skills support operational workflows such as incident triage, log, metric, and trace correlation, change tracking, and Database Monitoring analysis. If you’re using Claude Code, you can invoke a workflow such as` /sre-investigate` and receive a guided investigation that moves through logs, traces, deployments, and database telemetry while the agent calls Pup CLI commands behind the scenes. You can use runbook outputs directly in incident reviews and postmortems.


Pup CLI also supports CI/CD automation scenarios such as[deploy event creation](https://docs.datadoghq.com/dora_metrics/setup/?tab=apiorCLI) ,[source map uploads](https://docs.datadoghq.com/real_user_monitoring/guide/upload-javascript-source-maps/) ,[DORA metrics collection](https://docs.datadoghq.com/dora_metrics/) , and[SLO validation](https://docs.datadoghq.com/service_management/service_level_objectives/) . Each of these workflows uses the same OAuth-scoped authentication as local sessions, so no additional secrets need to be provisioned or rotated in CI configuration.


## Connect any AI client to Datadog locally


Some organizations use proprietary AI agents or clients that do not integrate directly with Datadog. Pup CLI connects those environments to Datadog through the` pup acp serve` command, which runs a local, OAuth-scoped AI server.


Terminal window


```text
1   pup     acp     serve
```


Pup CLI supports two protocols out of the box:


-


ACP (Agent Communication Protocol) for ACP-native clients


-


OpenAI-compatible` /chat/completions` endpoints for OpenCode, Cursor, and any` @ai-sdk/openai-compatible` client


By default, Pup CLI automatically discovers your first Datadog AI agent. You can also target a specific agent instance:


Terminal window


```text
1   pup     acp     serve     --agent-id     <uuid>     --port     9099
```


Once running, any compatible client can connect locally:


Terminal window


```text
1   http://127.0.0.1:9099
```


Running Pup CLI as a local server lets you connect internal or experimental AI tooling to live Datadog telemetry data without waiting for vendor-specific integrations. Datadog remains the operational source of truth while you retain flexibility in how you build and deploy AI workflows.


## Bring Datadog into your AI agent workflows


AI agents are becoming a standard part of software delivery and operations, but most still lack direct access to live production telemetry. Pup CLI gives shell-style agents a single command-line interface for interacting with the Datadog platform across debugging sessions, automation workflows, CI/CD pipelines, and custom agent ecosystems. It gives your agents OAuth-scoped access to live Datadog telemetry, with built-in skills for agent workflows and no long-lived API keys required. The result is a consistent way to bring Datadog telemetry into your agent workflows.


If you’re new to Datadog, you cansign up for a 14-day free trial to start giving your AI agents access to live Datadog telemetry data.


-
-
-
