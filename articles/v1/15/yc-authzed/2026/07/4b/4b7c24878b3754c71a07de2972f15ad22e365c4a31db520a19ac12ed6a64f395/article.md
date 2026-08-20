---
schema_version: "1.0.0"
document_id: "4b7c24878b3754c71a07de2972f15ad22e365c4a31db520a19ac12ed6a64f395"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-news-import-a541a2e95b08"
canonical_url: "https://authzed.com/changelog/november-2025-release"
published_at: null
first_seen_at: "2026-07-24T17:47:31.723915+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:5ded5f40acf45e581dc7358a0b3736db5d1f03448b93d666544a837083a75a4d"
---

# AuthZed November 2025 Release

This release includes schema enhancements for SpiceDB, new Materialize API capabilities in the Python client, and dashboard improvements for AuthZed Cloud.


### [SpiceDB v1.46.2](https://github.com/authzed/spicedb/releases/tag/v1.46.2)


#### Schema V2 Library Enhancements


The Schema V2 library now includes:


- Configurable flattening options for permission schemas
- Enhanced support for arrow operations (` ->` ) including conditional traversals (` .any()` and` .all()` )
- New utility functions for schema traversal and format conversion


These additions support developers building schema analysis and transformation tools.


### [authzed-py v1.24.0](https://github.com/authzed/authzed-py/releases/tag/v1.24.0)


#### Materialize API Support


This release adds support for the Materialize API, which helps maintain a real-time copy of permissions data in your own database systems.


When displaying lists or tables of accessible resources, checking permissions individually can be slow. The Materialize API addresses this by synchronizing permission relationships to your local systems.


The implementation includes:


- Initial data loading with efficient backfill capabilities
- Real-time synchronization as permissions change
- Support for both synchronous and asynchronous Python code with automatic runtime detection
- Local development client for testing without secure connections
- Cursor-based pagination for large datasets
- Quickstart guide with examples


### AuthZed Cloud


#### Dashboard Enhancements


- Performance charts now available in the dashboard for datastore monitoring
- Version management includes direct links to documentation for each release


#### Bug Fixes


Various visual and functional improvements.
