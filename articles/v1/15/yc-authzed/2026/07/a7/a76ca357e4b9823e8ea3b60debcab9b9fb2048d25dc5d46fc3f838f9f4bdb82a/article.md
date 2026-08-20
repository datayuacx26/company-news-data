---
schema_version: "1.0.0"
document_id: "a76ca357e4b9823e8ea3b60debcab9b9fb2048d25dc5d46fc3f838f9f4bdb82a"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-news-import-a541a2e95b08"
canonical_url: "https://authzed.com/changelog/april-2026-release"
published_at: null
first_seen_at: "2026-07-24T17:47:31.723915+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:97a936df143c5de8b63c2d77ff4c3784d62ede7a3f43892c9a685a2c8a224baa"
---

# AuthZed April 2026 Release

This release cycle brings two SpiceDB releases, plus the first stable` zed` v1.0.0, meaningful query planner progress, and improvements to how AuthZed Cloud surfaces errors during permission system setup.


---


### SpiceDB


#### YAML Validation File Support ([v1.52.0](https://github.com/authzed/spicedb/releases/tag/v1.52.0) )


SpiceDB's development toolchain now supports YAML-based validation files end-to-end. The DevContext server, the VS Code language server, and the` zed` CLI all understand the format. If your team has been maintaining validation files in YAML, the full workflow now works without workarounds.


#### Query Planner: Continued Progress ([v1.52.0](https://github.com/authzed/spicedb/releases/tag/v1.52.0) )


The experimental query planner (` --experimental-query-plan` ) received two significant additions this cycle:


- **Statistics-based optimizations** let the planner make smarter choices about execution order based on actual data distribution.
- **gRPC Dispatch API** enables query plans to be passed across SpiceDB instances, laying groundwork for distributed query execution.


Still experimental and not recommended for production, but the gap to general availability continues to close.


#### Cycle Detection for LookupResources ([v1.52.0](https://github.com/authzed/spicedb/releases/tag/v1.52.0) )


A new` withDebug` flag on LookupResources calls surfaces information about cycles in your relationship graph. If LookupResources is returning unexpected results or timing out, this flag makes it much easier to understand why.


#### Postgres Foreign Data Wrapper Cursor Support ([v1.52.0](https://github.com/authzed/spicedb/releases/tag/v1.52.0) )


The Postgres Foreign Data Wrapper (FDW) datastore now fully supports cursoring for LookupResources, LookupSubjects, and ReadRelationships. These were previously missing, which prevented proper pagination when using FDW.


#### Reliability and Resource Improvements ([v1.52.0](https://github.com/authzed/spicedb/releases/tag/v1.52.0) )


- Watch API memory consumption is reduced.
- Memory sizing for cache and watch buffers now respects container cgroup limits rather than host memory — important for containerized deployments with memory caps.
- A caveat context cache key collision affecting nested list values is fixed.
- Postgres read replicas no longer silently swallow "revision not found" errors.
- TLS configuration now correctly rejects setups where only one of cert/key is provided.


---


### ` zed` CLI


#### v1.0.0 — Official Stable Release ([v1.0.0](https://github.com/authzed/zed/releases/tag/v1.0.0) )


The first major version of the` zed` CLI has now been released ([view livestream](https://www.youtube.com/watch?v=yBEciwiScSs) ).


**What changed:**


- ` zed preview schema compile` →` zed schema compile`
- ` zed watch` →` zed relationship watch`
- ` zed permission lookup` →` zed permission lookup-resources`
- The` revision` flag is removed — use the consistency flags
- The` estimated-count` flag on bulk-delete is removed — use` limit`


Several bug fixes shipped alongside: YAML error line numbers,` import` behavior with` schema=false` , hostname-override flag persistence, and clearer error messages for` .zed` files that contain YAML validation content.


#### Limit and Debug Flags ([v1.1.0](https://github.com/authzed/zed/releases/tag/v1.1.0) /[v1.1.1](https://github.com/authzed/zed/releases/tag/v1.1.1) )


Two new flags make everyday use more practical:


- ` zed relationships read --limit <n>` caps how many relationships are returned. Useful when you just need to spot-check data rather than paginate through everything.
- ` zed permission lookup-resources --debug` exposes cycle information from SpiceDB, making it possible to diagnose data model cycles directly from the CLI.


---


### Go Client ([authzed-go v1.9.0](https://github.com/authzed/authzed-go/releases/tag/v1.9.0) )


#### Affected Permissions on Schema Changes


When a breaking schema change is detected, the Go client now includes which permissions are affected. This gives developers more specific information to act on rather than just knowing that a breaking change occurred.


#### LookupResources Debug Access


The client now exposes the` withDebug` flag introduced in SpiceDB v1.52.0, so Go applications can request cycle detection output directly.


---


### AuthZed Cloud


#### Clearer Provisioning Errors


When a permission system fails to provision due to a configuration or validation issue, the specific error message is now displayed in the console. Previously, a failed provisioning state showed no detail about the underlying cause. The improvement means less time spent opening a support ticket to find out what went wrong.
