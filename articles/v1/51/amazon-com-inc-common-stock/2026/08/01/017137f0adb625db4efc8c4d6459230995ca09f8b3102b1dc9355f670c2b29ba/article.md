---
schema_version: "1.0.0"
document_id: "017137f0adb625db4efc8c4d6459230995ca09f8b3102b1dc9355f670c2b29ba"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-transform-for-migrations-automates-post-launch-actions"
published_at: "2026-08-06T17:00:00+00:00"
first_seen_at: "2026-08-06T21:16:52.186535+00:00"
fetched_at: "2026-08-06T21:16:53.695254+00:00"
content_hash: "sha256:0d8d2ec3e3d9addedca2c73b64de9537365da1f066cbb6d3dcd8da9a8c351333"
---

# AWS Transform for migrations automates post-launch actions

AWS Transform now automates the configuration and execution of post-launch actions through the migration workflow. Define actions at the account level and apply them automatically to each source server across your target accounts, including multi-account migrations. Automating these actions removes the slow, error-prone work of configuring them server by server, so your team moves more servers with less hands-on effort.


Post-launch actions run through AWS Systems Manager (SSM) immediately after test or cutover launch. You can use predefined actions or bring your own SSM document. For source server bulk configurations, the migration inventory file now includes a new structure for post-launch actions, making it easier to review and modify actions per source server.


The AWS Transform for migrations agent automates your migration configuration end to end, including replication templates, EC2 launch templates, EC2 right-sizing, and post-launch actions, with the flexibility to create and edit any of these at the source server level.


This new capability is available in all AWS Regions where[AWS Transform is offered](https://docs.aws.amazon.com/transform/latest/userguide/transform-vmware-connect-target-account.html) .


To learn more, please visit the[AWS Transform User Guide](https://docs.aws.amazon.com/transform/latest/userguide/transform-vmware-migrate-servers.html#transform-vmware-ms-prereqs-and-defaults) .
