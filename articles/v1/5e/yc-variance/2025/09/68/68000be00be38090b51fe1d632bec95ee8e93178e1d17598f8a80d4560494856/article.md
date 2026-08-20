---
schema_version: "1.0.0"
document_id: "68000be00be38090b51fe1d632bec95ee8e93178e1d17598f8a80d4560494856"
company_key: "yc-variance"
company: "Variance"
source_id: "yc-variance-news-import-ff45b1fc661c"
canonical_url: "https://www.variance.com/blog/features-on-demand-using-clickhouse"
published_at: "2025-09-01T00:00:00+00:00"
first_seen_at: "2026-07-22T18:28:25.562528+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:48b94d0816a438cef6d07ff04f7b8939936479a774e19fcc23242a0c898290e4"
---

# Features on Demand: Ad-Hoc Feature Generation with ClickHouse

When studying robust systems, one emergent property we observe is their capacity to self-heal. Roman concrete is a classic example: while ordinary concrete cracks and crumbles within decades, the Pantheon still stands nearly two thousand years later. Its secret, unbeknownst even to the Romans, was the volcanic ash they used. This material allowed the concrete to slowly patch its own fractures, extending its life far beyond the empire itself.


We can think about modern infrastructure in much the same way. Robust systems are not defined solely by initial strength, but by resilience and the ability to adapt and recover from damage in an environment.


Fraud systems face an analogous challenge. The bottleneck isn't just better features, more powerful models, or greater compute. It is resilience in the face of change, which we call distribution shift. We call the rate at which model efficacy decays "adaptation," and currently, models must always be constantly retrained and redeployed with fresh data in order to keep up.


The faster you can get a model to market that captures the latest fraud patterns, the smaller the window of opportunity for exploitation. What if we could build a system that doesn't need to be bottlenecked on re-training pipelines or model deployments to adapt to changes in behavior?


We view AI agents as a critical part of the self-healing infrastructure of a modern fraud team. Unlike rigid rules and classifiers, AI agents can self-correct and adapt rapidly to changes in behavior. Since AI agents can autonomously query data and build their own plans, they're not bound by specific features and rules that must be manually managed by data scientists.


### Context is all you need


Key to this insight is that if we can architect **seconds-fresh context,** then we can build a system that has an edge in adversarial environments. The problem is that most stacks split their defenses into two worlds:


- **Real-time features** for transactional models and rules
- **Ad-hoc investigations** for human analysts poking at data lakes hours or days later


Unfortunately, real-time rules and classifiers can only evolve as quickly as traditional classifiers can be trained and human-crafted features can be built out. Investigative workloads, on the other hand, are bottlenecked by the human ability to build SQL queries on the fly, and model a fraud pattern using a query.


**Hippocampus,** our unified streaming feature store optimized for sub-second joins across millions of events built on top of ClickHouse, gives us both in one place. A little bit like the human Hippocampus, which allows us to form and store memories by connecting different experiences and contexts, our Hippocampus acts as a unified context store, allowing AI agents to retrieve hard-to-compute information on the fly and with extremely low latency.


### The situation


A national grocery delivery app launches a promotion resulting in a sign-up spike. Some are real, but others are bots that are chaining devices, cards, and SIMs. To separate the good from the bad, you need to look at the transactions, but you also need the answers to questions like:


- How many accounts has this **/24 IP range** created this week?
- Has this **SIM** been flagged before?
- Which **device fingerprints** or **card bin ranges** co-occur with this email domain?


For most teams, catching these patterns take hours, given that analysts need to wait for stale warehouse queries, while thousands of dollars are lost to fraud. With Hippocampus, the context remains fresh, and an agent evaluating a new account can pull in one fell swoop the account information as well as the "cluster" it belongs to.


This allows lengthy investigative and reactive workflows to be compressed into under 30-second decisions.


#### Generating features on demand


To fully take advantage of the flexibility that's now possible, we wanted to design a data backend that enables the querying of arbitrary features in real-time, while avoiding the cost and limitations of adopting a traditional online and offline feature store.


Additionally, we observed AI agents generate and execute queries much faster than human analysts, and truly benefit from low-latency real-time OLAP systems at scale. For our use case, the consistency constraints were acceptable given that we typically do our assessments in streaming environments.


Feature Rules/Feature store Hippocampus (Agents) BigQuery/Delta Lake


QPS fit Per-transaction Per-transaction Analyst/low QPS


Latency 100s of ms - s seconds - minutes minutes - hours


Flexibility Pre-computed only Fully dynamic Fully dynamic


Cost per query Low Low High


#### How we built it


We chose ClickHouse for its native support for fast point lookups + window scans, and leaned on incremental materialized views so that we can ingest our data once, and generate two read paths: one for stateful aggregations across time window scans, and another for compact snapshots for point lookups (the latest state per entity/field).


These materialized views are computed on write, and thus can materialize in seconds without requiring additional streaming or batch infrastructure. Since most queries are point lookups, we store events and roll-ups on a per-entity basis (such as users, IPs, devices, etc.), allowing for rapid lookups in ClickHouse. We also leverage ClickHouse's configurable bloom filter to further filter our data at query time, since many queries take the shape of looking up a particular subset of items matching a specific attribute pair (such as an IP address, or a device identifier).


