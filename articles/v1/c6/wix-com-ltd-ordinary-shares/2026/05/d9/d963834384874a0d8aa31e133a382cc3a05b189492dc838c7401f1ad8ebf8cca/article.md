---
schema_version: "1.0.0"
document_id: "d963834384874a0d8aa31e133a382cc3a05b189492dc838c7401f1ad8ebf8cca"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/how-we-built-a-zero-downtime-database-migration-service-at-wix"
published_at: "2026-05-14T07:19:17+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:13:08.113980+00:00"
content_hash: "sha256:30b6f17cbade9933dacad74c881d8163cbf41307bc25f2b13044845bc2627e58"
---

# How We Built a Zero-Downtime Database Migration Service at Wix

##


**The Challenge**


At Wix, multiple applications share the same DB cluster. The reasons vary: consolidating apps from the same domain, grouping several small apps that don’t justify a dedicated cluster, or simply optimizing cost and operational overhead.


However,


this setup comes with a significant risk:


every application on the cluster has the potential to impact all the others.


One real example: we had a critical service related to user authentication sharing a DB cluster with several other applications. When one of those applications ran into trouble,


the issues spread and directly affected authentication.


The solution was clear: move the critical service database to a dedicated, isolated cluster.


The hard part was the


*how* : migrating an application’s database from a shared cluster to a dedicated one while guaranteeing:


-


Reliability above all


-


Fully online,


**zero downtime**


-


Seamless for the application layer - no code changes


-


Strong data consistency, zero data loss


We operate at a huge scale, which makes database migrations extremely challenging, and having the ability to migrate our databases safely is essential for a variety of reasons.


Sometimes DB migration can be


**reactive** :a shared cluster is having problems, and a critical service needs to move out immediately.


Sometimes DB migration can be


**proactive** : an application grows in scale or business importance, it requires stricter privacy or compliance requirements, or the data team needs to upgrade DB versions, move data between clusters, or organize DB clusters.


In all of these cases, the database migration should be transparent to developers and owned/managed by the data infrastructure team - not delegated to individual product teams.


This is the context in which


**DB Mover** was born:


an internal Wix service that performs zero-downtime, transparent database migrations between clusters


at huge scale.


##


DB Topology


At Wix, our Data Infrastructure team operates ~200 MySQL DB clusters. Each MySQL cluster consists of 2 co-masters in Active/Passive setup spread between two geographic regions for HA and redundancy and several replicas nodes for read scalability and for CDC, BI and backup workloads and roles.


On top of each cluster we run a ProxySQL layer that routes read-write and read-only traffic to the appropriate nodes, so applications never connect to MySQL directly.


##


**Good Tools, Doesn't Fit**


In order to migrate our DB between different clusters, we tried the following solutions, those wasn’t satisfied our needs or comply them partially:


###


MySQL Dump


One of the classic DBA answers is dump, compress, transfer, restore. Simple and proven, but it cannot be performed with zero downtime.


###


Amazon DMS


A solid product for standard schemas. But Wix isn't standard. Our data access layer defines a unified schema pattern with a main JSON column, and multiple stored, and virtual columns derived from it, including a composite primary key. DMS does not fully support this pattern. For our most critical MySQL workloads, it simply doesn't fit.


###


Multi source replication channeling


We combined an existing cluster as the source with a new target cluster using a temporary multi‑source (auxiliary) replication channel. The procedure relied on a MySQL dump followed by channel-based replication. However, this approach was rejected due to the high risk associated with primary-node failure and the cumbersome topology.


##


How DB Mover Works


DB Mover is a Python microservice that orchestrates the full migration lifecycle, from the initial snapshot through continuous CDC streaming to the final traffic reroute. The architecture has three main components.


##


Debezium Connector


For each migration, we provision a dedicated Debezium connector. The connector is responsible for streaming the full dataset, starting from the initial snapshot and every change event from the source database into Kafka topics.


Critically,


we always read from a designated replica node, never from the primary, it


guarantees


that we have zero impact on production traffic.For serialization, we chose Avro over JSON. Avro is binary and compact, which


is mandatory for our scale. Avro also enforces strong typing, which is crucial when supporting multiple DB types and versions, and when generating writes into a destination database - field types must match exactly.


We use a schema registry (via AWSKafkaAvroConverter) so schemas are defined once and referenced by a small ID in every message, rather than embedded in full.


##


Amazon MSK


We provision a dedicated MSK cluster for each migration. It exists only for the duration of the migration,


and is deployed in the same


AWS region as the DB destination master node.


Partition count is one of the most


critical


decisions we make at the start of a migration, and it cannot be changed safely mid-flight. Increasing partitions during the migration changes key-to-partition mapping for new messages, breaking consistency guarantees per partition and forcing a full migration restart from scratch.


