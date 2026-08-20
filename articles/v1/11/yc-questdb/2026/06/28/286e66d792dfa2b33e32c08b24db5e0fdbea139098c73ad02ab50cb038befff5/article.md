---
schema_version: "1.0.0"
document_id: "286e66d792dfa2b33e32c08b24db5e0fdbea139098c73ad02ab50cb038befff5"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/questdb-9-4-2-release/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-23T21:54:08.144992+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:50fab21c9a327a1ba61888b562cd819bb03708783a2bb19a90eb4df65fc224c2"
---

# QuestDB 9.4.2: shareable queries, new aggregates, and a hardening pass

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


QuestDB 9.4.2 is a hardening release driven by continued fuzz testing and stricter query-result assertions. Under heavy ingestion, a few edge cases around Parquet tables and the new posting index could surface, sometimes only when combined with other commands. Those paths are now fixed.


We did not publish a separate post for 9.4.1, so this release also folds in the nicer additions that shipped there: two new aggregates, window functions for` DECIMAL` columns, and much faster materialized view refresh during out-of-order backfills.


---


## Share a query from the Web Console


The Web Console now lets you share a runnable link to any query. Open the drop-down next to the green run button, copy the quick-link, and send it to a colleague. The URL opens the console and runs the query for them, with no copy-paste of SQL into a chat window.


Copy a runnable quick-link straight from the editor's run button


