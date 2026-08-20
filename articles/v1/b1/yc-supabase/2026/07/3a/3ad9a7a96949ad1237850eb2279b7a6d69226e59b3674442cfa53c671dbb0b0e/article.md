---
schema_version: "1.0.0"
document_id: "3ad9a7a96949ad1237850eb2279b7a6d69226e59b3674442cfa53c671dbb0b0e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/pipelines"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:44053f3207d9b44542961aea6464e54cb6edcf2eddbead136697a86bffa3436c"
---

# [Public Alpha] Supabase Pipelines

## What's new#


Supabase Pipelines is a managed change-data-capture service, powered by the open-source[Supabase ETL](https://github.com/supabase/etl) engine, that streams changes from your Supabase Postgres database to external destinations in near real time. You pick a destination in the Dashboard, and Supabase runs and monitors the pipeline that keeps it in sync, reading directly from the Postgres write-ahead log.


What you can do:


1. **Near real-time streaming** — a complete initial copy of your selected tables, then near real-time replication of inserts, updates, deletes, and truncates, with at-least-once delivery.
2. **Granular control over what's replicated** — publish specific tables, a whole schema, or all tables. Narrow to column subsets, filter rows with a` WHERE` clause, and handle partitioned tables.
3. **Automatic schema change support** — supported changes (adding, removing, and renaming columns, and changing nullability and defaults) are detected and applied to the destination automatically.
4. **Full Dashboard management** — create, start, stop, and restart pipelines, add or remove tables without restarting replication from scratch, and tune advanced settings like batch wait time, sync workers, and slot recovery.
5. **Monitoring** — track pipeline status, metrics, and logs from the Dashboard.
6. **Reliable recovery** — replication resumes from the last acknowledged position after a restart, detecting and recovering from many transient failures automatically while surfacing issues that require intervention.
7. **Workload isolation** — replicate to an analytical destination so heavy queries run there, not on your production database.


**Destinations:** BigQuery is the first destination available to everyone in public alpha. ClickHouse, Snowflake, and DuckLake are available on request through the[early access form](https://supabase.com/go/supabase-pipelines-new-destinations) .


Pricing during the public alpha: $0.053 per hour per active pipeline, $0.60/GB for the initial table copy, and $3/GB for replicated data after that.


## How to use it#


1. Open the Dashboard and choose the tables to replicate.
2. Pick a destination — BigQuery, at launch (or request ClickHouse, Snowflake, or DuckLake through the[early access form](https://supabase.com/go/supabase-pipelines-new-destinations) ).
3. Pipelines runs an initial copy of the selected tables, parallelized across and within tables for faster loading.
4. After the copy completes, the pipeline switches to streaming mode. New changes are read from the replication slot, batched, and written to the destination.
5. Supported schema changes are detected and applied to the destination automatically.


Data is replicated as-is, without transformation.


## Why we built this#


Postgres is excellent for transactional workloads: reading a user profile, inserting an order, updating a subscription, or serving your application.


Analytics workloads are different. They often scan large amounts of data, aggregate across many rows, and power dashboards, reports, notebooks, and downstream systems. Running those queries directly on your production database can add load to the same system your application depends on.


Supabase Pipelines gives you a reliable way to move production data into systems built for analytics, while keeping your application workload on Postgres.
