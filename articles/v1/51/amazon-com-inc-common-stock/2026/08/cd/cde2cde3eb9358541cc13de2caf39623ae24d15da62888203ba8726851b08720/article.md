---
schema_version: "1.0.0"
document_id: "cde2cde3eb9358541cc13de2caf39623ae24d15da62888203ba8726851b08720"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/whats-new/2026/08/amazon-quick-deny-by-default-permissions/"
published_at: "2026-08-12T17:15:00+00:00"
first_seen_at: "2026-08-13T02:44:45.111424+00:00"
fetched_at: "2026-08-17T14:48:22.550886+00:00"
content_hash: "sha256:69ac70b0f053561d4bbb74b79afbfccbf7f8faf50f585a24e627d6d290eb84f4"
---

# Amazon Quick adds deny by default for custom permissions

Amazon Quick custom permissions now include deny by default, a governance setting that automatically restricts new AI capabilities before they reach users.


Previously, new AI capabilities were available to all users on release, requiring administrators to react after the fact. With deny by default, administrators restrict the AI capability category in a custom permissions profile and assign it to users, roles, or the entire account. Quick then denies any new AI capability at launch for those users. Restricting a category also restricts existing capabilities in it. Administrators explicitly allow each capability when ready. The restriction applies only to the profile you configure.


Configure deny by default in Manage account in Amazon Quick or through the AWS CLI. To learn more, see[Custom permissions deny by default](https://docs.aws.amazon.com/quick/latest/userguide/custom-permissions-governance.html) . Deny by default is available in all AWS Regions where[Amazon Quick is available](https://docs.aws.amazon.com/quick/latest/userguide/regions.html) .
