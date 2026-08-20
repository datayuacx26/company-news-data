---
schema_version: "1.0.0"
document_id: "6746ee96149a79e6643d2a56292530b643624425c12769a0a6ff736cb1640c5a"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-cloudwatch-logs/"
published_at: "2026-07-23T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:c8b1a9a13e27e5d205ae83c502316fc7c7d6c8874a59d6e9de69128dc110b62b"
---

# Amazon CloudWatch Logs now supports Application Load Balancer logs

Amazon CloudWatch Logs now supports Application Load Balancer (ALB) logs as vended logs, improving observability and simplifying debugging for network traffic patterns. You can now analyze ALB access, connection and health check logs directly in CloudWatch to gain insights into client connections, traffic distribution, connection status and target health, helping you identify and troubleshoot network issues faster. Additionally, you can set up[CloudWatch telemetry enablement rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/telemetry-config-rules.html) to automatically configure logging of both existing and newly created ALB resources, for your organization, specific accounts, or specific resources, ensuring consistent monitoring coverage without manual setup.


With this CloudWatch Logs integration, you can track detailed access patterns using CloudWatch Logs Insights queries, create metric filters for monitoring and alarming, and review traffic patterns in real time using Live Tail. ALB logs can be configured through the integrations tab of your application load balancer in AWS Management Console, AWS CLI, or SDKs. You can also configure delivery of ALB logs to Amazon Data Firehose or Amazon S3 with support for Apache Parquet format.


ALB logs delivery to CloudWatch is available in all AWS Commercial and GovCloud regions where Application Load Balancer and CloudWatch are available. ALB logs are charged as vended logs when delivered to CloudWatch Logs and Data Firehose, while delivery to Amazon S3 is free (Parquet conversion is charged at $0.035/GB - N. Virginia).


To learn more about configuring ALB logs in CloudWatch Logs,[please visit our documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-logs.html) . For pricing information, see[CloudWatch pricing page.](https://aws.amazon.com/cloudwatch/pricing/)
