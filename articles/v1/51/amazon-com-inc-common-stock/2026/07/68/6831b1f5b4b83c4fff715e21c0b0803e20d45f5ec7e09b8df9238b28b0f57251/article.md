---
schema_version: "1.0.0"
document_id: "6831b1f5b4b83c4fff715e21c0b0803e20d45f5ec7e09b8df9238b28b0f57251"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-rds-supports-latest-cu-gdr-microsoft-sql-server"
published_at: "2026-07-22T07:45:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:34340498a948c34691fd93ecd755fd2b0ca8b9adffa45b50f6f7f86feccc1969"
---

# Amazon RDS now supports the latest CU and GDR updates for Microsoft SQL Server

[Amazon Relational Database Service (Amazon RDS) for SQL Server](https://aws.amazon.com/rds/sqlserver/) now supports the latest Cumulative Updates (CU) and General Distribution Release (GDR) updates for Microsoft SQL Server. This release includes support for Microsoft SQL Server 2016 SP3+GDR[KB5089271](https://support.microsoft.com/help/5089271) (RDS version 13.00.6490.1.v1), SQL Server 2017 CU31+GDR[KB5090354](https://support.microsoft.com/help/5090354) (RDS version 14.00.3530.2.v1), SQL Server 2019 CU32+GDR[KB5090407](https://support.microsoft.com/help/5090407) (RDS version 15.00.4470.1.v1) and SQL Server 2022 CU25[KB5081477](https://learn.microsoft.com/en-us/troubleshoot/sql/releases/sqlserver-2022/cumulativeupdate25) (RDS version 16.00.4255.1.v1).


The GDR updates address vulnerabilities described in CVE-2026-40370. For additional information on the improvements and fixes included in these updates, see Microsoft documentation for KB5089271, KB5090354, KB5090407 and KB5081477. We recommend that you upgrade your Amazon RDS for SQL Server instances to apply these updates using Amazon RDS Management Console, or by using the AWS SDK or CLI. You can learn more about upgrading your database instance in the[Amazon RDS SQL Server User Guide for upgrading your RDS Microsoft SQL Server DB engine](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.SQLServer.html) .
