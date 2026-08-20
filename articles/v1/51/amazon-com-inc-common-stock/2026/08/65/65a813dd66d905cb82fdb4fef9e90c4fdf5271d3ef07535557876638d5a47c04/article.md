---
schema_version: "1.0.0"
document_id: "65a813dd66d905cb82fdb4fef9e90c4fdf5271d3ef07535557876638d5a47c04"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-quick-deny-by-default/"
published_at: "2026-08-19T15:36:00+00:00"
first_seen_at: "2026-08-19T18:07:51.144413+00:00"
fetched_at: "2026-08-19T18:07:53.834707+00:00"
content_hash: "sha256:5bd4ccb7e97e055dc135583684b8d969e573d336f8899837f3b9d7f7edee08d9"
---

# Amazon Quick adds deny by default for custom permissions

Amazon Quick custom permissions now include deny by default, a governance setting that automatically restricts new AI capabilities before they reach users.


Previously, new AI capabilities were available to all users on release, requiring administrators to react after the fact. With deny by default, administrators restrict the AI capability category in a custom permissions profile and assign it to users, roles, or the entire account. Quick then denies any new AI capability at launch for those users. Restricting a category also restricts existing capabilities in it. Administrators explicitly allow each capability when ready. The restriction applies only to the profile you configure.


Configure deny by default in Manage account in Amazon Quick or through the AWS CLI. To learn more, see[Custom permissions deny by default](https://docs.aws.amazon.com/quick/latest/userguide/custom-permissions-governance.html) . Deny by default is available in all AWS Regions where[Amazon Quick is available](https://docs.aws.amazon.com/quick/latest/userguide/regions.html) .
