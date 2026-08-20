---
schema_version: "1.0.0"
document_id: "2eee254e409ecf55dd54cf2a4670e62afe82597ac1bdfc0394336a13b48f97a4"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-rds-aurora-r8gd-m8gd-regions/"
published_at: "2026-07-15T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:0f8df42125f8bb673833d0a3279ad6fd52294bdbec26cfa5c5f15048c61e1ec3"
---

# Amazon RDS and Aurora expand R8gd and M8gd to additional Regions

Amazon Relational Database Service (RDS) now supports R8gd database instances in 12 additional regions and and M8gd database instances in 6 additional Regions with Optimized Reads for Amazon Aurora PostgreSQL, RDS for PostgreSQL, RDS for MySQL, and RDS for MariaDB.


R8gd and M8gd instances deliver up to 165% better throughput and up to 120% better price-performance over R6g instances for Aurora PostgreSQL. Optimized Reads uses local NVMe-based SSD block storage to store ephemeral data such as temporary tables, reducing network storage access and improving query latency. The result is improved query performance for complex queries and faster index rebuild operations. Aurora PostgreSQL Optimized Reads instances using the I/O-Optimized configuration also use the local storage to extend their caching capacity. Database pages that are evicted from the in-memory buffer cache are cached in local storage to speed subsequent retrieval of that data.


Customers can get started with Optimized Reads through the AWS Management Console, CLI, and SDK by modifying their existing Aurora and RDS databases or creating a new database using R8gd or M8gd instances. R8gd instances are available in the following additional regions: Europe (Ireland), Asia Pacific (Seoul), Asia Pacific (Malaysia), Europe (London), US West (N. California), Asia Pacific (Sydney), Canada (Central), Asia Pacific (Jakarta), Africa (Cape Town), Canada West (Calgary), South America (Sao Paulo) and Asia Pacific (Hong Kong). M8gd instances are available in the following additional regions: Europe (Ireland), Asia Pacific (Malaysia), Europe (London), Asia Pacific (Sydney), South America (Sao Paulo) and Canada (Central). For complete information on pricing and regional availability, please refer to the[pricing page](https://aws.amazon.com/rds/pricing/) . For information on specific engine versions that support these DB instance types, please see the[Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.DBInstanceClass.SupportAurora.html) and[RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.Support.html) documentation. ****
