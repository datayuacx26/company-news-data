---
schema_version: "1.0.0"
document_id: "f5f024e6e6ce9cf13bb4fed0fc5d7a176beefad96b47f2f753ae4140a8d144b8"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/streaming-500-million-rows-into-apache-arrow/"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T19:16:32.728478+00:00"
fetched_at: "2026-08-07T19:16:34.120281+00:00"
content_hash: "sha256:2f1a9e1edd87ec176e87ac7a16fc3ce819e6bc5b779f9f0ed0d731197bc4df9a"
---

# Streaming 500 million rows into Apache Arrow in 2.3 seconds

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


Most database benchmarks measure how fast a query engine can compute an answer. This one measures how fast the database can hand you the rows once it has them.


That's egress, and it matters because of what people do with the data next. Feeding a model, running a dataframe pipeline, backfilling a feature store. The query is often trivial, and the volume rarely is.


It wasn't always this way. For years an analytical database earned its keep by turning billions of rows into a handful: a dashboard chart, a nightly report, a single aggregate. The result was small by design, so how it left the database was nobody's concern. That has flipped. More and more workloads now go from billions of rows to millions or even billions of results: a downsampled series, raw rows enriched and passed to the next stage, a training set on its way into a model. When the output is that large, moving it becomes the bottleneck, and fast egress matters as much as the fast ingestion and fast queries QuestDB was built for.


