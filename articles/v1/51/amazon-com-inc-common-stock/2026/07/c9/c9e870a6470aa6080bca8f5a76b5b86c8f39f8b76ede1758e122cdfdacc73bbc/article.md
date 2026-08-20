---
schema_version: "1.0.0"
document_id: "c9e870a6470aa6080bca8f5a76b5b86c8f39f8b76ede1758e122cdfdacc73bbc"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/cloudwatch-managed-collectors/"
published_at: "2026-07-31T13:06:00+00:00"
first_seen_at: "2026-08-01T00:58:40.483111+00:00"
fetched_at: "2026-08-01T00:58:42.999034+00:00"
content_hash: "sha256:53b812ad16a3025694c0594867116028afe52aab9dd24827d9024b1ef505bd87"
---

# Amazon CloudWatch announces managed Prometheus collectors

Amazon CloudWatch now supports collecting Prometheus metrics from your AWS infrastructure using fully managed collectors — enabling you to monitor Amazon EKS, Amazon EC2, Amazon ECS, Amazon MSK, and Amazon OpenSearch Service workloads without deploying or managing any agents.


Previously, getting Prometheus metrics into CloudWatch required deploying, scaling, and maintaining a self-managed OpenTelemetry Collector. Managed Prometheus collectors eliminate that overhead. You provide a scrape configuration and a connection to your resources, and CloudWatch handles provisioning, scaling, and collection automatically. Metrics are delivered in OpenTelemetry format and can be queried alongside your AWS vended metrics using PromQL — providing unified alarming, dashboarding, and cross-service correlation in a single view.


Managed collectors support Kubernetes service discovery (EKS), DNS-based service discovery via AWS Cloud Map (ECS), direct instance scraping (EC2), and open monitoring endpoints (MSK, OpenSearch). Metrics for EKS, MSK and Open Search can be visualized in automatic dashboards, queried with PromQL and used in CloudWatch alarms.


This feature is available in all AWS Regions where the CloudWatch OTLP endpoint is available, except Asia Pacific (New Zealand). Managed Prometheus collectors are charged by the hour and standard CloudWatch OpenTelemetry metric ingestion pricing applies. To get started, see the[documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/managed-prometheus-collectors.html) .
