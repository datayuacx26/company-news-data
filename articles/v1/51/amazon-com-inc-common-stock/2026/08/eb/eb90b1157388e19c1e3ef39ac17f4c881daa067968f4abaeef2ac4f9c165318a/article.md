---
schema_version: "1.0.0"
document_id: "eb90b1157388e19c1e3ef39ac17f4c881daa067968f4abaeef2ac4f9c165318a"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-identity-center-accounts-optional/"
published_at: "2026-08-05T15:12:00+00:00"
first_seen_at: "2026-08-05T22:17:28.341507+00:00"
fetched_at: "2026-08-05T22:17:30.878857+00:00"
content_hash: "sha256:fba894073c5f3d2ea1fa9732c1cf54eedbf9513fcd10f1f292e8d35741bc84d0"
---

# AWS IAM Identity Center makes management of AWS account access optional for new organization instances

AWS IAM Identity Center now lets you decide whether to enable management of AWS account access when you create a new organization instance. This allows you to use IAM Identity Center to manage access to AWS applications only, without the need to manage access to AWS accounts. This feature is available at the time of initial configuration of an IAM Identity Center instance and does not affect existing IAM Identity Center instances.


IAM Identity Center enables you to connect your workforce identities to AWS once and offer AWS application owners across your organization streamlined access management. Application end users benefit from single sign-on, user awareness, and consistent authentication experience across AWS applications. Previously, this meant you also needed to manage access to AWS accounts. With this release, account management is now optional. When you choose not to enable management of AWS accounts, IAM Identity Center does not provision its service-linked role into your member accounts, which reduces the access surface in your environment. You can enable account management permissions later through instance settings or the UpdateInstance API.


This capability is available in all AWS Regions where IAM Identity Center is available. To get started, see Configure instance settings in the IAM Identity Center User Guide.
