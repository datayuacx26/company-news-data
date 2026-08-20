---
schema_version: "1.0.0"
document_id: "8886270ab8274113899911dd28f790b25621e49cb6c2c6e22ba97cb00f1c7bee"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-opensearch-serverless-supports-10000-collections-per-collection-group/"
published_at: "2026-08-10T15:51:00+00:00"
first_seen_at: "2026-08-10T21:25:41.621452+00:00"
fetched_at: "2026-08-10T21:25:43.316318+00:00"
content_hash: "sha256:e3db28f57d4c6e286721e9bec7a8a4659f784e7ad4fd14b4346e04aef50f7a05"
---

# Amazon OpenSearch Serverless now supports up to 10,000 collections per collection group

The next generation of Amazon OpenSearch Serverless now supports up to 10,000 collections within a single collection group, increased from the previous limit of 1,500. Collection groups organize multiple collections and enable them to share OpenSearch Compute Units (OCUs), even when the collections are encrypted with different AWS KMS keys. With this higher limit, you can consolidate significantly more collections into a single collection group and manage them under a shared set of capacity limits.


Customers use collection groups to reduce costs by sharing compute across many collections rather than provisioning separate OCUs for each KMS key, while still maintaining collection-level security and access controls. As customer workloads have grown, particularly for multi-tenant applications that provision a collection per tenant, the previous limit of 1,500 collections per group constrained how many tenants could benefit from a shared compute pool. Raising the limit to 10,000 collections on the next generation of Amazon OpenSearch Serverless lets you scale these workloads further, improve compute utilization, and lower per-collection cost, without creating and operating additional collection groups. The higher limit applies automatically to new and existing nextgen collection groups.


The increased limit is available on the next generation of Amazon OpenSearch Serverless in all AWS Regions where it is[available](https://docs.aws.amazon.com/general/latest/gr/opensearch-service.html) . To learn more, see Amazon OpenSearch Serverless[technical documentation](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless-create.html#serverless-create-nextgen-easy) and[quotas](https://docs.aws.amazon.com/general/latest/gr/opensearch-service.html#opensearch-limits-serverless) .
