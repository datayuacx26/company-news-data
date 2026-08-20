---
schema_version: "1.0.0"
document_id: "e33cf4efce77af50943150357459c789c0a08d74f649667994654aeaac9c00fa"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/from-batch-snapshots-to-near-real-time-data"
published_at: "2026-08-04T16:29:00+00:00"
first_seen_at: "2026-08-04T21:54:24.566726+00:00"
fetched_at: "2026-08-04T22:08:55.728008+00:00"
content_hash: "sha256:6a6e24fd574948d092748ed2f892fb15ce44aa4d3ae509a66f456129241cb760"
---

# From Batch Snapshots to Near-Real-Time Data

Change Data Capture (CDC) is often presented as a straightforward pipeline: read a database transaction log, publish each change, and apply those changes to another system. That description is accurate, but it leaves out many of the decisions that determine whether the resulting data can be trusted.


At Doximity, we already had a batch pipeline that periodically copied snapshots of application databases into our data warehouse. Those snapshots were reliable, but their freshness was measured in hours. We introduced CDC to make changes available in minutes so downstream transformations and operational analytics would not have to wait for the next batch snapshot. We continued using the batch pipeline for the consistency and recovery guarantees it already provided. Over a 12-day measurement window, 95% of events from a stratified sample of active tables reached the queryable intermediate layer within eight minutes of being published to Kafka.


The most interesting parts of the project were not the connections from a source database to Kafka or from Kafka to Snowflake, but four questions we had to answer around them:


- How could we reuse our existing, transactionally consistent batch snapshots as an on-demand starting point for CDC, without reprocessing every existing row?
- How could we onboard new tables and absorb schema changes from many source databases across our products without growing operational overhead for each one?
- How could new changes become queryable without waiting for the warehouse to merge them into place?
- How could we handle cascading child deletes that MySQL performs but never emits as individual binary-log events?


Our answers are the four design decisions in this article: a trusted batch snapshot, metadata-preserving routing, a base-plus-delta view, and synthetic cascade deletes. Together, they turned a stream of row changes into a system we could bootstrap, scale, validate, and recover. The sections that follow explain the tradeoffs and guardrails so readers can apply the same questions to their own CDC designs.


## Two paths with different strengths


Our batch and streaming pipelines solve related but different problems. The architecture preserves both rather than forcing one to replace the other.


The batch pipeline creates a point-in-time view of an application database using the database's restore-to-point-in-time capability (Amazon Aurora, in our case). Its tables come from the same database snapshot, keeping related rows in a transactionally valid state. The snapshot also gives us a complete dataset that can serve as a recovery and validation baseline.