Kafka uses a deterministic, homogeneous hash function on the primary key to distribute messages between partitions, balancing messages evenly among them. However, some primary keys naturally produce much more data than others, so we do have data skew. A higher partition count helps keep the skew manageable by reducing the impact of low‑cardinality primary keys, and by breaking partition queues into smaller segments that allow better parallel processing.


MSK default maximum message size is 1 MB, which is far too small for our workloads. We now configure all topics with the maximum size - 8 MB at creation time.


##


The Python Service


This is a Falcon-based service. It manages the migration through a state machine: initialization, schema replication, Kafka consumer setup, parallel execution across pods, error handling, and finally reroute.


###


Initialization


In this phase, the service validates the request (including row-size constraints, single-client ownership of the database), creates the migration metadata record, commit the full schema DDL on the destination database, generates a unique consumer group ID for this specific migration run, opens a dedicated Slack channel for monitoring and alerting, and schedules the migration job.


The unique consumer group ID guarantees that Kafka offset tracking is isolated per migration. It also allows us to stop and later resume the migration from the exact offset.


###


Main Core Loop


In the core run loop, t


he service continuously consumes events from Kafka, decodes the Avro payload,


gets


the operation type (insert, update, delete, or snapshot read),


generates


the corresponding DML, and executes it against the destination database via ProxySQL.


Kafka offsets are committed only after successful DML execution.


Offset commits are performed asynchronously to


avoid blocking the main processing loop.


Dynamic key operational parameters, including commit frequency, DB violation check intervals, Slack reporting flow, and pod scaling. Those are fully configurable at runtime, with no redeployment required. The service manages pod scaling dynamically: it automatically scales down when a DB violation error is detected, and scales back up once the violation is resolved and conditions are stable. The loop also handles graceful stop, abort, and resume among pod restarts.


###


Reroute


The reroute step


is the critical mechanism that enables


zero-downtime migration. This is the main focal point of the service.


Because the destination cluster has been continuously ingesting and applying changes from the source, the lag at cutover is near-zero seconds. When we’re ready to switch the traffic, we execute a controlled, staged cutover:


The cutover stages:


-


**Read‑only reroute** : We reroute only read‑only connections to the new cluster. This creates a real‑traffic validation window in which application teams are responsible for validating the data.Since the destination DB has already loaded the entire database, the cache is warm and read performance is excellent.


-


**Write cutover** :


After at least one hour or more in this state, and only after we receive a green light from the


application teams


, we cut over the write connections.


At this moment, write traffic is fully directed to the destination cluster. The migration is now considered complete and is marked as Done successfully.


From this point forward, rollback is not an option without losing data, the “new” data that has been committed only to the new cluster.


##


The Result


We successfully migrated multiple highly critical databases from shared clusters to dedicated ones, while fully maintaining our “Reliability above all” approach.


Additionally, another significant achievement was migrating multiple MongoDB databases between non-consecutive versions with no downtime and no multiple version upgrades.


All of the above was done while keeping our main goals:


-


Reliability above all, no user impact


-


Zero downtime


-


No alerts fired


-


No application code changes


##


**Summary: 6 Key Focal Points**


1.


**Process ownership and App transparency:** Database migrations belong in the data infrastructure layer, not the application layer.


**** The application should remain agnostic of database movements, as the data infrastructure team owns the process.


2.


**CDC for Live Migrations:** Change Data Capture (CDC) is the preferred primitive for live migrations. Streaming changes continuously keeps lag manageable and minimizes the cutover window.


3.


**Dedicated Replicas:** Always read from a dedicated replica. Neither the snapshot nor the CDC stream should touch the primary node to ensure production traffic remains unaffected.


4.


**Fixed Kafka Partition Counts:** Partition counts must be decided based on table size before migration begins. Changing partition counts mid-flight invalidates key ordering and breaks data consistency, necessitating a full restart.


5.


**Avro over JSON:** Use Avro for CDC at scale. Its binary encoding and schema registry support reduce message size and prevent type mismatches.


6.


**Staged Cutover:** Follow a "read first, then write" staging process. Moving read-only connections first allows for validation with real traffic without write risk and keeps rollback option if needed.


###


So, What's Next?


DB Mover is still evolving. Here's a glimpse to what we're working on for the next version:


-


**Multi-region MSK:** deploying MSK across all AWS regions.


-


**Cross-ProxySQL migrations:** supporting migrations between db clusters that sit behind two different ProxySQL instances, expanding the topologies we can handle.


-


**Dynamic per-table partition sizing:** instead of a hard coded partition number per schema size, we would like to enable different partition numbers per table in the same schema migration.


-


**Parallel Debezium snapshots** : enabling the snapshot phase to be read in parallel.


This post was written by


**Bar Shauli**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) |


[Instagram](https://www.instagram.com/wix_eng/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) ,


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
