---
schema_version: "1.0.0"
document_id: "4c7466ab9dbe037fb7c5fca0f5523d7279cba9ac35a0db57e6c868b5ceabc877"
company_key: "yc-influxdata"
company: "InfluxData"
source_id: "yc-influxdata-rss-012b8d0fa152"
canonical_url: "https://www.influxdata.com/blog/downsampling-guide-influxdb-3/"
published_at: "2026-08-19T08:00:00+00:00"
first_seen_at: "2026-08-19T07:08:21.412239+00:00"
fetched_at: "2026-08-19T08:00:02.443841+00:00"
content_hash: "sha256:0ce9d5fb95c581d73b15822d3ffd3999a2ccd79bc03afe7b30852cb929329bbf"
---

# A Guide to Downsampling Time Series Data with InfluxDB 3

Summary


Downsampling turns high-frequency time series data into lower-resolution summaries. In InfluxDB 3, you can calculate those summaries by querying with SQL or materialize them on a schedule with the Python Processing Engine.


Table of Contents


This tutorial demonstrates both approaches using the InfluxDB 3 Processing Engine’s built-in bird tracking simulator plugin. You will generate telemetry, aggregate it into 10-second windows, and validate the result with SQL. The same pattern works for infrastructure metrics, industrial sensors, application telemetry, and other time series workloads.


## Why downsample time series data?


High-resolution data is valuable while diagnosing a recent event, but its value often changes as it ages. A temperature reading collected every second may be useful for an active incident, while a daily report may only need 10-minute or hourly averages.


Downsampling helps you:


- scan fewer rows in long-range queries
- make dashboards over weeks or months more responsive
- retain useful historical trends at a lower resolution, reducing storage costs
- calculate common summaries once instead of repeating the work
- keep raw data only for as long as its full resolution is useful


Downsampling is not the same as deleting raw data; it creates a summarized view or table at a lower granularity. Retention is a separate decision and process where data is fully deleted after a set period of time.


#### Choose Query-Time or Persisted Downsampling


InfluxDB 3 gives you two practical patterns for downsampling:


Approach How it works Best for Main tradeoff


Query-time SQL Uses` DATE_BIN` and aggregate functions in a` SELECT` query Exploration, flexible dashboards, and changing aggregation requirements Recomputes the result each time and does not reduce stored raw data


Python Processing Engine Runs the official downsampler plugin on a schedule and writes aggregate rows to a target table Repeated long-range queries, predictable rollups, and tiered retention Requires you to choose aggregates and scheduling behavior in advance


A useful starting point is to develop and validate an aggregate in SQL. If the same query becomes a frequent or expensive workload, then use the downsampler plugin.


## Prerequisites


To follow this tutorial, you need:


- InfluxDB 3 Core or Enterprise with the Processing Engine enabled
- InfluxDB 3 CLI installed and connected to the server
- An authorization token with permission to create a database and triggers
- Outbound access to GitHub so InfluxDB can load the official plugins referenced by gh: paths


The examples assume a database named` bird_demo` :


```text
influxdb3 create database bird_demo
```


If you are using the local Docker environment that accompanies this article, InfluxDB is available at` http://localhost:8181` . InfluxDB 3 Explorer is available separately at` http://localhost:8888` for running the SQL and capturing visual results.


#### Generate Sample Telemetry with the Official Bird Simulator


InfluxData’s[bird data simulator](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/bird_data_simulator/?utm_source=website&utm_medium=direct&utm_campaign=downsampling_guide_influxdb_3&utm_content=blog) is a convenient fit for this tutorial. It creates repeatable time series behavior without requiring a separate data generator or hardware sensor.


The plugin writes to the` bird_tracking` table. Its tags include species and name, and its fields include` body_temp` ,` longitude` ,` latitude` ,` speed` , and` heading` .


First, install the plugin’s` Faker` dependency:


```text
influxdb3 install package Faker
```


```text
influxdb3 create trigger \
--database bird_demo \
--path gh:influxdata/bird_data_simulator/bird_data_simulator.py \
--trigger-spec every:1s \
--trigger-arguments bird_count=10 \
bird_tracking_demo
```


After the trigger has run for several seconds, confirm that data is arriving:


```text
SELECT *
FROM bird_tracking
ORDER BY time DESC
LIMIT 5;
```


At one write per bird per second, the simulator produces approximately 600 rows per minute for 10 birds.


#### Downsample at Query Time with SQL


InfluxDB 3 SQL supports time bucketing with` DATE_BIN()` . The following query groups the raw bird readings into 10-second intervals and calculates speed and body-temperature statistics for each bird:


```text
SELECT
DATE_BIN(INTERVAL '10 seconds', time) AS time,
species,
name,
AVG(speed) AS avg_speed,
MIN(speed) AS min_speed,
MAX(speed) AS max_speed,
AVG(body_temp) AS avg_body_temp,
COUNT(*) AS record_count
FROM bird_tracking
WHERE time >= now() - INTERVAL '2 minutes'
GROUP BY 1, species, name
ORDER BY 1 DESC, species, name;
```


