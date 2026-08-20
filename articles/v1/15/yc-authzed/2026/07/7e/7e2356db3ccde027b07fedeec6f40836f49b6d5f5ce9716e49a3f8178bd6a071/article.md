---
schema_version: "1.0.0"
document_id: "7e2356db3ccde027b07fedeec6f40836f49b6d5f5ce9716e49a3f8178bd6a071"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-news-import-a541a2e95b08"
canonical_url: "https://authzed.com/changelog/march-2026-release"
published_at: null
first_seen_at: "2026-07-24T17:47:31.723915+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:c86060142ede6e318c687a9f7eeb65c9a4206d91fa6d927514f184560c750cf9"
---

# AuthZed March 2026 Release

In this release, we've made significant enhancements to SpiceDB's schema management, moved closer to general availability of the Query Planner, and made improvements to our Python and Go clients.


---


### SpiceDB


#### Composable Schemas ([v1.51.0](https://github.com/authzed/spicedb/releases/tag/v1.51.0) )


Large permission schemas can now be split across multiple files. The new` use import` and` use partial` keywords let you define pieces of your schema in separate files and compose them together — the same way you'd organize any other large codebase.


This affects the full development workflow: SpiceDB, the VS Code extension, and the` zed` CLI all support composable schemas as of this release cycle.


Note: if you're using composable schemas, the root file must declare` use import` before any` import` statements, and` use partial` before any` partial` declarations.


[zed v0.36.1](https://github.com/authzed/zed/releases/tag/v0.36.1) ·[spicedb v1.51.1](https://github.com/authzed/spicedb/releases/tag/v1.51.1)


#### Experimental Query Planner: Significant Progress ([v1.49.2](https://github.com/authzed/spicedb/releases/tag/v1.49.2) ,[v1.50.0](https://github.com/authzed/spicedb/releases/tag/v1.50.0) )


The experimental query planner (enabled via` --experimental-query-plan` ) received substantial work over this period. In v1.49.2, it gained recursive direction strategies and a canonicalization framework for query plan outlines. In v1.50.0, it expanded to cover` LookupResources` and` LookupSubjects` endpoints, added in-memory statistics for informing optimizations, and can now prune branches during Check requests that cannot possibly lead to the expected subject type — reducing unnecessary work.


This is still experimental and not recommended for production, but it's meaningfully closer to general availability.


#### CockroachDB 26.1 Support ([v1.49.2](https://github.com/authzed/spicedb/releases/tag/v1.49.2) )


SpiceDB now correctly parses version information from CockroachDB 26.1 clusters. If you're planning to upgrade your CRDB cluster to 26.1, SpiceDB v1.49.2 or later is required.


---


### ` zed` CLI


#### Composable Schema Validation ([v0.36.1](https://github.com/authzed/zed/releases/tag/v0.36.1) )


` zed validate` now works with composable (multi-file) schemas. If your schema uses` import` or` partial` ,` zed validate` can now validate it correctly — including resolving imports and checking the composed result.


This requires declaring` use import` at the top of your root schema file before any` import` statements, and` use partial` before any` partial` declarations.


---


### Go Client ([authzed-go](https://github.com/authzed/authzed-go) )


#### Timestamp in DownloadPermissionSetsResponse ([v1.8.0](https://github.com/authzed/authzed-go/releases/tag/v1.8.0) )


` DownloadPermissionSetsResponse` now includes a timestamp field, making it easier to reason about the freshness of permission set snapshots in applications that use this API.


---


### Python Client ([authzed-py](https://github.com/authzed/authzed-py) )


` protovalidate` was moved from a required dependency to an optional/dev dependency, reducing installation footprint for users who don't need schema validation tooling.


---


### Examples


A new example was added showing how to build an agentic Retrieval-Augmented Generation (RAG) system with authorization enforced by SpiceDB. This covers a practical pattern for teams building AI applications that need to ensure users only retrieve documents they're permitted to see.


[authzed/examples](https://github.com/authzed/examples)
