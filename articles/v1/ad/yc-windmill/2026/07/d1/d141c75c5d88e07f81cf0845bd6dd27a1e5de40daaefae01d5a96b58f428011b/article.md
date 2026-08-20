---
schema_version: "1.0.0"
document_id: "d141c75c5d88e07f81cf0845bd6dd27a1e5de40daaefae01d5a96b58f428011b"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ducklake-scheduled-maintenance"
published_at: "2026-07-04T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:7c6115ccd0c87d00925da975fd0d8791d16256d0eab9a1c1277efbe0ffb42e4e"
---

# Scheduled Ducklake maintenance

### [Scheduled Ducklake maintenance](https://www.windmill.dev/changelog/ducklake-scheduled-maintenance)


Data pipelines


[Enterprise](https://www.windmill.dev/pricing)


[v1.748.0](https://github.com/windmill-labs/windmill/releases/tag/v1.748.0)


[Docs](https://www.windmill.dev/docs/core_concepts/persistent_storage/ducklake#scheduled-maintenance)


Ducklakes can now run scheduled maintenance to keep growth in check - snapshot expiry with a configurable retention window (default 7 days), compaction of adjacent small parquet files, and orphaned file cleanup. Configured per lake in the workspace Ducklake settings with a cron cadence (default daily at 03h UTC) and executed as observable jobs whose run history is the audit trail. Enterprise feature.


#### New features


- Per-lake maintenance settings in workspace Ducklake settings - enable toggle, snapshot retention in days (default 7), cron cadence (default daily at 03h UTC with a per-lake minute offset), and individual toggles for compaction and orphaned file cleanup
- Snapshot expiry reclaims files referenced only by snapshots older than the retention window; compaction merges adjacent small parquet files; orphaned file cleanup deletes files no longer referenced by the catalog
- Runs execute as normal jobs on the duckdb tag via a managed schedule at the reserved path f/ducklake_maintenance/<lake>, with run history as audit trail and a one-row summary result (expired snapshots, compacted file groups, cleaned and orphaned files deleted, remaining snapshots)
- Expired snapshots become non-queryable via time-travel (AT (VERSION => n)), so size the retention window to the history you need
- Safety margins - physical file deletion lags expiry by one day so long-running readers are not interrupted, and orphan cleanup never deletes files younger than one day; lake names are validated as \[A-Za-z0-9_-\]+
