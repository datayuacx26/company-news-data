---
schema_version: "1.0.0"
document_id: "296135038f58e097472ab2cd14519239a52e701ff649cdb80bdb8100d3cc4cf6"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/influxdb-3-10/"
published_at: "2026-06-17T12:00:00+00:00"
first_seen_at: "2026-07-20T23:23:58.693982+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:a01fd36429893c0d49a3faa4bcc07b52e19ef0d28907755f593577349a4161cd"
---

# What’s New in InfluxDB 3.10: Performance Beta Expanded with New Enterprise Features

Table of Contents


In our last release, we introduced a beta of performance updates designed for heavier, more complex time series workloads. InfluxDB 3.10 expands that beta to include enterprise features that give teams more control as they scale and manage larger workloads in InfluxDB 3.


This release adds end-to-end backup and restore, row-level deletes, bulk import from Parquet, user management, and an RBAC preview to the previous performance beta. It also includes cross-database plugin queries, a new readiness endpoint, and compaction improvements for InfluxDB 3 Enterprise. Together, these updates help teams evaluate the next phase of InfluxDB 3 performance and scale with more of the operational tooling they need to manage real workloads.


We’re inviting customers to test this next phase of InfluxDB 3 performance and scale, share feedback, and help shape the path to general availability.


## Expanded capabilities for the performance beta


The performance improvements[we previewed in InfluxDB 3.9](https://www.influxdata.com/blog/influxdb-3-9/?utm_source=website&utm_medium=direct&utm_campaign=influxdb_3_10_expanded_beta&utm_content=blog) continue to mature in beta. These updates are designed for teams testing heavier time series workloads, including higher ingest, wider schemas, sparse data, and more demanding recent-data queries.


The beta remains opt-in and is not yet the default, so existing deployments continue running unaffected unless teams explicitly enable it with the` --use-pacha-tree` flag.


Once users opt in to the beta, InfluxDB 3.10 adds operational capabilities, so teams can do more than test raw performance. They can protect data, recover from known-good states, remove bad or unnecessary rows, and bring existing Parquet datasets into InfluxDB 3 for evaluation. That matters for teams working with real production patterns, where data rarely arrives perfectly clean and workloads rarely stay fixed.


**The following capabilities are available only when using the performance beta, enabled with the -` -use-pacha-tree` flag** :


- **End-to-end backup and restore** : You can now run full backups that capture cluster state and compacted data for easy rollbacks. Restores run asynchronously, allowing you to recover data into a fresh store for disaster recovery or roll a live cluster back to an earlier point in time. We’ll soon be adding incremental restores to give you even more control over your restore points.
- **Row-level deletes** : Teams can now remove specific rows based on time ranges or tag predicates rather than dropping entire tables when data needs to be purged. The compactor applies these changes asynchronously in the background, allowing teams to clean up production data without interrupting operations.
- **Bulk import from Parquet** : A new bulk-import function makes it easier to bring historical or external data into InfluxDB 3. Teams can point InfluxDB at a generic Parquet file or an entire directory, use simple column mappings, and ingest each file as an independent import job.


## User authentication and RBAC preview


As time series workloads become more central to production systems, access control becomes increasingly important. The same database may support operators monitoring live systems, developers building applications, analysts exploring historical data, and automated services writing or transforming telemetry.


InfluxDB 3.10 Enterprise introduces a preview of multi-user authentication and role-based access control (RBAC). This feature is turned off by default for this release.


When enabled, operators can configure traditional username and password logins that issue JWTs, or opt for external identity management through OAuth and OIDC. 3.10 also introduces built-in roles, including Admin, Auditor, and Member, to enforce proper boundaries across your teams. Best of all, your existing token workflows will continue to work exactly as they do today without any breaking changes.


## General updates and improvements


InfluxDB 3.10 also includes several capabilities that work across all deployments to streamline data pipelines and improve cluster management.


#### Cross-Database Plugin Queries


Time series data often moves through stages: raw telemetry, cleaned data, downsampled rollups, forecasts, anomaly scores, and application-ready views. Those stages may live in different databases, but teams still need to connect them without building unnecessary external pipelines.


In 3.10, Processing Engine plugins are no longer restricted to querying their own database. Now, a plugin can query any database residing on that node. This unlocks read-from-one, write-to-another data pipelines, such as reading raw telemetry from a staging database and writing compacted rollups or machine learning forecasts to a production database.


#### Readiness Endpoint


Production deployments need health checks that reflect whether a node can actually serve traffic, not just whether a process is running. InfluxDB 3.10 adds a new /ready endpoint. Instead of a basic uptime check, this endpoint verifies whether the node can successfully reach its underlying object store, giving operators a more reliable signal for traffic routing.


#### Improved Compaction


Under heavy ingest, compaction needs to keep pace with incoming writes so query nodes can access optimized data. When compaction stalls, the path from raw writes to efficient queries slows down. InfluxDB 3.10 introduces parallel compaction, ensuring that query nodes access fully compacted data more quickly and serve queries faster.


## Get started with InfluxDB 3.10


InfluxDB 3.10 is available now. To get started, download the latest version or pull the newest Docker image for Core or Enterprise.


[InfluxDB 3 Core](https://www.influxdata.com/products/influxdb/?utm_source=website&utm_medium=direct&utm_campaign=influxdb_3_10_expanded_beta&utm_content=blog) remains free and open source under MIT and Apache 2 licenses, optimized for recent data and local workloads.[InfluxDB 3 Enterprise](https://www.influxdata.com/products/influxdb3-enterprise/?utm_source=website&utm_medium=direct&utm_campaign=influxdb_3_10_expanded_beta&utm_content=blog) adds long-range querying, clustering, advanced security, and full operational tooling for production deployments.


Check out the docs ([Core](https://docs.influxdata.com/influxdb3/core/release-notes/?utm_source=website&utm_medium=direct&utm_campaign=influxdb_3_10_expanded_beta&utm_content=blog) ,[Enterprise](https://docs.influxdata.com/influxdb3/enterprise/release-notes/?utm_source=website&utm_medium=direct&utm_campaign=influxdb_3_10_expanded_beta&utm_content=blog) ), try the release in your environment, and share your feedback in Discord or the Community Slack. We want your feedback as the performance beta continues to mature.