To see it in action, here is one we shared earlier: a[rolling realized-volatility query on EURUSD](https://demo.questdb.io/?query=DECLARE%0A++%40symbol+%3A%3D+%27EURUSD%27%2C%0A++%40lookback+%3A%3D+%27%24now+-+2d..%24now%27%0A%0AWITH+returns+AS+%28%0A++SELECT%0A++++timestamp%2C%0A++++symbol%2C%0A++++close%2C%0A++++ln%28close+%2F+lag%28close%29%0A++++++++OVER+%28PARTITION+BY+symbol+ORDER+BY+timestamp%29%29%0A++++++++AS+log_return%0A++FROM+market_data_ohlc_15m%0A++WHERE+symbol+%3D+%40symbol%0A++++AND+timestamp+IN+%40lookback%0A%29%2C%0Awith_stats+AS+%28%0A++SELECT%0A++++timestamp%2C%0A++++symbol%2C%0A++++close%2C%0A++++log_return%2C%0A++++avg%28log_return%29+OVER+w+AS+mean_return%2C%0A++++avg%28log_return+*+log_return%29+OVER+w+AS+mean_sq_return%0A++FROM+returns%0A++WHERE+log_return+IS+NOT+NULL%0A++WINDOW+w+AS+%28%0A++++PARTITION+BY+symbol+ORDER+BY+timestamp%0A++++ROWS+BETWEEN+19+PRECEDING+AND+CURRENT+ROW%0A++%29%0A%29%0ASELECT%0A++timestamp%2C%0A++symbol%2C%0A++round%28close%2C+5%29+AS+close%2C%0A++round%28log_return+*+100%2C+4%29+AS+return_pct%2C%0A++round%28%0A++++sqrt%28mean_sq_return+-+mean_return+*+mean_return%29%0A++++++++*+sqrt%28365+*+96%29+*+100%2C%0A++++2%29+AS+realized_vol_annualized%0AFROM+with_stats%0AORDER+BY+timestamp%3B&executeQuery=true) . Open it and the console runs the full query for you,` DECLARE` variables, CTEs, window functions and all, with no setup on your side.


For longer-lived collaboration you still have Tab Exporting to share and version-control entire editor tabs, or` CREATE VIEW` to publish production-ready queries. This release also improves the visibility of Storage Policy settings and makes query validation less eager, which smooths out editing.


---


## New aggregates: array_agg() and regr_r2()


[array_agg()](https://questdb.com/docs/query/functions/aggregation/#array_agg) rolls up the per-group values into a single` DOUBLE\[\]` , which is handy for feeding array functions or pulling a whole window of prices into one row:


Collect recent prices into an array per symbol


[Demo this query](https://demo.questdb.io/?query=SELECT%20symbol%2C%20array_agg(price)%20AS%20prices%0AFROM%20trades%0AWHERE%20symbol%20IN%20(%27BTC-USDT%27%2C%20%27ETH-USDT%27)%0A%20%20%20%20AND%20timestamp%20IN%20%27%24now%20-%202s..%24now%27%0AGROUP%20BY%20symbol%3B%0A&executeQuery=true)


```text
SELECT symbol, array_agg(price) AS prices   FROM trades   WHERE symbol IN ('BTC-USDT', 'ETH-USDT')       AND timestamp IN '$now - 2s..$now'   GROUP BY symbol;
```


symbol prices


BTC-USDT \[60881.7, 60883.9, 60883.9, 60883.8, 60884.9, ...\]


ETH-USDT \[1618.28, 1618.28, 1618.35, 1618.35, 1618.4, ...\]


[regr_r2(y, x)](https://questdb.com/docs/query/functions/finance/#regr_r2) is the standard SQL coefficient of determination: how well a linear regression of` y` on` x` fits, on a 0 to 1 scale. It is a quick way to tell a real trend from noise. Here it acts as a trending-versus-ranging gauge across FX pairs, where the freely floating majors track their open closely while the tightly managed USDHKD peg barely moves:


R-squared of close against open per pair


[Demo this query](https://demo.questdb.io/?query=SELECT%20symbol%2C%20regr_r2(close%2C%20open)%20AS%20r2%0AFROM%20market_data_ohlc_1d%0AWHERE%20symbol%20IN%20(%27USDCHF%27%2C%20%27EURUSD%27%2C%20%27GBPUSD%27%2C%20%27EURGBP%27%2C%20%27USDHKD%27)%0AORDER%20BY%20r2%20DESC%3B%0A&executeQuery=true)


```text
SELECT symbol, regr_r2(close, open) AS r2   FROM market_data_ohlc_1d   WHERE symbol IN ('USDCHF', 'EURUSD', 'GBPUSD', 'EURGBP', 'USDHKD')   ORDER BY r2 DESC;
```


symbol r2


USDCHF 0.8894


EURUSD 0.7952


GBPUSD 0.6934


EURGBP 0.1854


USDHKD 0.0521


---


## Window functions for DECIMAL columns


All six` DECIMAL` sub-types (D8 through D256) now work with the window functions` first_value` ,` last_value` ,` nth_value` ,` lag` ,` lead` ,` min` ,` max` ,` count` ,` sum` , and` avg` , across the same frame shapes already supported for the primitive numeric types. See the[window functions reference](https://questdb.com/docs/query/functions/window-functions/reference/) and the[DECIMAL type](https://questdb.com/docs/query/datatypes/decimal/) .


---


## Faster materialized view refresh


Refreshing a materialized view during an out-of-order backfill is now much faster. A new adaptive algorithm balances the number of rows refreshed against the number of commits, so performance holds up across varied workloads. Refreshing a 512-symbol view with a constant 12-hour backfill plus live ingestion dropped from around 160ms to roughly 2ms. Materialized views also discard no-op transactions earlier, avoiding wasted work.


---


## Hardening and bug fixes


Most of 9.4.2 is correctness work. Beyond the Parquet and posting-index hardening above, two query fixes worth calling out:


- ` count()` over a` GROUP BY` subquery no longer reports phantom duplicate rows.
- ` UNION ALL` no longer fails with a column mismatch when its branches sit under aggregates.


The full set of fixes spans the posting index, Parquet reads, temporal joins, and a range of other SQL queries. See the complete list on the[release notes page](https://questdb.com/release-notes/) .


---


## Breaking changes


- **A plain` INDEX TYPE POSTING` (no` INCLUDE` ) is now non-covering.** The` cairo.posting.index.auto.include.timestamp=true` flag adds the designated timestamp to your` INCLUDE` list automatically when you do not, so queries that return the timestamp still run through the covering index instead of falling back to a table scan.
- **` SHOW CREATE TABLE` now rejects views and materialized views.** It previously rendered a misleading` CREATE TABLE` statement for any entity. Use` SHOW CREATE VIEW` and` SHOW CREATE MATERIALIZED VIEW` instead.


---


That is QuestDB 9.4.2: shareable queries in the console, two new aggregates and` DECIMAL` window functions carried over from 9.4.1, and a solid pass of hardening across Parquet and posting indexes.


[Full changelog on GitHub](https://github.com/questdb/questdb/releases/tag/9.4.2)


Ready to try it?


```text
docker pull questdb/questdb   docker run -p 9000:9000 -p 8812:8812 questdb/questdb
```


Join our[Slack](https://slack.questdb.com/) or[Discourse](https://community.questdb.com/) communities to share feedback and results.
