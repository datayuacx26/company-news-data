---
schema_version: "1.0.0"
document_id: "61c0e7e9af37c82312223596d4b2ec8a07a1413065066b32d831990ea84f25f4"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/questdb-9-3-4-and-enterprise-3-2-4-release/"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:b2bd972a721fdb5d4f0172e253797b165d5f03a0059d45864dc666c08e2fad68"
---

# QuestDB 9.3.4: Dynamic WINDOW JOIN & Parquet Bloom Filters

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


QuestDB 9.3.4 and QuestDB Enterprise 3.2.4 are out. Parquet gets bloom filter pruning and per-column encoding controls,` WINDOW JOIN` supports dynamic ranges computed from column values, and new element-wise array functions bring order book analytics into SQL. On the enterprise side,` COPY PERMISSIONS` and automatic permission cleanup simplify access management.


---


## Dynamic windows in WINDOW JOIN


Until now,` WINDOW JOIN` required constant values for the` RANGE` clause. If different rows needed different lookback windows, you had to work around it with self-joins or application-side logic.


Now the` RANGE` bounds can reference columns or expressions from the driving table. Each row gets its own window, computed at query time.


Say you have a trades table where each row carries its own lookback interval - maybe it varies by venue or asset class. You can now write:


Per-row lookback from a column value


```text
SELECT       t.timestamp,       t.symbol,       t.price,       avg(m.best_bid) AS avg_bid,       count(*) AS quote_count   FROM fx_trades t   WINDOW JOIN market_data m       ON (t.symbol = m.symbol)       RANGE BETWEEN t.lookback MICROSECONDS PRECEDING AND CURRENT ROW       INCLUDE PREVAILING;
```


The time unit is optional. When present, the value is scaled to the left table's timestamp resolution. When omitted, the raw integer is interpreted in the left table's native resolution. Either or both bounds can be dynamic - you can mix a column reference on one side with a constant on the other.


Expressions work too. If you want to double the lookback:


Expression-based dynamic bound


```text
SELECT       t.timestamp,       t.symbol,       avg(m.best_bid) AS avg_bid   FROM fx_trades t   WINDOW JOIN market_data m       ON (t.symbol = m.symbol)       RANGE BETWEEN 2 * t.lookback MICROSECONDS PRECEDING AND 5 SECONDS FOLLOWING;
```


One thing to note: dynamic bounds disable the Fast Join (symbol-keyed) and vectorized (SIMD) execution paths. If a fixed window works for your use case, prefer static bounds for maximum performance.


Here is a working example you can try on the demo instance with a static range:


WINDOW JOIN on FX trades and quotes


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20t.timestamp%2C%0A%20%20%20%20t.symbol%2C%0A%20%20%20%20t.price%2C%0A%20%20%20%20avg(m.best_bid)%20AS%20avg_bid%2C%0A%20%20%20%20avg(m.best_ask)%20AS%20avg_ask%2C%0A%20%20%20%20count(*)%20AS%20quote_count%0AFROM%20fx_trades%20t%0AWINDOW%20JOIN%20market_data%20m%0A%20%20%20%20ON%20(t.symbol%20%3D%20m.symbol)%0A%20%20%20%20RANGE%20BETWEEN%205%20SECONDS%20PRECEDING%20AND%20CURRENT%20ROW%0A%20%20%20%20INCLUDE%20PREVAILING%0AWHERE%20t.symbol%20%3D%20%27EURUSD%27%0A%20%20%20%20AND%20t.timestamp%20%3E%20dateadd(%27m%27%2C%20-5%2C%20now())%0ALIMIT%2020%3B%0A&executeQuery=true)


```text
SELECT       t.timestamp,       t.symbol,       t.price,       avg(m.best_bid) AS avg_bid,       avg(m.best_ask) AS avg_ask,       count(*) AS quote_count   FROM fx_trades t   WINDOW JOIN market_data m       ON (t.symbol = m.symbol)       RANGE BETWEEN 5 SECONDS PRECEDING AND CURRENT ROW       INCLUDE PREVAILING   WHERE t.symbol = 'EURUSD'       AND t.timestamp > dateadd('m', -5, now())   LIMIT 20;
```


