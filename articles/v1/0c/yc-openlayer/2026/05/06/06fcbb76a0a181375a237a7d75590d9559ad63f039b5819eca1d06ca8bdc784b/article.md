---
schema_version: "1.0.0"
document_id: "06fcbb76a0a181375a237a7d75590d9559ad63f039b5819eca1d06ca8bdc784b"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/spring-2026"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-25T18:01:08.657570+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:56e31e393fa50b1b1a89df74d498af3e0a7133fdf304135befadc2ac15d87b77"
---

# Openlayer Gateway, customizable dashboards, and Salesforce Agentforce GA

May 15, 2026


[Openlayer Gateway, customizable dashboards, and Salesforce Agentforce GA](https://www.openlayer.com/changelog/spring-2026)


Openlayer Gateway is a new managed control layer for every LLM call your stack makes. The admin portal handles API-key management, per-key rate limits and freeze, and team-based organization — and every request that flows through is observable end-to-end in Openlayer.


## Customizable project dashboards


Project home pages are now fully customizable. Pin widgets for the metrics you watch most closely, see project notifications inline, and jump between alerts from a new notification center in the sidebar.


## Salesforce Agentforce Integration is live


Trace Agentforce conversations end-to-end. Pick your login domain (production, sandbox, or My Domain) and let the updated Builder schema capture every intermediate step.


## Also new this quarter


- **On-prem Helm chart.** Deploy Openlayer to Kubernetes via a Helm chart on Docker Hub OCI, with config for storage, database, replicas, image pull secrets, and Node heap.
- **Built-in governance frameworks.** EU AI Act, TRAIGA, E23 MRM, and the Openlayer Governance Framework, available out of the box.
- **Audit trails.** Full workspace activity log with export.
- **PII redaction in traces.** Workspace-wide redaction with per-project entity-type configuration.
- **New guardrails.** Llama Prompt Guard for prompt injection, English and Portuguese toxicity, and Brazilian PII recognizers (CPF, CNPJ, CEP, phone).
- **New tests.** Bias, Contains PHI, and Unauthorized tool call.
- **Universal search.** Jump across projects, sessions, users, and tests from one search bar.


Full feature, improvement, and fix lists are below.


## Features


- •


Integrations


Salesforce Agentforce integration (GA), with configurable login domain (production, sandbox, or My Domain) and updated Builder intermediate-step schema
- •


Integrations


Dialogflow integration
- •


SDKs


Azure Content Understanding tracer
- •


SDKs


PydanticAI OTel GenAI attribute parsing
- •


SDKs


Trace OpenAI Responses API from the TypeScript SDK
- •


SDKs


openlayer-ts now accepts .csv datasets
- •


SDKs


Trace decorator promote parameter — surfaces nested step outputs to the parent span for easier debugging
- •


Platform


Prompt-injection guardrail (Llama Prompt Guard)
- •


Platform


Toxicity guardrails for English and Portuguese, with opt-in chunking for long inputs and category filtering
- •


Platform


Brazilian PII recognizers in PIIGuardrail (CPF, CNPJ, CEP, phone), with checksum validation
- •


Platform


Bias test
- •


Platform


Unauthorized tool call test
- •


Platform


Contains-PHI test
- •


Security


Workspace-level PII redaction on trace processing
- •


Security


Per-project PII redaction settings — pick which entity types to redact
- •


Security


Audit trails — full activity log with export
- •


Security


Domain verification on the workspace settings page
- •


Security


Workspace setting to restrict workspace creation to deployment admins
- •


Security


Workspace setting to prevent users from a given domain from creating workspaces
- •


Platform


Built-in governance frameworks — EU AI Act, TRAIGA, E23 MRM, and Openlayer Governance Framework
- •


UI/UX


Customizable widgets on project home pages
- •


UI/UX


Project notifications surfaced on the home page
- •


UI/UX


Notification center in the sidebar
- •


UI/UX


Universal search across the workspace
- •


UI/UX


More keyboard shortcuts across the app
- •


UI/UX


Batch actions on rows, rules, and projects tables (including multi-select and delete)
- •


UI/UX


Filtering and sorting on sessions, users, data sources, projects, rules, and framework progress tables
- •


UI/UX


Hide columns in the users table
- •


UI/UX


Tag multiple tests at once
- •


Platform


Session-level aggregated metrics — view cost, latency, and token counts rolled up per session
- •


Platform


LLM cost configuration — view and edit per-model pricing used for cost calculations
- •


Platform


View, edit, and download custom-metric parameter bundles from the UI
- •


Platform


LLM evaluator settings now configurable for Ragas metrics
- •


Platform


Monitoring-data limit notifications
- •


Platform


Test-connection button for LLM configuration
- •


Platform


Openlayer Gateway — centralized AI gateway with admin portal, API key management (including rate limits, visibility, and freeze), and team-based organization
- •


Integrations


BigQuery data sources without a timestamp column
- •


CLI


Browser-based openlayer login (device code flow)
- •


API


Delete records in monitoring mode via API
- •


API


Retrieve sessions linked to a test result via API
- •


API


dateUpdated filter on the List Tests API
- •


On-Prem


Helm chart for on-prem deployments, published to Docker Hub OCI
- •


On-Prem


Set workspace admins via environment variables on deployment
- •


On-Prem


License usage and version reported from on-prem deployments to Openlayer
- •


UI/UX


Universal logout improvements
- •


Platform


Automatic threshold mode for custom metrics


## Improvements


- •


UI/UX


Trace viewer performance and navigation — faster resizing, better breadcrumbs, attachment rendering, and clearer tools view
- •


SDKs


LangChain callback handler latency reduced
- •


UI/UX


Project home page redesigned
- •


UI/UX


Redesigned settings pages
- •


UI/UX


Filter and table UI polish — multi-filter flows, virtualization for large result sets, loading states, and keyboard navigation
- •


Platform


Faster initial app load
- •


Security


MFA passcode auto-authenticates on entry
- •


Security


Deep-link redirect now works after auth and with SSO
- •


UI/UX


Universal search relevance improvements
- •


Platform


Built-in framework refinements
- •


Platform


Default test thresholds set to manual mode
- •


SDKs


Default session and user IDs assigned automatically if none supplied
- •


Integrations


BigQuery connection now supports Service Account secrets
- •


Docs


Custom CA certificates flow documented


## Fixes


- •


Platform


LLM-as-a-Judge reliability — rate-limit retries, temperature errors, and vague skip messages resolved
- •


Platform


Answer-relevance test no longer breaks on OpenAI embedding usage
- •


Platform


Judge score false positives when set to 1 fixed
- •


SDKs


Google ADK, Azure OpenAI, and OCI tracers handle edge cases correctly
- •


SDKs


log_context and log_output work when called from the same function
- •


UI/UX


Tool step type now displays actual tool name
- •


UI/UX


Trace modal stability — no longer reloads when new records stream in, and data no longer overflows
- •


Integrations


Salesforce Agentforce sync and session context fixes
- •


Integrations


Copilot Studio sessions: failing posts to API fixed
- •


Integrations


Slack workspace integration deletion works
- •


Integrations


Vertex AI configuration now correctly reports as connected
- •


Security


Login and auth flow fixes — SSO deep links, MFA, and WorkOS "org not found" issue
- •


Security


403 error when removing users from a workspace fixed
- •


Platform


Customizing the dashboard works for all roles
- •


UI/UX


Table and filter reliability — column widths, null filters, infinite scroll, and data display
- •


Platform


Custom metrics no longer break when adding filters or with multiple tests
- •


Platform


Framework sync no longer creates duplicate rules
- •


CLI


openlayer update no longer fails with "invalid semantic version"
- •


SDKs


LangChain callback handler correctly attributes sessions and user IDs
