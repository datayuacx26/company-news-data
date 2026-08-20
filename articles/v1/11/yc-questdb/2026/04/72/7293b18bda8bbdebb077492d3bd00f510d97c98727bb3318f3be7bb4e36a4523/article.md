---
schema_version: "1.0.0"
document_id: "7293b18bda8bbdebb077492d3bd00f510d97c98727bb3318f3be7bb4e36a4523"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/questdb-9-3-5-and-enterprise-3-2-5-release/"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:c034dc8f33ede0acceb91257a165c1bd34ed5f135ef0a75a6f6351ce8a214248"
---

# QuestDB 9.3.5: Lateral Joins, UNNEST & Window Stats

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


QuestDB 9.3.5 and QuestDB Enterprise 3.2.5 are out. Lateral joins land in QuestDB, UNNEST brings SQL-standard array and JSON expansion, and a new set of statistical window functions covers stddev, variance, covariance, and correlation. HORIZON JOIN now supports multiple right-hand-side tables in a single query, and SAMPLE BY gets corrected timezone handling during DST transitions (a breaking change worth reading about if you use timezones).


---


## Lateral joins


QuestDB now supports lateral joins. A subquery on the right side of a join can reference columns from the left side, so it is evaluated per row. This is the standard SQL pattern for "top-N per group", per-row aggregation, and dynamic filtering.


Here is a non-trivial example on the demo dataset. For each currency pair, we take the latest known row, then use a lateral subquery to find the top 3 trades by quantity in the last minute for that symbol - with an ASOF JOIN against` core_price` inside the subquery so each trade carries the prevailing bid/ask at the time of execution:


Top 3 trades per symbol with prevailing quotes


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20t.symbol%2C%0A%20%20%20%20sub.timestamp%2C%0A%20%20%20%20sub.side%2C%0A%20%20%20%20sub.price%2C%0A%20%20%20%20sub.quantity%2C%0A%20%20%20%20sub.ecn%2C%0A%20%20%20%20sub.bid_price%2C%0A%20%20%20%20sub.ask_price%0AFROM%20(%0A%20%20%20%20SELECT%20*%20FROM%20fx_trades%0A%20%20%20%20LATEST%20ON%20timestamp%20PARTITION%20BY%20symbol%0A)%20t%0AJOIN%20LATERAL%20(%0A%20%20%20%20SELECT%0A%20%20%20%20%20%20%20%20f.timestamp%2C%20f.side%2C%20f.price%2C%0A%20%20%20%20%20%20%20%20f.quantity%2C%20f.ecn%2C%0A%20%20%20%20%20%20%20%20c.bid_price%2C%20c.ask_price%0A%20%20%20%20FROM%20fx_trades%20f%0A%20%20%20%20ASOF%20JOIN%20core_price%20c%0A%20%20%20%20%20%20%20%20ON%20(f.symbol%20%3D%20c.symbol%20AND%20f.ecn%20%3D%20c.ecn)%0A%20%20%20%20WHERE%20f.symbol%20%3D%20t.symbol%0A%20%20%20%20%20%20%20%20AND%20f.timestamp%20IN%20%27%24now%20-%201m..%24now%27%0A%20%20%20%20ORDER%20BY%20f.quantity%20DESC%0A%20%20%20%20LIMIT%203%0A)%20sub%3B%0A&executeQuery=true)


```text
SELECT       t.symbol,       sub.timestamp,       sub.side,       sub.price,       sub.quantity,       sub.ecn,       sub.bid_price,       sub.ask_price   FROM (       SELECT * FROM fx_trades       LATEST ON timestamp PARTITION BY symbol   ) t   JOIN LATERAL (       SELECT           f.timestamp, f.side, f.price,           f.quantity, f.ecn,           c.bid_price, c.ask_price       FROM fx_trades f       ASOF JOIN core_price c           ON (f.symbol = c.symbol AND f.ecn = c.ecn)       WHERE f.symbol = t.symbol           AND f.timestamp IN '$now - 1m..$now'       ORDER BY f.quantity DESC       LIMIT 3   ) sub;
```


symbol timestamp side price quantity ecn bid_price ask_price


EURAUD 2026-04-13T20:26:27.824Z buy 1.6529 226426 Currenex 1.6517 1.6525


EURAUD 2026-04-13T20:26:21.075Z buy 1.6534 243280 Hotspot 1.6522 1.653


EURAUD 2026-04-13T20:26:21.075Z buy 1.6535 299227 Hotspot 1.6522 1.653


EURUSD 2026-04-13T20:26:37.360Z sell 1.1841 512870 Currenex 1.1848 1.1854


