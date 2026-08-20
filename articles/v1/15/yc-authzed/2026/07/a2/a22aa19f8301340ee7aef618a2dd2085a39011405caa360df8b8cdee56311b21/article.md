---
schema_version: "1.0.0"
document_id: "a22aa19f8301340ee7aef618a2dd2085a39011405caa360df8b8cdee56311b21"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-news-import-a541a2e95b08"
canonical_url: "https://authzed.com/changelog/december-2025-release"
published_at: null
first_seen_at: "2026-07-24T17:47:31.723915+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:e4cfa97e807d3976318dc9848cd67deacbe21f4431a4ea67bba43ad61c8597f9"
---

# AuthZed December 2025 Release

This release brings new documentation resources, expanded cloud provider support for Materialize, open source tooling for schema development, and reliability improvements to SpiceDB.


### SpiceDB v1.48.0: Memory Protection


[SpiceDB v1.48.0](https://github.com/authzed/spicedb/releases/tag/v1.48.0) introduces new safeguards that prevent the system from consuming excessive memory during operation. This means more stable and predictable performance, even under heavy load.


The Memory Protection Middleware is enabled by default. If your server's memory usage gets too high, incoming requests will be rejected with a` ResourceExhausted` code (HTTP 429) rather than risking system instability. You can disable this behavior with` --enable-memory-protection-middleware=false` if needed.


### Expiring Relationships: Now Generally Available


Expiring Relationships, which allows you to set automatic expiration times on relationships, is now generally available. This feature is useful for implementing time-limited access, temporary permissions, or session-based authorization.


[Learn more about Expiring Relationships](https://authzed.com/docs/spicedb/concepts/expiring-relationships)


### New Documentation: Understanding SpiceDB's APIs


One of the most common questions we see on Discord is about SpiceDB's various APIs and when to use each one. We've published a new guide that breaks down the available APIs and helps you choose the right approach for your use case.


[Querying Data in SpiceDB](https://authzed.com/docs/spicedb/concepts/querying-data)


### Materialize: Available on Azure


Materialize, our product for pre-computing permission results, is now supported on Azure. Materialize remains in early access as development continues to broaden support across more scenarios.


### Open Source: SpiceDB Parser for JavaScript


We've released[spicedb-parser-js](https://github.com/authzed/spicedb-parser-js) , a new open source project that houses the schema parser logic shared across tools like the Playground and VS Code extension.


This foundation will enable support for composable schemas in VS Code and makes it easier for the community to build tooling around SpiceDB schemas.


### AuthZed Cloud


This release includes numerous bug fixes to improve the overall stability and reliability of AuthZed Cloud.


### Technical Guide


[Build a Multi-Tenant RAG with Fine-Grain Authorization using Motia and SpiceDB](https://authzed.com/blog/building-a-multi-tenant-rag-with-fine-grain-authorization-using-motia-and-spicedb)


Learn how to combine retrieval-augmented generation with fine-grained permissions to build AI applications that respect your authorization model.
