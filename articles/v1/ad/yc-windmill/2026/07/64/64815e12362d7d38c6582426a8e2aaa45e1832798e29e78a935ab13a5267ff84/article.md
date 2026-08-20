---
schema_version: "1.0.0"
document_id: "64815e12362d7d38c6582426a8e2aaa45e1832798e29e78a935ab13a5267ff84"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/duckdb-macro-libraries"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:47d3e9161415fc733abb51612daa7be238d46a7534f18a88b732e4d142fa2aaf"
---

# Workspace DuckDB macro libraries

### [Workspace DuckDB macro libraries](https://www.windmill.dev/changelog/duckdb-macro-libraries)


Data pipelines


[v1.746.0](https://github.com/windmill-labs/windmill/releases/tag/v1.746.0)


[Docs](https://www.windmill.dev/docs/core_concepts/pipelines/macros)


Share SQL logic across a workspace with macro libraries. A DuckDB script annotated` // macros` publishes engine-native CREATE MACRO definitions on deploy; any DuckDB script that calls a registered macro gets the definitions injected at run time, with no import or compile step. Includes editor autocomplete, a workspace macros explorer, and library nodes in the pipeline graph.


#### New features


- Annotate a DuckDB script with \`// macros\` to publish its CREATE MACRO statements (scalar or table) workspace-wide on deploy
- Calling a macro just works: definitions and the providing library setup are injected at job time, in dependency order
- Late-bound like dbt packages: redeploying a library applies to the next run of every consumer without redeploying them; a local macro of the same name always wins
- \`// use <lib_path>\` force-injects a whole library for calls hidden in dynamic SQL, honored transitively across libraries
- Deploy-time validation with precise errors: workspace-unique names, no shadowing of DuckDB built-ins, definition order checks
- Discovery: DuckDB editor autocomplete with signatures, a workspace macros explorer drawer on the pipeline page, and library nodes with consumer edges in the graph