The CDC pipeline optimizes for freshness.[Debezium's MySQL connector](https://debezium.io/documentation/reference/stable/connectors/mysql.html) reads changes from the binary log (binlog) and publishes them to Kafka. A Snowflake sink connector writes the events to a shared raw table, and a router separates them by source. Consumer-facing views combine those changes with compacted base tables, making new state queryable before replication workers physically merge it.


CDC was not a replacement for snapshots. A single CDC-produced table can be fresher than its batch equivalent, but two related CDC tables may not represent exactly the same moment. One may have processed a transaction before the other. That difference matters for outer joins, watermarks, and incremental models.


Keeping both paths allowed consumers to choose between two useful guarantees: a consistent point-in-time dataset or lower-latency updates.


## Bootstrap from data we already trust


Debezium can take an initial snapshot before it begins streaming changes. We chose a different approach: reuse the output of our mature snapshot pipeline.


Our CDC connectors start without copying existing table data. To onboard or recover a destination table, we initialize it from the latest trusted batch snapshot and transition it to the event stream. This avoids a second full-copy mechanism and reuses the validation and operational experience already built around the batch pipeline.


Reusing the snapshot shifts the difficult part from copying data to managing the handoff. Events continue arriving as a snapshot is produced and loaded, so the snapshot and retained event history must overlap. During initial onboarding, we compare the snapshot's freshness with the earliest retained event. We wait until the snapshot covers that boundary, clone it as the destination baseline (a Snowflake zero-copy clone, so the copy is near-instant and initially duplicates none of the underlying table storage), and process the retained changes through the normal replication path. Reapplying an event already reflected in the snapshot may temporarily repeat an older state. We deliberately accept that overlap because replay eventually converges: primary keys identify the affected rows, source-log coordinates order competing events, and the table is considered caught up only after the retained history has been processed. What we cannot tolerate is a gap, where the snapshot is older than the earliest retained event and some changes exist in neither the snapshot nor the retained log. Those changes would have no replay path and could be permanently omitted.


Under normal operation, Debezium resumes from its stored offset, and the rest of the pipeline continues from durable stream positions. We reserve table rebuilds for less common cases in which that continuity is deliberately changed or can no longer be trusted: connector maintenance that requires an offset reset, a database migration or failover that changes binlog or GTID history, a retention gap that removes the required logs, or a consistency check that identifies a table in need of repair.


When that happens, a more precise version of the snapshot handoff supports the rebuild. The snapshot pipeline records the source GTID position and timestamp represented by each completed snapshot. To restore a table without losing changes captured before the rebuild, we:


1. Clone the trusted snapshot as the destination table's baseline.
2. Read the source position and timestamp associated with that snapshot.
3. Recover retained events from the intermediate table, beginning at the stream's previous offset.
4. Requeue only events at or after the snapshot's restore position.
5. Let the normal replication path apply those events before live processing continues.


Replaying only that bounded range bridges the interval between the batch snapshot and live streaming without requiring the CDC connector to copy the full source table. Initial onboarding and recovery use different levels of precision, but they preserve the same invariant: establish a trusted baseline, prove overlap with the log, and process every change after that boundary.


Keeping the initial copy out of Debezium also protects transactional workloads. A connector-managed MySQL snapshot can briefly acquire a global or table-level lock to establish a consistent position and capture schema. It then scans every included row inside a repeatable-read transaction. Our connectors use Debezium's[no_data snapshot mode](https://debezium.io/documentation/reference/stable/connectors/mysql.html#mysql-property-snapshot-mode) , capturing schema without emitting a baseline` READ` event for every row. The existing snapshot pipeline supplies that data from outside the live CDC path and avoids a second full scan of large tables on the application databases. Schema capture may still involve brief coordination, but the expensive row copy stays off those databases and keeps bootstrap work isolated from transactional workloads.


This strategy turns a trusted snapshot system with precise handoff metadata into a fast, repeatable bootstrap and recovery mechanism. Debezium can then focus on change capture. Teams without that foundation can use connector-managed snapshots to establish their initial baseline.


## Fan in first, then route by metadata


The next challenge is onboarding tables without continually expanding connector configuration. A conventional sink can map every Kafka topic to a separate warehouse table. That approach is easy to understand, but it creates ongoing work: each new source table can require a new topic mapping and a connector deployment.


We configured the sink connector to subscribe to a broad family of CDC topics and map them into one raw landing table. A simplified version of the pattern looks like this:


```text
topics.regex  =  ^cdc[.](source-a|source-b)[.].*
snowflake.topic2table.map  =  cdc[.]source-a[.].*:raw_cdc_events,  \
cdc[.]source-b[.].*:raw_cdc_events


```


In production, we use one regex mapping per source namespace rather than the fictional names above. Each table topic within those namespaces maps to the same raw table; schema-control topics follow a separate path. The exact syntax depends on the connector version and topic naming convention, but the important property is Snowflake's[many-to-one topic mapping](https://docs.snowflake.com/en/user-guide/kafka-connector/how-the-connector-works#explicit-topic-to-table-mapping) . Events from different tables can share storage because they retain metadata that identifies their source topic, database, and table.


A router uses that preserved identity to fan the events back out into table-specific intermediate tables. It creates the required routing and destination structures when it encounters an eligible source table for the first time. Consumer-facing views expose the routed changes immediately. Replication workers merge those changes into base tables later.


Automatic onboarding comes from four choices:


1. The source connector captures tables using broad rules with explicit exclusions.
2. The sink subscribes to matching topics using a regular expression.
3. A wildcard mapping sends those topics into a shared raw table.
4. A metadata-driven router creates and feeds table-specific destinations.


The wildcard is not sufficient on its own. Source metadata must survive ingestion, and the downstream router must understand how to turn that metadata into safe warehouse objects. New tables must also satisfy the platform's rules for keys, schemas, and supported data types.


The fan-in/fan-out design moved most onboarding work out of connector configuration and into reusable platform behavior. It also gave us one durable place to inspect raw events before table-specific processing.


## Make changes queryable before merging them


The event stream preserves history, but most analytical consumers want the current state. A conventional design might make those consumers wait until every event has been merged into its destination table. We wanted routing latency, not merge cadence, to determine when new data became visible.


Consumers query a view over two layers: a compacted base table and the unmerged append-only changes. The view selects the latest change for each primary key, overlays those changes on the base, and excludes rows whose latest operation is a delete. It can expose an event's effect as soon as the router writes it to the table-specific intermediate table.


We defer the physical merge instead of applying every event immediately. When the unmerged delta reaches an age or volume threshold, the replication worker compacts multiple changes for the same primary key and applies their final effect to the base table with Snowflake` MERGE` operations. Schema changes can also force an immediate merge so the base, delta, and view remain compatible.


This separates data availability from maintenance. The append-oriented router can scale for ingestion throughput without also scaling every merge. Replication workers merge larger batches on their own schedule. A delayed merge does not immediately make data stale because consumers continue to see the delta through the view.


Deferral is bounded rather than indefinite. A growing delta makes the view do more work at query time and increases the amount of state that still needs to be merged. Age and volume thresholds keep that read amplification under control. The design also puts significant correctness responsibility in the view: updates, deletes, event ordering, and schema changes must produce the same visible result before and after a merge.


The same boundary lets routing and merge processing use independent compute policies. The durable advantage is not a particular warehouse size or billing model. It is the ability to operate and scale ingestion, serving, and merge processing independently.


## Reconstruct deletes the log cannot show us


Cascading deletes exposed a less obvious correctness gap.


In our MySQL setup, when a foreign key performs an` ON DELETE CASCADE` ,[cascade-generated child deletes are absent from the binary log](https://debezium.io/documentation/faq/#why_dont_i_see_delete_events_in_some_cases) consumed by the pipeline. InnoDB applies the cascade internally and, under row-based logging, records only the parent delete, expecting a replica's own engine to reproduce the child deletions, so those rows never appear as events. Debezium sees the parent deletion, but a pipeline that only applies visible row events can leave orphaned child rows in the warehouse.


We addressed that gap by teaching the replication layer about database relationships.


A separate metadata process reads foreign-key definitions and referential actions from the source databases' information schema. It records which tables reference one another, which columns form each relationship, and whether the delete rule is cascading.


When the replication worker processes a parent delete, it consults that relationship map. For each cascading relationship, it finds matching child rows in the destination and constructs synthetic delete events. Those events are written into the same table-specific intermediate table used by ordinary CDC events.


Synthetic deletes reenter the normal path and receive the same processing, retries, and observability as events produced directly from the binary log. A synthetic child deletion can also participate in the same relationship handling when another table depends on it.


The mechanism has deliberate boundaries. Because most of our source schemas follow ActiveRecord conventions and use single-column surrogate keys, the implementation targets those keys; the rarer compound primary keys require a separate strategy. It also skips a relationship when the child destination, its intermediate table, or either relationship column is unavailable. These guardrails avoid guessing when warehouse state and relationship metadata temporarily disagree, but they make skipped-cascade monitoring and metadata freshness part of the operating model.


The broader lesson is that a transaction log does not necessarily describe every semantic consequence of a database operation. A CDC design must account for what the log omits, not only what it contains.


## Validate freshness against a stable reference


Every component in a CDC pipeline can fail or fall behind. Connectors can stop, events can be delayed, schema changes can surprise consumers, and application logic can uncover cases the replication code did not anticipate.


The batch pipeline gives us an independent reference for detecting those problems. Comparing a fresh CDC table directly with an older snapshot would produce expected differences, so the comparison first aligns both sides to the same logical point:


1. Clone the CDC-produced table and record its raw event position.
2. Clone the corresponding batch snapshot.
3. Identify the source position represented by the batch snapshot.
4. Apply retained events after that position to the snapshot clone.
5. Compare row counts and keys after both sides represent the same point.


The aligned results turn the older batch path into more than a fallback. They provide an independently produced dataset for repeatedly validating the streaming system.


The trusted snapshot also powers an ongoing self-healing check for eligible tables with a row-level timestamp such as` updated_at` . After a streaming table catches up to the snapshot, we compare recent rows by primary key and timestamp, excluding events that could still explain a difference. A snapshot row absent from the stream, or newer than its streaming counterpart, can reveal a missed insert or update; an older streaming row absent from a sufficiently fresh snapshot can reveal a missed delete. The repair directly upserts or deletes those rows in the streaming table and records the anomaly, so the batch snapshot is an active safety net rather than a passive baseline. The check requires a supported timestamp column and a single-column primary key, examines a bounded window, and safeguards against backfills that rewrite timestamps.


We measure routing lag from each Kafka record's` CreateTime` to the time the router writes it to a table-specific intermediate table. This makes freshness observable per table and helps separate source inactivity from pipeline delay. To validate this Kafka-to-queryable availability path, we sampled active tables across low, medium, and high event volumes over 12 complete UTC days. Among events from that stratified sample, 95% reached the queryable intermediate layer within eight minutes, and 99% arrived within 13 minutes. This event-weighted measurement does not include time before Kafka publication, nor does it wait for the deferred physical merge.


These checks form a continuous operating loop. Lag metrics, exported to Prometheus and visualized in Grafana, track active source databases; age and volume thresholds bound each unmerged delta; and regularly refreshed relationship metadata keeps cascading-delete rules current. Row-count checks and position-aligned comparisons surface drift, while skipped relationships are logged for investigation. When a table needs repair, the snapshot-and-replay path restores it without resetting the entire pipeline.


## What we learned


The architecture began with a latency goal, but the durable lessons were about correctness and operability:


- **Reuse trusted state when it gives you a precise handoff.** A snapshot plus a source position can be both an efficient bootstrap and a repeatable recovery strategy.
- **Preserve source metadata through shared ingestion.** Many-to-one ingestion remains flexible when every event retains enough identity to be routed safely.
- **Separate visibility from physical merges.** A base-plus-delta view can make changes available promptly while allowing physical merges to happen in bounded, efficient batches.
- **Treat automatic onboarding as a platform contract.** Wildcards remove configuration only when capture rules, schema handling, object creation, and validation work together.
- **Model database semantics that are absent from the log.** Cascades are one example; every source engine and logging configuration has its own edges.
- **Keep an independent reference while confidence grows.** The batch pipeline gave us a baseline for bootstrap, recovery, and data-quality comparison.
- **State consistency guarantees plainly.** Fresher tables are not necessarily a transactionally consistent group of tables.


CDC gave us a path from data freshness measured in hours to freshness measured in minutes, with 95% of events from the stratified sample becoming queryable within eight minutes of Kafka publication. More importantly, by combining streaming changes with trusted snapshots, we built a system we could bootstrap, scale, validate, and recover without treating the transaction log as more complete than it really is.


The event stream is only one part of a dependable CDC platform. The surrounding decisions are what make it useful.


---


*Be sure to follow[@doximity_tech](https://x.com/doximity_tech) if you'd like to be notified about new blog posts.*
