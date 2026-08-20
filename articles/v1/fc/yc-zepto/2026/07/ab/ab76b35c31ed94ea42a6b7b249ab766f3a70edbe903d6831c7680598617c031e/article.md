---
schema_version: "1.0.0"
document_id: "ab76b35c31ed94ea42a6b7b249ab766f3a70edbe903d6831c7680598617c031e"
company_key: "yc-zepto"
company: "Zepto"
source_id: "yc-zepto-rss-dc680377f8f2"
canonical_url: "https://blog.zepto.com/a-billion-events-a-day-how-we-built-ads-analytics-that-actually-works-c28904bbf8de"
published_at: "2026-07-27T07:46:06+00:00"
first_seen_at: "2026-08-10T05:06:20.120332+00:00"
fetched_at: "2026-08-20T03:19:45.384495+00:00"
content_hash: "sha256:5702f9c7613ec0563a1eed3c744896efdd76a7069e6d4317ab87826d8ddaa71b"
---

# A Billion Events a Day: How We Built Ads Analytics That Actually Works

*From a naïve Flink pipeline to a ClickHouse-powered analytics engine — the full story of deduplication, late events, aggregation windows, and the lessons we learned the hard way.*


### The Problem


When we serve an ad, we get back a stream of user events — views, clicks, and more — flowing in. We needed to consume this stream and answer some fundamental questions: *How much has each advertiser spent? Across which campaigns, search terms, categories, placements, and devices?* And critically — the same analytics engine would power advertiser billing and real-time budget enforcement.


**Functional Requirements**


- Spend analytics across campaigns, placements, keywords, and product categories
- Revenue attribution on the same dimensions
- Real-time budget enforcement — stop serving ads when a campaign’s spend limit is hit


**Non-Functional Requirements**


- Near real-time analytics to prevent overspend
- High reliability — downtime directly causes overspend
- Exactly-once semantics — duplicates overcharge advertisers, missing events lose revenue
- ~1 billion events per day throughput


### Phase 1 — The Flink Approach


Our first instinct was straightforward: write Flink jobs using 5-minute tumbling windows to aggregate event counts over the request dimensions. The result was a compact, queryable set of records in Postgres — regardless of how much traffic came in, the number of rows for a given day and dimension set stayed fixed. Querying for an hour or a day was just a matter of summing up 5-minute buckets.


This worked. Until it didn’t.


**Issue 1: No deduplication**


Pre-aggregating events made deduplication impossible. Events could be replayed due to failures, leading to duplicate counts. We also had business-level deduplication requirements — for example, a user should only be charged for their first click on an ad. With only aggregated data available, there was no reliable way to handle either of these cases.


**Issue 2: No raw event history**


Once aggregated, the click-level data was gone. Debugging discrepancies — *“why did the count spike here?”* — had no answer. We needed the ability to trace individual events.


The conclusion was clear: we needed to store raw events and run aggregations over them. That meant an OLAP database.


### Phase 2 — Rebuilding on ClickHouse


We evaluated our needs against standard OLAP requirements: heavy aggregate queries over specific columns (bid, impression_count), time-series analytics, columnar storage, and high write throughput. ClickHouse was the clear fit.


### Infrastructure Setup


We started with a single shard, three-replica cluster. Each node runs on 4 CPU / 16GB RAM / 1TB storage. All three replicas act as masters — writes land on any node and replicate to the others, so reads can be served from any node too. This gives us high availability out of the box: a failed node doesn’t take down the cluster, and rejoining a recovered node is automatic.


ZooKeeper handles coordination: it tracks which parts of a table live where, orchestrates merges, compactions, and replication. One important safety rail — if a node loses its ZooKeeper connection, it transitions its replicated tables to **READONLY** mode. It stops accepting writes while the other nodes continue, and once reconnected, it catches up and resumes. No silent data divergence.


For cluster management we used the **Altinity Kubernetes Operator** , which let us handle configuration, user management, and service endpoints entirely through YAML. Metrics flow into Grafana using Altinity’s published dashboard templates. Backups to S3 are configured via Altinity ClickHouse Backup with a defined schedule and rotation policy.


