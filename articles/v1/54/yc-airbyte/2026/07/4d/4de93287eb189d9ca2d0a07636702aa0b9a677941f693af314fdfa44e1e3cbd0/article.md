---
schema_version: "1.0.0"
document_id: "4de93287eb189d9ca2d0a07636702aa0b9a677941f693af314fdfa44e1e3cbd0"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/direct-loading-on-certified-destinations"
published_at: "2026-07-07T18:13:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:346c0183cd3863cd1f2a9b3157aaf11faaae1ace865378ed5ed08ac2a024be24"
---

# Direct Loading Now Available For All Certified Destinations

We've been chasing a question for the past year: how fast can data movement get? Typing and deduping gave Airbyte destinations reliable, fully typed final tables you could trust. The next step was making that same guarantee cheaper and faster, by cutting out the staging tables and end-of-sync SQL that stood between your data and your queries.


That step is done. With the Databricks migration shipping this month, all 11 of Airbyte's certified destinations now run on our new Bulk Load CDK with Direct Load. Data arrives in your final tables typed, deduped, and queryable while the sync is still running. No raw tables. No end-of-sync compute. The biggest upgrade to Airbyte destinations since Destinations V2 is complete, and you don't have to change a thing to benefit from it.


If you sync to any of these destinations, it's already yours:


- Snowflake
- BigQuery
- Redshift
- Postgres
- S3
- S3 Data Lake
- GCS Data Lake
- Azure Blob Storage
- ClickHouse
- MSSQL
- Databricks


Here's what changed under the hood and why it matters for your syncs and your warehouse bill.


## **The old way: raw tables and a big SQL query at the end**


Since Destinations V2, Airbyte destinations worked in two phases. During a sync, the connector wrote your records as JSON into a raw table in the warehouse. At the end of the sync, it ran a SQL query (the "T+D" query) to cast that JSON into typed columns, deduplicate on your primary keys, and load the results into your final table.


This design had real advantages over what came before it. But it carried two costs that compounded as data volumes grew.


First, your warehouse did the heavy lifting. The typing and deduping query ran on your compute, against your bill, at the end of every sync. For large tables, that query was often the most expensive part of the pipeline.


Second, you stored everything twice. The raw tables persisted in your warehouse alongside the final tables, holding untyped, non-deduped copies of data you'd already paid to store in its final form.


## **The new way: typed data, loaded directly**


Direct Load moves type casting out of your warehouse and into the connector itself. The connector validates and casts each record as it processes it, then loads fully typed data straight into your final table. No persistent raw tables. No end-of-sync T+D query burning your compute.


The final tables are identical to what typing and deduping produced. Same structure, same columns, same` _airbyte_meta` error tracking for records that fail validation. What disappears is the intermediate machinery.


A few behaviors improve as a side effect:


**Records land sooner.** Append syncs now insert directly into your target table as the sync runs. Under the old architecture, records sat in the raw table and didn't reach your final table until the sync finished. For long-running syncs, you can now query fresh data while the sync is still in progress.


**Your warehouse bill goes down.** You're no longer paying compute for the T+D query or storage for permanent raw tables. Dedup syncs still use a working table during the sync, but it's deleted once the sync completes.


**Failed typecasts don't fail your data.** Records with values that can't be cast to the declared type are still delivered, with the error recorded in` _airbyte_meta` . The reason code changed from` DESTINATION_TYPECAST_ERROR to DESTINATION_SERIALIZATION_ERROR` , but if you follow our recommended query pattern for checking` _airbyte_meta` , nothing breaks.


## **One CDK, every certified destination**


The migration wasn't just a feature swap. Each of these destinations was rebuilt on the Bulk Load CDK, our Kotlin framework for high-throughput destination connectors. That's what made it possible to roll Direct Load out consistently across warehouses (Snowflake, BigQuery, Redshift), databases (Postgres, MSSQL, ClickHouse), object storage (S3, GCS, Azure Blob), lakehouse formats (S3 Data Lake, Databricks), and everything in between.


A shared framework also means improvements now land everywhere at once. When we optimize the load pipeline, add a delivery guarantee, or fix an edge case in schema handling, every certified destination picks it up. The days of Snowflake getting a feature six months before Redshift are over.


## **What you need to do**


For most people: nothing. If you query your final tables, your syncs keep working and your tables keep the same shape. You'll just see the raw tables stop accumulating.


If you built downstream models against the raw tables, enable the Legacy raw tables option on your destination connector to keep that workflow intact. If you need both raw and final tables, that now requires two destination connectors running parallel connections; see the[Direct Load documentation](https://docs.airbyte.com/platform/using-airbyte/core-concepts/direct-load-tables) for details.


One behavior change worth knowing: schema evolution is stricter. If a column's type changes and historical records can't be cast to the new type, the sync will surface the failure instead of silently rebuilding the table with a slow, expensive soft reset. You stay in control of how to resolve it.


## **Why this matters**


Destinations are the least glamorous part of a data pipeline and the most expensive place to get things wrong. Every inefficiency in how records land in your warehouse gets multiplied by every sync, every day, forever.


Moving all 11 certified destinations to Direct Load removes a whole layer of duplicate storage and warehouse compute from that path. Your data arrives typed, deduped, and queryable, and it gets there faster than it did before.


Check your destination's changelog for the version that introduced Direct Load, and read the[migration guide](https://docs.airbyte.com/platform/using-airbyte/core-concepts/direct-load-tables) if you rely on raw tables. Everyone else: enjoy the smaller bill.
