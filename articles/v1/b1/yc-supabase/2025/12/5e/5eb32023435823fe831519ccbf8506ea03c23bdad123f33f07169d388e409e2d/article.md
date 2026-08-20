---
schema_version: "1.0.0"
document_id: "5eb32023435823fe831519ccbf8506ea03c23bdad123f33f07169d388e409e2d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/introducing-analytics-buckets"
published_at: "2025-12-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:4081d1499142992a134727d920c0ba9e96f8f6045189d363ca93a914ca557565"
---

# Introducing Analytics Buckets

Today Supabase is introducing Analytics Buckets, which you can use to store huge sets of data in Supabase Storage. Postgres is great for your app. But Postgres isn't designed for analytical workloads.


Analytics Buckets are a specialized storage type in Supabase designed for analytical workloads and built on[Apache Iceberg](https://iceberg.apache.org/) and Amazon S3. They store data in columnar Parquet format, which is optimized for scans, aggregations, and time-series queries.


Think of them as cold storage for your data, with a query engine attached.


Your hot transactional data stays in Postgres. Your historical data and analytical workloads live in Analytics Buckets. You query both using familiar tools.


## What do they do?#


Analytics Buckets give you:


-


**Cost-effective storage.** S3 pricing instead of database storage. Documented savings of 30-90% on storage costs for large datasets.


-


**Open table format.** Apache Iceberg means no vendor lock-in. Query your data from any compatible tool.


-


**Schema evolution.** Change your table schema without rewriting data.


-


**Time travel.** Query historical snapshots of your data. See what a table looked like at any point in time.


-


**Full audit history.** Every change is preserved. Track what changed, when, and how.


## When to use Analytics Buckets vs Postgres#


Analytics Buckets and Postgres are complementary. They serve different workloads.


### Keep data in Postgres when:#


-


You need low-latency reads for your application


-


Data changes frequently and consistency matters


-


Your dataset is small to medium size


-


You need real-time access from your app


### Use Analytics Buckets when:#


-


You are storing millions or billions of rows


-


You run heavy analytical queries that scan large tables


-


You need long-term retention at low cost


-


You want to query data from multiple tools


-


You need complete audit history and time travel


Many teams use both. Keep the last 90 days in Postgres. Archive everything to Analytics Buckets. Query historical data when needed.


## How they work#


Analytics Buckets use[Apache Iceberg](https://iceberg.apache.org/) , an open table format created for large analytical datasets. Here is what happens under the hood:


1.


Data is stored in Parquet files on S3


2.


Iceberg manages metadata including schema, partitions, and snapshots


3.


An Iceberg REST Catalog provides the interface for querying


4.


You connect using any Iceberg-compatible tool


The separation of compute and storage means you can scale each independently. Store petabytes of data and query only what you need.


## Creating an Analytics Bucket#


You can create an Analytics Bucket from the Dashboard or using the SDK.


### Using the Dashboard#


1.


Navigate to Storage in your Supabase Dashboard


2.


Click Create Bucket


3.


Enter a name for your bucket


4.


Select Analytics Bucket as the bucket type


5.


Click Create


You can use the Supabase Dashboard to define columns and set data types, including complex types like decimal with precision and scale. The foreign data wrapper schema will automatically be configured for you. Once your table is created, you can manage Analytics Buckets in the same way that you manage your Postgres tables.


### Using the SDK#


`
_10


import { createClient } from '@supabase/supabase-js'


_10


_10


const supabase = createClient('https://your-project.supabase.co', 'your-service-key')


_10


_10


await supabase.storage.createBucket('my-analytics-bucket', {


_10


type: 'ANALYTICS',


_10


})


`


## Connecting to Analytics Buckets#


Analytics Buckets require authentication with two services:


**Iceberg REST Catalog** manages metadata for your tables. It handles schema, partitions, and snapshots.


**S3-Compatible Storage** stores the actual data in Parquet format.


You authenticate using your Supabase service key for the catalog and S3 credentials for storage.


## Querying from Postgres#


You can query Analytics Buckets directly from Postgres using Foreign Data Wrappers. This lets you join hot data in Postgres with historical data in Analytics Buckets.


`
_17


-- Create a foreign server for your Iceberg data


_17


create server iceberg_server


_17


foreign data wrapper iceberg_wrapper


_17


options (


_17


aws_access_key_id 'your-access-key',


_17


aws_secret_access_key 'your-secret-key',


_17


region_name 'us-east-1'


_17


);


_17


_17


-- Import tables from your analytics bucket


_17


import foreign schema "analytics"


_17


from server iceberg_server


_17


into iceberg;


_17


_17


-- Query historical data


_17


select * from iceberg.events


_17


where event_timestamp > '2024-01-01';


`


## Data tiering pattern#


A common architecture is data tiering: keeping recent data in Postgres and archiving history to Analytics Buckets.


1.


Partition tables by time in Postgres, keeping a rolling window like the last 90 days


2.


Populate Analytics Buckets with your ingestion pipeline


3.


Drop old partitions from Postgres


4.


Query recent data from Postgres, historical data from Analytics Buckets


This keeps your Postgres database small and fast. Storage costs drop. Analytics queries run on data optimized for scans.


## Compatible tools#


Analytics Buckets work with any tool that supports the Iceberg REST Catalog API:


-


PyIceberg


-


Apache Spark


-


DuckDB


-


Amazon Athena


-


Trino


-


Apache Flink


-


Snowflake (via external tables)


-


BigQuery (via BigLake)


## Pricing and availability#


Analytics Buckets are free during the Private Alpha. Standard egress charges apply when you move data out of the region.


To request access, fill out the form at[forms.supabase.com/analytics-buckets](http://forms.supabase.com/analytics-buckets) .


## Get started#


1.


Request access to the Private Alpha


2.


Create an Analytics Bucket in the Dashboard


3.


Connect using PyIceberg, Spark, or your tool of choice


4.


Populate the bucket with your ingestion pipeline


Separate your transactional and analytical workloads. Keep Postgres fast. Store history at S3 prices. Query from any tool.


We are excited to see what you build.
