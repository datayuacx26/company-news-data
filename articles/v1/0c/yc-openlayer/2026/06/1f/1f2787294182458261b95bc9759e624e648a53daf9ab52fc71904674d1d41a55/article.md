---
schema_version: "1.0.0"
document_id: "1f2787294182458261b95bc9759e624e648a53daf9ab52fc71904674d1d41a55"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/june-2026"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:90039b5178889f32517315fdcf3275a944b8bfa5d7619b96677d45488a624d9a"
---

# Trace timeline, project lifecycles, and Claude Agent SDK tracing

June 4, 2026


[Trace timeline, project lifecycles, and Claude Agent SDK tracing](https://www.openlayer.com/changelog/june-2026)


A new Gantt-style view of any trace. Every span lines up by start time, duration, and nesting, so the slow step or the unexpected branch is obvious at a glance — no more reading a flat list to reconstruct what ran when.


## Project lifecycles and evidence


Track where each project stands in your governance process and attach the evidence that backs it up, right on the project. Lifecycle stages and supporting documents now live alongside your tests and results.


## Also new this quarter


- **Claude Agent SDK tracing.** First-party tracing for the Anthropic Claude Agent SDK in Python — tool calls, MCP tools, subagents, session continuity, and cost and tokens per run.
- **Automatic SDK instrumentation.**` openlayer.init()` now patches every installed supported LLM SDK automatically — OpenAI, Anthropic, Mistral, Groq, Gemini, LiteLLM, Portkey, Google ADK, and Azure.
- **Gateway guardrails enforcement.** The Gateway can now enforce guardrails policies inline on the LLM requests routing through it.
- **Rule deactivation reasons.** Supply a reason when deactivating a rule; the reason is shown on the rule page.
- **Compliance tests in results.** Results pages now show which tests are required for compliance, in context.
- **LangChain tool artifacts.** The LangChain callback now records` ToolMessage.artifact` , including document lists, instead of dropping it.


Full feature, improvement, and fix lists are below.


## Features


- •


UI/UX


Timeline view for traces — a Gantt-style visualization showing every span's start time, duration, and nesting side by side
- •


Platform


Project lifecycles and evidence — attach a lifecycle stage and evidence documents to a project for governance tracking
- •


SDKs


Claude Agent SDK tracing for Python — instrument the full agent loop, capturing tool calls, MCP tools, subagent nesting, session continuity, and cost and tokens
- •


SDKs


Automatic instrumentation via openlayer.init() — one call patches every installed supported LLM SDK (OpenAI, Anthropic, Mistral, Groq, Gemini, LiteLLM, Portkey, Google ADK, Azure); opt out with auto_instrument=False
- •


Platform


Gateway guardrails enforcement — apply guardrails policies inline to the LLM requests routing through the Gateway
- •


Platform


Gateway API key invite links — admins can generate invite links that let users self-provision their own Gateway API keys
- •


Platform


Rule deactivation reasons — supply a reason when deactivating a rule; the reason is shown on the rule page
- •


Platform


Compliance tests in the results page — results pages now show which tests are required for compliance, in context
- •


UI/UX


Rename data sources — rename a data source directly from its row menu
- •


UI/UX


Download row data in development mode — download data directly from the development-mode view


## Improvements


- •


SDKs


LangChain callback now captures ToolMessage.artifact (including document lists, promoted to trace-level context) instead of dropping it
- •


SDKs


Trace configuration unified around init() — settings merge incrementally instead of resetting on each call, and configure() is deprecated in favor of init()
- •


Docs


Copilot Studio native integration docs — full setup for the pull-based data source, covering Azure AD setup, agent discovery, sync frequency, and backfill; the Logic App and Batch API paths are now deprecated
- •


Docs


v1 Gateway documentation — setup, routing, and governance-framework listing for the Gateway
- •


Integrations


Dialogflow improvements — relevant metadata is promoted to the row level, and you can now edit the agent display name
- •


UI/UX


UI/UX polish — adjustable test and rule sidebar width, notifications moved to the navigation header, smoother popover and modal animations, and navigation drawers that auto-scroll to the selected resource


## Fixes


- •


Platform


Gateway — navigating directly to a route (for example /admin/keys) no longer returns a 404
- •


CLI


CLI — openlayer tests export no longer fails for projects that use tags
- •


Platform


Gateway — PII detection now uses the correct model required by Presidio
- •


UI/UX


Column filters now persist when switching tabs, no longer reset on their own, and are no longer duplicated in test data tables
- •


Platform


The dashboard page is no longer blank for viewer-role users
- •


UI/UX


Ground truths can now be edited after they're added
- •


Integrations


Agentforce sync reliability — fixed a sync timeout, a cross-agent session leak, and context-extraction failures in Builder-schema traces
