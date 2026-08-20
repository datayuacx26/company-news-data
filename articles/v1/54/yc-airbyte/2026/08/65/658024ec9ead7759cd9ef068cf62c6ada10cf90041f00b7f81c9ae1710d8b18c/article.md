---
schema_version: "1.0.0"
document_id: "658024ec9ead7759cd9ef068cf62c6ada10cf90041f00b7f81c9ae1710d8b18c"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/databricks-direct-loading-speed-mode"
published_at: "2026-08-18T06:54:00+00:00"
first_seen_at: "2026-08-18T20:05:17.677933+00:00"
fetched_at: "2026-08-18T20:05:20.495654+00:00"
content_hash: "sha256:44e078b2549dacbcde4d05b177917bc84929c0a6f733f32888ea8e1bba7a6045"
---

# Destination Databricks 4.0: Up to 4.5x Faster with Direct Loading and Speed Mode

With Destination Databricks 4.0.0, we've reworked the connector's data path end to end — from the source container all the way into your Delta tables. Syncs now run up to 4.5x faster on larger datasets while leaning on less warehouse compute. Three changes drive it:[Speed Mode](https://airbyte.com/blog/speed-improvements) , a faster parallel transport between containers;[Direct Loading](https://airbyte.com/blog/introducing-direct-loading) , which writes typed data straight into final tables; and a migration onto Airbyte's new Bulk CDK framework.


This is the latest stop in a rollout that's already reached[Snowflake](https://airbyte.com/blog/snowflake-destination-enhancements) ,[BigQuery](https://airbyte.com/blog/introducing-direct-loading) , and[Redshift](https://airbyte.com/blog/redshift-4) . Databricks is one of the most heavily used destinations on the platform, so moving it onto the same modern pipeline was a priority. What follows walks through the architecture changes, the parts of your setup that don't move, and where the benchmarks landed.


## **What Changed in 4.0.0**


Five changes define this release:


1. **Speed Mode** — A socket-based transport replaces JSON-over-stdio with Protobuf-over-Unix-domain-sockets, opening parallel data channels between the source and destination containers.
2. **Direct Loading** — The connector writes typed data directly to final tables, eliminating the intermediate raw tables and the Typing and Deduping SQL overhead that came with them.
3. **Avro staging format** — Staging files move from CSV + gzip to Avro + Snappy, so data arrives already typed and the warehouse no longer infers schema or casts on load.
4. **Schema evolution** — Column adds, drops, and type changes are handled with granular ALTER TABLE and CTAS operations instead of v3's full-table rebuild.
5. **Bulk CDK migration** — The connector is rebuilt on Airbyte's new Bulk CDK framework with Micronaut dependency injection, standardizing the loading pipeline and improving parallelism.


Speed Mode, Direct Loading, and the Bulk CDK migration are covered in depth in separate posts; here we focus on how they come together for Databricks, along with the two destination-specific changes — Avro staging and schema evolution detailed below.


## **Speed Mode: What It Means for Databricks**


Speed Mode redesigned how data moves between containers in Airbyte's architecture. The details are covered in our[engineering deep-dive](https://airbyte.com/blog/speed-improvements) , but here's the short version for Databricks syncs.


**Before (v3.x):** Data flowed from source to orchestrator to destination through standard I/O pipes. Every record was serialized as JSON, parsed by the orchestrator, re-serialized, and parsed again by the destination. The orchestrator sat in the middle of every byte of data.


**After (v4.0.0):** Records flow directly from source to destination over multiple Unix domain sockets, serialized as Protocol Buffers. A lightweight Bookkeeper replaces the orchestrator, handling only control messages (logs, state, metadata). The data channel never touches the Bookkeeper.


For Databricks specifically, this matters because:


- **Parallel Volume uploads:** Multiple data channels feed Avro files into Unity Catalog Volumes simultaneously, keeping the staging pipeline saturated.
- **Optimized batch sizing:** Files are aggregated into ~250MB batches, tuned for Databricks COPY INTO performance.
- **Reduced serialization overhead:** Protobuf's binary format with column-value arrays (instead of repeated key-value JSON) cuts both CPU usage and bytes on the wire.


## **Direct Loading: No More Raw Tables**


In previous versions, Destination Databricks used a two-step process called Typing and Deduping: write untyped JSON to a raw table (` _airbyte_raw_{stream}` ), then run SQL —` get_json_object` to extract fields, cast types, and` MERGE` into the final table.


The raw tables never stopped growing. Each sync appended to previous data, and Databricks ran transformation SQL on every sync, so compute costs climbed over time.


With Direct Loading, the connector handles type-casting in-memory before data reaches Databricks. Typed records are written directly to final tables — no persistent raw tables, no transformation SQL in your warehouse. The loading path still stages through Unity Catalog Volumes and loads with` COPY INTO` , and deduplication is still handled by` MERGE INTO` on the final table. What changed is what gets loaded, which is the next section.


## **Avro Staging: Typed Files Instead of JSON-in-CSV**


The staging format moved from **CSV + gzip** to **Avro + Snappy** — and this is what makes raw tables unnecessary.


In v3, every record was dumped into a raw table as a single JSON blob in` _airbyte_data` , with all type information flattened to strings. Databricks then had to infer schema on load (` inferSchema=true` ), extract fields with` get_json_object()` , and cast them in SQL. That path was slow and bug-prone — inferSchema guessed INT32 for` _airbyte_generation_id` where the column was BIGINT, forcing an explicit cast workaround inside a COPY INTO subquery.


In v4, the connector does all type work in the JVM and writes typed Avro: each field becomes its own column with a native Avro type, and the schema travels inside the file. Databricks knows every type at load time — no inference, no extraction, no casting. The COPY INTO collapses from a wrapped subquery with three format options to a single line:


SQL


```text
-- v3: the CSV file format with inferSchema computation enabled
COPY    INTO   `catalog`.`raw_ns`.`_airbyte_raw_stream`
FROM   (
SELECT   _airbyte_generation_id ::  bigint  ,  *    except   (_airbyte_generation_id)
FROM    '/Volumes/.../file.csv.gz'
)
FILEFORMAT  =   CSV
FORMAT_OPTIONS ( 'header'  =  'true'  ,  'inferSchema'  =  'true'  ,  'escape'  =  '"'  );
```


SQL


```text
-- v4: the Avro schema handles everything
COPY    INTO   `catalog`.`namespace`.` table  `
FROM    '/Volumes/.../file.avro'
FILEFORMAT  =   AVRO;
```


On compression, Snappy replaces gzip: a slightly larger file in exchange for much cheaper compression and decompression. For a staging file that lives for seconds before COPY INTO, throughput beats compression ratio. Combined with the typed-on-write design, the format change pulls work out of both phases that dominate sync time — staging and loading — and eliminates a class of CSV escaping and type-inference bugs along the way.


## **Schema Evolution: v3 vs v4**


Schema changes are where v3 and v4 differ most. When a source adds a column, changes a type, or drops a field, the two versions take fundamentally different paths.


**v3 — full-table soft reset.** v3 has no incremental schema evolution; there are zero ALTER TABLE statements in the codebase. On a schema mismatch it rebuilds the whole table: create a temp table with the new schema, clear every load marker in the raw table (` _airbyte_loaded_at = NULL` ), re-run Typing and Deduping over *every historical record* , then atomically swap the temp table in. For a table with millions of rows, one new column means reprocessing everything — expensive in both time and Databricks compute.


**v4 — granular operations.** v4 implements the CDK's` TableSchemaEvolutionClient` with three targeted operations:


- **Add column** —` ALTER TABLE ... ADD COLUMN` (preceded by a` delta.feature.timestampNtz` property when adding a TIMESTAMP_NTZ column). No reprocessing.
- **Drop column** —` ALTER TABLE ... DROP COLUMN` , after enabling` delta.columnMapping.mode = 'name'` . Existing data is untouched.
- **Type change** — an atomic` CREATE OR REPLACE TABLE ... AS SELECT` that casts the changed column in place:


SWIFT


```text
CREATE    OR    REPLACE    TABLE   `catalog`.`schema`.`table`  AS    SELECT
`_airbyte_raw_id`, `_airbyte_extracted_at`, `_airbyte_meta`, `_airbyte_generation_id`,
`retained_col`,
CAST  (`changed_col`  AS    STRING  )  AS   `changed_col`,
CAST  ( NULL    AS    STRING  )  AS   `new_col`
FROM   `catalog`.`schema`.`table`;
```


For adds and drops — the common case — evolution is a metadata-only operation that completes near-instantly regardless of table size. Only true type changes re-scan the final table, and there's no raw table to reprocess from.


## **What Stayed the Same**


Upgrading from v3.x to v4.0.0 is designed to be straightforward. Several core behaviors are unchanged:


- **Same configuration:** No changes required. (The one removal is` raw_schema_override` , which no longer applies now that there are no raw tables.)
- **Unity Catalog Volumes staging with COPY INTO:** Data is still staged via Volumes and loaded with COPY INTO.
- **Authentication:** Both OAuth (M2M) and Personal Access Token are still supported.
- **All sync modes:** Full refresh (overwrite/append) and incremental (append/dedup) behave as before.


## **What Else Improved**


### **Configurable CDC Deletion: Soft or Hard Delete**


v4 adds a` cdc_deletion_mode` option. v3 only supported hard deletes — a deleted source row was removed from the destination with no trace. v4 lets you choose: **hard delete** (default, preserves existing behavior) or **soft delete** , which retains the row as a tombstone with` _ab_cdc_deleted_at` set. Soft delete fits when you need an auditable deletion history or want downstream logic to react to delete events. It's a per-connection setting.


### **Library Upgrades**


Library v3 v4


**com.databricks:databricks-jdbc** 2.6.40 3.3.3


**com.databricks:databricks-sdk-java** 0.31.0 0.110.0


The JDBC driver jump from 2.6.40 to 3.3.3 brings improved connection stability, better Unity Catalog support, and eliminates the` --add-opens` JVM workarounds v3 required. The SDK upgrade brings improved Volume operations and OAuth support.


### **Robust Connection Checker**


The pre-flight check is now an end-to-end mini-sync: it creates a table, writes Avro through the full pipeline, runs COPY INTO, verifies the row count, and cleans up. Misconfigurations surface before the sync starts, not midway through.


### **Value Coercion and Validation**


Integer overflow (INT64 range) and decimal precision (28 integer digits for DECIMAL(38,10)) are checked before load. Out-of-range values are nulled with metadata recorded in` _airbyte_meta.changes` — visibility instead of silent corruption or a failed sync.


### **Higher Parallelism**


A multi-threaded aggregate pipeline buffers 250MB per aggregate, 1GB total, up to 5 buffered aggregates, maximizing throughput across multiple streams.


### **Improved Test Suite**


New component tests cover schema evolution, data coercion, and table operations, with Protobuf/socket acceptance tests running alongside the existing JSONL/stdio paths.


### **Updated Spec and Docs**


The spec is now annotation-driven with inline doc links (replacing hand-written JSON), and fields are reorganized into clearer groups: connection, sync behavior, and advanced.


## **Benchmarks**


Scenario v3 (MB/s) v4 (MB/s) Improvement


**Default — 1 GB** 8.04 13.10 +62.9%


**Boosted — 1 GB** 16.08 25.97 +61.5%


**Default — 10 GB** 9.54 42.47 +345.2%


**Boosted — 10 GB** 15.74 66.45 +322.2%


We benchmarked v3 (latest) against v4.0.0 using 1GB and 10GB datasets on both default and boosted Databricks configurations.


The improvement scales with data volume. On smaller 1GB syncs, where fixed startup overhead dominates, v4 is roughly 1.6x faster. On 10GB syncs the architectural wins take over: throughput jumps from 9.54 to 42.47 MB/s on default configurations (4.5x) and reaches 66.45 MB/s on boosted ones (4.2x). The gains come from every layer working together — faster data transfer (Speed Mode), less work in the warehouse (Direct Loading), a lighter staging format (Avro + Snappy), and a more efficient loading pipeline (Bulk CDK).


## **Migration Guide**


### **Upgrading to 4.0.0**


This version upgrades Destination Databricks to the Direct Loading paradigm. If you have specific requirements around record visibility or schema evolution, review the[Direct Loading documentation](https://docs.airbyte.com/platform/next/using-airbyte/core-concepts/direct-load-tables) for how it differs from Typing and Deduping.


**If you do not interact with raw tables** , you can upgrade safely. There is no breakage for this use case.


**If you interact with raw tables** (downstream dbt models, SQL queries, or dashboards that reference` _airbyte_raw_*` tables), follow these steps:


1. Update any downstream dbt models or SQL queries to reference the final tables instead of raw tables.
2. Upgrade the destination to version 4.0.0.
3. Verify data in the final tables after the first successful sync.
4. *(Optional)* Drop old raw tables (` _airbyte_raw_*` ) after verifying the new tables contain the expected data.


The deadline to upgrade to v4 is **2026-10-01** .


## **What's Next**


If you're running Databricks with Airbyte, you'll get these improvements automatically when you upgrade to v4.0.0. If you're not yet on Airbyte,[try it free](https://cloud.airbyte.com/signup) and see the difference.
