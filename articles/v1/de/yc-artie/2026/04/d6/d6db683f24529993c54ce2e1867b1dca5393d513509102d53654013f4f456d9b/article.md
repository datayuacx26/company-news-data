---
schema_version: "1.0.0"
document_id: "d6db683f24529993c54ce2e1867b1dca5393d513509102d53654013f4f456d9b"
company_key: "yc-artie"
company: "Artie"
source_id: "yc-artie-news-import-55a3a4fb236c"
canonical_url: "https://www.artie.com/blogs/stream-dynamodb-to-redshift-with-cdc"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-23T02:25:22.954541+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:8c0d7b789dc5782cf3086004d6f4bb7fd16e51e61921bd8a9baa074e90cbc1ef"
---

# Stream DynamoDB to Redshift with CDC

*This post is for data engineers and backend teams running DynamoDB in production who need SQL analytics on their NoSQL data. We assume basic familiarity with AWS and DynamoDB.*


## Why Replicate DynamoDB to Redshift


DynamoDB is great at what it does - fast key-value lookups at massive scale. But the moment someone asks "how many users signed up last week by region?" you're stuck. DynamoDB doesn't do SQL. It doesn't do joins. It doesn't do ad-hoc queries.


One of our customers runs a food ordering app on DynamoDB. Their table stores user profiles -` userId` ,` email` , a nested` preferences` map (favorite cuisine, dietary restrictions, dark mode), and a` lastActive` timestamp. The app reads and writes to this table thousands of times a second. DynamoDB handles that without breaking a sweat.


But when their PM asked for a cohort analysis - which users who signed up in January are still active? What's the average order value by cuisine preference? - they hit a wall. Answering that meant scanning the entire DynamoDB table, which is expensive (you're paying per read capacity unit) and slow. And they still couldn't join with their orders data in MySQL.


That's the core reason teams replicate DynamoDB to[Redshift](https://aws.amazon.com/redshift/) : to unlock SQL analytics on data that was never designed for it. Specifically:


- Run aggregations, cohort analysis, and funnel reports on DynamoDB data using SQL
- Join DynamoDB data with other sources (MySQL, Postgres) in a single query
- Stop running expensive table scans on DynamoDB for analytical workloads
- Build audit trails and compliance reports on change history


You get the best of both: DynamoDB for writes, Redshift for reads.


