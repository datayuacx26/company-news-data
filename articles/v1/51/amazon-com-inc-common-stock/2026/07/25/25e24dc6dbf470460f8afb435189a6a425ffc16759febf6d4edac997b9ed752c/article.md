---
schema_version: "1.0.0"
document_id: "25e24dc6dbf470460f8afb435189a6a425ffc16759febf6d4edac997b9ed752c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/postgresql-19-beta-2-amazon-rds-database-preview-environment/"
published_at: "2026-07-16T18:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:e935a29fa4de5f3639280aec1b53f20bba297d2579e5e572a0e927a4f784a67b"
---

# PostgreSQL 19 Beta 2 is now available in Amazon RDS Database Preview Environment

[Amazon RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/) 19 Beta 2 is now available in the[Amazon RDS Database Preview Environment](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/create-db-instance-in-preview-environment.html) , allowing you to evaluate the pre-release of PostgreSQL 19 on Amazon RDS for PostgreSQL. You can deploy PostgreSQL 19 Beta 2 in the Amazon RDS Database Preview Environment that has the benefits of a fully managed database.


PostgreSQL 19 introduces parallel autovacuum with configurable worker limits, so routine maintenance no longer bottlenecks large databases. The new REPACK CONCURRENTLY command rebuilds tables and reclaims storage online, keeping production databases accessible without third-party extensions. Native SQL Property Graph Queries (SQL/PGQ) let you express relationship traversals directly in standard SQL, eliminating separate application logic. Logical replication now synchronizes sequence values automatically and can be enabled dynamically without a server restart, reducing planned downtime. Beta 2 adds bug fixes and stability improvements from the Beta 1 testing period, including refinements to parallel autovacuum worker coordination and REPACK CONCURRENTLY lock handling. Please refer to[PostgreSQL community announcement](https://www.postgresql.org/about/news/postgresql-19-beta-2-released-3350/) for more details.


Amazon RDS Database Preview Environment database instances are retained for a maximum period of 60 days and are automatically deleted after the retention period. Amazon RDS database snapshots that are created in the preview environment can only be used to create or restore database instances within the preview environment. You can use the PostgreSQL dump and load functionality to import or export your databases from the preview environment.


Amazon RDS Database Preview Environment database instances are priced as per the[pricing](https://aws.amazon.com/rds/postgresql/pricing/) in the US East (Ohio) Region.
