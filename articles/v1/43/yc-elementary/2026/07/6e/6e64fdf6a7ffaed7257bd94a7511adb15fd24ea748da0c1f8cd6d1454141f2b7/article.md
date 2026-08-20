---
schema_version: "1.0.0"
document_id: "6e64fdf6a7ffaed7257bd94a7511adb15fd24ea748da0c1f8cd6d1454141f2b7"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/march-product-announcement"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:057dfeaceb901c56ceba35feffdd83b3854624d8ca426610bf24c353632cf59d"
---

# March Product Announcement

Last week we shared our[autonomous data management vision](https://www.elementary-data.com/future-of-data) . We're focused on two tracks to get there:


- Expanding what the agents know, the Elementary context engine.
- Expanding what the agents can do, with new tools and capabilities.


This release does a lot of both, with: new integrations across the pipeline (Fivetran for ingestion, Semantic Layers, Lightdash for analytics), new context (table usage stats) and new agent capabilities including code review, Plan Mode, and agents working across Jira, Slack, Teams, and email.


## New Agent: Code Review


Elementary now posts a structured comment on every PR that touches dbt models. Column-level impact analysis, test impact, active incidents, and a plain-language merge signal.


Catch issues before they reach production, not after. Supports GitHub Actions and GitLab CI.


👉[Setup guide](https://docs.elementary-data.com/cloud/features/ci)


## Ingestion Integration: Fivetran


Catch and manage issues as far upstream as possible. When a Salesforce sync, HubSpot import, or any other ingestion job fails, the incident surfaces in Elementary with the full pipeline context, so your team and agents can manage the full picture and triage accurately.


Alert notifications via Slack, PagerDuty, and other channels are coming soon.


👉[Setup guide](https://docs.elementary-data.com/cloud/integrations/pipeline/fivetran)


## Table Usage (Beta)


Elementary now collects query count per table and ranks each by usage percentile. This gives both your team and agents the context to understand what's actually being used and make decisions accordingly.


Starting with BigQuery, more warehouses coming soon.


Interested in early access? Reach out to the team to join the beta.


## Agent Capabilities


### Work with agents on Jira, Email, Slack and Teams


Agents can now access and manage Jira tickets, send emails, Teams, and Slack messages to channels, and reply to previous threads directly.


Up next: Chat with agents directly from messaging apps.


### Plan and Act at Scale


Agents now support "Plan mode" for complex tasks and bulk edits. Before making changes, the agent lays out exactly what it's going to do and waits for your review. Our improved context handling also allows handling multi-step tasks with accuracy.


In addition, you can now select any list of assets in the UI and pass them directly to the agent as context.


### New Investigation Tools


Agents can now dig deeper into execution history. New tools cover asset and test execution history, job invocations, run results, models, sources, and Elementary test results.


## Semantic Layer Integration


Snowflake Semantic Views now appear in Elementary's lineage graph. Metrics, dimensions, and facts are connected to the physical tables underneath them, so teams can trace all the way from raw data to the business logic built on top of it.


Support for dbt Semantic Layer, Cube, and other platforms is coming next.


👉[Learn more](https://docs.elementary-data.com/cloud/features/data-lineage/semantic-layer)


## Jira Sync


Elementary and Jira now work in both directions. When an incident is resolved in Elementary, the linked Jira ticket closes automatically, and vice versa.


👉[Setup guide](https://docs.elementary-data.com/cloud/integrations/alerts/jira#configure-a-jira-webhook-for-two-way-sync)


## Public Saved Views


Saved views can now be set as public, making them visible to everyone on the team. Private views still work the same way.


## Lightdash


Lineage now extends all the way through to your Lightdash dashboards, giving teams end-to-end context from raw data to the metrics your business runs on.


👉[Setup guide](https://docs.elementary-data.com/cloud/integrations/bi/lightdash#lightdash)
