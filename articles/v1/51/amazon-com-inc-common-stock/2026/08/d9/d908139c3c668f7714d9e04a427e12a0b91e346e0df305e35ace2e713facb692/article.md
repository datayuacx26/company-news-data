---
schema_version: "1.0.0"
document_id: "d908139c3c668f7714d9e04a427e12a0b91e346e0df305e35ace2e713facb692"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/web-search-amazon-bedrock/"
published_at: "2026-08-19T22:55:00+00:00"
first_seen_at: "2026-08-20T03:48:38.861444+00:00"
fetched_at: "2026-08-20T03:48:43.683071+00:00"
content_hash: "sha256:8132f2307da897d114dc8b0ca7c20c7b0f84345689febad3dae0ba527734bb30"
---

# Web Search in Amazon Bedrock AgentCore adds domain and published date filtering, expands to Europe and Asia Pacific

Web Search in Amazon Bedrock AgentCore now supports domain filtering and published-date filtering, giving agents per-request control over which web sources and time windows they search. Amazon Bedrock AgentCore provides the infrastructure to build, connect, and optimize AI agents, and Web Search enables those agents to ground responses in current web data.


With runtime domain filtering, agents can narrow search results to trusted sources or block unwanted domains on a per-call basis without requiring admin reconfiguration. Published-date filtering allows agents to constrain results to a specific time window using inclusive from and to date bounds, ensuring responses reflect only timely, relevant content.


With this launch, agents can pass include and exclude domain lists and a published-date range directly in each tool call, while admins gain new gateway-level allowlist support and an increased domain cap of up to 100 domains per list. These capabilities are ideal for regulated industries, research workflows, and applications that require strict control over information sources and recency.


The Web Search Tool is also expanding to Europe (Ireland) (eu-west-1) and Asia Pacific (Tokyo) (ap-northeast-1), joining the existing US East (N. Virginia) (us-east-1) availability. To learn more, read the[technical blog](https://aws.amazon.com/blogs/machine-learning/domain-and-publish-date-filters-for-web-search-on-agentcore/) about domain and published date filters , and review the Amazon Bedrock AgentCore[product documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-target-connector-web-search-tool.html) .
