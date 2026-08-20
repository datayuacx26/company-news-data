---
schema_version: "1.0.0"
document_id: "02e91312ed38375620dc88d8cd9bb3b723d411709d2a87072fb2f2a9315f14ca"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/scd2-history-materialization"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:1f5a43b9293004d211589280ac97ae80c68d1ff8127f2b51594e1aaf0d576c14"
---

# SCD2 history strategy for DuckLake materialization

### [SCD2 history strategy for DuckLake materialization](https://www.windmill.dev/changelog/scd2-history-materialization)


Data pipelines


[v1.745.0](https://github.com/windmill-labs/windmill/releases/tag/v1.745.0)


[Docs](https://www.windmill.dev/docs/core_concepts/pipelines/materialization#scd2-history-slowly-changing-dimensions)


New managed history strategy on` // materialize` keeps every version of every row (slowly changing dimension type 2). A change closes the prior version and opens a new one, with managed` valid_from` /` valid_to` /` is_current` columns, an auto-maintained` <table>_current` view, and support for effective-dated ASOF joins. Data tests and schema capture keep working, unlike hand-written` materialize manual` SQL.


#### New features


- Add \`key=<col> history\` to \`// materialize ducklake://…\` (or use the \`scd2\` keyword alias) to turn a keyed merge into a type-2 history
- The runtime manages \`valid_from\`, \`valid_to\` and \`is_current\`; your SELECT stays the current snapshot, one row per key
- \`track=<c1,c2,…>\` limits which column changes open a new version (default: all non-key columns)
- \`deletes=close\` closes the current version of keys that disappear from the snapshot; default is soft delete (absent keys stay current)
- A companion \`<table>_current\` view is maintained so the latest-version case needs no filter
- Idempotent by construction: an unchanged snapshot writes 0 rows and advances no DuckLake snapshot
- Built-in \`// data_test\` checks scope to current rows on SCD2 targets, so \`unique(<key>)\` keeps passing as history accumulates