QuestDB recently gained[QWP](https://questdb.com/docs/connect/wire-protocols/qwp-egress-websocket/) , a WebSocket protocol that can return query results as[Apache Arrow](https://arrow.apache.org/) batches. Arrow matters here because it's zero copy: the bytes that arrive off the socket are already in the layout that polars, pandas or DuckDB want, so nothing has to be deserialized row by row into Python objects. Combined with streaming, where you get the first batch while the rest is still in flight, that changes what's possible from a[Python client](https://questdb.com/docs/connect/clients/python/#querying) .


We wanted a ballpark number for ourselves. How fast can QuestDB stream query results now? And since we were building the harness anyway, how does that compare to other databases people actually use for this?


---


## The short version: 220 million rows per second into Arrow


One distinction runs through everything below. A **streaming** path hands you rows as they arrive: the first batch lands in milliseconds, and client memory stays flat no matter how large the result. A **buffered** path assembles the whole result in the client before returning any of it, so nothing arrives until everything has, and memory grows with the row count. Both can post fast throughput numbers, but they're not the same thing.


↑ Higher is better


Each engine at its own fastest measured configuration, 500M rows, 8 parallel readers


1.55x


vs ClickHouse native


2.35x


vs ClickHouse Arrow


32ms


to the first Arrow batch


- **QuestDB streams 220 million rows per second** into Arrow with eight readers, which is 500 million rows in 2.3 seconds, and the first Arrow batch lands after **32 milliseconds** .
- **ClickHouse's fastest path reaches 142 million** , but it does not stream. It materializes about 18 GB in the client and returns nothing at all for 15.3 seconds, then everything at once. Its fastest *streaming* path is 94 million.
- **TimescaleDB reaches 14 million** and is the only engine here whose limit is the speed of the database itself at every reader count we tried. QuestDB turns out to be server bound too, but for a different reason and more than an order of magnitude further up.
- **QuestDB also moves the fewest bytes** , 18.8 per row against 21.4, 27.0 and 64.3, because` SYMBOL` columns cross the wire as Arrow dictionaries instead of repeating the same strings 500 million times.
- **Our first three conclusions turned out to be artifacts of our own test rig** , not facts about any database. Catching them, and making sure every engine was measured at its best, is most of what this post is about.


---


## What we measured, and what we did not


The query is deliberately boring:


The entire workload


```text
SELECT symbol, side, price, amount, timestamp   FROM trades;
```


Every column, no filter, no aggregation, no join. If we'd written an interesting query we'd be measuring the query engine, and we wanted the opposite: as close to pure egress as we could get, so the number reflects the protocol and the client, not the planner.


The reader is equally naive. It takes the timestamp range, splits it into N equal slices, and gives each slice its own connection. Every reader counts the rows and the decoded Arrow bytes it receives, and nothing else. No processing, no conversion, no writing anywhere. We want the ceiling, not a realistic pipeline.


The dataset is 500 million rows of the same trades table used on[demo.questdb.io](https://demo.questdb.io/) :


The table, in QuestDB


```text
CREATE TABLE trades (     symbol SYMBOL,     side SYMBOL,     price DOUBLE,     amount DOUBLE,     timestamp TIMESTAMP   ) TIMESTAMP(timestamp) PARTITION BY DAY WAL;
```


The other two get the closest equivalent:` LowCardinality(String)` and` Float64` in a ClickHouse` MergeTree` ordered by timestamp, and` text` ,` double precision` and` timestamptz` in a TimescaleDB hypertable with one day chunks. Both string columns are genuinely low cardinality, 27 distinct symbols and 2 sides, which turns out to matter more than we expected.


### Where the bytes go


Since the whole exercise is about moving bytes, here is which column costs what. Decoded Arrow bytes per row, measured per column:


Column Type QuestDB ClickHouse Arrow Timescale ADBC


` symbol` low-cardinality string **4.02** 11.56 11.56


` side` low-cardinality string **4.00** 7.47 7.46


` price` float64 8.00 8.00 8.00


` amount` float64 8.00 8.00 8.00


` timestamp` microsecond 8.00 8.00 8.00


**total** **32.02** 43.02 43.02


The three numeric columns are byte for byte identical everywhere, 24 bytes of the row. The entire difference is in the two string columns.


QuestDB's` SYMBOL` type stores strings once and references them by integer, and QWP carries that representation through to the client: the columns arrive as Arrow dictionaries, a` uint32` index per row plus one shared dictionary of 27 values for the whole batch. Four bytes per row instead of the string itself. ClickHouse and TimescaleDB both send the raw UTF-8 for every row, so` BTC-USDT` costs its eight characters on row one and again on row five hundred million.


That's a data model advantage rather than a protocol trick, and it's most of why QuestDB moves fewer bytes over the wire later in this post. It also has an obvious limit: on high cardinality strings, where a dictionary stops paying for itself, this gap would narrow or disappear.


Info


ClickHouse can preserve` LowCardinality` as an Arrow dictionary too, via the` output_format_arrow_low_cardinality_as_dictionary` setting. It's off by default, so we left it off, as this is what a user would get out of the box.


The contenders, each on the fastest read path it offers:


Engine Path Streams?


QuestDB QWP over WebSocket, Arrow batches yes


ClickHouse HTTP +` FORMAT Arrow` via clickhouse-connect yes


ClickHouse native TCP + numpy via clickhouse-driver` execute()` no


TimescaleDB ADBC PostgreSQL driver, Arrow over binary COPY yes


TimescaleDB connectorx no


We included ClickHouse native for a specific reason. The native protocol itself streams the result as columnar blocks off the wire, and clickhouse-driver can consume it either way:` execute_iter()` streams those blocks row by row, while` execute()` drains all of them into numpy columns and returns only once the last one arrives. The driver documents` execute()` as the faster of the two, and we confirmed it: on a 30 million row local test` execute()` moved rows about **5x** faster than` execute_iter()` (5.4M rows/s against 1.1M), at the cost of buffering the whole result and holding roughly 14x the memory. So we split the two jobs. The Arrow variant already covered streaming, so native took the opposite question, how fast raw decode goes when nothing waits on streaming, and we ran it with` execute()` . That the native protocol is leaner than HTTP made it worth measuring. The question took us two wrong answers to resolve.


Info


This is not a benchmark. It's an informal test we ran for ourselves and then wrote up, on one dataset shape and one hardware shape, by the vendor of one of the engines being measured. Treat these as directional numbers for a narrow question: how quickly does each client path move rows into Arrow. Everything needed to re-run it, and to disagree with it, is in the repository linked at the end.


Measuring each engine at its best is the hard part, and the ClickHouse native path is the clearest example. It would have been convenient to publish its first result, which made QuestDB look roughly 8x faster. Instead we went looking for why it was so slow, found the problem was our own test harness rather than ClickHouse, and fixed it. That correction moved ClickHouse's best number up by 5.2x and cut our lead to 1.55x.


---


## The first run measured a disk


Our first serious attempt used a 16 GB instance. QuestDB came out at 41 million rows per second, and adding readers barely moved it. Something was clearly wrong, but it took a while to see what.


The dataset was the problem. QuestDB's 500 million rows occupy 15 GiB on disk in its uncompressed native columnar format, and the server had only 16 GB of RAM. Once the OS and the server took their share, the working set no longer fit in the page cache, so every read came off EBS and flattened out at the volume's provisioned ceiling of 1000 MiB/s. We weren't measuring protocol speed; we were measuring whether the bytes fit in RAM, with throughput sitting suspiciously close to a ceiling we had provisioned ourselves.


Moving the server to 123 GB of RAM, enough to hold all three engines' datasets in memory at once with room to spare, took QuestDB from 41 million to 220 million rows per second. A 5x swing from a memory sizing change, before we'd measured a single protocol.


If you're measuring egress, make sure the working set is cache resident, or you're measuring your disk with extra steps.


### How we measured


Everything below was measured on two AWS instances in the same availability zone and cluster placement group:


> **Database host:**` m8gn.8xlarge` , 32 vCPU, 123 GB RAM, 100 Gbps. **Client host:**` c8gn.8xlarge` , 32 vCPU, 61 GB RAM, 100 Gbps. Root volumes are gp3 at the maximum 16,000 IOPS and 1000 MiB/s.


Two separate machines, because on a single box the client and server share loopback with no round trip latency, and parallel readers buy almost nothing. Across a real network a single connection is round trip bound, which is the regime we care about.


No engine was tuned to win this. We left each on its defaults so the numbers reflect what a user gets out of the box: ClickHouse on the stock image, with only the password recent images require. The exception was TimescaleDB, which got` shared_buffers` raised to 2 GB and the parallel worker limits lifted, because the Postgres defaults of 128 MB and two workers per gather are laptop sized and would have made the comparison meaningless on a 123 GB machine. That was the only hand-tuning anywhere in the setup, and it favoured a competitor. The full compose file is in the repository.


Only the engine under test runs at any time. The other two containers are stopped, so nothing else competes for RAM, CPU or disk. Every cell is two warmup passes, then a 10 second pause, then the mean of three measured runs.


Warmups fill the cache but leave the server busy with merges and writeback, and that tail was landing in the first measured run. Adding the pause took one QuestDB cell from a 32% spread down to 3.8%.


---


## The results: rows per second by reader count


Rows per second, readers running as threads in a single Python process:


readers QuestDB ClickHouse arrow ClickHouse native Timescale ADBC Timescale connectorx


1 40,139,148 16,068,358 32,014,238 2,025,417 1,051,424


2 78,009,188 31,696,334 31,445,416 4,026,787 1,869,219


4 135,482,531 49,835,630 28,810,418 8,026,776 not run


8 **218,966,796** 93,788,141 27,140,742 14,156,600 not run


That's 500 million rows drained in 2.3 seconds at eight readers, and it answered our original question.


One number in that table looked wrong. ClickHouse native starts at **twice** ClickHouse Arrow's single-reader speed (32.0M against 16.1M), the leaner protocol paying off, then goes backwards as readers are added, ending at 27.1M while Arrow scales to 93.8M. We first blamed memory, since native buffers the whole result, but the collapse persisted on the 123 GB client, so we measured CPU during an eight reader run: on 32 vCPU, native was using about one and a half cores.


That's the Python[GIL](https://docs.python.org/3/glossary.html#term-global-interpreter-lock) . clickhouse-driver builds numpy arrays while holding the interpreter lock, so the eight threads take turns instead of running in parallel, while the Arrow paths decode in C++, release the lock, and scale. Rerunning native with each slice in its own process removes the bottleneck:


readers QuestDB threads QuestDB processes CH native threads CH native processes


1 40,139,148 40,162,355 32,014,238 32,237,368


2 78,009,188 69,470,073 31,445,416 62,493,086


4 135,482,531 130,084,145 28,810,418 88,134,499


8 218,966,796 220,429,878 27,140,742 **141,895,845**


Native jumps from 27.1M to 141.9M, a **5.2x** gain from nothing but the client's concurrency model, while QuestDB and Arrow move 1.01x and 1.00x because they never held the lock. In Python, "does not scale with threads" and "does not scale" are different claims, and the process run is what separates them. The honest comparison is the corrected one, QuestDB's 220M against native's 142M.


---


## What buffering actually costs


With native finally running properly at 141.9M, the question worth asking is what you give up to get there.


We timed how long each path takes to hand back its **first** batch, on a single reader over the full table:


Path Streams? First batch Total First batch as % of total


QuestDB QWP Arrow yes **0.032 s** 12.68 s 0.25%


ClickHouse Arrow yes **0.068 s** 31.30 s 0.22%


Timescale ADBC yes **0.157 s** 226.21 s 0.07%


ClickHouse native no **15.34 s** 15.34 s **99.999%**


QuestDB returns usable Arrow after 32 milliseconds. ClickHouse native returns nothing for 15.3 seconds, because its first row and its last row arrive at the same instant. That 99.999% is not a tuning artifact:` execute()` cannot return until the whole result exists.


Memory follows the same pattern. During an eight reader native run, client memory climbed to 17.9 GiB. Streaming paths hold constant memory whatever the result size. At 500 million rows buffering costs about 18 GB. At five billion it's not slow, it's impossible.


So the comparison is not one number against another:


Engine Best config rows/s at 8 readers Streams? Client memory


**QuestDB** QWP Arrow, threads or processes **220,429,878** yes constant


ClickHouse native TCP, 8 processes 141,895,845 no ~18 GB


ClickHouse Arrow (best streaming path) 93,788,141 yes constant


TimescaleDB ADBC 14,156,600 yes constant


Against ClickHouse's fastest configuration of any kind, QuestDB is 1.55x faster. Against its fastest streaming configuration, 2.35x. And the fast ClickHouse configuration is the one that materializes 18 GB before returning a row and needs eight operating system processes to avoid the GIL.


---


## What crosses the network: bytes per row


Since the whole point is moving bytes, we measured the client's network interface counters around each run rather than inferring from row counts.


At eight readers, each path in its best configuration:


Path Wire bytes Bytes/row Elapsed Wire throughput


QuestDB, 8 threads **9.40 GB** 18.8 2.31 s **4.07 GB/s (32.6 Gb/s)**


ClickHouse native, 8 processes 13.48 GB 27.0 3.67 s 3.68 GB/s (29.4 Gb/s)


ClickHouse Arrow, 8 threads 10.70 GB 21.4 5.37 s 1.99 GB/s (15.9 Gb/s)


Timescale ADBC, 8 threads 29.44 GB 64.3 28.88 s 1.02 GB/s (8.2 Gb/s)


QuestDB moves the fewest bytes per row of any path measured, and it's also the fastest. That combination is not the usual trade, and it follows directly from the column breakdown earlier: dictionary encoded symbols mean there is less to send. QWP then costs 18.8 bytes on the wire for a row that occupies 32 bytes once decoded into Arrow, so it's compressing on top of that.


The Postgres wire protocol is the outlier at 64.3 bytes per row, over three times QuestDB's cost for identical data. At peak QuestDB is pushing about a third of a 100 Gbps link from a single client.


Storage tells a related story:


Engine On disk Bytes/row


ClickHouse 6.0 GiB 12.9


QuestDB 15 GiB 32.2


TimescaleDB 48 GiB 112.5


These are each engine's default on-disk footprint. ClickHouse` MergeTree` compresses by default, while QuestDB's native format is uncompressed columnar, which is what we used, though QuestDB can also store partitions as Parquet and get compression that way. We didn't do that here, so QuestDB's 15 GiB is its uncompressed footprint and a compressed one would be smaller, depending on the compression ratio. Footprint matters beyond storage cost: it decides whether the working set fits in RAM, and cache residency dominated everything in this post.


TimescaleDB is a different story. Its per-tuple header and alignment padding are inherent to Postgres row storage rather than a compression setting, which is where the 8x footprint over ClickHouse comes from. It's also not client bound at all: at eight readers it moves 8.2 Gb/s on a 100 Gbps link while scaling at 87% of ideal, so the constraint is entirely inside Postgres.


---


## Scaling, and where the ceiling actually is


Path 1 to 8 readers % of ideal


Timescale ADBC 6.99x 87%


ClickHouse Arrow 5.84x 73%


QuestDB 5.49x 69%


ClickHouse native, processes 4.40x 55%


ClickHouse native, threads 0.85x -15%


The ranking here is upside down. TimescaleDB scales best precisely because it's nowhere near any limit. QuestDB scales worst of the streaming paths because at 32.6 Gb/s of wire traffic it's the closest of them to the hardware.


We read that as the machine running out rather than the protocol, which would make 220 million rows per second a floor for this setup rather than a ceiling. The floor part was right, the reason was wrong, and it took another round of testing to find out.


---


## Pushing it further: two billion rows and 64 readers


We had called 220 million rows per second a floor without testing it, so we swept the reader count well past eight, and then quadrupled the data.


Warning


Everything in this section ran on a rebuilt rig with a **larger client** :` m8gn.8xlarge` , 32 vCPU, 123 GB RAM, 100 Gbps, in place of the 61 GB` c8gn.8xlarge` above, because that type had no capacity in the region when we came back to it. Same core count and same network, more memory. Compare the numbers in this section with each other, and do not splice them into the tables above.


The sweep found a peak we had walked straight past. Eight readers isn't where QuestDB tops out. At 500 million rows it climbs to 266 million rows per second at twelve readers, then falls away steadily: 230 million at thirty-two, 216 million at sixty-four, below where it started. ClickHouse Arrow peaks later, at sixteen readers, and then stays flat out to forty-eight.


To check whether that held at scale we needed more rows, so we reloaded both engines with two billion. In QuestDB's uncompressed columnar format that's 64 GB on a box with 123 GB of RAM, which is the number that matters here: the working set still fits in page cache, so this measures the streaming path, not the disk. We checked that this time rather than assuming it, and found 66 GB resident with 31 GB to spare.


Readers QuestDB ClickHouse Arrow Ratio


12 **291,916,916** 165,484,533 1.76x


16 261,746,529 **173,833,531** 1.51x


24 253,041,955 167,990,905 1.51x


Each streaming path has its own best point: QuestDB at twelve connections, ClickHouse Arrow at sixteen. Peak against peak the gap is 1.68x, and QuestDB gets there using fewer connections. Two billion rows arrive in 6.85 seconds, at 9.35 GB/s of decoded Arrow. Neither path holds its peak as connections keep climbing, and QuestDB gives up more, 13% down by twenty-four readers against ClickHouse Arrow's 3%.


The floor held. Four times the data didn't slow either engine down.


Then we went looking for what does stop it, because we'd blamed the hardware and the counters disagreed. These diagnostics ran at 500 million rows, where QuestDB peaked at 266 million. At that peak, every resource we could measure had headroom:


- **Network:** iperf3 between the boxes sustained 99.8 Gb/s while the readers moved 5.0 GB/s, about 40 Gb/s of wire traffic.
- **Server CPU:** QuestDB's threads used 12.6 of the server's 32 cores.
- **Client CPU:** no core went past 86%.
- **Memory:** 3.2 instructions per cycle at a 0.45% cache miss rate, nothing stalling on memory.
- **Docker bridge:** host networking instead changed throughput by 1.3%, inside the run to run spread.


What grows with connection count is coordination cost: twelve readers to thirty-two took 42% more CPU for 14% less throughput, and those extra cycles were retiring instructions, not stalling. That's the same effect that made four times the data help rather than hurt: a larger request spreads a fixed per-request cost over more rows. So the ceiling isn't the machine, it's what the software spends per connection, and QuestDB reaches it while most of the server sits idle.


---


## Caveats


Some of these matter more than others, and one is a genuine flaw:


- **TimescaleDB holds 458 million rows, not 500 million.** We accidentally stopped its load early and only noticed at the end. Rates are unaffected, since each reader divides the rows it actually received by its own elapsed time, and since TimescaleDB was already the slowest by a wide margin we didn't rerun it at the full count.
- **connectorx has only 1 and 2 reader cells.** It ran at roughly half ADBC's speed and its 1 to 2 step was already sublinear, so the remaining cells weren't worth the hours. On a smaller dataset it degraded outright as readers were added.
- **One schema, one hardware shape.** Five columns, two of them low cardinality strings. A wide table, or high cardinality strings, would land differently.
- **Decoded byte counts are not comparable across engines.** QuestDB dictionary encodes its string columns at 32 bytes per row where the others send them plainly at 43. Wire bytes measured at the interface are the fair comparison, which is why we measured them.


---


## Takeaways


The number we came for is 220 million rows per second, or 500 million rows in 2.3 seconds, streaming into Arrow with the first batch available after 32 milliseconds. For a Python client feeding a model or a dataframe pipeline, that's far beyond what row based protocols can offer.


Our first three conclusions were artifacts of our own test rig, not facts about any database: the first run measured a disk, the slow ClickHouse native result measured Python's GIL, and the ceiling we blamed on the hardware was a server sitting half idle. Each had a comfortable explanation ready, which is why a trustworthy egress number takes this much checking of the things you assumed were fine.


The harness is deliberately naive and fully reproducible, and it lives in a[public repository](https://github.com/javier/questdb_qwp_egress_to_arrow_perf) . Four commands take it end to end: provision two instances, bootstrap them, run the sweep with per-engine isolation, pull the results back, and tear everything down. The compose file shows exactly what each engine was given, the raw per-run numbers behind every table are in the results, and the reader scripts are small enough to read in one sitting.


If you run it on your own hardware and get different answers, we'd rather hear about it than not.
