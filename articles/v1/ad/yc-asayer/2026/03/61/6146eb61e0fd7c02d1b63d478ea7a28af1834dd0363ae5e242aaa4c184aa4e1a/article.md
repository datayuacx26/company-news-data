---
schema_version: "1.0.0"
document_id: "6146eb61e0fd7c02d1b63d478ea7a28af1834dd0363ae5e242aaa4c184aa4e1a"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/clickhouse-cleanup-query/559"
published_at: "2026-03-07T00:03:53+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:f4885c5ddbc6e22de221e87fa24a5975bae26edfe0e25b966771e7f445ba6064"
---

# Clickhouse cleanup query

[juangh15](https://forum.openreplay.com/u/juangh15)


March 7, 2026, 12:03am 1


Hi, we have an installation of Openreplay on AWS, ubuntu, single instance, version v1.25.0.


We have all the componentes running with no issues but we started noticing the Clickhouse storage is growing too fast, but as the cleanup command only runs in postgres and s3/minio, the size of Clickhouse storage is not reduced after running the cleanup.


Could you please provide a query to perform cleanup of data on Clickhouse?


I can join the clickhouse pod with a clickhouse client if needed to run the query.


Thanks in advance!


[mccrackend](https://forum.openreplay.com/u/mccrackend)


March 23, 2026, 2:00pm 2


I have the same question and self hosting setup, except running in GCP. I’m trying to find any documentation about how we’re to manage clickhouse data but not seeing it mentioned in the docs anywhere yet.
