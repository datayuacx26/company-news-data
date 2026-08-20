---
schema_version: "1.0.0"
document_id: "34810b0e37331989efd827ed9f37b2c85d6a2a8ca5ca92f5dc915094a9f680f4"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/qwp-vs-ilp-ingestion-benchmark/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-07T04:34:11.142948+00:00"
fetched_at: "2026-08-07T04:34:11.988409+00:00"
content_hash: "sha256:142d9647b9e30916eab6977458204171a70c4059961af1de0fe41ac610edae72"
---

# Introducing QuestDB's new binary ingestion protocol: QWP

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


QuestDB has ingested data over[InfluxDB Line Protocol](https://questdb.com/docs/connect/compatibility/ilp/overview/) ("ILP") since the beginning. It is a text format, it is simple, and we have spent years making it fast, as documented in our[InfluxDB comparison](https://questdb.com/blog/influxdb-vs-questdb-comparison/) where QuestDB ingests up to 11.36M rows per second.


QuestDB now has a purpose-built ingestion protocol of its own: the[QuestDB Wire Protocol](https://questdb.com/docs/connect/wire-protocols/overview/) , QWP. It is a binary, columnar protocol carried over a WebSocket. Where ILP sends` cpu,hostname=host_0 usage_user=58i ...` as text that the server parses, QWP sends typed columns with symbols dictionary-encoded and numbers in their native width. On disk, the same 69 million rows are 24 GB of ILP text or 7.1 GB of QWP binary, about 3.4x smaller.


So how much faster is QWP for ingestion? We benchmarked it with[TSBS](https://github.com/questdb/tsbs) , the standard Time Series Benchmark Suite.


## Key results


Feeding both protocols wire data prepared ahead of time, QuestDB ingests QWP at:


- **33M rows per second** on a single machine, even at a million distinct series
- **19M over a real network** , against ILP's 5.3M, about 3.6x at every cardinality up to that million


It goes faster still at lower cardinality, 48M on one machine at 4,000 hosts, but the number that matters is that the lead holds at extreme scale.


The gain is the wire. QWP's rows are about 97 bytes against ILP's 347, so ILP fills the network first. QWP also covers both writing data in and querying it back out from one client, replacing the separate ingestion and query libraries QuestDB benchmarks have used until now.


↑ Higher is better


TSBS cpu-only at 1,000,000 hosts, wire data prepared ahead of time for both protocols


+3.6x


over the network


+3.1x


on a single machine


1M


distinct series


The full answer needs more than one number, and it starts with a result that looked wrong. We have outpaced the benchmark tool again.


## Setup


We matched the hardware from our published benchmarks so the numbers line up.


Instance AWS EC2 r8a.8xlarge, 32 vCPU, 256 GB RAM, AMD EPYC


Storage GP3 EBS, 20,000 IOPS, 1 GB/s throughput


OS Ubuntu 24.04


Database QuestDB 10.0.0


Client TSBS, built against the QuestDB Go client at the July 30 commit


Scenario TSBS` cpu-only` , seed 123


Cardinality 1,000, 4,000, 100,000 and 1,000,000 hosts


Schema 10 symbol columns and 10 integer columns per row


Loader workers 32 throughout


` SYMBOL` is QuestDB's type for repeated, low-cardinality strings, here the host and tag fields.


All ingestion figures below are **rows sent per second** , the throughput reported by the loader (TSBS's client program that pushes data into the database), which is the convention behind our published benchmarks.


## The co-located run measures the tool, not the protocol


We ran TSBS the way it has always been run, loader and QuestDB on the same machine, at 4,000 hosts, and QWP does not pull ahead:


transport rows sent/s


ILP over TCP 11.6M


QWP 11.1M


A binary columnar format 3.6x smaller on the wire comes in a hair behind plain text. That is not what a wire-format change should do, and[we have been here before](https://questdb.com/blog/optimizing-optimizer-time-series-benchmark-suite/) : the benchmark tool, not the protocol, is setting the number.


Two things do it, both artifacts of the shared box. **CPU:** sampling` /proc` during the load, the QWP loader burns about 10 of 32 cores encoding rows while the ILP loader burns under 2, because ILP's text is already the wire format and it only writes bytes to a socket. Every core the QWP client takes is one the server loses, so the two finish level.


**The free ride is in what TSBS puts in each file, not in the protocols.** TSBS pre-generates its data, and for ILP that file is already line protocol, the wire format, so the loader just streams bytes; QWP's file is an intermediate binary that the client still serializes into frames in the timed loop. No real pipeline pre-serializes to the wire: data arrives as structured values, every client serializes, and ILP formatting numbers to text is usually more work than QWP's binary encoding.


Both effects hide QWP on one box. Put the client on its own machine and they go away.


## Splitting the client and the server


We put the loader on its own r8a.8xlarge and pointed it at QuestDB over the private network, which is how a database is actually deployed. The picture separates cleanly.


scale ILP over TCP QWP


1,000 hosts 5.2M 9.2M


4,000 hosts 5.3M 9.4M


100,000 hosts 5.3M 9.0M


Even here, with the loader still re-encoding every QWP row, QWP ingests about **1.7x** faster than ILP over the network, at every cardinality. And it is doing that while filling only half the link, so there is more to come once the re-encoding is out of the way.


Change only the topology and the two protocols come apart. On one machine they were level; move the client to its own box and ILP drops by more than half while QWP barely moves. ILP falls because it now has to cross a real network with large rows, while QWP holds steady because its rows are small and it was never network-bound in the first place.


The reason is the wire. Line protocol text is about 347 bytes per row here, so 5.3M rows per second is 14.7 Gbit/s, and that is exactly the link speed this instance class provides. ILP is not limited by the protocol, the server, or the client at this point. It has simply filled the network. QWP's binary rows are about 97 bytes each, so the same 5.3M rows would be 4.1 Gbit/s, and QWP keeps going until it is moving about 9M rows per second while still using only half the link.


On a real network, the binary columnar format finally wins. It was invisible on a single box because loopback has no bandwidth limit, so the larger text format paid nothing for being larger.


## How fast QuestDB really ingests QWP


The split-host numbers above still carry the TSBS overhead from the last section. Inside the timed loop, the loader reads the intermediate file, rebuilds each row, and encodes it, and only then sends it. That is the harness, not the protocol. To measure QWP ingestion itself, we move the harness work out of the timed path, exactly the kind of limitation we described earlier.


The loader can encode all the QWP frames once, up front, and then time only sending those frames to QuestDB. The bytes on the wire are the same QWP frames a client sends. Nothing about the protocol changes. We have only taken the benchmark's file reading and row building out of the measurement, so what is left is QWP going into the server as fast as the network and the database allow.


how QWP is sent at 4,000 hosts at 1,000,000 hosts limited by


through the standard TSBS loader, over network 9.4M ~9M the TSBS harness


as prebuilt frames, over network 19M 19M the network


as prebuilt frames, on a single machine 48M 33M the server


Every row here is the same QWP, fully parsed and stored by the same server. The prebuilt path is not a replay trick: QuestDB still decodes every frame, interns every symbol, and writes every row to the table. The only thing that changes is how much of the benchmark sits in the timed loop. With the harness out of the way, QWP moves 19M rows per second over the network, which at about 97 bytes per row is 1.84 GB/s, filling the 14.7 Gbit/s link again, and 33M to 48M on a single machine, depending on cardinality.


The standard TSBS loader reaches 9.4M of that because it spends most of the timed loop reading a file and rebuilding rows, work a production client streaming from Kafka or an application does not do. That work is per-row and barely moves with cardinality, which is why the loader holds around 9M from a thousand hosts to a million. The ceiling there is the benchmark harness, not QuestDB and not the protocol.


ILP needs no such treatment: its file is already the wire format, so it was replaying prebuilt bytes all along. That puts both protocols on the same footing. Over the network QuestDB ingests QWP at 19M rows per second against ILP's 5.3M, about 3.6x, at every cardinality up to a million series. On a single machine it takes 48M rows per second at 4,000 hosts and holds 33M even at a million, against ILP's 10.5M there.


Last time the TSBS Go loader was the thing holding QuestDB back, and we optimized it. QWP raises the ingestion ceiling so far that the loader is the limit again. We are evolving TSBS to feed data fast enough to show QWP's real speed, so the benchmark keeps measuring the database rather than the tool around it.


## High cardinality holds, up to a million series


Replaying prebuilt frames, QWP's rate over the network barely moves with cardinality: about 19M rows per second whether the data is 1,000, 4,000, 100,000, or 1,000,000 distinct hosts. Every one of them, and ILP too, sits at about the same **1.84 GB/s** on the wire.


scale QWP over network ILP over network


1,000 18.3M (1.78 GB/s) 5.2M (1.80 GB/s)


4,000 19.2M (1.86 GB/s) 5.3M (1.84 GB/s)


100,000 19.2M (1.86 GB/s) 5.3M (1.84 GB/s)


1,000,000 18.9M (1.83 GB/s) 5.2M (1.80 GB/s)


↑ Higher is better


TSBS cpu-only over the network, prebuilt frames, rows sent per second


19M


QWP at every scale


5.3M


ILP at every scale


1.84 GB/s


on the wire, both


It is flat because it is network-bound, and it stays network-bound even at a million series because of how QWP encodes symbols. The dictionary that maps each tag value to an id is a session dictionary: it is built up across a connection and referenced by later frames, not re-sent with every frame. So a million distinct hostnames still cost about 97 bytes per row on the wire, the same as four thousand do, and QWP fills the same 14.7 Gbit/s link at every cardinality.


The server does more work per row as cardinality rises, but that shows only when the server is the bottleneck, which over a real network it is not. On localhost, where there is no link to fill, the cost surfaces: prebuilt QWP ingests 48M rows per second at 4,000 hosts, 37M at 100,000, and 33M at 1,000,000, as QuestDB maintains a larger symbol table. Over the network the link caps all of it at 19M first.


## One client for writing and reading


Ingestion is only half of TSBS, and this is where QWP does something ILP never could. Until now a QuestDB benchmark wrote data with one protocol, ILP, and read it back with a different one, the PostgreSQL wire protocol or HTTP. QWP covers both. Ingest and query are two QWP protocols sharing the same columnar encoding, on two endpoints, so a client still opens a connection per direction, but the library, the connect string and the types on the wire are the same either way.


For the standard` cpu-only` query suite at 4,000 hosts, all 16 query types, QWP returns results as columnar batches. The numbers below are queries per second, higher is better.


query rows returned PostgreSQL wire HTTP QWP


cpu-max-all-1 9 281 403 422


cpu-max-all-8 9 204 254 277


cpu-max-all-32-24 25 50.3 58.3 60.6


single-groupby-1-1-1 61 1162 1226 1364


single-groupby-1-1-12 720 353 323 357


single-groupby-1-8-1 61 949 1136 981


single-groupby-5-1-1 61 1198 1360 1278


single-groupby-5-1-12 720 352 326 313


single-groupby-5-8-1 61 818 924 911


double-groupby-1 52,000 28.8 16.7 28.1


double-groupby-5 52,000 20.6 8.6 20.1


double-groupby-all 52,000 15.6 5.7 14.5


high-cpu-1 810 200 210 231


high-cpu-all 1,596,723 1.98 1.17 2.47


lastpoint 4,000 610 492 853


groupby-orderby-limit 5 233 54.9 223


The pattern follows the result size. On the queries that stream many rows back, QWP's columnar result format pulls ahead: it wins` lastpoint` (4,000 rows) by 1.4x over the PostgreSQL wire and 1.7x over HTTP, and` high-cpu-all` (1.6M rows) by 1.25x and 2.1x, for the same reason it wins on ingestion, less to encode and less to move. It takes several of the small queries too, where the three are otherwise close, though the PostgreSQL wire trails the other two on the` cpu-max-all` group. HTTP serializes results to JSON and falls off hardest on the wide double-groupby scans, which the PostgreSQL wire edges.


These query numbers still undersell QWP, because a TSBS query is a request-and-response measurement. QWP streams: results arrive as a sequence of columnar batches that a client can start processing as they land, without waiting for the whole result set. TSBS was not built to exercise that, so it does not appear in these figures at all.


It is a large part of why QWP exists, and we will cover it properly in a post of its own soon. Streaming data out of QuestDB in Apache Arrow format, we measured almost **300 million rows per second** with the Python client, about 9.35 GB/s of Arrow data.


## Conclusion


QWP is faster than ILP for ingestion, but one multiplier does not cover it:


- **Feed both protocols prebuilt wire data** , which the ILP file already is, and QuestDB ingests QWP **about 3.6x faster than ILP over a network** (19M rows per second against 5.3M) at every cardinality, and 48M on a single machine at 4,000 hosts, holding 33M even at a million distinct series, against ILP's ~11M.
- **The gain is the wire.** QWP's rows are about 97 bytes against ILP's 347, so ILP fills the network first and the server has less to parse.
- **TSBS needs to catch up.** A standard run stores ILP as ready-to-send text but makes QWP re-encode, and it puts the loader next to the server, so the two protocols come out level. QWP now ingests faster than the benchmark can feed it, and closing that gap in our open-source[TSBS fork](https://github.com/questdb/tsbs) is work in progress. Contributions welcome.


Treat these numbers as a strong indicator, not a guarantee, and test QWP against your own workload before you rely on them. The switch should still help: the win comes from a wire a fraction of the size and a client that encodes less, and that holds whatever your data looks like.


ILP is not going anywhere. It is simple, well optimized, and compatible with the wide range of tools and older QuestDB versions that already speak it.


QWP is more than a faster wire, though. One client both writes data in and streams it back out in Apache Arrow. It buffers messages locally with built-in store-and-forward and replays them if the server disconnects, and given more than one host in its connection string it fails over on its own when a node goes down, which pairs with QuestDB Enterprise replication for high availability on both writes and reads.


As usual, try the latest QuestDB release and share your feedback on our[Community Forum](https://community.questdb.com/) , play with the[live demo](https://demo.questdb.io/) , and contribute to[QuestDB on GitHub](https://github.com/questdb/questdb) .
