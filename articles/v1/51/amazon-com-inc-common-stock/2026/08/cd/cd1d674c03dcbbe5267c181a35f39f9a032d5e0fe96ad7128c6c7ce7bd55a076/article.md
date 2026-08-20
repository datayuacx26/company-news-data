---
schema_version: "1.0.0"
document_id: "cd1d674c03dcbbe5267c181a35f39f9a032d5e0fe96ad7128c6c7ce7bd55a076"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-cloudwatch-centralization-tag-propogation/"
published_at: "2026-08-19T21:06:00+00:00"
first_seen_at: "2026-08-20T00:12:43.329206+00:00"
fetched_at: "2026-08-20T00:12:46.759003+00:00"
content_hash: "sha256:a06bb57c1aa4518b4926f470b8a44799fa39185204e4e1dfa701a2ab46849542"
---

# Amazon CloudWatch log Centralization now supports log group tag propagation

Amazon CloudWatch Centralization now copies log group tags from source accounts to the destination log groups created by centralization rules. CloudWatch Centralization aggregates log data from multiple accounts and Regions into one destination account. With tag propagation, the cost, ownership, and compliance tags you maintain at the source now apply to the copied log groups.


With today's launch, CloudWatch copies the tags of each source log group to its destination log group and keeps them in sync based on the tag propogation behaviour selected as part of the centralization rule setup. For example, a platform team can preserve Application and CostCenter tags on centralized log groups, then use those tags to scope access with IAM conditions and report centralized log spend by team in AWS Cost Explorer.


Tag propagation is available in all AWS Regions where CloudWatch Centralization is available. For a list of Regions, see the[AWS Regions table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) .


To get started, turn on tag propagation for a centralization rule in the Amazon CloudWatch console, or by using the AWS CLI or AWS SDKs. To learn more about centralizing logs while preserving their tags, see[Log Centralization User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatchLogs_Centralization.html) . For Centralization pricing, see[Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) .
