---
schema_version: "1.0.0"
document_id: "55b39b4b1dc04c1a5331849fea1d8bdb996614d1fc40b8965efb084b1beb11ac"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-opensearch-service-agent/"
published_at: "2026-07-15T14:44:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:c0dd4050e69022f01610e2b83a133ee264c603b4c20128e3e1cf60364e0e55ec"
---

# Amazon OpenSearch Service now supports the Agent Toolkit for AWS with a curated skill

Amazon OpenSearch Service now integrates with the[Agent Toolkit for AWS](https://aws.amazon.com/products/developer-tools/agent-toolkit-for-aws/) , enabling you to build, manage, and query OpenSearch Service domains and OpenSearch Serverless collections directly from AI coding agents such as Claude Code, Kiro, and Cursor. The integration is powered by the AWS MCP (Model Context Protocol) server, which executes AWS API calls on your behalf, paired with the curated *amazon-opensearch-service* skill that automatically routes natural-language requests to the right capability.


With this skill, you can describe a goal in plain language and the agent handles the rest across five areas. **Migration** moves you from self-managed OpenSearch into OpenSearch Service or OpenSearch Serverless. **Operations** provisions and manages domains and collections. **Search** builds vector, semantic, hybrid, and RAG search. **Log analytics** analyzes logs with PPL and OpenSearch Ingestion. **Trace analytics** investigates distributed traces with OpenTelemetry. The integration works with both managed domains and collections across all versions, requires no changes to your existing infrastructure, and is available at no additional charge. To learn more about these capabilities, see our[documentation](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/amazon-opensearch-service-skill.html) .


Support is available in all AWS Regions where Amazon OpenSearch Service and OpenSearch Serverless are offered. To get started, install the aws-data-analytics plugin in your agent — it bundles the AWS MCP Server configuration and the OpenSearch skill in a single step. For setup instructions, see[MCP Server and Agent Skills](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/aws-mcp-server.html) .