EURUSD 2026-04-13T20:26:36.937Z buy 1.1862 522897 Hotspot 1.1848 1.1854


EURUSD 2026-04-13T20:26:21.830Z buy 1.1859 657428 Hotspot 1.1845 1.1851


...


Each row is one of the top-3 trades by quantity for its symbol, enriched with the prevailing bid/ask from` core_price` at the time of execution via the ASOF JOIN inside the lateral subquery.


The outer query produces one row per symbol via` LATEST ON` . For each of those rows, the lateral subquery runs: it scans the last minute of` fx_trades` for that symbol, ASOF-joins against` core_price` to attach the prevailing bid/ask, sorts by quantity, and returns the top 3. The result is a compact table of the largest recent trades across all pairs, each enriched with the market context at execution time.


QuestDB's implementation uses a decorrelation optimizer that rewrites correlated subqueries into set-based execution when possible, so performance stays reasonable even on large tables.


` LEFT LATERAL` joins are also supported.


[Read the LATERAL JOIN documentation](https://questdb.com/docs/query/sql/lateral-join/)


---


## SQL-standard UNNEST


The new` UNNEST` operator expands arrays and JSON arrays into rows, turning nested data into something you can filter, join, and aggregate with normal SQL.


**Array expansion** works on QuestDB's native` DOUBLE\[\]` columns. The demo dataset's` market_data` table stores order book snapshots with` bids` and` asks` as` DOUBLE\[\]\[\]` (prices in the first sub-array, volumes in the second). Here we unnest both sides of the book simultaneously:


Expand bid and ask levels into rows


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20m.timestamp%2C%20m.symbol%2C%0A%20%20%20%20ub.bid_price%2C%20ub.bid_level%2C%0A%20%20%20%20ua.ask_price%2C%20ua.ask_level%0AFROM%20market_data%20m%2C%0A%20%20%20%20UNNEST(m.bids%5B1%5D)%20WITH%20ORDINALITY%20ub(bid_price%2C%20bid_level)%2C%0A%20%20%20%20UNNEST(m.asks%5B1%5D)%20WITH%20ORDINALITY%20ua(ask_price%2C%20ask_level)%0AWHERE%20m.symbol%20%3D%20%27EURUSD%27%0A%20%20%20%20AND%20m.timestamp%20IN%20%27%24now%20-%2010s..%24now%27%0A%20%20%20%20AND%20bid_price%20%3E%201.1844%0ALIMIT%2050%3B%0A&executeQuery=true)


```text
SELECT       m.timestamp, m.symbol,       ub.bid_price, ub.bid_level,       ua.ask_price, ua.ask_level   FROM market_data m,       UNNEST(m.bids[1]) WITH ORDINALITY ub(bid_price, bid_level),       UNNEST(m.asks[1]) WITH ORDINALITY ua(ask_price, ask_level)   WHERE m.symbol = 'EURUSD'       AND m.timestamp IN '$now - 10s..$now'       AND bid_price > 1.1844   LIMIT 50;
```


timestamp symbol bid_price bid_level ask_price ask_level


2026-04-13T20:27:58.299Z EURUSD 1.185 1 1.1852 1


2026-04-13T20:27:58.299Z EURUSD 1.185 1 1.1853 2


2026-04-13T20:27:58.299Z EURUSD 1.185 1 1.1854 3


2026-04-13T20:27:58.299Z EURUSD 1.1849 2 1.1852 1


2026-04-13T20:27:58.299Z EURUSD 1.1849 2 1.1853 2


2026-04-13T20:27:58.299Z EURUSD 1.1849 2 1.1854 3


...


Each row from the original` market_data` snapshot produces one row per combination of bid level and ask level.` WITH ORDINALITY` adds a 1-based position column per unnested array, so` bid_level` and` ask_level` tell you which depth of the book each row came from. Once unnested, the expanded columns are regular columns - you can filter on them, join on them, or aggregate them like anything else. The` bid_price > 1.1844` filter above narrows the result to only the book levels above that threshold.


