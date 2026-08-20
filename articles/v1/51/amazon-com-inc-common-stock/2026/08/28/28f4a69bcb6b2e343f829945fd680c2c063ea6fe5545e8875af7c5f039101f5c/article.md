---
schema_version: "1.0.0"
document_id: "28f4a69bcb6b2e343f829945fd680c2c063ea6fe5545e8875af7c5f039101f5c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-workspaces-observability-metrics"
published_at: "2026-08-06T15:00:00+00:00"
first_seen_at: "2026-08-06T21:16:52.186535+00:00"
fetched_at: "2026-08-06T21:16:53.695254+00:00"
content_hash: "sha256:973dc107416b5cb6f21b8b4ea5774c46652e1ed829fc122f8d3d62100798c505"
---

# Amazon WorkSpaces now publishes enhanced observability metrics

Amazon WorkSpaces now publishes additional performance and session health metrics to Amazon CloudWatch, enabling IT administrators to gain deeper visibility into their virtual desktop workloads. These new metrics span network performance, compute and storage resource utilization, and session lifecycle events — all available at no additional cost.


With these metrics, administrators can proactively identify and troubleshoot issues that impact end-user experience. For example, TCP retransmission rate and congestion window help pinpoint network degradation, GPU usage and CPU queue length surface compute bottlenecks, and storage metrics like disk I/O queue lengths and memory page hard faults provide visibility into disk saturation and memory pressure. Administrators can set CloudWatch alarms for rapid detection of performance issues, build custom dashboards for fleet-wide visibility, and reduce mean time to resolution.


These metrics are available in all AWS Regions where Amazon WorkSpaces is supported.


To get started, navigate to the Amazon CloudWatch console and observe these metrics or update your[WorkSpaces custom dashboards](https://docs.aws.amazon.com/workspaces/latest/adminguide/cloudformation-templates.html) . You can also monitor these metrics through[WorkSpaces automatic dashboard](https://docs.aws.amazon.com/workspaces/latest/adminguide/cloudwatch-dashboard.html) . To learn more, visit the[Amazon WorkSpaces documentation](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces.html) and the[CloudWatch metrics reference](https://docs.aws.amazon.com/workspaces/latest/adminguide/cloudwatch-metrics.html) .
