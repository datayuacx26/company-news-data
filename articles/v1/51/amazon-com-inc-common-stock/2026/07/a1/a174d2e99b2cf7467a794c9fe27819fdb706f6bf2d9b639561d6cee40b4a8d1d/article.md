---
schema_version: "1.0.0"
document_id: "a174d2e99b2cf7467a794c9fe27819fdb706f6bf2d9b639561d6cee40b4a8d1d"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/cloudwatch-coding-agent-insights/"
published_at: "2026-07-20T09:51:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:4933999ffe142c2fc4ab40aab9895c4f21fe0309339c622d38be20132fb97632"
---

# Amazon CloudWatch announces coding agent insights

Amazon CloudWatch announces the launch of coding agent insights, giving engineering leaders visibility into how AI coding tools are driving value across their organization. Coding Agent Insights integrates with[Claude apps gateway for AWS](https://aws.amazon.com/blogs/machine-learning/introducing-claude-apps-gateway-for-aws/) to collect telemetry from Claude Code without additional instrumentation. Other supported coding agents include Codex and GitHub Copilot.


As organizations scale AI coding agent adoption, they need to understand return on investment. Coding agent insights is built on OpenTelemetry metrics emitted by your coding agents and presents them alongside your existing CloudWatch operational data. This helps you answer questions like which teams would benefit from expanded access, where are agents accelerating delivery, and how can you right-size token budgets across departments. You can track spend trends, set proactive token billing alerts, correlate agent adoption with improvements in commit throughput and pull request velocity, or identify the models delivering the best cost-to-output ratio for your workloads.


CloudWatch coding agent insights is available in all AWS commercial regions except Middle East (UAE), Middle East (Bahrain), and Israel (Tel Aviv). Configure your Claude apps gateway to emit telemetry to CloudWatch using the[setup guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/coding-agents-claude-code-gateway.html) and view the Coding Agent Insights dashboard in the CloudWatch console. Standard CloudWatch OpenTelemetry metric ingestion pricing applies — see metrics[pricing](https://aws.amazon.com/cloudwatch/pricing/) for details. To learn more, see[the documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/coding-agents-insights.html) .
