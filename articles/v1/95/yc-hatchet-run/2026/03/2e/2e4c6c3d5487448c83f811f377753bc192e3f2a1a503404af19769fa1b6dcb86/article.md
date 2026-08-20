---
schema_version: "1.0.0"
document_id: "2e4c6c3d5487448c83f811f377753bc192e3f2a1a503404af19769fa1b6dcb86"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/supertoast-tables"
published_at: "2026-03-06T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:f06477384e8f54f68ab2fb3f2b116708d650932bd2d2a6dcf128141a51fcb25b"
---

# Supertoast tables

Alexander Belanger, Matt Kaye


Postgres wranglers


Hatchet


The first rule of the “just use Postgres” club—of which we’re dedicated members—is simple. For web applications, Postgres should be the starting point for any data storage and retrieval problem.


The reasoning is straightforward: Postgres is a general-purpose database engine, you likely already run your core OLTP workload on Postgres, and you probably don’t have time to become an expert in specialized storage systems. This, coupled with improvements in hardware and Postgres performance, means you can start with Postgres for[task queues](https://hatchet.run/blog/multi-tenant-queues) or[message queues](https://github.com/pgmq/pgmq) ,[caching](https://dizzy.zone/2025/09/24/Redis-is-fast-Ill-cache-in-Postgres/) ,[vector embeddings](https://github.com/pgvector/pgvector) ,[search](https://github.com/paradedb/paradedb) and file storage.


This has been our[primary approach](https://news.ycombinator.com/item?id=39643136) with Hatchet, and so far it’s worked out. Every engineer is well-versed in writing Postgres schemas and queries from scratch, and we’re aware of the tradeoffs that we make in battling the Postgres query planner and the MVCC model.


But there’s a limit to this approach for every growing startup, and we finally reached it: storing large amounts of` jsonb` data in our Postgres instances. Here’s the story of our migration from` jsonb` columns and toast tables to what we affectionately call *supertoast* tables.


### Why jsonb?


The foundational data structure in Hatchet is the task queue; it’s what durable workflows, DAGs, events, and nearly every other feature is built on. Each task in the queue contains an input, and after completion, an output. These inputs and outputs are arbitrary JSON payloads which enter the system rapidly.


An additional constraint is that Hatchet is designed to be fast; it takes less than an average of 25ms (and in the optimistic case, as fast as 9ms) for a task to be sent to the engine and start running on a worker. This rules out a set of candidate options. Object stores are much too slow, and many hosted databases can be tricky to work with because networked disks have restrictive IOPS. NVMe disks are a great fit, and we already run most of our hosted infrastructure on NVMe-backed Postgres already!


So like pretty much everything else in our system, we persist these payloads to Postgres using the` jsonb` column type.


### The downsides of jsonb


The downsides are clear. Even small payloads can take up over 50% of our database storage, and larger payloads can take up well over 90%. But only payloads from very recent tasks are accessed frequently. Payload access follows a power law; payloads from over a day ago are very, very infrequently accessed. This leaves a good chunk of the database storage just sitting idle on our NVMe disks, which is not ideal from a cost-efficiency perspective and also bloats our backups.


What happens if our database starts to fill up rapidly? While NVMe disks give us fantastic IOPS, they’re not networked, which means that swapping a disk requires that we provision an entirely new database. To make matters worse, Hatchet is a very high-churn system, which means that our WAL is *very large* compared to a more traditional SaaS read-heavy workload. New databases can sometimes take many hours to provision, which can get scary as the database nears 100% of its storage capacity.


A less obvious issue is the impact of large payloads on autovacuum operations on the database. We started to see extremely long-running processes on our Postgres instances resembling the following:


Loading syntax highlighting...


Yes, that’s an autovacuum nearing 18 hours! And there are[a lot of ways to tune this](https://www.cybertec-postgresql.com/en/tuning-autovacuum-postgresql/) . But more interesting is the table it’s vacuuming: a TOAST table.


### What’s a TOAST table?


TOAST stands for **The Oversized-Attribute Storage Technique.** Postgres utilizes this technique for any row values larger than 2kb; these large values then become written across multiple chunks of the toast table.


Toast tables are managed by Postgres, but expose some functionality to the user. For example, you can override autovacuum settings on tables with the` toast.` prefix to tune autovacuum separately for these tables (by default, toast tables inherit their autovacuum settings from the tables, according to[this forum response](https://www.postgresql.org/message-id/20121024201728.GH6853@alvh.no-ip.org) ).


As we saw before, toast tables are expensive to traverse for autovacuum, leading to really high IOPS load on the database—this will be important later.


### The ideal: a supertoast table


To solve the problem of infrequently accessed payloads filling up our disk space, we’d prefer all of our hot payloads are stored in Postgres (primarily in toast tables), while cold payloads are offloaded to S3 with a reference stored in the database for full consistency. This would give us fast access for latency-sensitive workloads but flexible and cheap storage for older tasks, where latency isn’t a concern.


We’ll call this a` supertoast` table:


Loading syntax highlighting...


Note that this table is partitioned by daily[time-based partitions](https://hatchet.run/blog/postgres-partitioning) —we’ll see why this is important later on! Once we cross the threshold of 24 hours, we’re going to offload all of the existing payloads in a given partition onto S3, leaving only the pointer to the S3 bucket as the key.


### Hitting a WAL with offloading


The offloading job was trickier than we initially thought.


This is a time-delayed data replication system, so we reached for a natural data structure: modeling the offloads as a write-ahead log (WAL) of our supertoast table. The idea was that we’d have a job constantly running which would pop rows off the WAL which met some age criterion, send the payloads to S3, and then update the source record with the key we’d just written.


We built and shipped this very quickly, and it had notable problems:


1. High disk and CPU pressure due to autovacuum reclaiming an entire partition’s worth of dead tuples at least once during the offload (which also contributes to table and index bloat during the processing of the WAL itself); and
2. S3 can get very expensive very quickly if you don’t think about request volume, which we casually glossed over when shipping the first version of this. PUT requests (the primary request type) are billed at $0.005 per 1000 requests in most regions.


It also turns out that a WAL isn’t the ideal data model, because writes to S3 are heavily parallelizable (and as we’ll see in a second, should be batched), so we wanted to grab huge chunks of data where we could. In addition, we realized that update and delete operations in the WAL don’t matter in any practical sense; for updates, we could simply rewrite a payload inline, and deletes would be reclaimed by S3 lifecycle policies with data consistency as long as the supertoast reference row was deleted properly.


### Batching in S3


We quickly had to figure out a way to reduce the costs of PUT requests to S3; in our case, a separate PUT request for each row would cost us tens of thousands of dollars per month! To solve this problem, instead of writing each payload individually and storing its key, we’d essentially compress individual payloads and concatenate them into a single, larger file, and we’d store a pointer to the start index and the length of each payload for retrieval.


For example, if we have two payloads:


Loading syntax highlighting...


We’d combine these payloads into a single string (a file), like this:


Loading syntax highlighting...


And then we’d note when each payload started and its length, like this:


Loading syntax highlighting...


And finally, we’d create a key like this for Payload B, for example:


Loading syntax highlighting...


And we’d store this key in the database. This is a colon-delimited key, which we can unpack into three values: The object key in S3, the start index, and the length. With this, we can read only the relevant byte range of the larger object in S3, and once we’ve read those bytes, we simply decompress them and are back to the payload we started with.


The underlying idea here was we could cut down on the number of S3 operations we needed by many orders of magnitude, and ideally also get improved throughput on the application side by decreasing the overall number of writes (and, therefore, round-trips to S3) significantly as well.


### Write-and-swap


Instead of offloading data by reading from a WAL, we designed an approach which I’ll refer to here as **write-and-swap.** As we discussed before, our payloads table is partitioned by date, and we thought we could leverage this, in addition to some handy features of Postgres, to rewrite the job in a way that solved for all of the issues we were having.


Each day, a cron kicks off a payload processing job around 7:00AM EST which is intended to process the payloads that were written the previous day. We start the job at 7:00AM so that if anything goes wrong, we’re online to deal with it. The job has a few phases.


First, we create a new table, which is an empty copy of the partition from the previous day. We immediately manually create a` CHECK` constraint on this table which mimics the partition constraint for the partition we’re replicating to S3:


Loading syntax highlighting...


Next, we create a trigger on the source payload partition to replicate any writes into it to the new partition, so we don’t drop any data as we’re paginating and offloading data:


Loading syntax highlighting...


Once we’ve done that, we start processing payloads in batches. Each batch is processed in a few steps:


1. Read a record from a separate table which stores an “offset” for the progress of the job. That helper table serves as a key-value store, where the key is the date for the partition and the value is a tuple that holds an entry for each column in the payload table’s primary key. In this case, each row contains a date which corresponds to the date of the partition, an` id` , and an` inserted_at` , which we can then use to paginate with` WHERE (id, inserted_at) > ($1, $2)` , where the parameters come from the stored offset.
2. Read a batch of payloads from the source partition, ordered by the primary key and offset as described in 1).
3. Split up the batch into chunks, which we’ll write to S3 in parallel.
4. Concurrently perform the compression algorithm we described previously on each chunk and store the results.
5. Write the results of these offloads into the new table we created (the empty copy of the original partition).
6. Update the offset in the helper table in the database to whatever the maximum primary key value for this iteration was.


We repeat this process until we’ve processed all of the batches of payloads. It looks something like this:


By the end of the job, we’ve paginated through the entire source partition in batches, and offloaded each batch to S3. And we now have also filled our copied partition with all of the same data as we had in the original partition, except with the keys in S3 instead of the actual payloads.


Here’s where the partitioning is really useful: we can simply drop the old partition which means **we don’t see any autovacuum pressure caused by row updates** . To do this safely, we acquire a lock on the partition tables in` ACCESS EXCLUSIVE MODE` , drop the old partition and triggers, rename the new partition, and attach it to the parent table as a replacement for the prior partition.


Loading syntax highlighting...


This is also where the check constraint comes in handy: since we created a check constraint that matched the partition constraint of the original partition when we created the copy, we can perform the attach without needing to validate that constraint again, which could take a few seconds. Since we don’t need to validate, the entire swap is pretty much instantaneous.


Once it’s attached, we release the lock, commit, and the data has been cut over!


### The outcome


We’ve been running this write-and-swap approach for a few months now, consistently offloading hundreds of millions of payloads each day while keeping database CPU usage and S3 costs down, and without falling behind! The key insight here is that a single write per payload into the partition copy is significantly more performant than the` UPDATE` /` DELETE` overhead from our WAL approach, since we can do the writes significantly faster than` UPDATES` into the original table, while causing less lock contention and less autovacuum pressure.


*Naming credit for` supertoast` goes to[Daniel Farina](https://github.com/fdr) from[Ubicloud](https://www.ubicloud.com/)*
