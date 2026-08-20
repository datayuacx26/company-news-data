---
schema_version: "1.0.0"
document_id: "51e4942ae3719bfe1c25ac2765b7226f91c9e6d6b3b03d0826547b253f8c4595"
company_key: "yc-openlayer"
company: "Openlayer"
source_id: "yc-openlayer-news-import-df137f62af3c"
canonical_url: "https://www.openlayer.com/changelog/summer-2026"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-11T02:21:02.577398+00:00"
fetched_at: "2026-08-11T02:21:04.004794+00:00"
content_hash: "sha256:2c3728f3f0d61a2e7b701266386db40fa26de250cf9d9381e4930fdde581ac46"
---

# AI summaries, semantic search filters, and the remote MCP connector

Sessions, traces, and test results now open with an AI summary — how the session went, what a trace did, and the dominant failure modes behind a test result, with drill-down from each failure mode to the exact rows behind it.


[Read the docs →](https://www.openlayer.com/docs/monitoring/ai-summaries)


## Semantic search filters


Build test subpopulations with embedding-based semantic matching instead of exact or keyword matching — a filter for "billing problems" also catches "I was charged twice", with no shared keywords.


[Read the docs →](https://www.openlayer.com/docs/tests/semantic-search-filters)


## Remote MCP connector


The Openlayer MCP server now runs as a hosted, OAuth-protected connector — add https://mcp.openlayer.com to Claude Code, claude.ai, Cursor, VS Code, and other MCP clients, and work with your workspace in natural language. Your coding agent can pull test results and LLM-judge explanations to auto-improve your codebase, and validate changes against the rules and policies of your governance frameworks.


[Read the docs →](https://www.openlayer.com/docs/openlayer-mcp)


## Also new this quarter


- **Openlayer Agent Skills.** Packaged skills that teach Claude Code, Cursor, and other AI coding assistants to wire a codebase into Openlayer — tracing, offline evals, tests, guardrails, and CI gating.[Learn more →](https://www.openlayer.com/docs/openlayer-skills)
- **LLM cost explorer.** A free public tool for browsing and comparing LLM pricing across providers, at llm-costs.openlayer.com — the same feed that powers Openlayer's cost tracking.[Try it →](https://llm-costs.openlayer.com/)
- **Workspace dashboards.** An at-a-glance view of cost, usage, and project health across the whole workspace.[Open your dashboard →](https://app.openlayer.com/)
- **Webhooks for test results.** Subscribe a webhook to test-suite outcomes and get notified when tests pass or fail.[Learn more →](https://www.openlayer.com/docs/security/webhooks/overview)
- **openlayer init and headless CLI.** One guided command takes a fresh directory to a linked, monitored, instrumented project;` link` ,` login` , and resource creation now run non-interactively for agents and CI.[Learn more →](https://www.openlayer.com/docs/api-reference/cli/commands/init)
- **IBM watsonx Orchestrate integration.** Joins Agentforce and Copilot Studio as a first-party agent integration.[Learn more →](https://www.openlayer.com/docs/integrations/ibm-watson-orchestrate)
- **Gateway upgrades.** Per-user usage limits and guardrails, multimodal` /responses` and` /messages` endpoints, semantic model-based routing, and a Helm chart for Kubernetes deployments.[Learn more →](https://www.openlayer.com/docs/gateway/overview)


Full feature, improvement, and fix lists are below.
