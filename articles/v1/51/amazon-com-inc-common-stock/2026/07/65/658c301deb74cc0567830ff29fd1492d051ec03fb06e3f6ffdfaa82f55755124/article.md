---
schema_version: "1.0.0"
document_id: "658c301deb74cc0567830ff29fd1492d051ec03fb06e3f6ffdfaa82f55755124"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-organizations-resource-control-policy-limit-increase-2000"
published_at: "2026-07-22T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:821486b6f8c4023722be0591e5d665a026af091fa524fc7715382ef46f85f3f8"
---

# AWS Organizations increases RCP quota to 2,000 per organization

AWS Organizations now supports up to 2,000 resource control policies (RCPs) per organization, doubling the previous limit of 1,000. RCPs enable centralized management of the maximum permissions available to resources across member accounts in your organization. This quota increase helps customers managing large, complex, multi-account environments define more granular resource access controls without encountering policy limits.


With RCPs, you can restrict which external principals can access resources across your organization's member accounts, helping enforce organization-wide access control guidelines at scale — without updating individual resource-based policies. This is especially valuable for organizations that require fine-grained, centralized permission management across hundreds of accounts, where the previous 1,000-policy limit created barriers to sufficiently detailed access control configurations.


The increased quota of 2,000 RCPs per organization is available at no additional cost in all AWS Regions where AWS Organizations is supported. No action is required — existing organizations automatically have access to the higher limit.
To learn more about resource control policies and managing quotas in AWS Organizations, visit the[AWS Organizations documentation](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_reference_limits.html) .
