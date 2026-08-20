---
schema_version: "1.0.0"
document_id: "8494f7c1cd2c3b8a19ac124a6cac8a6e7125ec0d98258a3a65c6d61f00aafa40"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/iam-policy-simulator-iam-console/"
published_at: "2026-07-30T17:43:00+00:00"
first_seen_at: "2026-07-31T16:43:36.463186+00:00"
fetched_at: "2026-07-31T16:43:37.565078+00:00"
content_hash: "sha256:c16391e1be5057a804830391462ffb08a1891443c6cc8767079c33543f41b92e"
---

# IAM Policy Simulator moves to the IAM console and adds additional capabilities

[AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) announces a major update to[IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) , the tool you use to test and validate the permissions your IAM policies grant before you deploy them. This update changes the simulator in three ways: it now lives in the IAM console, it can test service control policies (SCPs), and it adds flexibility to model more of the scenarios that security and platform teams simulate in practice.


IAM Policy Simulator is now part of the IAM console, replacing the standalone simulator site, so you can test policies in the same place you manage your identities and policies. You can also now include SCPs in your simulation to test how your organization's SCP hierarchy interacts with identity and resource policies, and through the API, test how condition keys such as Region restrictions and tag requirements affect the outcome. Finally, new flexibility lets you exclude specific policies to model "what if I remove this policy?" scenarios, and cross-account simulations now report per-policy decisions for identity and resource-based policies, with the matched statements returned for a denied request reflecting only the policies that drove the decision. Together, these changes help teams automate policy unit testing, detect over-permissive access, and validate guardrails with greater confidence.


These features are available in all AWS Regions where IAM Policy Simulator is available. You can access IAM Policy Simulator in the IAM console by choosing Policy simulator in the navigation pane.


To learn more, see the following resources:


-


[Testing IAM policies with the IAM policy simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html)


-


API reference on[SimulatePrincipalPolicy](https://docs.aws.amazon.com/IAM/latest/APIReference/API_SimulatePrincipalPolicy.html) and[SimulateCustomPolicy](https://docs.aws.amazon.com/IAM/latest/APIReference/API_SimulateCustomPolicy.html)
