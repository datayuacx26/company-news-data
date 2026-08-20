---
schema_version: "1.0.0"
document_id: "e7e7197ad5e7772c81fea99df66253bd64426cfcb45b59dd24e92d87ad064399"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/how-fast-is-iceberg-on-snowflake/"
published_at: "2024-10-30T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:c47d01a878ff10d290902526a84ab30fcc04b332b8c3745c02c2dc8de1b0c98e"
---

# How fast is Iceberg on Snowflake?

Iceberg is an open-source data storage format that has been gaining support among data platforms. As of recently, reading from Iceberg is at least partially supported by all of the major data warehouses, and new usability features and performance improvements are shipping every quarter.


Iceberg and other open table formats promise a number of benefits around things like multi-engine querying, data sharing, and simplified data pipelines – but querying from Parquet files on S3 existed long before Iceberg – is Iceberg actually better?


We wanted to explore exactly how Iceberg storage compares to external storage when queried from one of the leading data warehouses, so we ran the test. Here are the results:


## Preparing the data


We used DuckDB to generate a TPC-DS dataset of 100GB (scale factor: 100). This dataset was then mounted or loaded to Snowflake in 4 configurations, with no sorting or clustering:


- As external tables, backed by Parquet on S3 (following the recommended Snowflake Parquet file sizes and row group sizes).
- As Iceberg tables, backed by Parquet on S3 using the AWS Glue catalog
- As Iceberg tables, backed by Parquet on an external S3 volume using the Snowflake catalog
- (Bonus) As native tables in Snowflake


## Querying the data


We created a new Snowflake warehouse (size: small) and ran the 99 queries in the TCP-DS benchmark against the four datasets. We ensured all data & querying was colocated in us-east-1, and we disabled all caching.


## Results of Iceberg vs. external tables


In the chart below, you can see the high-level summary of our results. Iceberg tables were significantly more performant than external tables.


Snowflake released GA support for Iceberg Tables in June 2024 – only 4 months before this benchmark. The performance of Iceberg tables (even externally managed) vs. external tables is very impressive – both for Iceberg as a storage format and Snowflake’s implementation.


## Bonus – Iceberg vs. native tables


While this test was designed with external querying use cases in mind, we thought it’d be interesting to run the test against native Snowflake storage.


Native Snowflake tables performed better than Iceberg, but the margin is significantly smaller than between Iceberg and external tables. Given recent development momentum and Snowflake’s stated goals around Iceberg, it seems likely that the performance of Iceberg on Snowflake will only continue to converge toward the native Snowflake storage performance.


However, this test was geared towards external storage and caching features were turned off (for both Iceberg & native storage). We’re planning a follow up post that is focused on the differences between Iceberg vs. native storage formats for other types of workloads.


## What does this mean?


We find these results extremely exciting because they validate one of the core value propositions of open storage formats: query engines can query this data with much better performance than existing external storage architectures.


Based on these results, there are almost certainly a number of obvious opportunities for more streamlined architecture & cost-saving in areas such as data loading, multi-engine querying, and data sharing, just by leveraging Iceberg.


Iceberg and the other open storage formats are generating a lot of excitement in the data community – these results could mean that the hype is actually justified.