One honest caveat: if your DynamoDB table is small (say, under a few million rows) and you only need a report once a day, a nightly export to S3 might be all you need.[CDC](https://en.wikipedia.org/wiki/Change_data_capture) - change data capture - makes the most sense when your data changes frequently and you need analytics to stay fresh in near real-time.


## How DynamoDB CDC Works


[DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) is the CDC mechanism for DynamoDB. When you enable Streams on a table, DynamoDB captures every item-level change - inserts, updates, and deletes - as a stream record. Each record includes the change type (INSERT, MODIFY, REMOVE) and the item data.


We read these stream records, infer the schema, and replicate the data into Redshift. Here's how the pipeline looks:


DynamoDB


DynamoDB Table


►


DynamoDB Streams


►


Artie


DynamoDB Connector


►


Schema Inference


►


Redshift Sink


►


Redshift


Redshift


A few things worth knowing about how Streams works under the hood:


**Stream views** - When you enable Streams, you choose a view type.` NEW_IMAGE` gives you the item after each change.` NEW_AND_OLD_IMAGES` gives you both before and after. For replication,` NEW_IMAGE` is sufficient - we don't need old values to apply inserts, updates, and deletes. The view type is set when you enable Streams; changing it later requires recreating the stream.


**Shards** - DynamoDB Streams uses shards to parallelize delivery. We manage shard consumption and handle resharding automatically. When DynamoDB splits or merges shards, the pipeline continues without gaps. You don't need to think about this.


**24-hour retention** - This is the one you need to know. Stream records are retained for 24 hours. Under normal operation, we process well within that window (sub-minute latency). But if the pipeline is paused or falls behind for more than 24 hours, those records are gone and you'll need a backfill. More on that below.


**Stream reads are free from table capacity** - Reading from Streams uses separate throughput from your table. Your application's read and write performance is completely unaffected by the replication pipeline.


**Permissions** - We need[IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) credentials with` DescribeStream` and` GetRecords` permissions on the stream, plus` DescribeTable` on the table. Our[docs include sample IAM policies](https://artie.com/docs) you can copy.


## From Schema-Less to Structured Redshift


This is where DynamoDB-to-Redshift replication gets interesting - and different from replicating a relational database like MySQL or Postgres.


DynamoDB is schema-less. Every item can have different attributes. One user might have a` preferences` map with five keys; another might have two. A third might not have` preferences` at all. MySQL and Postgres have fixed schemas defined by[DDL](https://en.wikipedia.org/wiki/Data_definition_language) (data definition language - the` CREATE TABLE` and` ALTER TABLE` statements). DynamoDB has none of that.


Redshift, on the other hand, needs defined columns. So how do you replicate something with no schema into something that requires one?


We infer the schema from the stream records as they arrive. When a new attribute appears for the first time, we create a new column in Redshift automatically. No manual` ALTER TABLE` . No pipeline restart. It just works.


Here's what that looks like with the food ordering user profile:


We infer` userId` as VARCHAR,` email` as VARCHAR,` lastActive` as DECIMAL, and` preferences` as[SUPER](https://docs.aws.amazon.com/redshift/latest/dg/r_SUPER_type.html) - Redshift's semi-structured data type that handles nested JSON. If next week a user record shows up with a new` referralCode` attribute, we add that column automatically.


Here's how common DynamoDB types map to Redshift:


DynamoDB Type Redshift Type Notes


String VARCHAR


Number DECIMAL


Boolean BOOLEAN


Map (nested object) SUPER Query nested fields with dot notation or json_extract_path_text()


List (array) SUPER Query with Redshift array functions


StringSet / NumberSet SUPER (array) DynamoDB Sets become arrays


Binary VARBYTE


NULL NULL Omitted attributes become nullable columns


Once the data is in Redshift, you can query it like any other table:


And the real payoff - joining DynamoDB data with your relational data:


One trade-off to know: Redshift's SUPER type has depth limits for very deeply nested structures. If your DynamoDB items have maps nested more than 10 levels deep, you may need to flatten them or store them as VARCHAR. In practice, most DynamoDB data models don't nest that deeply, but it's worth checking yours.


## Getting Started and Backfills


Setting up the pipeline takes a few minutes:


1. Enable DynamoDB Streams on your table (choose` NEW_IMAGE` for the stream view type)
2. Connect your DynamoDB table in the Artie UI with your IAM credentials
3. Connect your Redshift cluster or Serverless endpoint
4. Deploy - we create the Redshift table and start streaming


For the initial load (getting all your existing data into Redshift before CDC takes over), we support two approaches:


**DynamoDB Export to S3** - If your table has[Point-in-Time Recovery (PITR)](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/PointInTimeRecovery.html) enabled, we can export a full snapshot to S3, load it into Redshift, and then seamlessly hand off to stream-based CDC. The export runs asynchronously and doesn't impact your table's performance. Changes that happen during the export are captured by Streams and applied after the initial load - no duplicates, no gaps.


**Parallel table scan** - For tables without PITR, we run a parallel scan to pull the initial dataset. This does consume read capacity, but we use parallel workers to minimize impact and get it done quickly.


Once the initial load is done, CDC takes over and keeps Redshift in sync with sub-minute latency.


**Common issues and fixes:**


Problem What to do


Streams not enabled Enable DynamoDB Streams on the table before connecting. Choose NEW_IMAGE or NEW_AND_OLD_IMAGES.


Pipeline fell behind 24+ hours Stream records are gone after 24 hours. Trigger a backfill from export or scan to catch up, then CDC resumes.


Deeply nested maps hitting SUPER limits Flatten structures beyond 10 levels, or store as VARCHAR and parse in queries.


Export to S3 failing Check S3 bucket permissions and DynamoDB table status. We retry automatically - monitor for persistent failures.


Redshift Serverless cold start Serverless clusters may cold-start after idle periods, adding a few seconds of latency. Provisioned clusters avoid this.


We also integrate with[Datadog](https://www.datadoghq.com/) for monitoring. Track replication lag, rows per second, and Redshift MERGE duration (MERGE is how CDC changes get applied to your Redshift tables - it matches incoming rows by primary key and inserts, updates, or deletes accordingly) from the Artie dashboard or your existing observability setup. Set alerts for replication lag approaching the 24-hour stream retention limit.


---


For[DynamoDB Global Tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html) : choose one replica as the CDC source. Items replicated from other regions also flow through that replica's stream, so you'll get full coverage. If the same item is modified in multiple regions simultaneously, deduplication may be needed.


---


## FAQ


### **Do I need DynamoDB Streams enabled?**


Yes. We use DynamoDB Streams for CDC - it's the only way to capture item-level changes in real time. Enable it on your table with the` NEW_IMAGE` view type. Streams have a[small cost per read request](https://aws.amazon.com/dynamodb/pricing/) ; check AWS pricing for your expected volume.


### **How do you handle schema-less DynamoDB data in Redshift?**


We infer the schema from stream records as they arrive. New attributes automatically become new columns. Nested maps and lists map to Redshift's SUPER type. No manual DDL required.


### **Does replication impact my DynamoDB table's performance?**


No. We read from DynamoDB Streams, which uses separate throughput from your table. Your application's read and write performance is unaffected.


### **How is this different from replicating MySQL or Postgres to Redshift?**


MySQL uses[binlog](https://dev.mysql.com/doc/refman/8.0/en/binary-log.html) (binary log - a record of all changes to the database). Postgres uses[WAL](https://www.postgresql.org/docs/current/wal-intro.html) (write-ahead log). Both have fixed schemas. DynamoDB uses Streams and has no schema at all, so we infer and evolve it automatically. The mechanics differ, but the outcome is the same: your data in Redshift, seconds behind production.


### **When should I NOT use CDC for DynamoDB to Redshift?**


If your table is small, changes infrequently, and you only need daily analytics, a nightly export to S3 loaded into Redshift might be simpler and cheaper. CDC is most valuable when your data changes frequently and your analytics need to stay fresh.
