---
schema_version: "1.0.0"
document_id: "5e81d779c760b89700706038488e254680d044cefe82d6937986b4ffbeea246e"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/rds-sql-server-supports-tde-for-maz/"
published_at: "2026-07-27T07:00:00+00:00"
first_seen_at: "2026-07-27T23:03:42.191739+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:52a424420ec2b5e5da37b4422dc3ab3ecf0735bb8298e0f3806f86746d0eefeb"
---

# Amazon RDS for SQL Server now supports restoring TDE databases on Mult-AZ instances

[Amazon Relational Database Service (Amazon RDS) for SQL Server](https://aws.amazon.com/rds/sqlserver/) now supports restoring Transparent Data Encryption (TDE)-enabled SQL Server databases on Multi-AZ instances and instances configured with a read replica in the same region, using native backup and restore. Previously, TDE-enabled database restore was available only for Single-AZ instances, requiring you to disable TDE or migrate to a Single-AZ configuration before restoring encrypted databases.


You can restore TDE-enabled database backups directly to Amazon RDS for SQL Server Multi-AZ instances and instances configured with a read replica in the same region. Back up your existing TDE certificate, store it in Amazon S3, and restore it to your Amazon RDS instance with the TDE option enabled. Then, restore your TDE-enabled database backup from Amazon S3 using Amazon RDS native backup and restore. This simplifies your migration and recovery workflows when you require both encryption at rest with TDE and the high availability of Multi-AZ deployments.


This feature is available in all AWS Regions where Amazon RDS for SQL Server is supported. To learn more, see the[Amazon RDS for SQL Server User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.TDE.html) .
