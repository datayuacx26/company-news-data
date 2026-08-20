---
schema_version: "1.0.0"
document_id: "54b5902acd5737491697f8ba9cbda5a44f7386c0b03e9b533f94852cf38cbe41"
company_key: "yc-iomete"
company: "IOMETE"
source_id: "yc-iomete-news-import-000d9716a3eb"
canonical_url: "https://iomete.com/resources/blog/iceberg-maintenance-operations"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-22T00:31:44.448316+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:ac65bc537fdc199d40bd2c9847f7c866970f539701de3e7a3223c92bcab7de7d"
---

# Apache Iceberg Table Maintenance: What Iceberg Ships and What It Doesn't

The default value of` Integer.MAX_VALUE` means compaction does not trigger based on delete-file accumulation alone. Out of the box, rewrites are driven almost entirely by file size thresholds.


On high-write Merge-on-Read (MoR) tables, this can become a major operational issue. Delete files continue accumulating while reads gradually become more expensive, often without obvious visibility until query latency noticeably degrades.
