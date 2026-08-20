---
schema_version: "1.0.0"
document_id: "371ea369754195161de777926504de873cf5393808a5b198bee99e8fb9216506"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/analytics-buckets"
published_at: "2025-07-15T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:6982225c8fdf6550388bf07b2699f8d30d0efc2f35eac5fca3a00df32192109a"
---

# Supabase Analytics Buckets with Iceberg Support

Today we're launching **Supabase Analytics Buckets** in private alpha. These are a new kind of storage bucket optimized for analytics, with built-in support for the[Apache Iceberg](https://iceberg.apache.org/) table format.


Analytics buckets are integrated into Supabase Studio, power table-level views instead of raw files, and can be queried using the new **Supabase Iceberg Wrapper** , also launching in alpha.


## Why Iceberg#


Apache Iceberg is a high-performance, open table format for large-scale analytics on object storage. It brings the performance and features of a database to the flexibility of flat files.


We chose Iceberg for its bottomless data model (append-only, immutable history), built-in snapshotting and versioning (time travel), and support for schema evolution. Iceberg is also an open standard widely supported across the ecosystem. Supabase is committed to[open standards and portability](https://supabase.com/blog/open-data-standards-postgres-otel-iceberg) , and Iceberg aligns with that goal by enabling users to move data in and out without being locked into proprietary formats.


## Setting up Analytics Buckets#


Once your project has been accepted into the alpha release program, Analytics buckets can be created via Studio and the API. To create an analytics bucket, visit` Storage > New bucket` in Studio.


Analytics buckets are a separate bucket type from standard Supabase Storage buckets. You can't mix file types between the two.


They're stored in a new system table:` storage.buckets_iceberg` . These buckets are not included in the` storage.buckets` table and objects inside them are not shown in` storage.objects` . However, the` listBuckets()` endpoint returns a merged list of standard and analytics buckets for consistency with Studio and API consumers.


After creating the bucket, we're met with connection details. Copy the` WAREHOUSE` ,` VAULT_TOKEN` , and` CATALOG_URI` values and and create an Iceberg namespace and table using your preferred method. The example below uses pyiceberg to create a namespace` market` with table` prices` :


`
_38


import datetime


_38


import pyarrow as pa


_38


from pyiceberg.catalog.rest import RestCatalog


_38


from pyiceberg.exceptions import NamespaceAlreadyExistsError, TableAlreadyExistsError


_38


_38


# Define catalog connection details (replace variables)


_38


WAREHOUSE= ...


_38


VAULT_TOKEN = ...


_38


CATALOG_URI= ...


_38


_38


# Connect to Supabase Data Catalog


_38


catalog = RestCatalog(


_38


name="catalog",


_38


warehouse=WAREHOUSE,


_38


uri=CATALOG_URI,


_38


token=VAULT_TOKEN,


_38


)


_38


_38


# Schema and Table Names


_38


namespace_name = "market"


_38


table_name = "prices"


_38


_38


# Create default namespace


_38


catalog.create_namespace(namespace_name)


_38


_38


df = pa.table({


_38


"tenant_id": pa.array(\[\], type=pa.string()),


_38


"store_id": pa.array(\[\], type=pa.string()),


_38


"item_id": pa.array(\[\], type=pa.string()),


_38


"price": pa.array(\[\], type=pa.float64()),


_38


"timestamp": pa.array(\[\], type=pa.int64()),


_38


})


_38


_38


# Create an Iceberg table


_38


table = catalog.create_table(


_38


(namespace_name, table_name),


_38


schema=df.schema,


_38


)


`


Back in Studio, we can see the newly created our newly created Namespace with` 0/1 connected tables`


Click connect and select a` Target Schema` to map the Iceberg tables into. It is recommended to create a standalone schema for your tables. Do not use the` public` schema because that would expose your table over the project's REST API.


## Querying Analytics Buckets#


Viewing an analytics bucket in Supabase Studio redirects you to the Table Editor. Instead of exposing raw Parquet files, the system shows a table explorer, powered by the[Supabase Iceberg Wrapper](https://fdw.dev/catalog/) .


The wrapper exposes Iceberg tables through a SQL interface, so you can inspect and query your data using Studio, or any SQL IDE. This makes analytical data feel like a native part of your Supabase project.


In this case the corresponding SQL query to access the data would be


`
_10


select


_10


*


_10


from market_analytics.prices;


`


## Writing to Analytics Buckets#


Writing is a work in progress. For now, populate Analytics Buckets with your own ingestion pipeline or Iceberg-compatible tooling. We'll also add write capability to the Supabase Iceberg Wrapper as soon as write support lands in the upstream[iceberg-rust client library](https://github.com/apache/iceberg-rust) . This will complete the workflow of **write → store → query** , all inside Supabase.


Once live, this gives you analytical storage using open formats. As a bonus, Iceberg gets us time travel for free.


## Alpha Launch Limits#


Analytics Buckets are launching in private alpha with the following constraints:


- Two analytics buckets per project
- Up to five namespaces per bucket
- Ten tables per namespace
- Pricing will be announced in a few weeks
- You cannot store standard objects in analytics buckets


## Roadmap and What's Next#


This launch marks the first step toward full analytical capabilities in Supabase. Over the next few months, we'll introduce SQL catalog support so you can explore Iceberg table metadata directly from the database. Studio will also gain deeper integration for schema inspection, column-level filtering, and time travel queries. Our goal is to make Supabase a full-featured HTAP backend, where you can write, store, and query analytical data seamlessly.


## Try It Out#


[Join the waitlist here](https://forms.supabase.com/analytics-buckets) to get early access and start working with bottomless, time-travel-capable analytics data inside Supabase.
