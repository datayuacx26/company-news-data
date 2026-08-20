---
schema_version: "1.0.0"
document_id: "64e03b6a253515420227792b2b79dd547540f51b19d8b3db632dca9311649a0d"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/questdb-qwp-binary-wire-protocol/"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T05:08:18.800684+00:00"
fetched_at: "2026-08-18T05:08:20.452532+00:00"
content_hash: "sha256:51c1e5c613b9063e348701216005479a4630d48734c97bd511a2c52505dfa9e2"
---

# QWP: QuestDB's own binary wire protocol for ingestion and queries

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


QuestDB 10.0 ships with QWP, the QuestDB Wire Protocol: binary, columnar, and the first protocol custom designed for QuestDB. For years, writing meant[ILP](https://questdb.com/docs/connect/compatibility/ilp/overview/) , a text format built for InfluxDB, and reading back meant[PGWire](https://questdb.com/docs/connect/compatibility/pgwire/overview/) , a row protocol built for Postgres. Performance is what drove the replacement, in both directions: QWP can ingest 19M rows a second against ILP's 5.3M, and stream results back at 220 million. There's more to QWP than speed: a single client now streams reads and writes, dataframes and Arrow travel in both directions, and high availability works for both, with the client following the primary around a cluster and buffering rows while it moves.


ILP, PGWire and the[REST API](https://questdb.com/docs/connect/compatibility/rest-api/) all still work, they're all still supported, and none of them are going anywhere. ILP and PGWire are how QuestDB fits into the tooling built for InfluxDB and Postgres, and they're very good at it.


The REST API is our own, designed around how QuestDB ingests CSV and runs SQL, and nothing beats a` curl` when you want a table loaded or a query answered from a shell script. What it doesn't give you is a session: one request, one response, and you're done.


QWP is the one we'd start a new project on.


Tip


If you're connecting through a third-party tool, Telegraf, Grafana, a BI tool, or any Postgres driver, keep using ILP and PGWire. QWP is for the code you write yourself.


---


## QWP performance: 19M rows/s ingest, 220M rows/s egress


We already wrote two posts on QWP performance, so we won't repeat them here.


For ingestion, read[QWP vs ILP ingestion](https://questdb.com/blog/qwp-vs-ilp-ingestion-benchmark/) . For query results, read[Streaming 500 million rows into Apache Arrow](https://questdb.com/blog/streaming-500-million-rows-into-apache-arrow/) . Both come down to the same thing: nothing on the wire is text, and nothing on either side is a row.


The rest of this post is about everything else QWP brings.


---


## One client for ingestion and SQL queries


Until QWP, talking to QuestDB from your own code meant two dependencies: the QuestDB client for writes and a Postgres driver for reads. Two libraries, two configurations, and two things to get right in your Kubernetes secrets.


QWP is bidirectional. Ingestion, queries and DDL all speak the same protocol, from the same client, configured by one[connect string](https://questdb.com/docs/connect/clients/connect-string/) . Behind the scenes, the client opens separate connections for reading and writing, and pools both.


In Java, the entry point is a single` QuestDB` handle that owns pools for both directions:


```text
try (QuestDB db = QuestDB.connect("ws::addr=localhost:9000;")) {
// Write       try (Sender sender = db.borrowSender()) {           sender.table("trades")                 .symbol("symbol", "ETH-USDT")                 .symbol("side", "sell")                 .doubleColumn("price", 2615.54)                 .doubleColumn("amount", 0.00044)                 .atNow();       }
// Read, same handle, same connect string       try (Query q = db.borrowQuery()) {           q.sql("SELECT timestamp, symbol, price FROM trades "                 + "WHERE symbol = 'ETH-USDT' LIMIT 10")            .handler(handler)            .submit()            .await();       }   }
```


DDL runs through that same query client, so the` CREATE TABLE IF NOT EXISTS` in your startup code no longer needs a Postgres driver.


### Migrating from ILP to QWP is a connect string change


If you already ingest over ILP from Java, the row-building chain doesn't change at all. The schema does:


```text
// Before: ILP over HTTP   try (Sender sender = Sender.fromConfig("http::addr=localhost:9000;")) {       sender.table("trades")             .symbol("symbol", "ETH-USDT")             .doubleColumn("price", 2615.54)             .atNow();   }
// After: QWP over WebSocket. Same rows, same calls.   try (Sender sender = Sender.fromConfig("ws::addr=localhost:9000;")) {       sender.table("trades")             .symbol("symbol", "ETH-USDT")             .doubleColumn("price", 2615.54)             .atNow();   }
```


` http::` becomes` ws::` ,` https::` becomes` wss::` , and that's the migration. What changes underneath:


ILP over HTTP QWP over WebSocket


Auto-flush rows 75,000 1,000


Auto-flush interval 1,000 ms 100 ms


Error model Synchronous,` flush()` throws Async, via an error handler


Store-and-forward Not available Available (` sf_dir` )


Querying Not available Same client


Flushing is where the two differ most. Over HTTP,` flush()` fails loudly if the server rejects the batch. Over QWP it returns as soon as the rows reach the background sender, and problems come back later through the error handler, so code that relied on a throwing` flush()` needs another look.


The[Java client page](https://questdb.com/docs/connect/clients/java/#migration-from-ilp-httptcp) has the full migration table. Every other client keeps your row-building code as it is, but check your language's page under[connecting to QuestDB](https://questdb.com/docs/connect/overview/) for how its flush and error semantics change.


---


## Columnar all the way down: typed columns, no row parsing


QWP sends typed column bodies, not rows. A batch carries the schema once, then each column's values packed at their native width, with` SYMBOL` columns dictionary-encoded so that repeating` "ETH-USDT"` five million times costs one dictionary entry and five million small keys. There's no per-row parse on the server and no per-row materialization on the client.


QuestDB was built around ingesting millions of rows per second, and for years nobody asked the opposite question, because the answer to a query was usually small: a dashboard panel, a nightly report, one aggregate. That has changed. More and more, the database sits between an ingestion pipeline and something else that wants the rows back: a feature store, a dataframe pipeline, a model in training. When the result set is millions of rows rather than fifty, moving it becomes the bottleneck.


Say you store every tick as it arrives, at microsecond resolution, and you want a gap-free, one-second series to train on:


Downsample ticks to a uniform, gap-free 1s series


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20timestamp%2C%0A%20%20symbol%2C%0A%20%20last(price)%20%20AS%20price%2C%0A%20%20sum(amount)%20%20AS%20volume%0AFROM%20trades%0AWHERE%20symbol%20%3D%20%27BTC-USDT%27%0A%20%20AND%20timestamp%20IN%20%27%24now%20-%201h..%24now%27%0ASAMPLE%20BY%201s%20FILL(PREV)%3B%0A&executeQuery=true)


```text
SELECT     timestamp,     symbol,     last(price)  AS price,     sum(amount)  AS volume   FROM trades   WHERE symbol = 'BTC-USDT'     AND timestamp IN '$now - 1h..$now'   SAMPLE BY 1s FILL(PREV);
```


Computing that was never the slow part. Before QWP, getting the answer out meant PGWire serializing it row by row, and your client turning those rows back into a column layout it could actually use. QWP streams it as columns, in batches, and the client keeps those columns as columns.


### How QWP maps to Apache Arrow, and what is not zero-copy


The bytes on the wire are QWP's own columnar format, not Arrow. We wrote our own because it packs tighter than Arrow IPC would let us. What the Arrow-capable clients do is decode a batch and hand the resulting column buffers to Arrow, adopting the bulk numeric payload **by reference** rather than rebuilding it element by element. A` DOUBLE` column with no nulls becomes an Arrow buffer that points into the received frame.` VARCHAR` string arenas are adopted the same way. On the write side, an Arrow` RecordBatch` is walked once and written straight into the outbound buffer, with no intermediate staging and no per-row dispatch.


What still costs a pass:


- **Nullable fixed-width columns.** QWP sends only the non-null values, so the decoder expands them into a full-width buffer.
- **Booleans and Gorilla-compressed timestamps** , which are bit-packed on the wire and expanded on arrival.
- **` SYMBOL` dictionaries** , which are copied when built, though a per-cursor cache amortizes that across a streaming result.
- **Null bitmaps** , which are inverted because QWP marks nulls and Arrow marks valid.
- **zstd-compressed batches** , which decompress into a pooled buffer first.


Even so, no rows are ever materialized, and the wide numeric columns arrive by reference.


That` SYMBOL` handling is also why` to_polars()` and` to_pandas()` are worth reaching for when a dataframe is the destination. They intern the connection's dictionary once and build the categorical from it.` to_arrow()` and` iter_arrow()` hand you a generic Arrow form instead, leaving the consumer to reconcile the per-batch dictionaries afterwards, which shows on` SYMBOL` -heavy results.


Info


Arrow support, for both ingestion and query results, ships in the **Python, Rust, and C/C++** clients. In Rust and C/C++ it is an opt-in build feature.


### Dataframes end to end in Python and Rust: pandas, polars and Arrow


If you live in pandas or polars, dataframes are a first-class argument in both directions. The examples below are Python; Rust mirrors them behind the` polars-ingress` and` polars-egress` cargo features, with` flush_polars_dataframe()` on the way in and` fetch_all_polars()` or` iter_polars()` on the way out.


Ingest a frame:


```text
import polars as pl   import questdb
df = pl.DataFrame({       "symbol": ["ETH-USDT", "BTC-USDT"],       "price": [2615.54, 65432.10],       "amount": [0.00044, 0.00120],       "timestamp": [1735689600000000000, 1735689601000000000],   }).with_columns(pl.col("timestamp").cast(pl.Datetime("ns", "UTC")))
with questdb.connect("ws::addr=localhost:9000;") as db:       db.dataframe(df, table_name="trades", symbols=["symbol"], at="timestamp")
```


Query one back:


```text
with questdb.connect("ws::addr=localhost:9000;") as db:       with db.query(           "SELECT timestamp, symbol, last(price) AS price "           "FROM trades "           "WHERE symbol = $1 AND timestamp IN '$now - 1h..$now' "           "SAMPLE BY 1s FILL(PREV)",           ["BTC-USDT"],       ) as result:           training_set = result.to_polars()
```


` db.dataframe()` also takes pandas frames, pyarrow tables and record batches, and anything exposing the Arrow C Data Interface. On the way out,` to_pandas()` ,` to_polars()` and` to_arrow()` materialize the whole result, while` iter_pandas()` ,` iter_polars()` and` iter_arrow()` hand you batches as they arrive, so client memory stays flat regardless of how many rows the query returns.


---


## Multi-host failover: the server tells the client where to go


On every connection, a QuestDB server advertises what it is: its role (standalone, primary, replica, or a primary still catching up after a promotion), its zone, and its cluster identity. QWP clients read that on the WebSocket upgrade and route accordingly.


So` addr` takes a list:


```text
wss::addr=node-a:9000,node-b:9000,node-c:9000;token=YOUR_TOKEN;
```


The client picks a host, tracks the health of every entry in the list, and walks to another one when the current connection breaks. Writers automatically follow the primary wherever it moves. Readers get a filter:


```text
# Queries that must see the freshest data   wss::addr=node-a:9000,node-b:9000;target=primary;
# Analytical queries, kept off the node handling ingest,   # preferring a replica in the local zone   wss::addr=node-a:9000,node-b:9000;target=replica;zone=eu-west-1a;
```


Servers advertise the zone they're in, so a reader can prefer the replica sitting in its own availability zone and keep the analytical traffic off a cross-zone hop. Nothing is pinned: the client sorts candidates by health first and[zone](https://questdb.com/docs/high-availability/client-failover/concepts/#zone-tier) second, so a known-good host across the region still beats an untried local one. Writers ignore it, because a writer has to follow the primary wherever it happens to live.


A host that rejects you because it's in the wrong role is not treated as a failure. The client recognises the rejection, skips the backoff, and tries the next endpoint immediately. A host that has been promoted but hasn't finished catching up is[retried rather than written off](https://questdb.com/docs/high-availability/client-failover/concepts/) .


---


## Store-and-forward: the client buffer that survives an outage


The other half of staying connected is what your application does while nobody is listening.


QWP clients put a store-and-forward queue between your code and the socket. You keep calling` row()` or` dataframe()` , the client keeps appending to the queue, and a background I/O thread drains it. When the connection drops, the appends don't stop and nothing throws. The queue absorbs the outage, the I/O thread reconnects, and everything buffered goes out before the live stream resumes.


By default that queue lives in memory. Point it at a directory and it survives your process:


```text
wss::addr=node-a:9000,node-b:9000;token=YOUR_TOKEN;   sf_dir=/var/spool/questdb;   sender_id=ingest-01;   sf_durability=periodic;   sf_max_total_bytes=10g;
```


Written on one line, as connect strings are, but split here so it fits.


- ` sf_dir` turns on disk mode. The slot lives at` <sf_dir>/<sender_id>/` .
- ` sender_id` is that slot's identity. Give every sender sharing an` sf_dir` its own, and it survives a restart: bring the process back up with the same pair and the client replays whatever the server never acknowledged, on the I/O thread, in parallel with the new rows your application is already writing.
- ` sf_durability=periodic` checkpoints to stable storage on an interval. The default,` memory` , survives a process crash but not a power cut, because the page cache goes with the host.
- ` sf_max_total_bytes` sets how long an outage the queue can absorb. Hit it and you get backpressure, not data loss.


The ingress reconnect loop never gives up on the clock. It retries with capped exponential backoff for as long as it takes, so what limits you is the size of your queue, not a timeout.


Warning


Replay is at-least-once. The client resends only the frames that were in flight when a connection dropped, but it cannot tell "the server never got this" from "the server committed it and the ack died with the socket", so it assumes the first. That window opens on any reconnect: a failover, a rolling upgrade on a single host, or a sender restart replaying its` sf_dir` . Declare[DEDUP](https://questdb.com/docs/concepts/deduplication/)` UPSERT KEYS(...)` covering row identity on any table you ingest over QWP.


---


## High availability: what QuestDB open source and Enterprise each give you


The client knows the cluster topology and the buffer outlives the connection. Together, that means your application doesn't notice a server restart, a rolling upgrade or a primary migration. Your code keeps calling the same methods, reads fail over query by query, and writes carry on into the queue until the new primary starts answering.


**On QuestDB open source** , all of the client machinery above works. The multi-host list, the health tracking, the store-and-forward queue, the replay on reconnect. What open source doesn't have is a replication layer. There's no replica to fail over *to* . The most you can build is two independent nodes fed by the same producers, and those two nodes start drifting the moment anything goes wrong on one of them. The protocol keeps your writes flowing, but it cannot give you a consistent second copy that was never written.


**On[QuestDB Enterprise](https://questdb.com/enterprise/)** , replication is there from the start, and the pieces line up. The primary ships WALs to object storage and then acknowledges the client, so with` request_durable_ack=on` your store-and-forward queue only trims frames that are genuinely safe. If the primary dies before shipping a WAL, those frames are still in your buffer and get replayed against the new primary. That's the data-loss window a transport-level ack leaves open.


In QuestDB Enterprise 4.0, a replica can be promoted to primary without restarting the instance. No process bounce means no dropped connections to reconnect, and no warm query cache thrown away on the node that just became your writer. Combined with client failover, your producers ride straight through a primary migration.


Info


As of the writing of this post, QuestDB Enterprise 4.0 has not shipped. It is expected later this week. Until it does, promoting a replica restarts the instance, and clients reconnect across the restart as described above.


---


## Which protocol should you use: QWP, ILP, PGWire or REST?


Situation Use


New application, code you control **QWP** , via an official client


Telegraf, Flink, existing ILP collectors ILP


Grafana, BI tools, ORMs, any Postgres driver PGWire


` curl` , shell scripts, CSV imports REST API


A language with no QWP client yet ILP + PGWire, or write a client


ILP and PGWire are not deprecated, not in maintenance mode, and not on a removal schedule. They're how QuestDB fits into an ecosystem it did not design. The REST API isn't going anywhere either: it's the shortest path to QuestDB when you don't want a client library at all. QWP is what we reach for when nothing else is in the way.


If your language doesn't have a QWP client yet, the protocol is specified rather than merely implemented. The[wire protocol reference](https://questdb.com/docs/connect/wire-protocols/overview/) and the[client behaviour spec](https://questdb.com/docs/connect/wire-protocols/qwp-client-behavior/) document it at the byte level, with the Java client as the reference implementation.


---


## Getting started with QWP


QWP ships in QuestDB 10.0:


```text
docker pull questdb/questdb:10.0.0
```


Then pick your language and use a` ws::` connect string:


- [Java](https://questdb.com/docs/connect/clients/java/)
- [Python](https://questdb.com/docs/connect/clients/python/)
- [Rust](https://questdb.com/docs/connect/clients/rust/)
- [C and C++](https://questdb.com/docs/connect/clients/c-and-cpp/)
- [Go](https://questdb.com/docs/connect/clients/go/) (beta)
- [.NET](https://questdb.com/docs/connect/clients/dotnet/) (beta)


The[connect string reference](https://questdb.com/docs/connect/clients/connect-string/) is the one page worth bookmarking. Every option in this post, and every option you'll need later, is a key in that string, and the vocabulary is the same in every language.


Questions, or something that doesn't behave the way this post says it does? Come find us in[QuestDB Community Slack](https://slack.questdb.com/) or on[Discourse](https://community.questdb.com/) .