### Ingesting Raw Events


ClickHouse’s Kafka Engine made ingestion straightforward. We attached materialized views to the engine — one per version of our event schema — each feeding into the same canonical table contract. We also captured Kafka virtual fields (_topic, _partition, _offset) in each row, giving us a precise audit trail for debugging event loss or corruption.


The raw events table uses ReplicatedMergeTree: columnar storage, sparse primary index, custom partitioning, and cross-node replication.


### Deduplication Strategy


The obvious candidate was ReplacingReplicatedMergeTree, where ClickHouse itself deduplicates on merge. We rejected it for three reasons:


1. **Eventual consistency only.** Deduplication happens during part merges. At our ingest rates, merge frequency isn’t guaranteed — reads between merges will see duplicates.
2. **No custom dedup logic.** Dedup rules differed by ad slot type. A single merge key couldn’t express that.
3. **Irreversibility.** Once duplicates are merged away, you can’t reconstruct them. Future rule changes would have no raw data to reprocess against.


Our solution: **store everything, deduplicate at query time.** Each ingested record includes a _timestamp field set to the arrival timestamp in nanoseconds. To get the canonical version of any event, we group on the fields that define uniqueness and use argMax to pick the latest values:


```text
SELECT      eventId,      advertiserId,      campaignId,      bidType,      argMax(bid,  _timestamp) AS bid,      argMax(event_timestamp, _timestamp) AS created_at,      argMax(session_id, _timestamp) AS session_id  FROM ad_impressions  GROUP BY      eventId, advertiserId, campaignId, campaignId .....  -- eventId + advertiserId + campaignId + campaignId ... define uniqueness  -- argMax picks the latest value for all non-key fields
```


This keeps raw history intact, supports custom dedup logic per event type, and remains fully reprocessable if rules change.


### Partitioning and Ordering


With 1B events a day, how data is physically laid out has a direct impact on both query latency and ingestion throughput.


**Partition key:** Day partitioning on event time. Real-time ingestion writes to a single partition. Analytics queries rarely span more than two days, so ClickHouse reads at most two partitions instead of scanning a large monthly slab.


**Order key:** (advertiserId, campaignId, bidType, …). Our most common query pattern filters by advertiser and campaign, so those come first. Columns are ordered by increasing cardinality to maximize compression — similar values cluster within the same granule, reducing storage and speeding up scans.


### Building the Analytics Layer


### The Aggregation Problem


With raw events stored and deduplicated, the next challenge was aggregating them into queryable metrics across all dimensions. We chose 5-minute windows — fine-grained enough for near real-time budget enforcement, and easy to roll up into hours, days, or months by summing buckets.


Our first attempt used a ClickHouse materialized view on the raw events table with an AggregatingMergeTree engine. It failed in two ways: dedup logic couldn't be applied before aggregation, and late-arriving events got bucketed to the wrong window.


We switched to a **scheduled worker** — a cron-like job that fires every hour and runs a SQL command to:


1. Deduplicate raw events over the last 4 hours using argMax
2. Tag each event to the correct 5-minute window using toStartOfFiveMinute(event_timestamp)
3. Insert the aggregated result into the analytics table, overwriting prior calculations for those windows


The 4-hour lookback handles late events: an event delayed by network issues or replay still lands in the right window based on its original event_timestamp, not its arrival time.


### Controlling Query Scope


Running a 4-hour dedup-and-aggregate over all events every hour would be expensive. The key insight: once a campaign stops receiving events, recalculating its aggregates is wasted work. We added a subquery to filter only the brands and campaigns that had events in the last hour:


```text
WHERE (advertiserId, campaignId) IN (      SELECT advertiserId, campaignId      FROM   ad_impressions      WHERE  event_timestamp >= (now() - toIntervalHour(1))      GROUP BY advertiserId, campaignId  )  AND event_timestamp >= toStartOfFiveMinute(now() - toIntervalHour(4))
```


