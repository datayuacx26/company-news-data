---
schema_version: "1.0.0"
document_id: "56ecf39423b297fbf27df568b6230fcdf01034777a6ab85142c0ef7d9a8d4410"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/postgresql-19-beta-3-amazon-rds-database-preview-environment/"
published_at: "2026-08-18T16:00:00+00:00"
first_seen_at: "2026-08-18T22:19:11.708664+00:00"
fetched_at: "2026-08-18T22:19:13.543416+00:00"
content_hash: "sha256:91ade26a05afa878088b4b03da00fb1c33e4b7f87a092362c84af3162d603c9f"
---

# PostgreSQL 19 Beta 3 is now available in Amazon RDS Database Preview Environment

Starting today,[Amazon RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/) 19 Beta 3 is available in the[Amazon RDS Database Preview Environment](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/create-db-instance-in-preview-environment.html) , allowing you to evaluate the pre-release of PostgreSQL 19 on Amazon RDS for PostgreSQL.


PostgreSQL 19 Beta 3 adds new capabilities for query performance and autovacuum management. The new pg_stat_autovacuum_scores view helps you monitor and tune autovacuum prioritization. Parallel autovacuum can now use multiple workers to speed up maintenance on large tables. The new pg_plan_advice module lets you lock in efficient query plans to avoid unexpected slowdowns. Eager aggregation improves analytical queries by grouping data earlier, so queries process fewer rows and complete faster. Beta 3 also includes bug fixes and stability improvements from the Beta 2 testing period. Refer to the[PostgreSQL community announcement](https://www.postgresql.org/about/news/postgresql-186-1711-1615-1519-1424-and-19-beta-3-released-3365/) for more details.


Amazon RDS Database Preview Environment database instances are retained for a maximum period of 60 days and are automatically deleted after the retention period. Amazon RDS database snapshots that are created in the Preview Environment can only be used to create or restore database instances within the Preview Environment. You can use the PostgreSQL dump and load functionality to import or export your databases from the Preview Environment. Amazon RDS Database Preview Environment database instances are priced as per the[pricing](https://aws.amazon.com/rds/postgresql/pricing/) in the US East (Ohio) Region.
