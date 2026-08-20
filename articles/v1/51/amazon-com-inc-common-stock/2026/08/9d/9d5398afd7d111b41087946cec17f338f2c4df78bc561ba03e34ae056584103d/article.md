---
schema_version: "1.0.0"
document_id: "9d5398afd7d111b41087946cec17f338f2c4df78bc561ba03e34ae056584103d"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-iam-role-manager"
published_at: "2026-08-12T18:00:00+00:00"
first_seen_at: "2026-08-13T02:44:45.111424+00:00"
fetched_at: "2026-08-13T02:44:49.794038+00:00"
content_hash: "sha256:e685af509e74ad1d1a382139ca4438a27b2318a6e65fb4591fffd340ad58d8aa"
---

# AWS IAM now provides role manager to set up IAM roles automatically

Today, AWS announces the general availability of role manager, a capability in AWS Identity and Access Management (IAM) that automatically sets up the IAM roles your AWS services need. When you set up a supported service in the console, role manager creates a default role on your behalf, or reuses one that already exists in your account if it already matches the required permissions. You can enable or disable role manager at any time, as well as inspect the AWS-managed templates that role manager deploys on your behalf.


Role manager supports 6 AWS service consoles at launch, including AWS Lambda and Amazon EventBridge. For example, when you create an AWS Lambda function, role manager applies the AWS-managed template for that workflow. Roles created via role manager appear in the IAM console as standard IAM roles that you fully control, and you can identify the ones role manager created. When you are ready to tighten permissions, you can disable role manager and use IAM Access Analyzer to refine each role to only the permissions it needs.


Role manager is available in all AWS Regions, except the AWS GovCloud (US) Regions and the China Regions.


To learn more, see[How AWS IAM role manager rethinks the starting point for IAM roles](https://aws.amazon.com/blogs/security/how-aws-iam-role-manager-rethinks-the-starting-point-for-iam-roles/) on the AWS Security Blog, or[Create roles automatically with role manager](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_role-manager.html) in the IAM User Guide.
