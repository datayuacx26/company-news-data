---
schema_version: "1.0.0"
document_id: "aa72a77b49f539da64fcceeade726af8704a189ea2e01f63078712f0912dc31b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-s3-tables-variant-iceberg-v3/"
published_at: "2026-07-28T07:00:00+00:00"
first_seen_at: "2026-07-28T17:12:38.617849+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:393f781b034a0bd9de2a1225488e261b86d7cd41053a8152e0fe5a84aa23dbf6"
---

# Amazon S3 Tables now support the Variant data type for Apache Iceberg V3

Amazon S3 Tables now support the Variant data type as defined in the Apache Iceberg Version 3 (V3) specification. You can now write semi-structured data like JSON directly to S3 Tables without defining a fixed schema in advance, allowing you to land data faster while still getting efficient analytical query performance.


With the Variant data type, Apache Iceberg V3-compatible engines shred your semi-structured data into hidden columns as you write it, generating Parquet column statistics that query engines use for optimizations like file pruning, which reduces the data your analytical queries scan. S3 Tables also provide ongoing table maintenance, including compaction, for Variant columns, so you can consolidate data from small files into large files that Iceberg engines can read.


Variant support in S3 Tables is available in the following AWS Regions: US East (N. Virginia), US East (Ohio), US West (Oregon), Asia Pacific (Mumbai), Asia Pacific (Seoul), Asia Pacific (Singapore), Asia Pacific (Sydney), Asia Pacific (Tokyo), Canada (Central), Europe (Frankfurt), Europe (Ireland), Europe (London), Europe (Paris), Europe (Stockholm), and South America (São Paulo).


To learn more, see[Amazon S3 Tables](https://aws.amazon.com/s3/features/tables/) and the[AWS Prescriptive Guidance for working with Iceberg table format specification version 3](https://docs.aws.amazon.com/prescriptive-guidance/latest/apache-iceberg-on-aws/table-spec-v3.html) .
