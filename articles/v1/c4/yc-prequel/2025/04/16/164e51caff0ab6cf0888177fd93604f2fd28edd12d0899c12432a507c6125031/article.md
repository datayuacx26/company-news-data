---
schema_version: "1.0.0"
document_id: "164e51caff0ab6cf0888177fd93604f2fd28edd12d0899c12432a507c6125031"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/delta-lake-format/"
published_at: "2025-04-22T00:00:00+00:00"
first_seen_at: "2026-07-24T10:10:51.593417+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:99c00823d8d3fbaeab08b2fc5037c401fc1b04fbf639ef675595a14cb733c3b7"
---

# New: Send customers Delta Lake formatted data

We’ve seen a growing number of companies choose to land exported data in storage buckets instead of importing data directly into their databases. This approach has several advantages — compliance, different security primitives, etc. — but it also has one significant disadvantage: slower query times.


Starting today, Prequel customers can export[Delta Lake](https://docs.prequel.co/docs/destinations-delta-lake) formatted data to their customers’ object storage containers, allowing them to query exported data much faster than before.


## Availability


Delta Lake format is available on all the[object storage services we support](https://docs.prequel.co/docs/destinations-overview) , including S3, GCS, and R2. Delta files can be read performantly from most query engines, including Snowflake, BigQuery, Athena, and, of course, Databricks. To receive Delta Lake formatted data, companies simply check the Delta Format box when they sign up for data exports using Prequel’s[embeddable signup form](https://docs.prequel.co/docs/react-sdk-overview) .


## What is Delta Lake?


Delta Lake is an open-source table format that delivers high-performance data processing on object storage. Today, more than 10,000 companies worldwide use it, including more than 60% of the Fortune 500.


For more information about exporting data in Delta Lake format,[click here to set up a time to chat with us](https://www.prequel.co/request-product-demo) .
