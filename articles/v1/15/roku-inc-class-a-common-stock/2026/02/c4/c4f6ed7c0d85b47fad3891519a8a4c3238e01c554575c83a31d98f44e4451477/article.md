---
schema_version: "1.0.0"
document_id: "c4f6ed7c0d85b47fad3891519a8a4c3238e01c554575c83a31d98f44e4451477"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/reducing-storage-costs-at-petabyte-scale-partition-aware-data-retention-at-roku"
published_at: "2026-02-18T13:32:15+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:656cb3e53fb2741b5c555e5aab2994518f8d9911e0cb0b06cb52b01b298c8773"
---

# Reducing Storage Costs at Petabyte Scale: Partition-Aware Data Retention at Roku

By Bharath Krishna, Abhilash Mittapalli *@ Roku Big Data Platform Team*


In the world of big data, storage isn’t just a technical challenge—it’s a significant cost driver, typically accounting for a noticeable share of total infrastructure spend


. At Roku, with petabytes of streaming data powering our platform, optimizing these costs while maintaining data accessibility and privacy compliance is critical. Since launching our partition-aware retention service in early 2025, we’ve archived petabytes of historical data and deleted a comparable volume of unused and orphaned data,


resulting in substantial cost savings while improving our data governance.


This post explores how we built a retention system that goes beyond traditional cloud-native solutions to deliver smarter, more cost-effective data lifecycle management.


## The Storage Cost Challenge


In an open-source big data environment, the majority of data is stored in object stores like S3 or Google Cloud Storage, while a metadata management solution like Apache Hive maps this data to tables for querying. While this architecture scales well, it comes with a fundamental challenge: not all data are


created equal.


Recent data tends to be accessed frequently (hot data), while older data is rarely touched (cold data). Cloud providers recognize this and offer tiered storage options—STANDARD, NEARLINE, COLDLINE, ARCHIVE—each with progressively lower costs. The problem? Most existing solutions fall short in several critical ways


:


- **Region Lock-in** : Solutions like GCP Auto-class and S3 Intelligent-Tiering optimize storage within a single cloud region. This means your hottest and coldest data must reside in the same expensive region, leaving significant cost optimization opportunities on the table. For example, archiving data in GCP’s us-central1 costs roughly half of what us-east4 charges for archive storage—a difference of 1000s of dollars per petabyte per month


.
- **Inaccurate Recency Tracking** : Traditional lifecycle rules rely on object last-modified timestamps or Hive partition add times. This breaks down in several common scenarios:


- **Data backfills** : When you backfill 2021 data today, the system incorrectly treats it as recent data
- **Cloud migrations** : When migrating data across cloud providers


all objects received current timestamps, making years of historical data appear “new” to lifecycle rules. While some object stores support metadata fields for creation timestamps, relying on these is fragile and inconsistent across platforms.
- **Pipeline maintenance** : Partition drops and re-adds during schema changes reset timestamps


- **Inflexible Schedules** : Cloud-native solutions transition data based on fixed schedules (30, 60, 180 days of no access) and charge management fees for billions of objects. T


hese fees add up quickly in large-scale data warehouses.
- **Limited Granularity** : Lifecycle rules are applied at the bucket level with hard limits (e.g., 100 lifecycle rules per GCP bucket). In a data warehouse with thousands of tables, each requiring different retention policies, managing bucket-level rules becomes impractical. You can’t easily set table-specific or partition-specific retention policies without complex bucket organization strategies.


We needed a solution that understood the semantic meaning of our data, not just when files were touched.


## Requirements: Building the Right Solution


Our retention system needed to address several key requirements:


- **Partition Semantics Awareness** : Understand the true recency of data based on partition values (e.g., date=2021-01-15


), not file timestamps
- **Multi-Region Flexibility** : Archive data to different regions or even different cloud providers to optimize costs
- **Fault Tolerance** : Handle failures gracefully when dealing with millions of objects without data loss or exposing incomplete data to users
- **Governance** : Prevent unauthorized retention policy changes through proper access controls
- **Flexibility** : Support various retention periods across different table categories (3 months for raw facts, 3 years for aggregates, etc.)


## Impact and Results


