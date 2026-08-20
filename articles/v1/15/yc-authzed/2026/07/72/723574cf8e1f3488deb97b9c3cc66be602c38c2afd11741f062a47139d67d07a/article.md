---
schema_version: "1.0.0"
document_id: "723574cf8e1f3488deb97b9c3cc66be602c38c2afd11741f062a47139d67d07a"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-news-import-a541a2e95b08"
canonical_url: "https://authzed.com/changelog/february-2026-release"
published_at: null
first_seen_at: "2026-07-24T17:47:31.723915+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:9cc03569cdda36d0366eb868902bab6d1785d3a8b3628d45e4f48a07204fcb40"
---

# AuthZed February 2026 Release

In this release, we've focused on optimization in SpiceDB, introducing new experimental features to optimize the way you plan and create queries, as well as a new keyword to simplify your schema. We've also added other enhancements to the` zed` CLI, our client libraries, and AuthZed Cloud.


---


### SpiceDB


#### Experimental: Postgres Foreign Data Wrapper


The new` spicedb postgres-fdw` command exposes SpiceDB as a Postgres Foreign Data Wrapper, allowing users to write permission checks as` SELECT` statements and express relationship writes using standard SQL.


This is a work in progress and not yet recommended for production, but if you're interested in querying SpiceDB through your existing Postgres connection, we encourage you to experiment and provide feedback.


[spicedb v1.49.1](https://github.com/authzed/spicedb/releases/tag/v1.49.1)


#### Experimental: Query Planner


A new` --experimental-query-plan` flag enables[SpiceDB's in-progress query planner.](https://authzed.com/blog/introducing-spicedb-query-planner) There's still work to do on statistics sources and optimization before it will provide consistent performance benefits, so we're not recommending it for production yet — but it's available if you want to explore how it works.


#### ` self` Keyword in Permissions


SpiceDB v1.49.1 adds a` self` keyword to schema permissions. If you've ever needed to express "a user can view themselves," you previously had to create a relation and write a relationship from the user back to itself — an extra round-trip to the database and a relationship that needed to stay in sync.


With` self` , you can express this directly in your permission definition. It's less schema to maintain, fewer relationships to write, and no extra database lookup at check time.


[Docs](https://authzed.com/docs/spicedb/concepts/schema#the-self-keyword) ·[spicedb v1.49.1](https://github.com/authzed/spicedb/releases/tag/v1.49.1)


---


### ` zed` CLI


#### Backup Improvements ([v0.34.0](https://github.com/authzed/zed/releases/tag/v0.34.0) )


` zed backup` can now back up SpiceDB instances that don't expose the` ExportBulk` API — including very old versions and Serverless deployments. Several reliability fixes shipped alongside this:


- Relationships with expiration values now back up correctly
- Relationships with caveats now import correctly
- Schema compilation edge cases that previously caused failures are resolved


#### ` use self` Validation ([v0.35.0](https://github.com/authzed/zed/releases/tag/v0.35.0) )


` zed validate` now understands the` use self` schema keyword, so schemas using it will validate correctly.


---


### Python Client ([authzed-py](https://github.com/authzed/authzed-py) )


#### Schema Diffing and Reflection Types Now Exported ([v1.24.1](https://github.com/authzed/authzed-py/releases/tag/v1.24.1) )


` DiffSchema` and` ReflectSchema` are now part of the public API, making it easier to build tooling that inspects or compares SpiceDB schemas from Python.


---


### Go Client ([authzed-go](https://github.com/authzed/authzed-go) )


API usage examples were added to the library, covering common patterns and addressing frequently-asked questions. The library also picked up the new` DATASTORE_NOT_MIGRATED` error code from the upstream API, giving Go applications a clearer signal when SpiceDB hasn't been migrated yet.


---


### Node.js Client ([authzed-node](https://github.com/authzed/authzed-node) )


Package compatibility checking via` publint` was added to the build pipeline. This catches compatibility issues before packages are published to npm, improving reliability for downstream consumers.


---


### AuthZed Cloud


#### Unlimited Metrics Query Range


Metrics queries are no longer capped at a maximum time range. You can now query the full history available, which is useful when investigating longer-term performance trends or capacity planning.


#### SDK Code Examples in the Connect Dialog


The connect dialog now shows ready-to-use code snippets for Python, Go, Node.js, and other languages. When you're setting up a new application, you can copy working connection code directly from the console.


---


### Examples


A[Grafana dashboard example](https://github.com/authzed/examples) was contributed to the examples repo, providing a starting point for teams that want to visualize SpiceDB metrics in Grafana.
