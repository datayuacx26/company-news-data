---
schema_version: "1.0.0"
document_id: "4f9011619c7c7fec6a85623a33b0f11fceda0436b6d77030752babb5e33ed2f6"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/pipeline-schema-contracts"
published_at: "2026-07-05T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:8f3a6c224711ea505bb7cfd0b41a1e56ce34fa4a8e79798301bf0b580a558f69"
---

# Schema contracts for pipeline consumers

### [Schema contracts for pipeline consumers](https://www.windmill.dev/changelog/pipeline-schema-contracts)


Data pipelines


[v1.748.0](https://github.com/windmill-labs/windmill/releases/tag/v1.748.0)


[Docs](https://www.windmill.dev/docs/core_concepts/pipelines/materialization#schema-contracts)


Consumer scripts referencing materialized DuckLake tables are now validated against the captured producer schemas at save time. Missing columns, dropped lineage sources and mismatched relationship types surface as warnings (never errors) after deploy, as live editor squiggles, and column names autocomplete with their types when typing` .` after a` ducklake://` reference on annotation lines. Producers can declare a deliberately unstable schema with` on_schema_change=ignore` on the` // materialize` line to collapse downstream warnings into a single note.


#### New features


- Save-time check: deploying a consumer script diffs its \`ducklake://\` references (body column reads, \`// column\` lineage sources, \`// data_test relationships\` refs) against the latest captured schema and toasts warnings
- Warnings never block a save or deploy: a deliberate upstream reshape does not fail every consumer
- Live editor squiggles anchor the same warnings to the offending column read or annotation line as you type
- Typing \`.\` after a \`ducklake://…\` reference on an annotation line autocompletes column names with their captured types
- Column names compare case-insensitively, \`{partition}\` tokens are stripped, \`_wm_partition\` is always accepted, and a \`<table>_current\` view checks against its SCD2 base table
- \`// materialize … on_schema_change=ignore\` marks a producer schema as deliberately unstable and collapses downstream warnings into one informational note
- Each warning cites the schema version and capture time it was checked against
