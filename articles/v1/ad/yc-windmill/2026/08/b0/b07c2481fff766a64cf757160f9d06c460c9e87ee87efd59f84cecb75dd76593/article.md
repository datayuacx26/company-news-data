---
schema_version: "1.0.0"
document_id: "b07c2481fff766a64cf757160f9d06c460c9e87ee87efd59f84cecb75dd76593"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/dbt-runtime"
published_at: "2026-08-01T00:00:00+00:00"
first_seen_at: "2026-08-04T18:19:21.078377+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:190423cb9a6ede08a2448c0318250b5ac4789c450ca0adedfb6533f4ff24ed9c"
---

# Run dbt projects as a first-class Windmill runtime

An unmodified dbt project now runs as a Windmill script. Copy the project into a` <script>__dbt/` folder,` wmill sync push` , and Windmill runs` dbt build` on your own workers with live per-model progress, per-model results,` dbt retry` and row previews. Since v1.777.0, dbt is in the new-script language picker and opens in an editor of its own - the project's file tree, its descriptor, the run form and a model graph that` Refresh models` redraws from a` dbt parse` of the files as they are in the editor, labelled with where it came from. Its models, sources, seeds and snapshots become` dbt://warehouse/schema/model` assets with their` ref()` lineage, so a Python or DuckDB script reading a mart appears on the same graph. Warehouses are configured once per workspace and named from the optional` wm_dbt.yaml` descriptor, so the project carries no connection and stays runnable locally. Three engines are selectable (dbt Core 1.x by default, dbt Core 2.x, dbt Fusion), all fetched or built on first use and cached per worker. Everything is in the community edition except the mssql and oracle adapters.
