---
schema_version: "1.0.0"
document_id: "e8abe96524394da156db8512f89f50a4a7825a3f2a13fe5ffb0039a7758eaa70"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-neptune-tbac/"
published_at: "2026-07-27T22:00:00+00:00"
first_seen_at: "2026-07-28T00:43:02.218346+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:897766f805278b925b939eb78ff6216cb0a7cc7e8668b0163e2b87aea81d3855"
---

# Amazon Neptune now supports tag-based access control for IAM

[Amazon Neptune](https://aws.amazon.com/neptune/) Database now supports tag-based access control (TBAC) for IAM, enabling customers to use AWS resource tags and IAM principal tags as conditions in IAM policies and Service Control Policies (SCPs) to control access to Neptune data-plane operations. Neptune already provides robust security through VPC isolation, TLS encryption, and IAM authentication, but customers managing multiple clusters at scale needed a dynamic, attribute-based mechanism to enforce organizational access boundaries. TBAC addresses this by allowing administrators to govern cluster access without enumerating specific cluster ARNs in every policy.


With TBAC, IAM principals can only perform \`neptune-db:*\` actions against Neptune clusters whose tags match their own — for example, a principal tagged \`Project=FraudDetection\` is automatically restricted to clusters sharing that same tag. This eliminates lateral access risk within shared VPC environments, enforces team and environment-level isolation across projects, and supports federated identity workflows using SAML or OIDC session tags from external identity providers. Granular permissions like neptune-db:QueryLanguage can be used alongside TBAC for more fine-grained access control.


This feature is available in all AWS Regions where Amazon Neptune is available and requires Neptune engine version 1.2.0.0 or later with IAM authentication enabled.


To learn more about configuring tag-based access control for Amazon Neptune, including how to tag your DB clusters and IAM principals and deploy organization-wide guardrails using SCPs, visit the[Amazon Neptune documentation](https://docs.aws.amazon.com/neptune/latest/userguide/iam-data-tbac.html) .
