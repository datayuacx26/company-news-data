---
schema_version: "1.0.0"
document_id: "fc7f7ebb0ccb48176a51775ccd63df9c35b4ab262cc00f2ef5c24e4bb6931568"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/hydra-joins-supabase"
published_at: "2026-02-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:21:08.932956+00:00"
content_hash: "sha256:e6a2db412aff408eca2b36ab32911f3d69b2665c5a5038d2610f82a984682ece"
---

# Hydra joins Supabase

Today we're welcoming[Joe](https://x.com/JoeSciarrino) , the co-creator of Hydra, to the Supabase team. He is joining us to build Supabase Warehouse: an open data warehouse architecture for developers.


## Hydra + pg_duckdb#


Hydra co-developed[pg_duckdb](https://github.com/duckdb/pg_duckdb) , a popular open source (MIT-licensed) extension that accelerates analytics queries on Postgres by over 600x.


### Analytics on Postgres#


Joe's work on Hydra pioneered the modern lakebase architecture before most even considered it possible.


Last year, Supabase started looking at warehousing, announcing Analytics Buckets to store data in Iceberg format in S3. This year, Hydra and` pg_duckdb` will make it easier than ever to get fast analytics on large data volumes in Postgres.


Joe will oversee our Open Warehouse Architecture initiative and help push our Postgres + Analytics roadmap forward.


### Open Warehouse Architecture#


We're building an open warehouse architecture that keeps Postgres at the center while unlocking modern analytics workflows.


This effort includes:


- A tighter integration between Postgres and object storage.
- Serverless analytics workflows that feel native to Supabase.
- A foundation for open table formats and analytics-first tooling.


### Next Steps for Hydra#


Everything will stay open source. We'll help[MotherDuck](https://motherduck.com/) with` pg_duckdb` contributions and take over the maintenance of Hydra's existing repos. We will work in the open as we develop the Open Warehouse Architecture and the broader Postgres + Analytics vision.


## Join us to build the Open Warehouse#


We are assembling a team to build the Open Warehouse Architecture on Supabase. If you are a C++ programmer or storage engineer and want to get involved,[apply online](https://supabase.com/careers) .
