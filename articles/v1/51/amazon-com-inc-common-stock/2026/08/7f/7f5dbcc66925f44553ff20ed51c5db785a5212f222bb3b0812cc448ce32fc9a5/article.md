---
schema_version: "1.0.0"
document_id: "7f5dbcc66925f44553ff20ed51c5db785a5212f222bb3b0812cc448ce32fc9a5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-iam-quota-increase/"
published_at: "2026-08-19T15:00:00+00:00"
first_seen_at: "2026-08-20T00:12:43.329206+00:00"
fetched_at: "2026-08-20T00:12:46.759003+00:00"
content_hash: "sha256:c55d2cd59b790603faa4aa05f99860c72a1b61f79983ba98093b2fcd5c549d02"
---

# AWS IAM now supports 20 managed policies per role by default

[AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) has increased the default quota for managed policies per role from 10 to 20.


This higher default quota reduces the need to make Service Quota requests when following IAM best practices like separating permissions into purpose-specific policies or when onboarding to AWS Partner products that require attaching additional managed policies. If you need more than 20 managed policies per role, you can[request a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) up to 25 using Service Quotas.


This change is available in all commercial AWS Regions, AWS GovCloud (US) and China Regions and applies automatically to all IAM roles in your account with no action required. To learn more, see[IAM and AWS STS quotas](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-quotas.html) in the IAM User Guide.
