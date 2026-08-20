---
schema_version: "1.0.0"
document_id: "0bbf12405f5e9b66418e36aad4a31bd425145ddbf78e51d05b54b9113819221b"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/sql-maxis-constructing-json-objects-in-athena-using-sql/"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:1eec75be653d00657572a377f656cee6ba83ab4677c18d95e93bb5dc8274667d"
---

# SQL Maxis: Constructing JSON objects in Athena using SQL

*This is the second post in our SQL Maxis series — in which we explain how we do Hard things relatively easily using SQL.*


We recently launched our import product, which allows platforms to ingest data from their customers’ databases, data warehouses, data lakes, lakehouses, etc. When building the product, we made the decision to schema-tenant the data in the import destinations (i.e., in the data platforms receiving the data), such that a different schema would be created for each of the recipient’s customers. Schema tenanting ensures that imported raw data is isolated without risks of table name or type conflicts.


We also decided to borrow patterns from[Lakehouse Architectures](https://www.cidrdb.org/cidr2021/papers/cidr2021_paper17.pdf) . Specifically, we follow[Write-Audit-Publish](https://medium.com/everything-full-stack/action-position-data-quality-assessment-framework-d833f6b77b7) and[Medallion Architecture](https://dataengineering.wiki/Concepts/Medallion+Architecture) patterns. We write raw data to tenant schemas (one for each of our customer’s customers). The individual tenant schemas are used as bronze layers. For each model we sync, we create a corresponding bronze layer table with only a primary key,` last_updated_at` , and` json` column. The` json` column contains all the data for a given row in the source system. We publish data after validation by adding a partition to a unified table in a centralized` prequel_imports` schema. This schema represents our silver layer. We then create another view in the silver layer over the unioned table. The view explodes each field in the` json` column into its own typed column. Our customer can then build gold layer views/tables on top of this with their preferred tooling.


## Converting Rows to JSON in Athena


In order for the above to work, we need to coerce each row of the source data into a JSON object that we can insert into the destination as its own column. The naive way to approach that would be to construct that object in our application layer, but this would have both performance and cost implications. Our transfer workers are currently single process workers, and introducing a lot of JSON serialization to the process would drastically increase how CPU-bound our load is vs. IO-bound.


We realized that for many sources, like Athena, our customers’ customers (the data provider) are billed on the volume of data scanned, not on compute usage. These warehouses are also distributed systems with native parallel processing ability. Because of this, it would be faster and cheaper to have Athena serialize the rows to JSON.


While this sounds great in theory, it’s not trivial to figure out how to get Athena to create that JSON. Athena lacks a simple helper function like Postgres’` json_build_object` . We tried to look for alternatives in the AWS docs, but found the documentation lacking. Luckily, Athena is a fork of[Trino](https://trino.io/) and[their docs](https://trino.io/docs/current/functions/json.html?highlight=json#) are substantially easier to reason about. If you are looking to build your own JSON objects in Athena, the key is creating a dynamic struct and casting that struct to JSON.


```text
SELECT
CAST  (
CAST  (
ROW  (id,   name  , created_at)   AS   ROW  (id   BIGINT  ,   name   VARCHAR  , created_at   TIMESTAMP  )
)   AS   JSON
)   AS   json_row
FROM   source_table;
```


In this code block, we’re first (in the inner-most` CAST` ) creating a typed row object using the Athena` ROW` keyword. Next, we’re casting that row to JSON. After both casts, a table of JSON rows is created with one JSON object per row.


Optionally, wrap the outermost` CAST` expression in the` JSON_FORMAT` function if you need to serialize` json_row` to a JSON string. Athena can only work with the JSON type dynamically and cannot save JSON types to many formats (such as Parquet) — in many cases, JSON data needs to be serialized to a string before persisting.


## Conclusion


Once again SQL is king, and our problem can be solved without adding more weight to our application layer. By putting in a bit of extra effort to learn the toolchain, we’ve walked away with a faster and cheaper solution than we would have had otherwise. As a final note, this was only tested with Athena 3 so ensure your[workgroup](https://docs.aws.amazon.com/athena/latest/ug/user-created-workgroups.html) is set to use[Athena engine version 3](https://docs.aws.amazon.com/athena/latest/ug/engine-versions-reference-0003.html) before attempting.
