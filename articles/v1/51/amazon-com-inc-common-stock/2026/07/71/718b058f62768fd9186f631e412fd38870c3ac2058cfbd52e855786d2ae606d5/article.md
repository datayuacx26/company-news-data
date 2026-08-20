---
schema_version: "1.0.0"
document_id: "718b058f62768fd9186f631e412fd38870c3ac2058cfbd52e855786d2ae606d5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-msk-express-version-42/"
published_at: "2026-07-15T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:fca592ee30363f19fa9a59405379e3a33a0f428a89587df9522715d08b24786a"
---

# Amazon MSK Express Brokers adds support for Apache Kafka version 4.2

Amazon Managed Streaming for Apache Kafka (Amazon MSK) Express Brokers now supports Apache Kafka version 4.2. This release includes Eligible Leader Replicas (ELR) enhancements that strengthen availability with improved leader election correctness. It also introduces a new consumer rebalance protocol that helps ensure smoother and faster group rebalances, and a new Streams Rebalance Protocol that extends broker coordination capabilities to Kafka Streams for optimized task assignments. For a complete list of improvements and bug fixes, please refer to the[Apache Kafka release notes for version 4.2](https://archive.apache.org/dist/kafka/4.2.0/RELEASE_NOTES.html) .


MSK Express Brokers are designed to deliver up to three times more throughput per broker, scale up to 20 times faster, and reduce recovery time by 90 percent. This launch brings the latest open-source reliability and performance improvements to MSK Express.


To get started, simply select version 4.2.x when creating a new cluster with Express Brokers via the AWS Management Console, AWS CLI, or AWS SDKs. You can also upgrade existing MSK Express Brokers with an in-place rolling update. Amazon MSK orchestrates broker restarts to maintain availability and protect your data during the upgrade. Kafka version 4.2 support is available today across all[AWS regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) where Amazon MSK Express Brokers is offered. To learn how to get started, see the[Amazon MSK Developer Guide](https://docs.aws.amazon.com/msk/latest/developerguide/getting-started.html) .
