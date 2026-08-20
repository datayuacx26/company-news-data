---
schema_version: "1.0.0"
document_id: "e49f5cd440d2829082d91f121c5b4492c0a8c99757ff2275e681e000300fce2c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-iam-identity-center-extends-multi-region-support-to-identity-center-directory"
published_at: "2026-07-29T07:00:00+00:00"
first_seen_at: "2026-07-29T21:38:06.257307+00:00"
fetched_at: "2026-07-29T21:38:07.883784+00:00"
content_hash: "sha256:50f81f147dee566f8ff355fb0ce34d56dc8d8ff2eda307fd65808285efc53daa"
---

# AWS IAM Identity Center extends multi-Region support to Identity Center directory

IAM Identity Center helps you configure the single sign-on experience of your workforce to AWS accounts and applications. You can now replicate IAM Identity Center from the primary AWS Region where you first enabled it to additional Regions of your choice when using Identity Center directory as your identity source. This extends the multi-Region support capability, previously available for Identity Center organization instances connected to external identity providers, to instances that use the Identity Center directory to manage and authenticate their workforce. This feature enhances resilience of user access to AWS accounts and helps you deploy AWS applications in the AWS Regions that best align with your business needs such as application data residency and proximity to users.


When you enable this feature, IAM Identity Center automatically replicates your identities, entitlements, and other information from the primary Region to additional Regions. If IAM Identity Center is affected by a disruption in the primary Region, IAM Identity Center users continue to have access to their AWS accounts using the already provisioned entitlements in the additional Regions.


AWS application administrators can use the standard application deployment workflow to deploy their application in an additional Region while you continue to administer IAM Identity Center in the primary Region.


IAM Identity Center multi-Region support is currently available in the[17 enabled-by-default commercial AWS Regions](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-regions.html?refid=6e78b6f1-dfd8-49b4-ae84-4527ca1881cb#manage-acct-regions-regional-availability) for organization instances of IAM Identity Center. The IAM Identity Center organization instance must be configured with a multi-Region customer managed KMS key (CMK). To find out which AWS applications support deployment in additional Regions, visit[AWS applications that you can use with IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/awsapps-that-work-with-identity-center.html?refid=6e78b6f1-dfd8-49b4-ae84-4527ca1881cb) . Standard[AWS KMS charges apply](https://aws.amazon.com/kms/pricing/) for storing and using CMKs. IAM Identity Center is provided at no additional cost. To learn more about IAM Identity Center, visit the[product detail page](https://aws.amazon.com/iam/identity-center/?c=sc&sec=srvm&trk=6e78b6f1-dfd8-49b4-ae84-4527ca1881cb&sc_channel=ps) . To get started, see the[IAM Identity Center User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/multi-region-iam-identity-center.html) .
