---
schema_version: "1.0.0"
document_id: "2b097bc53e600c7bb86a602563f184cd9435aef8179b3dd49fa051886591ff91"
company_key: "yc-helixdb"
company: "HelixDB"
source_id: "yc-helixdb-rss-35492c1029c4"
canonical_url: "https://docs.helix-db.com/database/helix-db/start-here/release-notes"
published_at: "2026-08-06T16:31:21+00:00"
first_seen_at: "2026-08-09T23:01:51.910754+00:00"
fetched_at: "2026-08-09T23:01:54.090581+00:00"
content_hash: "sha256:4ce27070f323871c7d6ff47985f99c39529e54308d50bd57ee812ef8ec4eb7f3"
---

# HelixDB Enterprise v1.0.0

Reference


> For the complete documentation index optimized for AI agents, see[llms.txt](https://docs.helix-db.com/llms.txt) .


​


- **Open-source database** — the engine moved out of` helix-hyperscale` into the public, modular` helix-db` workspace.
- **New query planner** — a dedicated logical and physical planning layer optimizes the typed operation-tree AST before execution.
- **[Embedded database](https://docs.helix-db.com/database/helix-db/start-here/local-development/embedded-database)** — run the same engine and queries in process with memory, disk, or object storage.
- **[Vector prefiltering](https://docs.helix-db.com/database/helix-db/query-guides/filtering#vector-prefiltering)** — traverse and filter an exact candidate set before vector ranking, so results cannot escape graph or permission boundaries.


​


- **[Row bindings](https://docs.helix-db.com/database/helix-db/query-guides/advanced)** —` bind` +` projectBindings` /` projectDistinctBindings` correlate values captured at different hops of a single traversal.
- **Python SDK** — new zero-dependency synchronous SDK (` helix-db` , imported as` helixdb` ).
- **CLI 3.0.6** —` --path` flag on` helix add` .


​


- **Go SDK**
- **Better cache metrics for better query insights**
- **Query plan improvements**
- **` helix chef` command for local bootstrap**
- **CLI DX improvements**


​


- **Local docker deployment**
- **Query plan improvements**
- **Query insights, metrics and suggestions**
- **Stability and reliability improvements**
- **Better memory usage and caching**


​


## ​


Launch 🚀


- Graph database with vector search and full-text search
- Fully ACID
- Queries via Rust DSL
- Backed by object storage
- Multi-tenancy included by default
