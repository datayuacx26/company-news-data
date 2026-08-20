---
schema_version: "1.0.0"
document_id: "21bce0d56437c2eafaf13db23ea3dc0b7d37ed62782e451f2abbdf1b81d6fa40"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ecr-increased-replication-rules-limit"
published_at: "2026-08-17T19:01:00+00:00"
first_seen_at: "2026-08-18T01:21:09.373952+00:00"
fetched_at: "2026-08-18T01:21:12.465123+00:00"
content_hash: "sha256:8f169242351c721c315b2c7322b3d37441cce7aaa9c3fc971e84647c430546b3"
---

# Amazon ECR now supports 25 replication rules per registry

Amazon Elastic Container Registry (Amazon ECR) has increased the maximum number of replication rules per registry from 10 to 25.


Previously, customers with complex multi-region or multi-account architectures were constrained to 10 replication rules per registry, requiring them to consolidate replication configurations to work within that constraint. With this update, customers can define up to 25 replication rules per registry, enabling more precise replication strategies for use cases like distributing images across many regions for low-latency pulls, or replicating to multiple production and staging accounts.


This service limit increase is available in all AWS Regions where Amazon ECR is supported. To learn more, visit the[Amazon ECR product page](https://aws.amazon.com/ecr/) and refer to the[Amazon ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/) .