` DATE_BIN` aligns each timestamp to a 10-second boundary.` GROUP BY 1` refers to the first expression in the` SELECT` list while` species` and` name` preserve one series per bird.


This is true query-time downsampling: the server returns fewer, summarized rows, but it does not write them to another table. Change the interval or aggregates whenever the question changes. For example, a long-range dashboard might replace` INTERVAL '10 seconds'` with` INTERVAL '1 hour'` .


Always include a bounded time predicate. It reduces the data scanned and makes the query’s intended resolution explicit.


#### Persist Downsampled Data with the Python Processing Engine


For an aggregate that many users or dashboards repeatedly request, calculate it on a schedule with InfluxData’s official[downsampler plugin](https://docs.influxdata.com/influxdb3/core/plugins/library/official/downsampler/?utm_source=website&utm_medium=direct&utm_campaign=downsampling_guide_influxdb_3&utm_content=blog) . The plugin queries the source table, computes aggregates, and writes the results to a target table.


This trigger creates one average speed and body-temperature row per bird for every 10-second interval:


```text
influxdb3 create trigger \
--database bird_demo \
--path gh:influxdata/downsampler/downsampler.py \
--trigger-spec every:10s \
--trigger-arguments \
'source_measurement=bird_tracking,target_measurement=bird_tracking_10s,interval=10s,window=2min,offset=10s,calculations=speed:avg.body_temp:avg,specific_fields=speed.body_temp' \
bird_tracking_downsample
```


The trigger arguments control the rollup:


- ` source_measurement` is the raw source table.
- ` target_measurement` is the table that receives aggregates
- ` interval=10s` defines the time-bin width.
- ` calculations` assigns an aggregate function to each field
- ` specific_fields` limits processing to the fields needed by this rollup.
- ` offset=10s` delays the queried window so its newest bin has time to close.


#### Why the Offset Matters


The offset is not cosmetic. Without it, the simulator and downsampler could execute on the same 10-second scheduler boundary. Without an offset, the downsampler may query the newest bin while writes are still arriving, producing partial aggregates.


With` offset=10s` , the preceding completed bin and every aggregate in the validation window should have the expected 10 source points. In production, set the offset to at least the maximum delay you expect between an event occurring and becoming queryable. Workloads with late or out-of-order data may need a larger offset and lookback window.


#### Inspect the Persisted Result


The downsampler names calculated fields by appending the aggregate function, so` speed` becomes` speed_avg` and` body_temp` becomes` body_temp_avg` . It also writes` record_count` ,` time_from` , and` time_to` metadata.


The plugin stores the data with nanosecond precision and converts it with` TO_TIMESTAMP_NANOS()` when you want readable timestamps:


```text
SELECT
time,
species,
name,
ROUND(speed_avg, 2) AS avg_speed_mph,
ROUND(body_temp_avg, 2) AS avg_body_temp_c,
record_count,
TO_TIMESTAMP_NANOS(time_from) AS source_start,
TO_TIMESTAMP_NANOS(time_to) AS source_end
FROM bird_tracking_10s
ORDER BY time DESC, species, name
LIMIT 20;
```


#### Sample Output


Time (UTC) Species Bird Avg. speed Avg. body temp. Source points Source start


2026-08-14 21:21:00 American Robin Tracy 22.47 41.94 10 21:21:00


2026-08-14 21:21:00 Blue Jay Sara 21.59 41.67 10 21:21:00


2026-08-14 21:21:00 Cactus Wren Brandon 18.78 41.74 10 21:21:00


2026-08-14 21:21:00 Great Blue Heron Brenda 6.38 41.10 10 21:21:00


2026-08-14 21:21:00 Great Blue Heron Pamela 24.97 41.35 10 21:21:00


#### Validate the Reduction with SQL


Do not judge a downsampling job only by whether the target table contains rows. Compare a fixed, completed interval so active writes cannot change the counts during validation.


The following query compares two completed minutes of source data with the corresponding persisted rollups. **Replace the timestamps with a completed interval from your own run** :


```text
WITH raw AS (
SELECT COUNT(*) AS raw_rows, COUNT(DISTINCT name) AS birds
FROM bird_tracking
WHERE time >= TIMESTAMP '2026-08-14T21:20:00Z'
AND time " TIMESTAMP '2026-08-14T21:22:00Z'
),
downsampled AS (
SELECT
COUNT(*) AS downsampled_rows,
SUM(record_count) AS represented_raw_rows,
AVG(record_count) AS avg_points_per_row
FROM bird_tracking_10s
WHERE time >= TIMESTAMP '2026-08-14T21:20:00Z'
AND time " TIMESTAMP '2026-08-14T21:22:00Z'
)
SELECT
raw_rows,
birds,
downsampled_rows,
represented_raw_rows,
avg_points_per_row,
ROUND(
100.0 * (
1.0 - CAST(downsampled_rows AS DOUBLE) / CAST(raw_rows AS DOUBLE)
),
1
) AS row_reduction_percent
FROM raw CROSS JOIN downsampled;
```


#### Example Output


The equality between` raw_rows` and` represented_raw_rows` is an important completeness check. The 90% figure is a reduction in rows for this query shape, not a promise of an identical reduction in bytes on disk. Actual storage depends on schema, tags, fields, compression, and retention settings.


You should also compare aggregates against the query-time SQL version before applying any raw-data retention policy.


## Downsampling best practices for production


The tutorial uses short intervals so you can see results quickly. A production workload might keep second-level raw data briefly, create 5-minute summaries for operational dashboards, and return hourly summaries for long-term reporting.


When designing that pipeline:


- ` window=2min` gives each execution a lookback window large enough to reprocess recent data.
- Start with the questions users need to answer, then choose the interval. A bin that is too wide can hide short-lived behavior.
- Preserve the tags you need for filtering and grouping. Removing a dimension during aggregation cannot be reversed later.
- Select aggregates that match the signal. Gauges often need average, minimum, and maximum; counters may need sums or rates.
- Account for ingestion delay with` offset` , and use a lookback window that can catch expected late data.
- Validate counts and values over multiple completed intervals before shortening raw-data retention.
- Monitor the Processing Engine logs and query latency after deployment.


## Troubleshoot a scheduled downsampler


If the target table remains empty, inspect the Processing Engine logs:


```text
SELECT
trigger_name,
log_level,
log_text,
event_time
FROM system.processing_engine_logs
ORDER BY event_time DESC
LIMIT 20;
```


Then check the most common causes:


- The source table or a field name does not match the trigger arguments.
- The source does not contain data within the plugin’s window and offset.
- A Python dependency required by a plugin has not been installed.
- The server cannot retrieve a remote` gh:` plugin.
- The selected aggregate is not valid for a field’s data type.
- The newest interval is incomplete because the offset is too small.
- Getting started with downsampling with InfluxDB 3


InfluxDB 3 makes it possible to use the same SQL aggregation logic at two stages of a workload: interactively at query time and operationally as a persisted rollup. Start with` DATE_BIN` to confirm the right interval and dimensions. When the query becomes a stable, repeated access pattern, schedule the official Python downsampler and include an offset so it works on completed data.


Try the tutorial with[InfluxDB 3](https://www.influxdata.com/influxdb-signup/?utm_source=website&utm_medium=direct&utm_campaign=downsampling_guide_influxdb_3&utm_content=blog) and the[official InfluxDB 3 plugin library](https://github.com/influxdata/influxdb3_plugins/tree/main/influxdata/?utm_source=website&utm_medium=direct&utm_campaign=downsampling_guide_influxdb_3&utm_content=blog) . Once your results match the raw source over a completed interval, adapt the interval, aggregates, offset, and retention strategy to your production workload.


## FAQs


What is downsampling? Downsampling is the process of grouping time series points into wider time intervals and calculating summaries such as averages, minimums, maximums, sums, or counts. InfluxDB 3 can calculate those summaries at query time with SQL or persist them with a Python Processing Engine.


How do I downsample data with InfluxDB 3 SQL? Use` DATE_BIN` to place timestamps into fixed intervals, apply aggregate functions such as` AVG` or` MAX` , and group by the binned time plus any tags you want to preserve. SQL downsampling returns aggregate rows but does not automatically write them to another table.


When should I use the Python Processing Engine instead of SQL alone? Use the Processing Engine when you want to precompute and store a rollup on a schedule. It is a good fit for aggregates used repeatedly by dashboards or long-range reports. Use query-time SQL when you need flexible intervals, are still exploring the data, or do not want another stored representation.


How do I prevent partial downsampling windows? Set the downsampler plugin’s \`offset\` so it queries intervals that have finished receiving data. The offset should cover normal ingestion latency and expected late arrivals. Validate completeness by comparing the sum of \`record_count\` in the aggregate table with the raw row count over the same closed interval.


Does downsampling delete the original data? No. Both a SQL aggregate query and the downsampler plugin leave the source data intact. The plugin writes additional rows to a target table. Raw-data deletion or expiration is controlled separately through retention settings.


What does record_count mean in the downsampled table?. \`record_count\` is the number of source records represented by an aggregate row. It is useful for detecting incomplete bins and for comparing the target table with the source over the same period.


Can I calculate more than one aggregate per interval? Yes. A SQL query can return multiple functions such as AVG(speed), MIN(speed), and MAX(speed) in the same group. The official downsampler accepts calculation mappings for multiple fields. Choose only the summaries your queries need to avoid unnecessary write and storage overhead.


Does this approach work with InfluxDB 3 Cloud Serverless or Cloud Dedicated? The embedded Python Processing Engine workflow in this tutorial applies to InfluxDB 3 Core and Enterprise. For InfluxDB Cloud Serverless, follow the documented client-library downsampling pattern: query aggregates with SQL, write the results back, and schedule the client externally. For Cloud Dedicated, you can use the same general query-and-write pattern with an InfluxDB 3 client library and an external scheduler.


How much storage will downsampling save? There is no universal percentage. The tutorial reduced the number of rows returned for its fixed test window by 90%, but byte-level storage depends on the schema, data types, tag cardinality, compression, the aggregates stored, and how long you retain each resolution.
