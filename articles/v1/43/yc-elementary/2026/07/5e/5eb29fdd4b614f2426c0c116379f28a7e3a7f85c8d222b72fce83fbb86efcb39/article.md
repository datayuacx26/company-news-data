---
schema_version: "1.0.0"
document_id: "5eb29fdd4b614f2426c0c116379f28a7e3a7f85c8d222b72fce83fbb86efcb39"
company_key: "yc-elementary"
company: "Elementary"
source_id: "yc-elementary-news-import-909d1fa3bc2c"
canonical_url: "https://www.elementary-data.com/post/april-product-announcements"
published_at: null
first_seen_at: "2026-07-25T02:31:24.735806+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:69f7f8aa179b8d58d540d92fa5e8b121f06a59489eda2e1af7e673f3e84f8bc4"
---

# April Product Announcements

This release brigs a unified agent experience, smarter incident management in beta, table usage across more warehouses, and more context and control for agents across the board.


### Unified Agent Experience


The usage of Elementary agents grows every month, and we keep learning from how you work.


What we saw? Switching between agents mid-conversation slows you down and requires you to think about the tool, not the task. This changes today.


One unified agent now orchestrates everything automatically, choosing the right capabilities for each step as your work evolves. Just set your mode, "ask" or "edit", and let it run.


### New Incident Grouping


Related failures can now merged into one incident, reducing alert fatigue and speeding up resolution. Once merged, you can see all failures in a single place, as well as the lineage between them.


Merge the incidents manually, or ask the agent to find and merge related ones for you.


Interested in early access? Reach out to the team.


### Table Usage: More Warehouses (Opt-in)


Table usage stats are now available for Snowflake, Redshift, Databricks, and BigQuery. For every asset, you can see query volume and number of unique users, so you always know what data actually matters to your organization.


Usage data is visible in the catalog, and available as context for the agent.


👉[Learn more,](https://docs.elementary-data.com/cloud/features/table-usage) and reach out to the team for access.


### Cloud Test Improvements


**Agent/MCP Cloud Volume and freshness tests management**


Use the unified agent or the MCP to create, configure, enable/disable, and mute/unmute tests at scale.


**Control training period**


You can now set and edit the training period for cloud volume and freshness monitors, up to three months.


### Data Contract Test


Elementary now supports a data contract cloud test. It validates that a table's schema conforms to a defined contract and detects column additions, removals, type changes, and nullability changes.


👉[Learn more](https://docs.elementary-data.com/cloud/features/data-tests/data-contract-test)


### Fivetran Job Alerts


As another milestone in our Fivetran integration, sync failures now trigger alerts straight to Slack, Teams, or any configured destination.


To enable it, add "job failures" to your alert rules.


👉[Learn more](https://docs.elementary-data.com/cloud/integrations/pipeline/fivetran#fivetran)


### CI: More Control Over PR Settings


Project repo settings are now user-configurable: labels, commit message prefix, branch prefix, and ticket ID requirements.


When required ticket ID is enabled, every PR opened by Elementary must include a ticket ID, prepended to the branch name and title.


### dbt Artifact Tools


dbt artifact tools are now exposed in the MCP and available to agents: query dbt invocations, run results, models, and sources directly.