This scopes the expensive in-memory dedup step to only active campaigns, cutting query load dramatically during off-peak hours.


### The Full Aggregation Query


Here’s an excerpt of the query that ties it all together:


```text
INSERT INTO advertiser_analytics  SELECT      advertiserId, campaignId, bidType,      inventoryId, searchTerm, deviceType,      cityName, page, widget
```


```text
sum(coalesce(impressions, 0))        AS impressions,      sum(coalesce(clicks, 0))             AS clicks,      toStartOfFiveMinute(event_timestamp) AS created_at,      now()                                AS _timestamp   ( /* deduplicated raw events subquery */ )  GROUP BY      advertiserId, campaignId, bidType,      inventoryId, searchTerm, deviceType,      cityName, page, widget, created_at
```


### Handling Re-aggregation Without Bloat


Every hourly run recalculates windows in the last 4 hours. If we simply append, the analytics table accumulates multiple rows for the same window — daily or monthly rollups would double-count. We solved this with ReplacingMergeTree on the analytics table, using the combination of dimension columns as the primary key and _timestamp (time of aggregation) to track which row is latest.


> ***Gotcha:*** ** *ReplacingMergeTree only deduplicates during part merges, which aren't instant. We expose the analytics data through a view that applies the same* *argMax(_timestamp) pattern at read time — guaranteeing callers always see the latest, non-duplicated aggregations.*


### Phase 3 — Scaling to a Billion Events


With new advertisers onboarding steadily, event volume kept climbing until we crossed 1 billion events a day. We had already upgraded the cluster to 32 CPU / 90 GB RAM / 3 TB storage per node, tightened the dedupe window to 4 hours, and dropped the job to hourly. It wasn’t enough.


Going back to the whiteboard, we identified two distinct bottlenecks.


### Bottleneck 1 — Aggregating All Advertisers Together


The hourly aggregation job was running dedup across the entire raw events table for all active advertisers in a single query. The dedup logic — grouping, sorting, and argMax over millions of rows — is inherently memory-intensive. Running it for every advertiser in one shot meant memory pressure grew with the total event volume, not with any individual advertiser's share.


The fix was to make aggregation **event-driven and per-advertiser** .


Instead of one monolithic query, the job now does two things:


1. **Compute the work list.** At the start of each cycle, a lightweight query identifies every brand that received events in the last 4 hours.
2. **Enqueue per-brand tasks.** Each brand (or small batch of brands) becomes an independent task published to a Kafka topic. A pool of worker processes consumes these tasks and executes the aggregation query scoped to that brand alone.


This matters because query cost doesn’t scale linearly with row count — memory and time grow faster. Processing smaller slices independently is strictly cheaper in aggregate: if cost is proportional to *n²* , then 10² > 5² + 5². Beyond the math, it also gave us a natural parallelism knob — we can scale workers up or down independently of the ClickHouse cluster, and a slow brand doesn't block every other advertiser.


### Bottleneck 2 — The Ever-Growing 5-Minute Table


The 5-minute aggregation table had become a problem for advertiser-facing queries. It holds multiple high-cardinality dimension columns — device, keyword, city, category, placement, and more — at 5-minute granularity. When an advertiser queries a quarter’s worth of data, the engine still needs to scan and aggregate millions of rows, even though the advertiser only needs a handful of rolled-up views.


The insight: **advertisers don’t need all dimension combinations.** They don’t need device × keyword data. What they need is day-level analytics sliced by city, by keyword, or by page — independently.


We built a set of **low-cardinality day-level tables** tailored to the actual query patterns advertisers use. The 5-minute table still exists but is now reserved for internal platform analytics, which has lower throughput requirements and no tight latency SLA. Advertiser-facing dashboards hit the pre-rolled day-level tables instead — dramatically smaller, dramatically faster.


### Bottleneck 3 — Budget Enforcement Can’t Wait an Hour


