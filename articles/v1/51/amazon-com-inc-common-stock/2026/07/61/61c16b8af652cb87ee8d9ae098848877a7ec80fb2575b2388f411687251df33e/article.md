---
schema_version: "1.0.0"
document_id: "61c16b8af652cb87ee8d9ae098848877a7ec80fb2575b2388f411687251df33e"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-ecs-action-logs/"
published_at: "2026-07-21T15:30:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:f3e09402acb99d16caa4663b962b32a24ae0101b6fb452b9d2d1d53ac7437117"
---

# Amazon ECS now provides Action Logs for deployment and orchestration visibility

Today,[Amazon Elastic Container Service](https://aws.amazon.com/ecs/) (Amazon ECS) introduces Action Logs, a new observability feature that delivers detailed, timestamped records of the actions Amazon ECS performs on behalf of customers during service deployments and ECS Managed Daemon updates. By surfacing service-side operations that were previously invisible, Action Logs help you monitor and troubleshoot your workloads directly, without contacting AWS Support or manually correlating data from multiple sources.


With Action Logs, you gain visibility into key deployment state transitions of service deployments, Managed Daemon updates. Each log entry includes the event name, log level(INFO, WARN, OR ERROR), relevant resource ARNs, and a status reason, helping you reduce mean time to resolution when issues arise. You can opt in at the cluster level through the Amazon ECS console or by using Amazon CloudWatch vended logs APIs, and choose to deliver logs to Amazon CloudWatch Logs, Amazon S3, or Amazon Kinesis Data Firehose depending on your operational needs. At launch, Amazon Q in the Amazon ECS console integrates with Action Logs to automatically detect deployment issues such as circuit breaker rollbacks and unstable service revisions, providing customers with root cause analysis, resource-level comparisons, and step-by-step remediation guidance without leaving the console. Standard CloudWatch Logs, Amazon S3, or Amazon Data Firehose pricing applies for log ingestion and storage. For pricing details, see[Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/) .


Amazon ECS Action Logs are available in all AWS Regions, including the AWS GovCloud (US)[Regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) . To learn more, refer[Monitor Amazon ECS operations with Action Logs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/action-logs.html) in[Amazon ECS Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/) .