Since launching in early 2025, our retention system has delivered substantial benefits:


Cost Savings


- **$1k-$2k per PB per month archived** when moved from high cost to lower-cost storage regions
- **10s of PB deleted** after retention periods expired
- **Break-even on transfer costs** within 3 months
- **Eliminated intelligent tier management fees** for billions of archived objects


For data retained in archive for at least 6 months, we see significant ongoing savings compared to same-region archival.


### Operational Flexibility


Unlike cloud-native solutions that work on fixed schedules (30, 60, 180 days), we can:


- Archive data after exactly 93 days for certain table categories
- Restore archived data and automatically delete it after 30 days (vs. waiting 365 days with Auto-class)
- Archive to lower-cost regions within GCP to optimize storage expenses


## Solution Architecture


Our retention system consists of five main components working together to provide intelligent, partition-aware data lifecycle management:


### **1. Retention Tagging System**


We leverage Hive’s TABLEPROPERTIES


feature to let users tag tables with retention metadata. Here’s an example DDL:


```text
CREATE TABLE user_events (
user_id STRING,
event_type STRING,
timestamp BIGINT
)
PARTITIONED BY (date STRING)
TBLPROPERTIES (
'TABLE_CATEGORY'='enrichedFact6m',
'time_partition_column'='date',
'time_partition_format'='yyyy-MM-dd'
);
```


The *TABLE_CATEGORY* property maps to a centralized configuration that defines retention policies:


```text
[
{
"table_category": "enrichedFact3m",
"retention_period": 93,
"retention_policy": "ARCHIVE"
},
{
"table_category": "stage",
"retention_period": 31,
"retention_policy": "DELETE"
},
{
"table_category": "dailyAgg",
"retention_period": 1116,
"retention_policy": "ARCHIVE"
},
{
"table_category": "weeklyAgg",
"retention_period": -1,
"retention_policy": "ARCHIVE"
}
]
```


A retention period of -1


means data is retained indefinitely. This centralized approach ensures consistency across our ~10’s of thousands of tables.


### 2. Partition Semantics Engine


The heart of our system is the partition semantics engine—a producer component that understands the true meaning of partition values. This is what sets our solution apart from traditional lifecycle rules.


**The Challenge** : Our data warehouse uses diverse partitioning schemes across thousands of tables:


- date=2024-01-15


- year=2024/month=01/day=15


- dt=20240115


- event_date=2024-01-15/hour=14


**Our Approach** :


1. **Exhaustive Format Library** : We maintain a comprehensive list of datetime formats used across the warehouse. When we encounter a new format that doesn’t parse, our alerting system notifies the team to add it to the library.
2. **Nested Partition Parsing** : For nested partitions, we parse from outermost to innermost, using the first successfully parsed partition as the recency indicator.
3. **Backfill Resilience** : Even if you backfill data from 2021 today, the system reads the partition value ( date=2021-01-15


), not the file modification time, correctly identifying it as old data eligible for archival.


The producer runs as a scheduled Kubernetes CronJob, scanning tables with retention policies, parsing partition semantics, and logging partitions that exceed their retention period to a MySQL metadata store for consumption.


### 3. Fault-Tolerant Consumer Pipeline


The consumer component is where the archival magic happens. It processes partitions queued by the producer through a carefully ordered four-step process designed for idempotency and fault tolerance:


**Step 1: Serialize Metadata**


```text
# Serialize partition metadata as JSON and copy to archive bucket
partition_metadata = {
"database": "analytics",
"table": "user_events",
"partition": "date=2023-01-15",
"location": "fs://prod-data/warehouse/user_events/date=2023-01-15",
"schema": table_schema
}
# Copy to fs://archive-storage/metadata/analytics/user_events/date=2023-01-15/partition.json
```


This enables future restoration if needed and provides a complete audit trail.


**Step 2: Transfer Data**


We create a Storage Transfer Service job


configured to:


- Copy data to the archive bucket (potentially in a different region)
- Delete files at the destination that differ from the source
- Delete files that only exist at the destination


This configuration makes the transfer idempotent—we can retry unlimited times without worrying about partial state.


**Step 3: Drop Partition**