Moving to an hourly aggregation job introduced a new problem: a campaign burning through its daily budget could overspend for up to an hour before we caught it. The analytics job and the budget enforcement job had very different latency requirements, and we had been conflating them.


We separated them into two independent jobs:


**Job 1 — Analytics (hourly).** Builds the full multi-dimensional, multi-metric aggregations for advertiser dashboards and platform insights. This is the expensive job and it’s fine running hourly — advertisers checking yesterday’s keyword performance don’t need sub-minute freshness.


**Job 2 — Daily spend (frequent, lightweight).** Calculates only the cumulative spend for each campaign for the current day. No dimensions, no secondary metrics. Critically, it only reads the event type that actually drives charges for that campaign — clicks for CPC campaigns, impressions for CPM campaigns. Everything else in the raw events table is ignored entirely.


This selectivity is what makes the job fast enough to run frequently. By reading only the billing-relevant event type, the raw events scanned per campaign drop dramatically. Combined with scoping execution to campaigns that had events in the last 3 hours, the query is lightweight enough to run at a cadence where overspend risk is acceptably small.


Like the analytics job, this one is broken down per-advertiser and enqueued as async tasks — so it inherits the same parallelism and isolation benefits without adding load to the main aggregation pipeline. The job runs every 3 minutes and completes in around 1 minute, meaning spend data is never more than a few minutes stale — a far tighter safety net than the hour-level window we had before.


### Further Tuning


Beyond the two structural changes, a few targeted optimizations made a meaningful difference:


**Resource isolation per use case.** High-throughput advertiser queries were competing with the background worker jobs for CPU and memory on the same ClickHouse nodes. We created separate ClickHouse users for each workload — advertiser queries, aggregation workers, and internal analytics — with explicit CPU and memory limits per user. A runaway advertiser query can no longer starve the aggregation pipeline.


**Spilling to disk for heavy queries.** For queries that genuinely need to process large working sets, we configured max_bytes_before_external_group_by and max_bytes_before_external_sort. When in-memory usage crosses the threshold, ClickHouse spills intermediate state to disk rather than OOM-killing the query. It's slower than in-memory, but it's controlled and predictable — which is far better than an outage.


**Tiered storage — hot local, cold S3.** Raw events and 5-minute aggregations are queried heavily for recent data and rarely for anything older than a few weeks. We set up partition-level archival policies to move older partitions to S3. Because we had partitioned by date from the start, the archival configuration was straightforward — ClickHouse’s toDate(created_at) partitions map cleanly to an age-based policy. Local NVMe stays lean; cold data stays accessible.


### What We’d Tell Ourselves Earlier


**1. Don’t aggregate away your raw data.** Pre-aggregation is tempting for storage efficiency, but it makes deduplication, debugging, and rule changes impossible. Store raw, aggregate at query time.


**2. Design for late events from day one.** Network delays, retries, and replays are the norm, not the exception. A 4-hour late-event window combined with event-time bucketing absorbed all the edge cases we encountered.


**3. Partition and order keys are your most consequential schema decisions.** We chose toDate(created_at) as partition key and (advertiserId, campaignId) as the leading order key. These two choices had more impact on performance than any query optimization we did afterward.


**4. Scope your aggregation jobs tightly.** Only recalculating active campaigns (those with events in the last hour) cut our per-run compute significantly. Know what you can safely skip.


**5.** **ReplacingMergeTree needs a view on top.** Relying on merge-triggered dedup for reads will burn you. Always expose aggregated data through a dedup view that applies argMax inline.


*The system today handles over a billion events per day, provides spend analytics to advertisers across dozens of dimensions, and powers the billing engine without overcharging or undercounting. The architecture is still evolving — but the core principles above have held across every iteration.*


---


[A Billion Events a Day: How We Built Ads Analytics That Actually Works](https://blog.zepto.com/a-billion-events-a-day-how-we-built-ads-analytics-that-actually-works-c28904bbf8de) was originally published in[Zepto TechXPress](https://blog.zepto.com/) on Medium, where people are continuing the conversation by highlighting and responding to this story.
