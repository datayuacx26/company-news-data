---
schema_version: "1.0.0"
document_id: "11763709d27bc24fef21a23dca408d24bb1033fa646505df991f3a3dfa0989a5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-iam-identity-center-supports-one-click-multi-region-option-new-organization-instances"
published_at: "2026-08-07T16:00:00+00:00"
first_seen_at: "2026-08-07T20:32:10.069700+00:00"
fetched_at: "2026-08-07T20:32:12.526409+00:00"
content_hash: "sha256:c93835739e961714af99a6c34dbce317c229abb048c45d07b9b1617f663e3128"
---

# AWS IAM Identity Center supports one-click multi-Region option for new organization instances

AWS IAM Identity Center now makes it easier to enable multi-Region support when creating a new organization instance. Previously, enabling multi-Region support required multiple steps including creating a customer managed KMS key, configuring key policies, and manually adding Regions. Now, customers creating a new IAM Identity Center instance in supported Regions can enable multi-Region in one click.


When enabling a new organization instance, you can choose from three instance configuration options: single-Region instance, multi-Region instance, or custom instance. The multi-Region instance option automatically creates a customer managed multi-Region KMS key in your account and replicates your instance to an additional Region. This enables resilient AWS account and application access — your workforce can continue to access their AWS accounts even if IAM Identity Center experiences a disruption in the primary Region. The custom instance option lets you configure your Region settings individually, including the ability to use an existing customer managed KMS key from your account.


Instance configuration options are available in[17 enabled-by-default commercial AWS Regions](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-regions.html?refid=6e78b6f1-dfd8-49b4-ae84-4527ca1881cb#manage-acct-regions-regional-availability) for organization instances of IAM Identity Center. Standard AWS KMS charges apply for the customer managed key created with the multi-Region instance option. IAM Identity Center is provided at no additional cost.


To get started, see the[IAM Identity Center User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-identity-center.html) . To learn more about multi-Region support, see[Using IAM Identity Center across multiple AWS Regions](https://docs.aws.amazon.com/singlesignon/latest/userguide/multi-region-iam-identity-center.html) . To learn more about IAM Identity Center, visit the[product detail page](https://aws.amazon.com/iam/identity-center/) .
