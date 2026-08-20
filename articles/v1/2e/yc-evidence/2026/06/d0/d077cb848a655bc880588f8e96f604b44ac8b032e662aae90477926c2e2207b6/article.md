---
schema_version: "1.0.0"
document_id: "d077cb848a655bc880588f8e96f604b44ac8b032e662aae90477926c2e2207b6"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/native-warehouse-support"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:4ee9413e295c2f846b4cd882f239e37d7da2790311c795cd56257a30375945c8"
---

# Native Warehouse Support in Evidence Studio

# Native Warehouse Support in Evidence Studio


*Evidence Studio now runs directly on your data warehouse.*


[Adam McAskill June 7, 2026 · 3 min read](https://www.linkedin.com/in/adam-mcaskill-74515720/)


Today, we’re launching native support for Snowflake, Microsoft Fabric, and BigQuery in Evidence Studio.


Evidence can now compile reports into warehouse-specific SQL and run those queries directly in your warehouse.


We’ve been working closely with several customers on this release, and we’re excited to share it more broadly.


For teams with Snowflake, Fabric, or BigQuery at the centre of their data stack, this is now our recommended architecture.


For teams without a data warehouse, or teams running on a data lake or a more complex collection of source systems, we continue to recommend our managed ClickHouse query engine.


## Support for dbt Environments


One of the most common patterns we see with our customers is to version control their Evidence project alongside their dbt project in the same repo.


They use the same coding agent to work on both the reporting and the modelling layers of their data stack, and they often coordinate changes across both in the same PR.


Native warehouse support improves this workflow significantly. Evidence can switch between dbt target environments while reports are being developed, enabling you to understand how changes to dbt models affect the reports that depend on them before those changes reach production.


## Native Row Level Security


Customers all told us the same thing: where possible, they want row and column level access policies managed natively in the warehouse. Each warehouse is different, but where possible Evidence pushes down the necessary user information to enforce row level security policies using the native policies in each of the warehouses we support.


## More Warehouses Coming


We’re launching with Snowflake, Microsoft Fabric, and BigQuery, but more warehouses are coming soon.


If there’s a warehouse you’d like us to support next, we’d love to hear from you.
