---
schema_version: "1.0.0"
document_id: "6dfb55d79f0935c2b2217c648ae812bf6dad0f9ba03e9f38ccf19c935b3f840a"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-rds-mariadb-1232-available/"
published_at: "2026-08-11T08:54:00+00:00"
first_seen_at: "2026-08-11T17:03:23.922129+00:00"
fetched_at: "2026-08-11T17:03:27.183313+00:00"
content_hash: "sha256:aa6250ec89245ef799b81ca8860c94564212f567c84944ec4fe5340f35ca68a0"
---

# Amazon RDS for MariaDB now supports MariaDB 12.3

Starting today, Amazon RDS for MariaDB supports MariaDB major version 12.3, the latest Long-Term Support release from the MariaDB community. This release supports MariaDB 12.3.2 minor version.


MariaDB 12.3 includes Oracle TO_DATE() function compatibility, reducing the code changes needed when migrating applications from Oracle to MariaDB. It adds an IS JSON predicate, so you can validate JSON documents natively in the database rather than in application code. The query optimizer now handles reorderable LEFT JOIN statements and ordered scans over RANGE partitions more efficiently, improving performance for these queries without application changes. For more details, refer to the[MariaDB 12.3 release notes](https://mariadb.com/docs/release-notes/community-server/12.3) and[RDS MariaDB release notes](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MariaDB.Concepts.VersionMgmt.html) .


You can upgrade your database using[Amazon RDS Blue/Green Deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/blue-green-deployments.html) , in-place upgrade, or restore from a snapshot. Learn more about performing major version upgrades in the[Amazon RDS User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.MariaDB.html) . You can also migrate to RDS for MariaDB 12.3 from external MariaDB sources using[AWS Database Migration Service](https://aws.amazon.com/dms/) .


Amazon RDS for MariaDB makes it simple to set up, operate, and scale MariaDB deployments in the cloud. Learn more about pricing details and regional availability at[Amazon RDS for MariaDB](https://aws.amazon.com/rds/mariadb/pricing/) . Create or update a fully managed Amazon RDS for MariaDB database in the[Amazon RDS Management Console](https://console.aws.amazon.com/rds/home) .
