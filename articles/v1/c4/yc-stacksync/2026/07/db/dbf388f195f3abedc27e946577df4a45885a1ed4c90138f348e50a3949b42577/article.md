---
schema_version: "1.0.0"
document_id: "dbf388f195f3abedc27e946577df4a45885a1ed4c90138f348e50a3949b42577"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/two-way-sync-aurora-postgresql-netsuite"
published_at: "2026-07-23T10:00:00+00:00"
first_seen_at: "2026-07-29T17:32:43.435424+00:00"
fetched_at: "2026-07-29T17:32:44.790239+00:00"
content_hash: "sha256:c9f06a8e800fd8e52917757a9ec1fba09ded4716d626050c769bff566b6de23e"
---

# Zero-ETL Is Not Two-Way Sync: Aurora PostgreSQL and NetSuite

Search for two-way sync between AWS Aurora PostgreSQL and NetSuite and the first two answers you get are zero-ETL and AWS DMS. Neither one is a two-way sync. Aurora PostgreSQL zero-ETL is a managed one-way replica into Amazon Redshift or Amazon SageMaker Lakehouse, and what DMS calls bidirectional replication is two independent one-way tasks with loopback prevention. Both are useful. Neither will put a credit hold set in NetSuite back on the customer row in your application database.


This is an evaluation guide rather than a tutorial. It covers what the Aurora cluster contributes to the problem, what NetSuite contributes, and how the five categories of solution compare on the three things that never go away: loop prevention, conflict policy and the replication slot lifecycle. The[AWS Aurora PostgreSQL connector page](https://www.stacksync.com/connectors/aws-aurora-postgresql) has the short version.


If you are on RDS for PostgreSQL rather than an Aurora cluster, the sibling write-up is[two-way sync between Amazon RDS and NetSuite](https://www.stacksync.com/blog/two-way-sync-amazon-rds-netsuite) , which covers the EBS-backed surface instead. If you are still choosing a platform, start from the[enterprise iPaaS guide for Aurora PostgreSQL](https://www.stacksync.com/blog/enterprise-ipaas-aurora-postgresql) ; if the other side is a CRM, read[how to sync Aurora PostgreSQL with Salesforce](https://www.stacksync.com/blog/sync-aurora-postgresql-with-salesforce) . The[Aurora origin story](https://www.stacksync.com/blog/the-redo-log-is-the-database-the-origin-story-of-amazon-aurora) explains the shared storage layer underneath all of it.


## Zero-ETL is not two-way sync, and neither is DMS bidirectional replication


Zero-ETL moves data in one direction only. Aurora PostgreSQL zero-ETL integrations target Amazon Redshift, generally available since October 2024, and Amazon SageMaker Lakehouse since October 2025, and AWS describes the data as available in the target within seconds of the write. There is no documented write-back path. It is a managed low-latency analytics replica of the cluster, not a link between two systems that both accept writes.


That matters because zero-ETL is very good at the job it does. If the question is how to run analytics on live Aurora data without operating a pipeline, zero-ETL is the right answer. If the question is how a fulfilment status reaches the sales order in the ERP, a one-way analytics replica cannot help, however fast it is.


A managed replica and a two-way sync are not the same shape of thing. The difference is everything on the right.


AWS DMS gets closer without arriving. It does support what it calls bidirectional replication, and the design is worth reading carefully: two independent one-way tasks, A to B and B to A, with loopback prevention so a change replicated in one direction is not sent back the other way. AWS is unusually direct about what that does and does not give you.


> "Bidirectional replication in AWS DMS isn't intended as a full multi-master solution including a primary node, conflict resolution, and so on."


**AWS Database Migration Service documentation**


Two more constraints matter. DMS reads Aurora PostgreSQL through one of two logical decoding plugins:` test_decoding` , or` pglogical` when it has been set up on the source.` pglogical` filters unwanted tables at the slot level, so less write-ahead log volume, CPU and network traffic leave the cluster;` test_decoding` filters inside DMS and adds latency on long transactions. Separately, CDC from a PostgreSQL source requires primary keys, and PostgreSQL sources do not support a custom CDC start time, because there is no mapping from a wall-clock timestamp to a log sequence number.


And DMS does not speak NetSuite. Its targets are databases, warehouses and streams, so the ERP half is still a custom build: SuiteTalk REST calls, an auth flow, dependency ordering, retries, echo suppression and a reconciliation report, all of it yours to own.


## What the Aurora cluster contributes to the problem


Aurora's contribution is the writer. Logical replication is enabled with` rds.logical_replication` , a DB cluster parameter that defaults to 0, and since you cannot edit a default parameter group, step one is a custom cluster parameter group. Set it to 1, reboot the writer instance, and` wal_level` then reads` logical` . You never set` wal_level` yourself.


Two consequences follow immediately. Turning the flag on increases WAL generation even if no replication slot is ever created, which shows up on the bill as higher` VolumeWriteIOPS` . And slots live on the publisher, which on Aurora means the writer instance and only the writer instance. AWS puts it plainly: PostgreSQL 16 added support for logical decoding from read replicas, and that feature is not supported on Aurora PostgreSQL. Change capture cannot be moved onto a reader, so it shares the writer with production traffic. A write-through WAL cache, added in Aurora PostgreSQL 14.5, 13.8, 12.12 and 11.17, cuts its disk I/O without removing that cost.


Then four ordinary cluster events each leave your slot in a different state than you left it.


-


**Failover.** The cluster endpoint is one DNS name that always resolves to the current primary, and Aurora repoints it when the primary changes. Aurora DNS zones use a 5 second TTL, failover is typically restored in under 60 seconds and often under 30, and AWS names DNS propagation as the largest contributor. RDS Proxy can cut failover time by up to 66%.


-


**Blue/Green deployments.** Each database needs a logical replication slot for the green environment, and AWS's documented guidance is to drop your own self-managed slots and subscriptions before the switchover and recreate them afterwards. Your CDC slot is not carried through.


-


**Major version upgrades.** Aurora requires every logical replication slot to be dropped, including inactive ones, before the upgrade will proceed.


-


**A consumer that stops.** An inactive slot keeps retaining WAL, which can eventually lead to insufficient storage, and it blocks autovacuum on the catalog tables.` OldestReplicationSlotLag` ,` ReplicationSlotDiskUsage` ,` TransactionLogsDiskUsage` and` FreeStorageSpace` in CloudWatch are the metrics to alarm on.


Treat that list as a design requirement. Slot survival across an Aurora failover is not a documented, solved behaviour, so anything you run in production has to assume the slot can disappear, rebuild it, and resume from a durable watermark it stored itself.


One cost note on Aurora Serverless v2. Scale-to-zero (the 0 to 256 ACU range on Aurora PostgreSQL 16.3, 15.7, 14.12 and 13.15 and later, where 1 ACU is roughly 2 GiB of memory) pauses after an idle interval you set from 300 to 86,400 seconds. Logical replication changes that budget: the writer and the readers in failover tiers 0 and 1 do not auto-pause, because Aurora keeps a minimal amount of activity going to health-check the replication connection. The sync does not break. The scale-to-zero saving you planned for goes away.


## What NetSuite contributes to the problem


NetSuite has no write-ahead log and no native change-data-capture feed of any kind. The standard way to find what changed is scheduled polling: a SuiteQL query or a saved search filtered on` lastmodifieddate` . For genuine push you need a SuiteScript User Event script making an outbound HTTPS call, or NetSuite's Event Subscriptions feature, which is trigger-based notification rather than a log you can rewind and replay.


` lastmodifieddate` is a full datetime with sub-second precision, which is good news and a trap in the same sentence. Polling windows are built as` >=` and` <` comparisons against it, and the boundary is exactly where records go missing or arrive twice. Account time zone and data centre handling differ from your job's clock, so a window that looks airtight in one region drops or duplicates records around midnight and at daylight-saving transitions. Overlap the windows and make the write idempotent instead.


Then there is governance, where most integrations actually fail. NetSuite meters concurrent requests from a single unified pool shared across SuiteTalk SOAP, SuiteTalk REST and RESTlets, so your sync competes with every other integration in the account. Base limits are 5 concurrent requests on Standard, 15 on Premium, 20 on Enterprise and 20 on Ultimate, with each SuiteCloud Plus licence adding 10; development and partner accounts are fixed at 5. Cross the line and REST returns HTTP 429 while SOAP and SuiteScript raise` SSS_REQUEST_LIMIT_EXCEEDED` . A backfill that ignores the pool takes the account's other integrations with it.


The last NetSuite input is a clock. 2025.2 is the last release with full SOAP feature parity, from 2026.1 new integrations are expected to use REST with OAuth 2.0, from 2027.1 no new SOAP or token-based authentication integrations can be created, and in 2028.2 the SOAP endpoints are switched off entirely. Anything you scope today on SuiteTalk SOAP has a published end date, and that belongs in the build-versus-buy arithmetic.


## What actually happens to one record


Both directions are easier to hold in your head as the states one record passes through than as a set of jobs and schedules. Take a sales order that exists as a row in Aurora and as a transaction in NetSuite.


The states one record moves through, including the three ways a write ends up somewhere other than confirmed.


Going out of Aurora, a commit lands in the write-ahead log, the change is decoded off a slot on the writer, the row is mapped to a NetSuite record type and field set, and the write goes out over SuiteTalk REST inside the sync's share of the concurrency pool. Dependencies have to be ordered, because a sales order line referring to a customer that does not exist yet is a validation error, not a transient failure to retry. That write then bumps` lastmodifieddate` , which the inbound poll sees on its next pass and has to recognise as the sync's own echo rather than a user edit.


Coming the other way, a change found by a` lastmodifieddate` window or pushed by a User Event script is applied as an` INSERT ... ON CONFLICT DO UPDATE` keyed on the NetSuite internal id, so a retry updates rather than duplicates. That write lands in the Aurora WAL and comes straight back out of the slot, indistinguishable from a user edit unless the engine tagged its own origin. Without the tag the record bounces between the two systems until somebody notices the API bill.


Three states carry the whole argument. **Conflict** is the same field moving on both sides inside one window, and the honest answer is not last writer wins, which makes the outcome depend on which job ran last; it is field-level precedence agreed once per record type. **Backing off** treats a 429 from the NetSuite pool as a normal event to absorb, not an incident to page on. **Resuming** accepts that the slot can vanish underneath you, and that the durable watermark is what makes the restart safe.[Bi-directional sync explained](https://www.stacksync.com/blog/bi-directional-sync-explained-3-real-world-examples) works through the same three states on other pairs.
