---
schema_version: "1.0.0"
document_id: "58d50b1145a037d5ca06b49393affaf910e052e4d5448c38236cc2e398bb80f0"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-glue-data-catalog-s3-tables/"
published_at: "2026-08-05T15:42:00+00:00"
first_seen_at: "2026-08-06T14:35:10.902896+00:00"
fetched_at: "2026-08-06T14:35:12.413888+00:00"
content_hash: "sha256:09ef5eee70d4fd5aa6a23523ba98493988b73f65ee91f501595f20a41a3cc82e"
---

# AWS Glue Data Catalog now supports metadata exports to S3 Tables (Preview)

AWS Glue Data Catalog is a centralized metadata repository that helps you discover and understand data, with semantic search and support for adding business context to datasets currently in preview. Today, we are adding two new capabilities to this preview: exporting catalog metadata exports to S3 Tables for metadata querying, and expanding the semantic search preview to catalogs encrypted with AWS Key Management Service (AWS KMS) customer managed keys.


When you enable the Glue metadata export, your catalog's technical and business metadata, including glossary terms, custom metadata attachments, and asset descriptions, are written to tables in the managed aws-catalog S3 table bucket. Metadata is stored in Apache Iceberg format, enabling you to query, audit, and perform time travel queries for historical analysis using standard SQL in query engines that support Iceberg, such as AWS analytics services like Amazon Athena and Amazon QuickSight as well as third-party tools. This lets you join catalog metadata with other AWS service data for additional analysis, such as auditing glossary usage and metadata changes. Catalogs encrypted with customer managed keys now support the same business context enrichment and semantic search.


You can use these capabilities in the following AWS Regions: US East (N. Virginia), US East (Ohio), US West (Oregon), and Europe (Ireland). S3 Tables requests and storage are charged based on[S3 pricing](https://aws.amazon.com/s3/pricing/) . To learn more, see the[AWS Glue User Guide](https://docs.aws.amazon.com/glue/latest/dg/catalog-export-s3-tables.html) .
