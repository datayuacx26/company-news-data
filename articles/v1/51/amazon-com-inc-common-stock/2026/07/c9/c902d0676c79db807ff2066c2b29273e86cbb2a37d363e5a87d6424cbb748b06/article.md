---
schema_version: "1.0.0"
document_id: "c902d0676c79db807ff2066c2b29273e86cbb2a37d363e5a87d6424cbb748b06"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-control-tower-account/"
published_at: "2026-07-16T04:07:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:adc994c5259c01c24ae2a4552c42b8d5d90b62b341c5c3477eb3cd7241785fa1"
---

# AWS Control Tower Account Factory for Terraform now re-applies customizations when accounts move between OUs

AWS Control Tower Account Factory for Terraform (AFT) can now automatically re-apply an account's customizations when that account moves to a different Organizational Unit (OU). Previously, moving an enrolled account between OUs required manually triggering customization re-application, creating operational overhead and risk of configuration drift. With this capability, you can opt in to automatic re-application in your AFT deployment, so accounts stay consistent with their OU-specific configuration as soon as they're moved.


To enable this capability, set aft_customization_triggers = \["account_move"\] in your AFT configuration. The re-application workflow skips the bootstrap and provisioning phases, running only global and account-level customizations for faster execution. Individual accounts can be excluded from this behavior by setting account_skip_customization_triggers = "true", giving teams precise control over which accounts participate in automated re-application.


This release also includes additional improvements: support for custom Terraform Cloud and Enterprise workspace naming variables, tighter access controls on the AFT logging bucket, and improved scaling for large-scale AWS Enterprise Support enrollment. Organizations enforcing compliance or security baselines tied to OU membership will benefit most from these combined enhancements.


This capability is available today across all AWS regions where AWS Control Tower Account Factory for Terraform is offered. To learn more about enabling automatic customization re-application and upgrading to the latest AFT release, visit the[AFT documentation](https://docs.aws.amazon.com/controltower/latest/userguide/aft-overview.html) and review the[AFT release notes on GitHub](https://github.com/aws-ia/terraform-aws-control_tower_account_factory/releases) .
