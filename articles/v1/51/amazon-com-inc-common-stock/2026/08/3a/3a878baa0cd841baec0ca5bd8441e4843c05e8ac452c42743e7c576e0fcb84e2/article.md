---
schema_version: "1.0.0"
document_id: "3a878baa0cd841baec0ca5bd8441e4843c05e8ac452c42743e7c576e0fcb84e2"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-workspaces-applications-observability-metrics"
published_at: "2026-08-06T15:00:00+00:00"
first_seen_at: "2026-08-06T21:16:52.186535+00:00"
fetched_at: "2026-08-06T21:16:53.695254+00:00"
content_hash: "sha256:c46b657fe133ee25066e6140972083b8acfd7ff10e1f9c98cb9873e74f1faf09"
---

# Amazon WorkSpaces Applications now publishes enhanced observability metrics

Amazon WorkSpaces Applications now publishes additional performance and session health metrics to Amazon CloudWatch, enabling IT administrators to gain deeper visibility into their application streaming workloads. These new metrics span network performance, compute resource utilization, and session lifecycle events — all available at no additional cost.


With these metrics, administrators can proactively identify and troubleshoot issues that impact end-user experience. For example, metrics such as TCP retransmission rate and congestion window help pinpoint network degradation, while GPU utilization and memory page hard faults surface resource bottlenecks before they affect session quality. Session lifecycle metrics like connection failures and connection duration enable teams to set CloudWatch alarms for rapid detection of connectivity issues, build custom dashboards for fleet-wide visibility, and reduce mean time to resolution.


These metrics are available in all AWS Regions where Amazon WorkSpaces Applications is supported.


To get started, navigate to the Amazon CloudWatch console and observe these metrics or update your[WorkSpaces Applications custom dashboards](https://docs.aws.amazon.com/appstream2/latest/developerguide/custom-cloudwatch-dashboards.html) . You can also monitor these metrics through[WorkSpaces Applications automatic dashboard](https://docs.aws.amazon.com/appstream2/latest/developerguide/cloudwatch-automatic-dashboard.html) . To learn more about metric availability by operating system, visit the[Amazon WorkSpaces Applications documentation](https://docs.aws.amazon.com/appstream2/) and the[CloudWatch metrics reference](https://docs.aws.amazon.com/appstream2/latest/developerguide/monitoring.html) .
