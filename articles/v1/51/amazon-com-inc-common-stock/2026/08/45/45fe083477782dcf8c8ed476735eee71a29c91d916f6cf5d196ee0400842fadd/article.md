---
schema_version: "1.0.0"
document_id: "45fe083477782dcf8c8ed476735eee71a29c91d916f6cf5d196ee0400842fadd"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/s3-additional-policy-details-access-denied-error-messages/"
published_at: "2026-08-13T13:00:00+00:00"
first_seen_at: "2026-08-13T17:28:45.755307+00:00"
fetched_at: "2026-08-13T17:28:48.814467+00:00"
content_hash: "sha256:54f51141fda7acd1674f1bed29b9140ad1c3de9921bcba3c5dc7632b8e59c683"
---

# Amazon S3 adds additional policy details to access denied error messages

Amazon S3 now includes the specific AWS Identity and Access Management (IAM) and AWS Organizations policy Amazon Resource Name (ARN) in HTTP 403 Access Denied error messages for same-account and same-organization requests. This helps you quickly identify the exact policy responsible for a denied request and remediate the issue directly.


Previously, S3 access denied error messages included the policy type and reason for denial, but when multiple policies of the same type existed, you still had to manually inspect each one to pinpoint the root cause. Now the error message includes the specific policy ARN for explicit deny cases, covering Service Control Policies (SCPs), Resource Control Policies (RCPs), identity-based policies, session policies, and permission boundaries.


This capability is available in all AWS Regions, including the AWS GovCloud (US) Regions and the AWS China Regions. To learn more about how to troubleshoot access denied errors in Amazon S3, visit the[S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/troubleshoot-403-errors.html) and the[IAM troubleshooting documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_access-denied.html) .
