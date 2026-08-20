---
schema_version: "1.0.0"
document_id: "4176a47ff7546dc20472763de7662d0bea367af7639268050ba1f4722348aa76"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-msk-express-brokers-delivers-to-amazon-s3"
published_at: "2026-07-30T14:07:00+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:7bbae8de5a1d0f6d8c832b0aae9d040d6351050bce301f790e0e9994f9b385fd"
---

# Amazon MSK Express brokers now delivers Apache Kafka data to Amazon S3

[Amazon MSK Express brokers](https://aws.amazon.com/msk/features/express-brokers-for-amazon-msk/) now delivers data to Amazon S3 general purpose buckets, providing a fully managed capability to deliver Apache Kafka data in Amazon S3 for downstream processing in the easiest and most reliable way. This capability automatically scales to deliver high-throughput Kafka data to S3 with end-to-end reliability for mission-critical workloads, while reducing ingestion and delivery costs by up to 60% compared to self-managed alternatives.


Customers deliver Apache Kafka data to Amazon S3 for use cases such as log archival, compliance retention, Kafka replay, and training AI/ML models, and typically build these pipelines with self-managed connectors that grow costly and operationally complex as workloads scale, forcing teams to build or source S3 connector plugins, secure approvals to deploy them, and continually scale capacity, and apply security updates across connector fleet. With this capability, MSK Express automatically handles scaling, retries, and backpressure so customers no longer manage connector fleets or coordinate across teams. MSK Express supports throughput of up to 10 GB/s for data delivery to Amazon S3, and manages routine operations such as capacity scaling and version upgrades without introducing delivery gaps. Additionally, customers add this delivery capability without provisioning additional broker egress throughput, which eliminates the incremental infrastructure costs that scaling connector-based pipelines typically incurs, so customers scale delivery to actual workload demand rather than provisioning for peak, achieving reliable, high-throughput delivery to Amazon S3 while removing operational overhead and lowering costs.


Amazon MSK data delivery to Amazon S3 is available today in every[AWS Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) where Amazon MSK Express brokers are offered. For pricing information, visit the[pricing page](https://aws.amazon.com/msk/pricing/) . To learn more, visit the[Amazon MSK Developer Guide](https://docs.aws.amazon.com/msk/latest/developerguide/msk-data-delivery.html) and[Amazon MSK AI skills](https://github.com/aws/agent-toolkit-for-aws/tree/main/skills/specialized-skills/analytics-skills/managing-amazon-msk) .
