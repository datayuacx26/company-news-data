---
schema_version: "1.0.0"
document_id: "7ee56eea4a001a95952758e0af43631bac6f6a7a21fb84c6cd4d8fccb20612d1"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/rds-sqlserver-supports-sqlserver-2025"
published_at: "2026-07-21T07:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:5248de328784b2af958f0c5cbbde2122f4739b034f113c34291b88fb3c9666ed"
---

# Amazon RDS for SQL Server now supports Microsoft SQL Server 2025

Amazon Relational Database Service (Amazon RDS) for SQL Server now supports Microsoft SQL Server 2025 for Enterprise, Standard, and Developer editions.


SQL Server 2025 brings AI integration directly into the database engine, enabling customers to invoke external REST endpoints from T-SQL without additional middleware. Customers running RDS for SQL Server can use this capability to integrate existing database workloads securely with AWS services such as Amazon Bedrock, Amazon SageMaker, Amazon S3, and AWS Lambda, without re-architecting applications. This enables scenarios such as AI-powered query advisor, automated performance analysis, event-driven workflows, and calling custom web services on Amazon EC2.


SQL Server 2025 introduces a new free edition for development and testing without licensing costs (Standard Developer Edition, or Dev-SE), and significant Standard Edition capacity increases up to 32 cores and 256 GB buffer pool memory. Standard Edition also gains Resource Governor, previously exclusive to Enterprise Edition. SQL Server 2025 also introduces a native vector data type for storing and querying vector embeddings directly within the database.


Customers running earlier SQL Server versions on RDS can[upgrade](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.SQLServer.html) to SQL Server 2025 by modifying the DB engine version, and customers running SQL Server on premises can migrate to take advantage of these capabilities with fully managed infrastructure. For more information, see the[Amazon RDS for SQL Server User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SQLServer.html) . See[Amazon RDS for SQL Server Pricing](https://aws.amazon.com/rds/sqlserver/pricing/) for up-to-date pricing and regional availability.