[Read the WINDOW JOIN documentation](https://questdb.com/docs/query/sql/window-join/)


---


## Parquet row group pruning with bloom filters


If you query Parquet files with selective filters, QuestDB now reads bloom filters and min/max statistics embedded in each row group and skips the ones that cannot match your predicate. For highly selective queries over wide Parquet files, this dramatically reduces I/O.


Selective query benefits from row group pruning


[Demo this query](https://demo.questdb.io/?query=SELECT%20symbol%2C%20count(*)%20AS%20trades%2C%20avg(price)%20AS%20avg_price%0AFROM%20read_parquet(%27trades.parquet%27)%0AWHERE%20symbol%20%3D%20%27BTC-USDT%27%0AGROUP%20BY%20symbol%3B%0A&executeQuery=true)


```text
SELECT symbol, count(*) AS trades, avg(price) AS avg_price   FROM read_parquet('trades.parquet')   WHERE symbol = 'BTC-USDT'   GROUP BY symbol;
```


The engine skips row groups where the bloom filter proves` 'BTC-USDT'` is not present, and where min/max statistics show the timestamp range does not overlap. No syntax changes needed - the optimization is automatic.


---


## Array analytics functions


New element-wise functions for` DOUBLE\[\]` columns:[array_elem_min()](https://questdb.com/docs/query/functions/array/#array_elem_min) ,[array_elem_max()](https://questdb.com/docs/query/functions/array/#array_elem_max) ,[array_elem_avg()](https://questdb.com/docs/query/functions/array/#array_elem_avg) , and[array_elem_sum()](https://questdb.com/docs/query/functions/array/#array_elem_sum) .


Unlike the existing` array_min` /` array_max` (which return a single scalar from the whole array), the` array_elem_*` variants operate position-by-position and return arrays. They work in two modes:


- **Multi-argument** :` array_elem_min(arr1, arr2)` returns the element-wise minimum across arrays in the same row
- **Aggregate** :` array_elem_min(arr_col)` in` GROUP BY` /` SAMPLE BY` computes the element-wise minimum across rows


Arrays of different lengths are handled naturally - positions beyond the end of a shorter array receive no contribution from that array. NULL elements are skipped at each position.


The demo dataset has` market_data.bids` and` market_data.asks` as` DOUBLE\[\]\[\]` order book arrays, which makes a good test case. Here is an example computing the tightest and widest top-of-book per symbol over 10-minute windows:


Element-wise min/max on order book snapshots


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20timestamp%2C%0A%20%20%20%20symbol%2C%0A%20%20%20%20array_elem_min(bids)%20AS%20tightest_bids%2C%0A%20%20%20%20array_elem_max(asks)%20AS%20widest_asks%0AFROM%20market_data%0AWHERE%20symbol%20%3D%20%27EURUSD%27%0A%20%20%20%20AND%20timestamp%20%3E%20dateadd(%27m%27%2C%20-10%2C%20now())%0ASAMPLE%20BY%201m%3B%0A&executeQuery=true)


```text
SELECT       timestamp,       symbol,       array_elem_min(bids) AS tightest_bids,       array_elem_max(asks) AS widest_asks   FROM market_data   WHERE symbol = 'EURUSD'       AND timestamp > dateadd('m', -10, now())   SAMPLE BY 1m;
```


[Read the array functions documentation](https://questdb.com/docs/query/functions/array/)


---


## Per-column Parquet encoding and compression


You can now configure encoding and compression on a per-column basis when creating tables that use Parquet partitions. This gives you full control to balance storage size versus query speed for each column individually.


CREATE TABLE with per-column Parquet config


```text
CREATE TABLE sensors (       ts TIMESTAMP,       temperature DOUBLE PARQUET(rle_dictionary, zstd(3)),       humidity FLOAT PARQUET(rle_dictionary),       device_id VARCHAR PARQUET(default, lz4_raw),       status INT   ) TIMESTAMP(ts) PARTITION BY DAY;
```


You can also modify existing tables:


ALTER TABLE to set column encoding


```text
ALTER TABLE sensors       ALTER COLUMN temperature       SET PARQUET(rle_dictionary, zstd(3));
```


The default VARCHAR encoding has also changed to` RLE Dictionary` in this release (see breaking changes below). If downstream tools depend on the previous` Delta Length Byte Array` encoding, override it explicitly.


[Read the Parquet export documentation](https://questdb.com/docs/query/export-parquet/) |[CREATE TABLE per-column config](https://questdb.com/docs/query/sql/create-table/#per-column-parquet-encoding-and-compression) |[ALTER TABLE PARQUET encoding](https://questdb.com/docs/query/sql/alter-table-alter-column-set-parquet/)


---


## Performance improvements


- **Faster ASOF and WINDOW JOIN** - initial frame positioning now uses binary search instead of linear scanning, reducing first-lookup cost for large right-hand-side tables
- **ORDER BY** - pre-computed sort keys for fixed-width types reduce function evaluations from O(N log N) to O(N); a dedicated fast path for` SYMBOL` columns avoids string comparisons entirely
- **HORIZON JOIN parallelization** - improved parallel execution across various data distributions on multi-core systems
- **Vectorized GROUP BY** - more non-keyed queries now use SIMD computation
- **Parquet I/O** - faster row group decompression and column materialization on reads; reduced write amplification and improved throughput on writes
- **WAL writer** - new` cairo.wal.writer.madvise.mode` config option for tuning OS page cache hints under memory pressure (` none` ,` sequential` , or` random` )


---


## QuestDB Enterprise: COPY PERMISSIONS


Onboarding a new team member or spinning up a service account no longer requires manually re-granting dozens of permissions.` COPY PERMISSIONS` clones the full permission set from one principal to another in a single statement:


Clone permissions between principals


```text
COPY PERMISSIONS FROM analyst_jane TO analyst_new_hire;
```


This includes table-level and endpoint grants. It works across principal types too - copy from a user to a service account, or from a group to a user.


---


## QuestDB Enterprise: Automatic permission cleanup


Permissions are now automatically revoked when dropping users, groups, service accounts, or database objects. No more stale permission entries accumulating over time as team members rotate or tables get dropped.


---


## Breaking changes


**Parquet VARCHAR encoding default changed.** Parquet export now uses` RLE Dictionary` encoding for VARCHAR columns instead of` Delta Length Byte Array` . Existing Parquet files are unaffected. If downstream consumers require the previous encoding, set it explicitly with the new per-column encoding configuration.


**Constant expression folding.** The SQL engine now folds constant expressions at compile time. As a side effect,` Infinity` and` -Infinity` in constant float/double expressions are collapsed to` NULL` , consistent with QuestDB's existing runtime behavior. If you have` CASE` /` SWITCH` expressions branching on` Infinity` or` -Infinity` , those branches will no longer match.


---


## Bug fixes


Both releases include numerous bug fixes across Parquet I/O,` WINDOW JOIN` , decimal columns, and enterprise replication. See the full details on the[release notes page](https://questdb.com/release-notes/) .


---


That is QuestDB 9.3.4 and QuestDB Enterprise 3.2.4. Dynamic` WINDOW JOIN` ranges let every row define its own time window. Parquet queries skip data they do not need. Element-wise array functions bring order book analytics into SQL. The engine keeps getting faster underneath.


```text
docker pull questdb/questdb   docker run -p 9000:9000 -p 8812:8812 questdb/questdb
```


Try a dynamic` WINDOW JOIN` first. We think you will see the appeal.


Join our[Slack](https://slack.questdb.com/) or[Discourse](https://community.questdb.com/) communities to share feedback and results.


---


Self-managed enterprise customers will find the binaries at the usual download location. BYOC enterprise customers will be contacted for upgrading.


Not on QuestDB Enterprise yet? Learn more about[QuestDB Enterprise](https://questdb.com/enterprise/) and[BYOC](https://questdb.com/byoc/) , or[contact the QuestDB team](https://questdb.com/enterprise/contact/) for a conversation or a demo.
