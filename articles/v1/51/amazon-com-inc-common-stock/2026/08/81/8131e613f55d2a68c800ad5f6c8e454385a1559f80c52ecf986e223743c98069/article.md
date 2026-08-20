---
schema_version: "1.0.0"
document_id: "8131e613f55d2a68c800ad5f6c8e454385a1559f80c52ecf986e223743c98069"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-waf-salt-security-managed-rules/"
published_at: "2026-08-06T21:10:00+00:00"
first_seen_at: "2026-08-07T16:36:12.175527+00:00"
fetched_at: "2026-08-07T16:36:14.654834+00:00"
content_hash: "sha256:87b1d53b004dbec48f461cedc391aebd626baf921e88b7c3d5629397acac5ac0"
---

# AWS WAF now supports a Salt Security managed rule group for API and MCP threat detection

AWS WAF now supports the Salt Security managed rule group, available through AWS Marketplace:[Salt Managed Rules for AWS WAF - AI Agent & API Security](https://aws.amazon.com/marketplace/pp/prodview-yaljl5tu6bkye) . This rule group gives AWS WAF customers detection and mitigation for API-focused attacks and for traffic from AI agents and Model Context Protocol (MCP) endpoints, without writing or maintaining custom rules.


The rule group detects common and complex API attack vectors, including credential brute force, excessive GraphQL queries, server-side request forgery (SSRF), prototype pollution, and JSON Web Token (JWT) anomalies. It identifies and labels traffic from Model Context Protocol (MCP) endpoints, blocks unauthenticated MCP access, and adds observability into MCP interactions in AWS WAF. The rule group also applies rate limiting to sensitive request parameters, such as user identifiers and email addresses, to help mitigate enumeration and abuse. To support detection and downstream analysis, it labels request attributes including authorization headers, user identifiers, and GraphQL queries.


You can subscribe to the rule group and add it to a web ACL directly in the AWS WAF console through AWS Marketplace, with no additional configuration. The rule group supports versioning, and pricing is set by Salt Security through AWS Marketplace. For a full list of supported Regions, visit the AWS Regional Services page.


To get started, visit the AWS WAF console or find the Salt Security rule group in AWS Marketplace. For more information, see the[AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-rule-groups.html) .
