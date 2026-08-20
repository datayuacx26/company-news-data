---
schema_version: "1.0.0"
document_id: "226e029f1eba0b96af85b8168b67cacaaa845aa80ee69936a6752bef7751572b"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/one-copy-one-engine-no-seams/"
published_at: "2026-06-19T14:00:58+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:573d5384fd53b784cfa9b578bdebaac42a19e5251a9f42f1aa4fcbcef13401e5"
---

# One Copy, One Engine, No Seams

# One Copy, One Engine, No Seams


## You can’t call HTAP a failure and then chase the same goal under a new name


Jun 19, 2026


•


14 min read


•


[Nadeem Asghar, Chief Technology Officer](https://www.singlestore.com/blog/author/nadeem-asghar/)


I’ve spent a big part of my career living on the seam between two databases. It’s the part of the stack nobody demos. It’s the pipeline that wakes an engineer at 2 a.m. because a schema changed upstream, the sync quietly fell behind, and now the morning dashboard is wrong and no one knows yet. Reynold Xin got a knowing laugh on stage this week when he said CDC really stands for “Continuous Data Corruption.” It landed because every data engineer in that room has lived it.


So when Databricks stood up at the Data + AI Summit and told a 40-year-old story about the wall between operational and analytical data, I paid attention. And the most revealing thing they said was a contradiction. They argued, at length, that the wall must come down for the age of agents and in the same breath declared HTAP, the category built to take it down, a failure, walking away from it under a new name: LTAP.


I ’m going to engage this the way I’d talk to my own engineering team the morning after a competitor’s launch. Not with applause, and not with spin. With the questions that decide whether a 40-year-old problem is actually solved, or just rebranded.


## **The convergence is real, and AI is what made it urgent**


For forty years we lived with a split that made sense at the time. Operational databases ran the business one row at a time. Analytical systems answered the big questions over history. We bridged them with pipelines and replicas and a layer of ETL that everyone paid for and nobody loved. When humans wrote software at human speed, that was a fair trade.


Agents broke the trade. An agent doesn’t work like a person, and it doesn’t even work like the apps we built for people. It reads for context, loops, tries something, writes back, and does it again hundreds or thousands of times before a human would have finished their coffee. At that pace, every hop between the live system and the analytics system becomes the thing slowing everything down. The agent needs current state and deep history in the same instant, and it needs the two to agree. On the problem statement, Databricks and I don’t disagree.


## **About that “HTAP failed” claim**


Reynold said it plainly: HTAP “largely failed.” The systems are proprietary, they have no ecosystem, and by trying to be one engine they compromised on both transactions and analytics. He reached for Michael Stonebraker’s “One Size Fits All: An Idea Whose Time Has Come and Gone” to argue a single engine can’t be great at everything. This is the load-bearing claim of their whole pitch, so it’s worth taking apart.


He’s describing the HTAP of ten years ago, and about those systems he’s right. They did compromise. Many were proprietary islands with thin ecosystems. But that’s a verdict on a generation of products, not on the idea and it’s being used to retire the idea.


But “one size fits all is dead” was an argument against bolting every feature onto one general-purpose engine until it’s mediocre at all of them. It was never an argument against engineering a storage format, on purpose, so that a point write and a big scan both land fast. That distinction is the entire ballgame, and it’s the thing we spent a decade building. SingleStore’s Universal Storage isn’t a general engine with analytics duct-taped on. It’s a single format designed from the start to serve both. “It’s hard, so we’ll keep three engines and a shared bucket instead” is a legitimate engineering choice. It is not a law of physics.


And here’s the part I’d say to Reynold directly, with respect: you don’t get to call HTAP a failure and then spend the next twenty minutes describing why the world needs exactly what HTAP promised. Unifying OLTP and OLAP so an agent can read and write in one place is the HTAP goal, whatever you print on the slide. Renaming it LTAP changes the marketing. It doesn’t change the physics, and it doesn’t retire the questions.


There’s also one specific word in that critique I’d hand right back: proprietary. The knock on HTAP was that the systems are proprietary with thin ecosystems. Fine — then hold the new architecture to the same bar. Reyden is a brand-new engine that is one hundred percent proprietary and closed, with about as young an ecosystem as anything in this market; by definition it has none yet. And Lakebase doesn’t get to borrow Postgres’s open-source halo. The managed Lakebase product — its serverless control plane, its lake integration, its enterprise layer — is proprietary Databricks software that speaks Postgres. It is not “open Postgres.” The open table formats underneath are real and they matter, but an open file on disk is not the same as an open engine reading and writing it. If proprietary engines were a strike against HTAP, they don’t become a virtue just because the data beneath them happens to be Iceberg.


## **Shared storage is not the same as a unified engine**


Reynold framed the core problem the same way I would. OLTP needs row-oriented storage to find a needle in a haystack and update it fast. Analytics needs column-oriented storage to scan huge ranges. That dichotomy is real, and it’s the heart of everything. The question is what you do about it.


LTAP’s answer is to unify at the storage layer: keep separate engines Postgres (Lakebase) for transactions, Spark and Photon for large-scale analytics, the new Reyden engine for real-time reads and point all of them at one open copy in object storage. If you’ve ever kept four copies of the same data alive across four systems, that sounds like relief, and it is an improvement. I’ll give it that.


But “one copy” is a claim about storage, not about the engine. Three engines still sit on top, each with its own cache, its own sense of how fresh the data is, and its own way of failing at the worst possible moment. And by Databricks’ own framing, a row layout and a columnar layout are different things. If a write lands in a row representation for Postgres and analytics reads a columnar representation, then you have two physical shapes of the same data, and something has to keep them in step. You can call that conversion whatever you like. I’ve maintained that “something” with my own hands. It has a freshness lag, and eventually it breaks, usually on a Friday.


And I’ll be candid about why this particular shape makes the hair on my neck stand up. I spent years in the Hadoop world, at Hortonworks and then Cloudera. We had an engine for everything: Hive for SQL, HBase for key-value, Impala for fast queries, Storm for streaming, Solr for search, Spark coming up fast behind them. Each one was genuinely good at its slice. Together they were a nightmare not because any single engine was weak, but because all the pain lived in the seams between them. The integrations that drifted. The security models that never quite lined up. The data shuffled from one engine to the next. The 3 a.m. page when one piece fell a version behind the others. The whole industry eventually voted with its feet and consolidated away from that sprawl. So when I see three engines bolted onto shared storage to solve one problem, I don’t see a breakthrough. I see a smaller, tidier version of a movie I’ve already watched to the end more like Hadoop v2. I’m not rooting against it. I changed the channel for a reason.


## **Where we stand, and why**


We took the other road, and I’ll be honest that it was the harder one to build. One engine. One storage layout. We made that bet because real unification is an engine problem, and you can’t bundle your way out of it. Three ideas hold the whole thing up.


## **One copy of data, for transactions and analytics**


Universal Storage is a single format: an in-memory rowstore tier over columnstore files, in one layout. It gives you columnstore scan speed and rowstore write speed from the same bytes. There is no operational copy and analytical copy to reconcile, no shadow table quietly drifting out of sync while you sleep. The transaction and the query are looking at the same data, because there is only one.


## **No gap between data landing and being queryable**


This is the one I care about most, because it’s the one agents live or die on. In SingleStore, the moment a write commits, it’s queryable. Not after a sync. Not after a materialization job catches up. Right then. Real-time has to include the write path, or it isn’t real-time, it’s just fast reads of data that’s a little bit stale. Worth noting: Databricks said themselves that Lakehouse//RT is launching “starting with read-only workloads.” Reads first. That’s an honest admission, and it’s exactly the gap I mean. When an agent updates an account and, in the same breath, asks a question that joins that account against a year of history, it needs to see its own write. You get that for free when both workloads run in one engine on one copy. You fight for it, and often lose, when a commit has to cross a format boundary into another engine and wait its turn.


## **Multi-model, in the same engine**


Real applications stopped being purely relational a long time ago, and agents put that on the critical path. They want relational tables, JSON documents, vectors for semantic search, full-text, time-series, sometimes all inside a single query. We run those natively in one engine, so one SQL statement can filter on a column, run a vector similarity search, and join the result against a JSON field without ever leaving the database. I’ve watched good teams stitch together a warehouse, a vector store, and a search cluster to do what should have been one query, and then spend the next year keeping all three honest with each other. Unification isn’t only OLTP plus OLAP. It’s refusing to make your people babysit five systems that were never meant to agree.


## **Four questions I’d ask before calling it solved**


These are the questions I’d raise in the room, not to win an argument but because I’ve been burned by skipping them. Databricks didn’t answer them on stage, and that’s fair — it was a keynote, not a design review. But they’re the questions that separate architecture from announcement, and if Databricks answers them well, all of us are better off. I mean that.


## **Is Reyden append-only, or does it really handle updates and deletes?**


Reyden is quick on simple high-concurrency reads. But Lakehouse//RT launched, in Databricks’ own words, “starting with read-only workloads.” Append-only and read-mostly is the easiest way to be fast, and it makes for a beautiful benchmark. Real data isn’t append-only. Orders change status. Balances move. A customer invokes their right to be forgotten, and GDPR does not care how quickly you can append. On the lake, mutation is genuinely hard: Iceberg does row-level deletes through merge-on-read, where the reader reconciles delete files at query time, or copy-on-write, where you rewrite files on every update. Both add cost read amplification or write amplification at the exact moment you wanted low latency. So the question is plain: when Reyden takes writes, does it hold its millisecond numbers on data being updated and deleted hard, or is that the precise spot where, in Reynold’s own word, HTAP “failed”? We chose to handle in-place updates and deletes as a first-class thing, because that’s what operational workloads actually do to you.


## **What’s the consistency guarantee across a multi-table join?**


This is the one that keeps me up at night, in the good way. Picture an analytical query joining five tables: orders, payments, inventory, customers, events. In a single MVCC engine, that query reads one consistent snapshot across all five. It sees a transaction everywhere or nowhere. Now picture each of those tables as an independent open-format table that materializes from the transactional engine asynchronously, on its own schedule. What guarantees that a join across them sees a consistent set of snapshots? If orders materialized at 12:00:03 and payments at 12:00:05, a join run at 12:00:04 can stitch together a picture of your business that never actually existed. That isn’t a rounding error. That’s a wrong answer, and an agent will act on it before anyone notices. Iceberg gives you snapshot isolation per table. It does not, on its own, hand you a consistent multi-table snapshot across asynchronous writers. We answer that question by construction: one engine, one MVCC snapshot, every table in the join telling the same story.


## **Is Lakebase data true, spec-compliant Iceberg that any engine can read?**


Reynold made “ecosystem” a centerpiece of the HTAP critique that the old systems were proprietary and nobody else could read their data. Fair. So let’s hold the new architecture to the same bar. The promise is that Lakebase data is open and any reader that understands Iceberg or Delta can consume it. The thing I’d test, before I believed it, is the hard version: is the output fully spec-compliant Iceberg correct manifests, partition specs, v2/v3 delete files, schema evolution that Trino, Flink, Spark, DuckDB, or Snowflake can read at full fidelity, including the live, mutating tables with their deletes and consistent snapshots? Or is it Iceberg-flavored, and really only happy inside Databricks? “We emit Iceberg” and “any engine can correctly read our changing tables” are two different promises, and only the second one is the one you actually need. For the record, SingleStore speaks MySQL wire and ingests and egress open Iceberg and Parquet the ecosystem critique cuts in directions worth thinking about.


## **What’s the role of Unity Catalog, and what happens to everyone else’s?**


Unity Catalog is doing two jobs at once. It’s the governance plane identity, permissions, lineage, audit and it’s the technical catalog that tracks table metadata and snapshots. Inside Databricks, that unification is a real strength. The enterprise reality is messier. Almost nobody standardizes on one catalog. They have AWS Glue, Hive Metastore, Snowflake’s Polaris and Horizon, and a few homegrown things nobody wants to touch. So the question I’d actually ask isn’t “do you support the Iceberg REST catalog.” It’s “does my non-Databricks engine get the same security and the same consistent view, on the same data, that your engine gets?” If the answer is no, then you have open formats with a closed front door, and you deserve to know that going in, before it’s load-bearing in your architecture.


## **The two architectures, side by side**


**What matters** **SingleStore** **Databricks LTAP + Lakehouse//RT**


**Engine model** One engine: OLTP + OLAP + real-time Three engines: Postgres + Spark/Photon + Reyden


**Data copies** One (Universal Storage) One open copy + row/columnar representations


**Landing-to-query gap** None — write is queryable on commit Row→columnar conversion; a freshness window


**Updates / deletes** In-place, first-class Lakehouse//RT launched read-only


**Multi-table join** One MVCC snapshot across all tables Per-table snapshots; cross-table coordination unclear


**Multi-model** Relational, JSON, vector, full-text, time-series Primarily relational on the lake


**Open / ecosystem** MySQL wire; ingest/egress Iceberg/Parquet Native Delta/Iceberg + Unity Catalog (their strength)


**Maturity** GA, 10+ years in production Lakebase GA; Lakehouse//RT Beta; LTAP “coming soon”


*Reyden is fast and their governance story is real — those two rows are theirs. Every row that decides a real-time application — writes, freshness, a join that tells the truth — is ours.*


## **One last thing**


Databricks made a lot of noise this week, and some of it was earned. But noise isn’t a unified engine, and a new acronym isn’t a solved problem. I keep coming back to the contradiction at the center of the keynote. You can call HTAP a failure, or you can spend twenty minutes explaining why the world needs exactly what HTAP set out to do. You can’t do both and expect the rest of us not to notice. Solving this isn’t bolting three engines onto shared storage and hoping the seams stay hidden. It’s one copy, one engine, no seams. A write you can query the instant it lands. A join that’s always telling you the truth. A platform your team doesn’t have to hold together with both hands and a prayer. Those aren’t taglines to me. They’re the things we’ve built, supported, and yes, gotten paged for, for ten years.


And to every engineer who has spent a weekend nursing the CDC pipeline that holds two databases together so the rest of the company never had to think about it: I see you. I’ve been you. This whole conversation is, in the end, about giving you that weekend back. I’m glad the rest of the industry is finally in the room. Now let’s ask the real questions, out loud, and build the thing properly.


The need for HTAP or LTAP is absolutely real in this agentic world and time is NOW. One thing I'd ask of all of us, Databricks, Snowflake, SingleStore, everyone building in this space: let's collaborate & hold the architecture to the standard we're claiming. Open table formats are a genuine step forward. But Iceberg or Delta on disk aren't enough if the control plane, the catalog, and the security layer are all proprietary. In the agentic era, data has to be genuinely portable, readable and writable by any engine that earns the customer's trust, not Iceberg-flavored behind a closed front door. Agents will operate across systems in ways none of us can fully predict, and the right response to that isn't higher walls around your preferred runtime. I am very keen to work with the brilliance of Databricks, Snowflake and others and let’s do the right thing for the customers. Let's go!


## On this page


- The convergence is real, and AI is what made it urgent
- About that “HTAP failed” claim
- Shared storage is not the same as a unified engine
- Where we stand, and why
- One copy of data, for transactions and analytics
- No gap between data landing and being queryable
- Multi-model, in the same engine
- Four questions I’d ask before calling it solved
- Is Reyden append-only, or does it really handle updates and deletes?
- What’s the consistency guarantee across a multi-table join?
- Is Lakebase data true, spec-compliant Iceberg that any engine can read?
- What’s the role of Unity Catalog, and what happens to everyone else’s?
- The two architectures, side by side
- One last thing


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fone-copy-one-engine-no-seams%2F)


Last modified - June 22nd, 2026


[Engineering](https://www.singlestore.com/blog/category/engineering/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog Real-Time Data Convergence: The Architecture Hi-Tech Teams Are Building for Agentic AI Engineering](https://www.singlestore.com/blog/real-time-data-convergence-architecture-agentic-ai-hitech/)


[Blog Why SingleStore Is a High-Performance Analytics Database for Financial Services Engineering](https://www.singlestore.com/blog/why-singlestore-is-a-high-performance-analytics-database-for-financial-services/)


[Blog Trust Over Passwords: Mutual TLS in SingleStore Engineering](https://www.singlestore.com/blog/trust-over-passwords-mutual-tls-in-singlestore/)


[Blog Why AI Search Needs More Than Vectors Engineering](https://www.singlestore.com/blog/why-ai-search-needs-more-than-vectors/)


[Blog How Database Indexing Techniques Impact AI Workloads Engineering](https://www.singlestore.com/blog/how-database-indexing-techniques-impact-ai-workloads/)