Before deleting any data, we drop the partition from the Hive metastore:


```text
ALTER TABLE analytics.user_events DROP PARTITION (date='2023-01-15');
```


This critical ordering ensures users never query incomplete data during deletion. If deletion fails, the partition is already gone from the metastore, preventing access to potentially incomplete data.


**Step 4: Delete Source Data**


Finally, we delete the data from non-archive storage. If this step fails, we log the location to a cleanup queue for future processing.


**Fault Tolerance** : If any step fails, the entire process is retried from the failed step. The idempotent design of each step ensures we can retry safely without data corruption or loss.


The consumer runs continuously as a Kubernetes deployment managed via Helm, processing partitions from the queue with configurable concurrency and retry logic.


### Supporting Modern Table Formats: Apache Iceberg


While our system was initially designed for Hive tables, we’ve extended support to Apache Iceberg—as we have been migrating hive tables to this increasingly popular open table format.


**Why Iceberg integration matters** : Iceberg provides ACID(Atomicity, Consistency, Isolation, Durability)


transactions, schema evolution, and improved performance over traditional Hive tables. As organizations modernize their data lakes, supporting Iceberg ensures our retention system remains relevant.


**The workflow adaptation** : Our Producer component queries the Iceberg table manifest instead of directly scanning partition metadata. This allows us to:


- Collect accurate statistics (number of files, total bytes) from Iceberg’s metadata layer
- Leverage Iceberg’s efficient partition pruning capabilities
- Maintain the same partition semantics parsing logic across both table formats


The Consumer follows a similar four-step archival process:


1. Archive partition metadata as JSON
2. Use


Storage Transfer Service to copy data to archive storage
3. Drop the partition from the Iceberg/Hive metastore
4. Delete source data


This unified approach means data engineers can use either Hive or Iceberg tables with the same retention policies—simply tag tables with TABLE_CATEGORY


and our system handles the rest. As the industry continues adopting Iceberg for its superior performance and features, our retention solution scales alongside without requiring fundamental redesign.


### 4. Data Restoration System


Sometimes you need archived data back—for regulatory requests, special analyses, or debugging. Our restoration system handles this gracefully:


Users submit a restoration request specifying:


- Database, table, and partition range
- Start and end datetime
- Expiry date for the restored data


The restoration component:


1. Copies data from archive storage back to non-archive storage
2. Restores partition metadata to the Hive metastore (for Hive tables) or uses Iceberg’s add_files


procedure for Iceberg tables
3. Marks restored partitions as read-only


**Iceberg Restoration** : For Iceberg tables, we follow the same data copy approach as Hive to copy data from archive to actual table location, then leverage the add_files


stored procedure to register the restored files into the table metadata:


```text
CALL system.add_files(
table => 'analytics.user_events',
source => 'fs://prod-data/warehouse/user_events/date=2023-01-15/*'
);
```


This procedure reads the Parquet file footers from the restored location, extracts schema and partition information, and commits the metadata changes atomically. This approach is significantly faster than traditional INSERT


operations while ensuring data consistency.


**Key Design Decision** : Restored data remains in archive storage even after restoration. This avoids:


- Round-trip transfer costs
- Early deletion charges from archive storage (most archive tiers require minimum retention periods)


Once the restored data reaches its expiry date, the consumer automatically cleans it up from non-archive storage, while the archive copy remains untouched.


### 5. Metastore Retention Hook: Governance Layer


To prevent unauthorized retention policy changes, we implemented a custom listener in Hive Metastore 3.1.2. This hook intercepts DDL transactions and enforces governance rules:


**Table Creation** : Reject if no retention policy is specified


```text
-- This will be REJECTED
CREATE TABLE my_table(id INT, value STRING);


-- This will be ACCEPTED
CREATE TABLE my_table (id INT, value STRING)
TBLPROPERTIES ('TABLE_CATEGORY'='enrichedFact6m');
```


**Retention Property Updates** : Only users with the RETENTION_ADMIN


role can modify TABLE_CATEGORY


or related properties.


**Partition Additions** : Reject partitions older than the retention period unless added by a RETENTION_ADMIN


.


This ensures that retention policies can only be changed through a formal approval process, maintaining data governance integrity.


