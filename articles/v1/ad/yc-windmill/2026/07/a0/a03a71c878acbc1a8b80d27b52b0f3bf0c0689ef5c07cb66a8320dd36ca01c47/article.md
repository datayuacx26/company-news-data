---
schema_version: "1.0.0"
document_id: "a03a71c878acbc1a8b80d27b52b0f3bf0c0689ef5c07cb66a8320dd36ca01c47"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/partition-backfill"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:54b6e48b4fe87cd972c88af6a1beb0cd6f4043a8d47540ef6ec0161861494594"
---

# Backfill a range of partitions from the asset drawer

### [Backfill a range of partitions from the asset drawer](https://www.windmill.dev/changelog/partition-backfill)


Data pipelines


[Enterprise](https://www.windmill.dev/pricing)


[v1.746.0](https://github.com/windmill-labs/windmill/releases/tag/v1.746.0)


[Docs](https://www.windmill.dev/docs/core_concepts/pipelines/materialization#partition-status-and-backfill)


On a partitioned DuckLake asset, the Backfill button opens a range picker that previews which partitions are missing, failed or materialized, then re-runs the producing script once per partition with an explicit partition argument. Runs are sequential and idempotent, a failed partition does not stop the rest, and progress streams in the dialog and drawer header. Range backfill is an Enterprise feature; single-partition runs stay available in all editions.


#### New features


- Backfill button on the partition-status grid of a materialized ducklake:// asset opens a from/to range picker
- The preview classifies every partition in range as missing, failed or materialized; a toggle (default on) restricts the run to missing and failed partitions
- One deployed run per partition with an explicit partition argument, sequential to avoid catalog commit contention; each run is idempotent
- A failed partition does not stop the rest; progress streams in the dialog and, when closed, in the drawer header while the grid refreshes per slice
- Cancel stops after the in-flight partition and cancels its job
- Headless alternative: wmill pipeline run <folder> --partition <value>
