---
schema_version: "1.0.0"
document_id: "df1f30bdd951d8c4c43cea01174186ff94febaa54b27c849959e22122d73b7d6"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-cloudwatch-intelligent-tiering/"
published_at: "2026-07-15T20:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:40b3d8ade8f26ab8ad52683f87b779a08b94c6884c9c18c846cc8d009162f15f"
---

# Amazon CloudWatch Logs announces intelligent tiering for storage

Amazon CloudWatch Logs now supports intelligent storage tiering, which automatically classifies your log data across three storage tiers - Standard (existing), Infrequent Access, and Archive Instant Access based on access patterns. This allows you to store logs in Amazon CloudWatch for extended periods at lower-cost tiers without any operational overhead.


With today's launch, customers can now retain high-volume verbose logs needed to be stored for longer periods at a lower cost in Amazon CloudWatch. Instead of filtering these logs or exporting them, you can now keep them natively in Amazon CloudWatch and benefit from the same query experience regardless of which tier your data resides in. Amazon CloudWatch monitors access patterns and automatically reclassifies data not accessed for 30 days to the Infrequent Access tier, and data not accessed for 90 days to the Archive Instant Access tier. When you access older data, it is automatically promoted back to the Standard tier for 30 days. By consolidating all your logs in CloudWatch, you get full visibility in one tool, thereby eliminating the operational overhead of managing multiple storage solutions and reducing your Mean Time to Resolution (MTTR) by analyzing, and alerting on all your logs in a single place.


Amazon CloudWatch Logs Intelligent-Tiering is available in all AWS commercial regions except Middle East (Bahrain) and Middle East (UAE). You can enable intelligent tiering at the account level in the AWS Management Console, AWS SDKs or through AWS CLI. Learn more about CloudWatch Logs intelligent tiering[pricing](https://aws.amazon.com/cloudwatch/pricing/) and[documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cwl_intelligent_tier.html) .
