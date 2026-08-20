---
schema_version: "1.0.0"
document_id: "06c40ab12317a9479460fb8c10e4bea4159e2de7575c95e811a0c9374582b3c4"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/vec2pg"
published_at: "2024-08-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:64bbb4b163a23c9ad6ab93f120eeb40863ab8d2df3eff3424f31f8dfca1a9c1a"
---

# vec2pg: Migrate to pgvector from Pinecone and Qdrant

[vec2pg](https://github.com/supabase-community/vec2pg) is a CLI utility for migrating data from vector databases to[Supabase](https://supabase.com/) , or any Postgres instance with[pgvector](https://github.com/pgvector/pgvector) .


## Objective#


Our goal with[https://github.com/supabase-community/vec2pg](https://github.com/supabase-community/vec2pg) is to create an easy on-ramp to efficiently copy your data from various vector databases into Postgres with associated ids and metadata. The data loads into a new schema with a table name that matches the source e.g.` vec2pg.<collection_name>` . That output table uses[pgvector's](https://github.com/pgvector/pgvector)` vector` type for the embedding/vector and the builtin` json` type for additional metadata.


Once loaded, the data can be manipulated using SQL to transform it into your preferred schema.


When migrating, be sure to increase your Supabase project's[disk size](https://supabase.com/dashboard/project/_/database/settings) so there is enough space for the vectors.


## Vendors#


At launch we support migrating to Postgres from[Pinecone](https://www.pinecone.io/) and[Qdrant](https://qdrant.tech/) . You can vote for additional providers in the[issue tracker](https://github.com/supabase-community/vec2pg/issues/6) and we'll reference that when deciding which vendor to support next.


Throughput when migrating workloads is measured in records-per-second and is dependent on a few factors:


- the resources of the source data
- the size of your Postgres instance
- network speed
- vector dimensionality
- metadata size


When throughput is mentioned, we assume a[Small](https://supabase.com/docs/guides/platform/compute-add-ons) Supabase Instance, a 300 Mbps network, 1024 dimensional vectors, and reasonable geographic colocation of the developer machine, the cloud hosted source DB, and the Postgres instance.


### Pinecone#


vec2pg copies entire Pinecone indexes without the need to manage namespaces. It will iterate through all namespaces in the specified index and has a column for the namespace in its Postgres output table.


Given the conditions noted above, expect 700-1100 records per second.


### Qdrant#


The` qdrant` subcommand supports migrating from cloud and locally hosted Qdrant instances.


Again, with the conditions mentioned above, Qdrant collections migrate at between 900 and 2500 records per second.


## Why Use Postgres/pgvector?#


The main reasons to use Postgres for your vector workloads are the same reasons you use Postgres for all of your other data. Postgres is performant, scalable, and secure. Its a well understood technology with a wide ecosystem of tools that support needs from early stage startups through to large scale enterprise.


A few game changing capabilities that are old hat for Postgres that haven't made their way to upstart vector DBs include:


### **Backups**#


Postgres has extensive supports for backups and point-in-time-recovery (PITR). If your vectors are included in your Postgres instance you get backup and restore functionality for free. Combining the data results in one fewer systems to maintain. Moreover, your relational workload and your vector workload are transactionally consistent with full referential integrity so you never get dangling records.


### **Row Security**#


[Row Level Security (RLS)](https://supabase.com/docs/guides/database/postgres/row-level-security) allows you to write a SQL expression to determine which users are allowed to insert/update/select individual rows.


For example


`
_10


create policy "Individuals can view their own todos."


_10


on public.todos


_10


for select


_10


using


_10


( ( select auth.uid() ) = user_id );


`


Allows users of Supabase APIs to update their own records in the` todos` table.


Since` vector` is just another column type in Postgres, you can write policies to ensure e.g. each tenant in your application can only access their own records. That security is enforced at the database level so you can be confident each tenant only sees their own data without repeating that logic all over API endpoint code or in your client application.


### **Performance**#


pgvector has world class performance in terms of raw throughput and dominates in performance per dollar. Check out some of our prior blog posts for more information on functionality and performance:


- [What's new in pgvector v0.7.0](https://supabase.com/blog/pgvector-0-7-0)
- [pgvector 0.6.0: 30x faster with parallel index builds](https://supabase.com/blog/pgvector-fast-builds)
- [Matryoshka embeddings: faster OpenAI vector search using Adaptive Retrieval](https://supabase.com/blog/matryoshka-embeddings)


Keep an eye out for our upcoming post directly comparing pgvector with Pinecone Serverless.


## Next Steps#


To get started, head over to the[vec2pg GitHub Page](https://github.com/supabase-community/vec2pg) , or if you're comfortable with CLI help guides, you can install it using` pip` :


`
_10


pip install vec2pg


`


If your current vector database vendor isn't supported, be sure to weigh in on the[vendor support issue](https://github.com/supabase-community/vec2pg/issues/6) .
