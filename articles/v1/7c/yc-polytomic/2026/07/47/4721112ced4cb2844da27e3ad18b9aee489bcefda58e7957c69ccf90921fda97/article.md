---
schema_version: "1.0.0"
document_id: "4721112ced4cb2844da27e3ad18b9aee489bcefda58e7957c69ccf90921fda97"
company_key: "yc-polytomic"
company: "Polytomic"
source_id: "yc-polytomic-news-import-c5d7ea331448"
canonical_url: "https://www.polytomic.com/blog-posts/bidirectional-syncing-with-apache-parquet"
published_at: null
first_seen_at: "2026-07-25T19:31:15.738590+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:9f15af9006aa79b31ede2d5b0d71371e258b25d3eb8bc44a8b69d589885df53b"
---

# Bidirectional syncing with Apache Parquet

Starting today, Polytomic supports syncing both to and from[Parquet](https://parquet.apache.org/) files across all three major cloud storage providers:


- Amazon S3
- Azure Blob Storage
- Google Cloud Storage


## Bidirectional syncing with Parquet


Parquet's profile has been rising due to the increasing popularity of[data lakehouse](https://en.wikipedia.org/wiki/Data_lake#Data_lakehouses) architectures. Polytomic's support for Parquet is thus comprehensive, supporting **bidirectional syncing** with Parquet and any other system:


- Sync from your CRMs, SaaS applications, data warehouses, databases, spreadsheets, and arbitrary APIs to Parquet files on any cloud storage provider.
- Sync from your Parquet files to CRMs, SaaS applications, data warehouses, databases, spreadsheets, and arbitrary webhooks.


## Multi-file archive support


Polytomic's Parquet support extends to supporting multi-file archives. That is, if a table's data is split across multiple Parquet files, Polytomic has the ability to treat all those files as one table.


For example, if you're syncing from Parquet, all you have to do is select the **Multiple files** option in your Polytomic model editor:


## Documentation


More details on Parquet support can be found in our documentation:[https://docs.polytomic.com/docs/parquet](https://docs.polytomic.com/docs/parquet) .


As always, please email us atsupport@polytomic.com with any questions.


[Back to blog](https://www.polytomic.com/blog)
