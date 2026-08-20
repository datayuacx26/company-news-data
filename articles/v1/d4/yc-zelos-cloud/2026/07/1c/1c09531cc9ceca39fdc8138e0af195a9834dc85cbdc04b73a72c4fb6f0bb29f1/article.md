---
schema_version: "1.0.0"
document_id: "1c09531cc9ceca39fdc8138e0af195a9834dc85cbdc04b73a72c4fb6f0bb29f1"
company_key: "yc-zelos-cloud"
company: "Zelos Cloud"
source_id: "yc-zelos-cloud-news-import-c65d5f69242c"
canonical_url: "https://docs.zeloscloud.io/26.0.4/reference/release-notes/sdk/"
published_at: null
first_seen_at: "2026-07-27T06:12:43.410034+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:8cc10d8e8968946e1e33d30d5ed5557c9305ba6e40443e538e4f1fcb93b85731"
---

# SDK - Zelos Documentation

# SDK


0.0.9


### Added¶


- ` FolderPickerWidget` for Actions, enabling folder selection in extension UIs
- ` pyarrow` as a direct dependency — no longer needs to be installed separately for TraceReader


### Fixed¶


- NaN and Infinity float values are now properly stored as null and read back as` None` instead of causing serialization errors


0.0.8


### Added¶


- **TraceReader API:** Read` .trz` trace files for offline analysis, debugging, and post-processing.[Read more](https://docs.zeloscloud.io/sdk/how-to/read-files/) .


- Query specific signals and time ranges from trace files
- Discover available fields hierarchically (sources → events → fields)
- List data segments and metadata
- Full PyArrow integration for efficient data processing


- **Trace File Utilities:**


- Hook to open existing trace files (useful for merging WAL and TRZ after ungraceful exit)


0.0.7


### Added¶


- Added value-table support to` TraceSourceCacheLast`
- Additional widgets and validation hooks for Actions


### Fixed¶


- Addressed lingering lint warnings across pytest helpers and ensured artifact directories are configurable in automated runs.


0.0.6


### Added¶


- Action decorators now capture a` field_type` , enabling richer, type-aware rendering in the Zelos App without custom widgets.
- Action parameters default to` required=True` , surfacing missing inputs during validation instead of at execution time.


### Improved¶


- Updated examples and tests to exercise the stricter defaults so extension authors can adopt the new metadata with confidence.


0.0.5


### Added¶


- **Zelos Actions:** A powerful new way to script interactions with your devices and services.


- Define actions with simple Python functions and decorators.
- Automatic discovery and registration of actions.
- Support for various field types for action inputs, including` object` .
- Note: Only available in the python release of the SDK


### Changed¶


- **Dependencies:** Removed the` setuptools` dependency.


### Fixed¶


- **Zelos Trace:**


- Resolved a deadlock that could occur in async contexts.
- Fixed an issue that could prevent querying data if a` TraceSegmentStart` message was missed.
- Fixed URL handling in the` TracePublishClient` .