#### Two-Tier Governance: Data Contracts vs. Self-Service


Our hook enforces different governance workflows based on retention duration, balancing detailed reviews with developer agility :


**Long-term retention (>6 months)** : Must be specified through **data contracts** —formal agreements that define table schemas, ownership, and lifecycle policies. When the hook detects a table with retention exceeding 6 months, it enforces:


- A valid data contract file in the repository with retention policy declared
- Sign-off from a designated retention approver who evaluates the business justification for extended storage
- Proper TABLE_CATEGORY


mapping to approved retention tiers (e.g., dailyAgg


for 3+ years)


This approval workflow prevents expensive long-term retention from proliferating without cost-benefit analysis. Data contracts can specify retention through:


```text
# data_contract.yaml
tables:
name: critical_business_metrics
retention:
table_category: dailyAgg # Maps to 3+ years retention automatically
justification: "Required for annual compliance reporting"
approver: data-governance-team
```


**Short-term retention (<6 months)** : Users can specify retention directly via table properties for agility:


```text
CREATE TABLE temp_analytics (
session_id STRING,
event_count INT
)
PARTITIONED BY (date STRING)
TBLPROPERTIES (
'TABLE_CATEGORY'='enrichedFact3m', -- 93 days, self-service
'time_partition_column'='date',
'time_partition_format'='yyyy-MM-dd'
);
```


No additional approvals needed—the hook validates the category exists and accepts the table immediately.


**Why this matters** : This two-tier model prevents the most common cost pitfall in data platforms: tables created for short-term experiments that accidentally persist for years. By requiring explicit approval for retention policies exceeding 6 months, we ensure every long-lived table has clear business justification. Meanwhile, engineers retain the flexibility to iterate quickly on transient datasets without bureaucratic overhead.


## Key Takeaways: When to Build vs. Buy


Cloud-native solutions like S3 Intelligent-Tiering and GCP Auto-class work well for many use cases. However, you might need a custom solution when:


1. **Your data has semantic meaning** that’s more accurate than access patterns or file timestamps
2. **Cost optimization requires multi-region or multi-cloud strategies** beyond what native solutions support
3. **You have complex partitioning schemes** across thousands of tables that need unified handling


For us, the investment in building a custom retention system paid off through substantial cost savings and operational flexibility that cloud-native solutions couldn’t provide.


## Current Limitations and Scope


While our retention system handles the vast majority of use cases in our data warehouse, it’s important to understand its current boundaries:


### Partitioned Tables with Date-Based Keys Only


**Current Requirement** : The system currently supports **only partitioned tables** where at least one partition key represents a date or timestamp.


**What’s supported** :


- Single date partition: PARTITIONED BY (date STRING)


- Nested date partitions: PARTITIONED BY (year INT, month INT, day INT)


- Date with additional dimensions: PARTITIONED BY (date STRING, region STRING)


**What’s NOT supported** :


- **Non-partitioned tables** : Tables without any partition keys cannot be processed by the retention system. The partition semantics engine requires at least one date-based partition to determine data recency.
- **Non-date based partitions only** : Tables partitioned solely by non-temporal dimensions (e.g., PARTITIONED BY (region STRING, product_id STRING)


) are not eligible, as the system cannot determine which data is “old” without a temporal component.


We recognize this limitation and are actively exploring solutions to support non-partitioned tables in future iterations.


## Conclusion


Managing petabytes of data cost-effectively i


s a complex challenge. By building a retention system that understands partition semantics rather than relying on file timestamps, we’ve created a solution that’s resilient to backfills, flexible in storage strategy, and capable of delivering significant cost savings.


The key innovation—parsing partition values to determine true data recency—might seem simple, but it unlocks capabilities that generic lifecycle rules can’t match. Combined with fault-tolerant processing and strong governance, this approach has become a cornerstone of our big data platform’s cost and compliance strategy.


---


The post[Reducing Storage Costs at Petabyte Scale: Partition-Aware Data Retention at Roku](https://engineering.roku.com/reducing-storage-costs-at-petabyte-scale-partition-aware-data-retention-at-roku) appeared first on[Engineering Blog](https://engineering.roku.com/) .
