---
schema_version: "1.0.0"
document_id: "9f867c36c32087be098fbb6fce1a9823fdf4bab909e0a9b56e94a23643a718f5"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/audit-logs-object-storage"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-25T01:07:57.288074+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:afcde8c4329abcd4d01f6222def6fd138eaa9380adfb98802fea8a4e35dce94d"
---

# Export audit logs to object storage

### [Export audit logs to object storage](https://www.windmill.dev/changelog/audit-logs-object-storage)


[Enterprise](https://www.windmill.dev/pricing)


Audit logs


[v1.704.0](https://github.com/windmill-labs/windmill/releases/tag/v1.704.0)


[Docs](https://www.windmill.dev/docs/core_concepts/audit_logs#exporting-audit-logs-to-object-storage)


Continuously export audit logs as newline-delimited JSON to a dedicated logs/audit/ folder in instance object storage, for SIEM forwarding and archival.


#### New features


- Opt-in export of audit logs to S3, Azure Blob or Google Cloud Storage
- Newline-delimited JSON written to a dedicated logs/audit/ folder, partitioned by day
- Incremental and runs in the background off the audit log hot path, with a single exporter under high availability
- Exported files are never deleted from object storage, so database retention can be set much lower
- Recommended setup to forward audit logs to a SIEM for long-term security analysis
