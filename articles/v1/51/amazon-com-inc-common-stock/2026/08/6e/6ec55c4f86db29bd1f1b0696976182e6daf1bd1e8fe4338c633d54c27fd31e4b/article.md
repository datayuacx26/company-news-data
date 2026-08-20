---
schema_version: "1.0.0"
document_id: "6ec55c4f86db29bd1f1b0696976182e6daf1bd1e8fe4338c633d54c27fd31e4b"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/rds-sqlserver-publish-sql-audit-to-cw/"
published_at: "2026-08-04T07:00:00+00:00"
first_seen_at: "2026-08-04T23:02:13.139824+00:00"
fetched_at: "2026-08-04T23:10:21.652752+00:00"
content_hash: "sha256:9d0c793824ff9bdd0d0293ee9aff8dab79cc578a253e26459adf1697f87fc641"
---

# RDS SQL Server now supports publishing SQL Server Audit logs to CloudWatch

[Amazon Relational Database Service (Amazon RDS) for SQL Server](https://aws.amazon.com/rds/sqlserver/) now supports publishing SQL Server Audit logs to CloudWatch. SQL Server Audit is a native SQL Server feature that allows tracking and logging events that occur on the Database Engine. On Amazon RDS, you can create audits and audit specifications in the same way that you create them for on-premises SQL Server database servers.


Now you can publish the audit logs to S3, CloudWatch, or both. If you enable both S3 and CloudWatch options, the audit log publication will be marked as "completed" only after the audit log files are uploaded to both S3 and CloudWatch. Once the audit logs are in CloudWatch, you can perform real-time analysis of the log data. If you enable retention, RDS keeps your audit logs on your DB instance for the configured period of time.


For more information, see[SQL Server Audit (database engine)](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) in the SQL Server documentation. For detailed configuration instructions, see the[Amazon RDS for SQL Server User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.SQLServer.Options.Audit.html) . This feature is available in all AWS Commercial and AWS GovCloud (US) Regions where Amazon RDS for SQL Server is available.