### Sample agent query patterns


What makes this architecture so simple is that, at this point, we can simply query our data using ordinary SQL. AI agents are already very proficient at building SQL queries for analytics, but this is a novel application where AI agents are applied for online risking use cases.


#### Point snapshot (current state for an entity)


```text
SELECT    mapFromArrays  (    groupArray  (  field  )  ,    groupArray  (  finalizeAggregation  (  value_state  )  )     )     AS     data  ,     max  (  finalizeAggregation  (  updated_at_state  )  )     AS   data_updated_at    FROM   feature_store  .  entity_fields_latest    WHERE   entity_id   =     'driver_123'     GROUP     BY   entity_id  ;
```


#### Count a specific pair in a time window


```text
-- How many times did (device_fp, ip) appear together in the last 7 days?     SELECT   countDistinct  (  event_id  )     AS   cooccurrences    FROM   feature_store  .  semantic_events    WHERE   occurred_at   >=     now  (  )     -     INTERVAL     7     DAY      AND   event_type   IN     (  'signup'  ,  'login'  )     AND   JSONExtractString  (  data  ,     'device_fp'  )     =   {device_fp}    AND   JSONExtractString  (  data  ,     'ip'  )     =   {ip}  ;
```


#### Top co-occurring pair in a window


```text
SELECT     JSONExtractString  (  data  ,     'device_fp'  )     AS   device_fp  ,     JSONExtractString  (  data  ,     'ip'  )        AS   ip  ,     countDistinct  (  event_id  )           AS   c    FROM   feature_store  .  semantic_events    WHERE   occurred_at   >=     now  (  )     -     INTERVAL     24     HOUR       AND   event_type   IN     (  'signup'  ,  'login'  )       AND   device_fp   !=     ''     AND   ip   !=     ''     GROUP     BY   device_fp  ,   ip    ORDER     BY   c   DESC     LIMIT     100  ;
```


### Benchmarks


To help test this data model, we generated a benchmark of 5 million simulated user logins, devices, and signup events. Note that in production, we have seen this model scale to billions of events, across petabytes of data for our largest customers.


With our existing infrastructure, we can query our datasets across three different use cases: point lookups, windowed aggregation against specific device and IP pairs, and a top-k aggregation against all device and IP pairs. These query patterns closely simulate real contextual features used in real-time risking engines. These results were conducted on a single ClickHouse instance running with 8GB of memory. We use the ClickHouse benchmark tool to run these benchmarks for 50 iterations with a concurrency of 8 per query.


Query Window Runs Concurrency p50 (ms) p95 (ms) p99 (ms) Dataset (rows) Nodes / Storage


SNAPSHOT — 50 8 16.0 51.0 72.0 5,000,000 1 node / local NVMe


COOCC_PAIR_7D 7 days 50 8 55 110 180 5,000,000 1 node / local NVMe


COOCC_TOP_24H 24 hours 50 8 95 180 260 5,000,000 1 node / local NVMe


As we can see, we are able to achieve sub-second latency for these types of queries, and for certain point lookups, we have latencies that are comparable to point lookups to real-time key value stores, despite using an OLAP store for these loads.


### Caveat on Agents and RBAC Security


Letting AI agents write SQL is powerful, but it can threaten stability and security if unscoped.


Beyond prompt constraints, we enforce least privilege at the database boundary using Clickhouse's RBAC and row-level security. We provision a specific role that's coupled to a particular workspace or customer, that only has read-only access.


```text
CREATE   ROLE   IF     NOT     EXISTS   namespace_scoped_role  ;     GRANT     SELECT     ON   feature_store  .  semantic_events   TO   namespace_scoped_role  ;     GRANT     SELECT     ON   feature_store  .  entity_updates   TO   namespace_scoped_role  ;     GRANT     SELECT     ON   feature_store  .  entity_updates_by_field   TO   namespace_scoped_role  ;     GRANT     SELECT     ON   feature_store  .  entity_snapshots   TO   namespace_scoped_role  ;     CREATE     ROW   POLICY   IF     NOT     EXISTS   tenant_filter   ON   feature_store  .  semantic_events  ,   feature_store  .  entities    FOR     SELECT     USING   namespace_id   =   getSetting  (  'SQL_namespace_id'  )     TO   namespace_scoped_case_agent_role  ;     CREATE     USER     IF     NOT     EXISTS   tenant_scoped_case_agent_for_1   IDENTIFIED   BY     '<password>'    SETTINGS SQL_namespace_id   =     1     DEFAULT   ROLE namespace_scoped_role  ;
```


Although this is sufficient for our use cases for now, we think we can further harden this security for even more granularity with static validation of the generated SQL so that the agent can only query a certain universe of entities.


### Final thoughts


Hippocampus serves as the first system that is able to self-generate its own context in real-time, giving AI agents the memory needed to fight fraud as it happens. We're already pushing the limits of what AI agents can do for investigations, fraud reviews, and anomaly detection, and we're excited by what comes next.


If you're thinking about projections, partitions, and the shape of time, let's compare notes, as this is where the next decade of anti-abuse will be written.