**JSON expansion** works on VARCHAR columns containing JSON arrays. You declare the output columns and their types, and QuestDB extracts them. This example uses the response format from the[Coinbase trades API](https://api.exchange.coinbase.com/products/BTC-USD/trades?limit=3) :


Extract fields from a Coinbase trades response


```text
SELECT u.trade_id, u.price, u.size, u.side, u.time   FROM UNNEST(       '[{"trade_id":994619709,"side":"sell","size":"0.00000100","price":"69839.36000000","time":"2026-04-06T10:32:55.517183Z"},         {"trade_id":994619708,"side":"buy","size":"0.00000006","price":"69839.35000000","time":"2026-04-06T10:32:55.418434Z"},         {"trade_id":994619707,"side":"buy","size":"0.00000006","price":"69839.35000000","time":"2026-04-06T10:32:55.024765Z"}]'       ::VARCHAR       COLUMNS(           trade_id LONG,           price DOUBLE,           size DOUBLE,           side VARCHAR,           time TIMESTAMP       )   ) u;
```


trade_id price size side time


994619709 69839.36 0.000001 sell 2026-04-06T10:32:55.517183Z


994619708 69839.35 0.00000006 buy 2026-04-06T10:32:55.418434Z


994619707 69839.35 0.00000006 buy 2026-04-06T10:32:55.024765Z


Multiple arrays can be unnested simultaneously - shorter arrays are padded with NULLs to match the longest, following PostgreSQL semantics.


[Read the UNNEST documentation](https://questdb.com/docs/query/sql/unnest/)


---


## Statistical window functions


New window functions for rolling and cumulative statistics:


- ` stddev_pop()` ,` stddev_samp()` /` stddev()` - standard deviation
- ` var_pop()` ,` var_samp()` /` variance()` - variance
- ` covar_pop()` ,` covar_samp()` - covariance
- ` corr()` - Pearson correlation coefficient


All support` ROWS` ,` RANGE` , and` PARTITION BY` frame modes with bounded and unbounded frames. Non-removable frames use Welford's online algorithm for numerical stability.


Here is a rolling 60-trade price stddev on BTC-USDT:


Rolling stddev over a 60-row window


[Demo this query](https://demo.questdb.io/?query=SELECT%20timestamp%2C%20symbol%2C%20price%2C%0A%20%20%20%20stddev(price)%20OVER%20(%0A%20%20%20%20%20%20%20%20PARTITION%20BY%20symbol%0A%20%20%20%20%20%20%20%20ORDER%20BY%20timestamp%0A%20%20%20%20%20%20%20%20ROWS%20BETWEEN%2059%20PRECEDING%20AND%20CURRENT%20ROW%0A%20%20%20%20)%20AS%20rolling_stddev%0AFROM%20trades%0AWHERE%20symbol%20%3D%20%27BTC-USDT%27%0A%20%20%20%20AND%20timestamp%20IN%20%27%24now%20-%201h..%24now%27%3B%0A&executeQuery=true)


```text
SELECT timestamp, symbol, price,       stddev(price) OVER (           PARTITION BY symbol           ORDER BY timestamp           ROWS BETWEEN 59 PRECEDING AND CURRENT ROW       ) AS rolling_stddev   FROM trades   WHERE symbol = 'BTC-USDT'       AND timestamp IN '$now - 1h..$now';
```


timestamp symbol price rolling_stddev


2026-04-13T19:29:30.993Z BTC-USDT 72853.7 null


2026-04-13T19:29:32.379Z BTC-USDT 72853.7 0.0


2026-04-13T19:29:33.564Z BTC-USDT 72853.8 0.058


2026-04-13T19:29:37.473Z BTC-USDT 72857.5 1.668


2026-04-13T19:29:58.966Z BTC-USDT 72849.3 2.955


2026-04-13T19:29:58.966Z BTC-USDT 72844.9 4.177


2026-04-13T19:29:58.966Z BTC-USDT 72839.7 5.956


2026-04-13T19:30:01.473Z BTC-USDT 72841.9 5.792


...


The stddev grows as the 60-trade window accumulates a mix of prices from a brief sell-off (72857 down to 72839), then stabilizes as the window fills.


And a rolling correlation between two pairs:


Rolling correlation between BTC and ETH


[Demo this query](https://demo.questdb.io/?query=WITH%20paired%20AS%20(%0A%20%20%20%20SELECT%20b.timestamp%2C%0A%20%20%20%20%20%20%20%20b.price%20AS%20btc_price%2C%0A%20%20%20%20%20%20%20%20e.price%20AS%20eth_price%0A%20%20%20%20FROM%20trades%20b%0A%20%20%20%20ASOF%20JOIN%20(%0A%20%20%20%20%20%20%20%20SELECT%20timestamp%2C%20price%20FROM%20trades%0A%20%20%20%20%20%20%20%20WHERE%20symbol%20%3D%20%27ETH-USDT%27%0A%20%20%20%20)%20e%0A%20%20%20%20WHERE%20b.symbol%20%3D%20%27BTC-USDT%27%0A%20%20%20%20%20%20%20%20AND%20b.timestamp%20IN%20%27%24now%20-%202h..%24now%27%0A)%0ASELECT%20timestamp%2C%0A%20%20%20%20corr(btc_price%2C%20eth_price)%20OVER%20(%0A%20%20%20%20%20%20%20%20ORDER%20BY%20timestamp%0A%20%20%20%20%20%20%20%20ROWS%20BETWEEN%20119%20PRECEDING%20AND%20CURRENT%20ROW%0A%20%20%20%20)%20AS%20rolling_corr%0AFROM%20paired%3B%0A&executeQuery=true)


```text
WITH paired AS (       SELECT b.timestamp,           b.price AS btc_price,           e.price AS eth_price       FROM trades b       ASOF JOIN (           SELECT timestamp, price FROM trades           WHERE symbol = 'ETH-USDT'       ) e       WHERE b.symbol = 'BTC-USDT'           AND b.timestamp IN '$now - 2h..$now'   )   SELECT timestamp,       corr(btc_price, eth_price) OVER (           ORDER BY timestamp           ROWS BETWEEN 119 PRECEDING AND CURRENT ROW       ) AS rolling_corr   FROM paired;
```


timestamp rolling_corr


2026-04-13T18:28:34.943Z null


2026-04-13T18:28:38.269Z 0.9944


2026-04-13T18:28:38.528Z 0.9966


2026-04-13T18:28:38.528Z 0.9951


2026-04-13T18:28:38.528Z 0.8398


2026-04-13T18:28:38.528Z 0.7907


2026-04-13T18:28:40.586Z 0.6235


2026-04-13T18:28:44.216Z 0.5707


2026-04-13T18:28:50.175Z 0.6656


...


The correlation starts near 1.0 (BTC and ETH moving in lockstep), then drops toward 0.6 as the two series diverge over the window.


[Read the window functions documentation](https://questdb.com/docs/query/functions/window-functions/overview/)


---


## Multi-table HORIZON JOIN


` HORIZON JOIN` now accepts multiple right-hand-side tables in a single query. Previously you needed self-joins or CTEs to correlate across multiple sources. Now you write it directly. Here we compare consolidated quotes from` market_data` against ECN-level prices from` core_price` at each horizon offset:


Consolidated vs ECN-level quotes at each horizon


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20h.offset%20%2F%201000000%20AS%20horizon_ms%2C%0A%20%20%20%20t.symbol%2C%0A%20%20%20%20avg(m.best_bid)%20AS%20consolidated_bid%2C%0A%20%20%20%20avg(c.bid_price)%20AS%20ecn_bid%0AFROM%20fx_trades%20AS%20t%0AHORIZON%20JOIN%20market_data%20AS%20m%0A%20%20%20%20ON%20(t.symbol%20%3D%20m.symbol)%0AHORIZON%20JOIN%20core_price%20AS%20c%0A%20%20%20%20ON%20(t.symbol%20%3D%20c.symbol%20AND%20t.ecn%20%3D%20c.ecn)%0A%20%20%20%20LIST%20(-1s%2C%200%2C%201s%2C%205s)%20AS%20h%0AWHERE%20t.symbol%20%3D%20%27EURUSD%27%0A%20%20%20%20AND%20t.timestamp%20IN%20%27%24now%20-%201h..%24now%27%0AGROUP%20BY%20horizon_ms%2C%20t.symbol%0AORDER%20BY%20t.symbol%2C%20horizon_ms%3B%0A&executeQuery=true)


```text
SELECT       h.offset / 1000000 AS horizon_ms,       t.symbol,       avg(m.best_bid) AS consolidated_bid,       avg(c.bid_price) AS ecn_bid   FROM fx_trades AS t   HORIZON JOIN market_data AS m       ON (t.symbol = m.symbol)   HORIZON JOIN core_price AS c       ON (t.symbol = c.symbol AND t.ecn = c.ecn)       LIST (-1s, 0, 1s, 5s) AS h   WHERE t.symbol = 'EURUSD'       AND t.timestamp IN '$now - 1h..$now'   GROUP BY horizon_ms, t.symbol   ORDER BY t.symbol, horizon_ms;
```


horizon_ms symbol consolidated_bid ecn_bid


-1000 EURUSD 1.17845 1.17845


0 EURUSD 1.17844 1.17844


1000 EURUSD 1.17844 1.17844


5000 EURUSD 1.17845 1.17845


Four horizon offsets, two price sources. The consolidated and ECN-level bids are nearly identical here (EURUSD is deep and well-arbitraged), but on less liquid pairs or during volatile periods the gap would be wider.


[Read the HORIZON JOIN documentation](https://questdb.com/docs/query/sql/horizon-join/)


---


## Per-column bloom filter configuration


You can now tag columns for bloom filter generation at table creation time. When partitions are converted to Parquet, those columns get bloom filters embedded in the file metadata, enabling row group pruning on equality and` IN` queries:


Create table with bloom filter on symbol


```text
CREATE TABLE market_ticks (       ts TIMESTAMP,       symbol VARCHAR PARQUET(BLOOM_FILTER),       price DOUBLE,       qty DOUBLE   ) TIMESTAMP(ts) PARTITION BY DAY WAL;
```


You can also specify bloom filters explicitly during partition conversion:


Convert with explicit bloom filter columns


```text
ALTER TABLE market_ticks       CONVERT PARTITION TO PARQUET       WHERE ts < '2026-04-01'       WITH (           bloom_filter_columns = 'symbol',           bloom_filter_fpp = 0.01       );
```


The` bloom_filter_fpp` parameter controls the false positive probability (default 0.01). Lower values produce larger filters with fewer false positives.


---


## Performance improvements


- **Hash joins** - SYMBOL key columns now compare as integers instead of strings, extending the optimization already used by ASOF/LT joins
- **ASOF, HORIZON, and WINDOW JOIN** - faster execution on large partitioned tables
- **ASOF and LT JOIN** - faster execution on symbol columns specifically
- **Wide tables** - reduced partition open overhead for queries touching many columns
- **WAL commit batching** - optimized batching for trailing INSERT transactions
- **Array access** - faster element access for 1D and 2D arrays


---


## Breaking change: SAMPLE BY timezone alignment


Sub-day` SAMPLE BY` queries with` ALIGN TO CALENDAR TIME ZONE` now produce correct bucket alignment during DST transitions. This is a correctness fix, but it changes output for queries that run across DST boundaries:


- Sub-day buckets are now uniformly spaced in UTC using the timezone's standard offset. During DST fall-back, repeated local hours produce separate rows instead of being merged.
- ` FROM` /` TO` boundaries are now interpreted as local time in the specified timezone (previously UTC).
- Queries without` TIME ZONE` , with fixed-offset timezones, or with super-day strides (day+) are unaffected.


If you have` SAMPLE BY` queries with explicit timezone handling, review the updated documentation to confirm your queries produce the expected buckets.


[Read the SAMPLE BY timezone documentation](https://questdb.com/docs/query/sql/sample-by/#time-zone-transitions)


---


## Also in this release


- ` ALTER TABLE ADD/DROP COLUMN` now works on tables with Parquet-formatted partitions
- ` arg_max(varchar, ...)` group-by functions for timestamp, double, long, and int key types
- RLE dictionary encoding support for STRING columns in Parquet
- Imprecise dates in tick expressions (e.g.` '2026-04'` matches the whole month)
- ` SHOW TABLES` results sorted alphabetically (behind` cairo.metadata.cache.snapshot.ordered` config)


---


## Bug fixes


This release includes fixes for Parquet export (inlined VARCHAR writing NULLs, multi-dictionary page corruption, row group size limits after O3 merge), ASOF join result correctness with overlapping symbol indices, SIGSEGV under memory pressure in group-by queries, and several join and PGWire edge cases. See the full details on the[release notes page](https://questdb.com/release-notes/) .


---


## QuestDB Enterprise 3.2.5


QuestDB Enterprise 3.2.5 ships with all changes from QuestDB 9.3.5 above, plus enterprise-specific fixes:


- Fixed a rare backup command failure caused by empty partitions
- Fixed a rare replica crash during shutdown
- Fixed column resolution for join columns wrapped in functions


---


That is QuestDB 9.3.5 and QuestDB Enterprise 3.2.5. Lateral joins and UNNEST open up query patterns that previously required application-side logic. Statistical window functions bring risk analytics into SQL. The engine keeps getting faster underneath.


```text
docker pull questdb/questdb   docker run -p 9000:9000 -p 8812:8812 questdb/questdb
```


Try a lateral join first - top-N per group in a single query is hard to go back from.


Join our[Slack](https://slack.questdb.com/) or[Discourse](https://community.questdb.com/) communities to share feedback and results.


---


Self-managed enterprise customers will find the binaries at the usual download location. BYOC enterprise customers will be contacted for upgrading.


Not on QuestDB Enterprise yet? Learn more about[QuestDB Enterprise](https://questdb.com/enterprise/) and[BYOC](https://questdb.com/byoc/) , or[contact the QuestDB team](https://questdb.com/enterprise/contact/) for a conversation or a demo.
