---
schema_version: "1.0.0"
document_id: "a549718ff8e3a112e0d0bf35ae9e0cf21e698c3cdbe78db009ddb8f5a00c9833"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-security-hub-ai/"
published_at: "2026-07-14T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:6ca072728967154e0e1fb8a37df26ebe5f0460d046a854fa09dfa6821bb9110a"
---

# AWS Security Hub now provides AI inventory for organization-wide visibility of AI assets

Today, AWS announces that AWS Security Hub now provides an AI inventory, giving central security teams a continuously updated, organization-wide view of AI assets and their security posture. As organizations rapidly deploy AI agents, models, and pipelines, security teams may lack visibility into what AI assets exist across their organization. Without centralized visibility connecting AI assets to active threats and misconfigurations, organizations cannot secure what they don't know exists.


Security Hub AI inventory automatically discovers and catalogs AI workloads across your AWS environment through three discovery methods. For managed AI services, Security Hub inventories AWS Config resources from Amazon Bedrock, Bedrock AgentCore and Amazon SageMaker, with no additional configuration. For self-hosted AI workloads, Security Hub leverages the software bill of materials (SBOM) analysis from Amazon Inspector, which has been enhanced to identify inference endpoints, models and AI agents installed on Amazon EC2 instances and Amazon ECR container images, including frameworks such as Ollama, vLLM, Hugging Face TGI, and others. Security Hub also leverages Amazon GuardDuty DNS telemetry to discover external AI API endpoints (such as calls to third-party model providers) being accessed from your EC2 instances, revealing third-party AI dependencies that may not have been previously identified.


Each discovered AI asset is mapped to its underlying infrastructure and correlated with security findings from across the AWS security stack, including threat findings from Amazon GuardDuty. Teams can filter, group, and query their AI inventory by account, resource type, discovery method, and specific model identity, enabling them to prioritize remediation based on which AI workloads are actively under threat and carry the highest organizational risk.


AI Inventory is included with Security Hub Essentials at no additional cost and requires no new enablement. It is available in all AWS commercial Regions where Security Hub is offered. To learn more, see the[AWS Security Hub User Guide](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub-v2.html) and the[AWS Security Hub product page](https://aws.amazon.com/security-hub/) .
