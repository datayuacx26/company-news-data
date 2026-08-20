---
schema_version: "1.0.0"
document_id: "7c6285bc65b0f9d3badae748fcc69ecc91834a0a001a527a1b2cb0c17e0f0b58"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/announcing-support-for-databricks-delta-uniform-tables"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:01abcdfd1c6a6773794d81e3a25e5ff71e9cbf9562de38cff89d971729846d14"
---

# Announcing support for Databricks Delta UniForm tables

Today we are announcing read and write support for Databricks Delta UniForm. You can now use Polytomic to both ETL/ELT data from any sources into Databricks as Delta UniForm tables and Reverse ETL from Delta UniForm tables into any other systems and applications.


## What is Delta UniForm?


[Delta UniForm](https://www.databricks.com/blog/delta-uniform-universal-format-lakehouse-interoperability) ('Universal Format') is a data lakehouse storage format. Rather than being yet another distinct format, it's a unified format that enables compatibility with three existing lakehouse formats, thus avoiding the need to commit to a single one:


-[Delta Lake](https://delta.io/)
-[Apache Iceberg](https://iceberg.apache.org/)
-[Apache Hudi](https://hudi.apache.org/)


Delta UniForm provides this universal compatibility without creating additional copies of your data: a single copy of your data is made compatible with the above formats. This compatibility is achieved at by augmenting the meatadata layer for the underlying[Apache Parquet](https://parquet.apache.org/) files.


## Enabling Delta UniForm support in Polytomic


To enable this, simply check the **Enable Delta UniForm tables** setting on your Polytomic Databricks connection. That's it 😊.


As always, email us at support@polytomic.com with any questions.


‍


[Back to blog](https://www.polytomic.com/blog)
