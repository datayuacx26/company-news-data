---
schema_version: "1.0.0"
document_id: "f942b550b846a3dca0cc0ba4f08ed01c71ee566051c2e32a4f587c9f3b63e457"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-rds-upto-four-storage-modifications-24-hours"
published_at: "2026-07-15T16:15:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:2cb78641007818322778ec879f291f8c16ec931ed33ae7ec518236cb1009dde7"
---

# Amazon RDS now supports up to four storage modifications in 24 hours

Amazon RDS now allows up to four storage modifications per database instance within a rolling 24-hour window. These modifications let you increase the size, change the type, and adjust the performance of your RDS storage volumes. You can start a new modification right after storage optimization for the previous modification is complete without having to wait for the six-hour cool-off period to complete.


This enhancement improves operational agility for scaling storage capacity or adjusting performance during sudden data growth or unexpected workload spikes. With RDS storage modifications, you can modify your volumes without downtime, keeping applications running with minimal performance impact.


The feature is automatically enabled on all Amazon RDS for PostgreSQL, Amazon RDS for MariaDB, Amazon RDS for MySQL, Amazon RDS for Db2, Amazon RDS for Oracle, and Amazon RDS for Microsoft SQL Server instances in all commercial AWS Regions and the AWS GovCloud (US) Regions. To learn more, refer the[Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIOPS.StorageTypes.html) .
