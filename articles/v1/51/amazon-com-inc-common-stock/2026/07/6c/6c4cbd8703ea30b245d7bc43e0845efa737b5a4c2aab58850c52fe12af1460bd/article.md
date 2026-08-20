---
schema_version: "1.0.0"
document_id: "6c4cbd8703ea30b245d7bc43e0845efa737b5a4c2aab58850c52fe12af1460bd"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/ec2-dedicated-hosts-hrg/"
published_at: "2026-07-24T21:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:620e8a8cfc79ee90adc905c242e4fcab0f0e8cd643d5e4c7aa4277c1f541994b"
---

# Amazon EC2 Dedicated Hosts now support host resource groups without self-managed licenses

Starting today, customers can create Host Resource Groups (HRGs) for EC2 Dedicated Hosts without the previously required step of creating Self-Managed Licenses (SMLs) and associating AMIs through AWS License Manager.


This flexibility is particularly valuable for EC2 Mac Instance customers and for customers who need Dedicated Hosts for hardware-level isolation rather than Bring Your Own License (BYOL). Customers with BYOL workloads can continue to create HRGs with SMLs to ensure that only instances from associated AMIs can be launched on the host and track host-level license consumption.


To create an HRG without SML, uncheck the "Restrict to AMIs associated with self-managed license" option when creating a Host Resource Group in the EC2 Console, or set instance-launch-option to license-configuration-required via the AWS CLI.


This feature is available in all AWS Regions where Host Resource Groups are supported. To learn more, visit the[Host Resource Group User Guide](https://docs.aws.amazon.com/license-manager/latest/userguide/host-resource-groups.html)
