---
schema_version: "1.0.0"
document_id: "592864de54d891cad78695a8ae2052a53c3e5cdcb8d59536320c076c26eaa38f"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/orioledb-launch"
published_at: "2024-12-01T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:58:43.171296+00:00"
content_hash: "sha256:a6c7a8170348dbf7c92f05a11aead8dbcd214dc7b4a2409091a7cbf911f40494"
---

# OrioleDB Public Alpha

# OrioleDB Public Alpha#


Today, we're releasing the Public Alpha of[OrioleDB](https://www.orioledb.com/) on the Supabase platform.


### What’s OrioleDB?#


OrioleDB is a **storage extension** which uses Postgres' pluggable storage system. It’s designed to be a drop-in replacement for Postgres’ default Heap storage.


You can read more about OrioleDB[here](https://www.orioledb.com/blog/orioledb-beta7-benchmarks) and learn why you might choose it over the default Postgres storage engine.


### Limitations#


This initial release is a Public Alpha and you should *not* use it for Production workloads. The release comes with several limitations:


- The release is restricted to Free organizations. You will not be able to upgrade OrioleDB projects to larger instance sizes. If you want to run OrioleDB on a larger instance we suggest following the[Getting Started](https://www.orioledb.com/docs/usage/getting-started) guide on OrioleDB’s official website.
- Index support is restricted to the Postgres default B-Tree index type. Other indexs like GIN/GiST/BRIN/Hash, and pgvector's HNSW/IVFFlat are not supported.


### Should you use it today?#


At this stage, the goal of adding OrioleDB to the platform is to make it easier for testers to give feedback. If you’re running Production workloads, stick to the standard options available.


### Getting started and more info#


To get started today, go to[database.new](http://database.new/) and choose “Postgres with OrioleDB” under the "Advanced Configuration" section when launching a new database.


If you want to learn more about OrioleDB and their vision for the future, check out the[blog post the OrioleDB team released today](https://www.orioledb.com/blog/orioledb-beta7-benchmarks) .
