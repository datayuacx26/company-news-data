---
schema_version: "1.0.0"
document_id: "cb35d2077c05fe693444863534daf7038cebdaffe5a74c90218f26f66eac28dc"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/the-man-who-named-cassandra-then-replaced-it-the-origin-story-of-yugabytedb"
published_at: "2026-03-23T14:33:10+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:73ca8fc5e5f42e1cc460ac86319e023c7edb360ff379cc32d237dd515c25cc56"
---

# The Man Who Named Cassandra, Then Replaced It: The Origin Story of YugabyteDB

## A Cassandra Engineer Walks Out of Facebook


### Sunnyvale, California. 2013. A Cassandra Engineer Walks Out of Facebook.


The thing about working on a database at Facebook's scale is that you stop thinking about what databases *can* do and start thinking, obsessively, about what they *can't* .


Karthik Ranganathan had been inside the machine since 2007. When he arrived, Facebook had roughly 30 to 40 million monthly active users. He thought it might double. By the time he left six years later, the number had crossed one billion. A 25x miscalculation that compressed the kind of distributed systems learning that takes most engineers a career into a handful of years.


He had been the third or fourth person to touch Cassandra's codebase before it was open-sourced — before it was even called Cassandra. He had, in a detail that almost nobody knows, given the database its name. And in the years that followed, as Cassandra was adopted by Apple, Netflix, and eBay and became one of the most widely deployed open-source databases in history, Karthik had watched its fundamental design limitation become the single most repeated frustration in his working life.


The limitation wasn't performance. It wasn't scale. It was a conversation that kept happening.


A product engineer would walk up and ask for something reasonable. A transaction that touched multiple rows. A secondary index that worked reliably under concurrent writes. A consistent read across nodes. And the infrastructure team would have to say: *Cassandra doesn't do that.*


Not "Cassandra doesn't do that yet." Not "Cassandra doesn't do that efficiently." Just: *Cassandra doesn't do that. That's not what it's for.*


Cassandra had been built on a deliberate trade-off: sacrifice relational guarantees in exchange for horizontal scale. The CAP theorem said you couldn't have both consistency and availability under network partitions — so Cassandra chose availability, accepted eventual consistency, and told applications to deal with the consequences. In 2008, that trade-off was defensible. By 2013, it was becoming the bottleneck.


The gap between what distributed databases *could* do and what applications *needed* them to do — ACID transactions, SQL semantics, secondary indexes, joins, consistent multi-row operations — was growing every year. That gap was, eventually, the company.


---


## Who Are These Three Engineers?


### The Man Who Named Cassandra


Karthik Ranganathan is the kind of engineer who doesn't appear on magazine covers. He runs on a different frequency — detail-oriented, architecturally precise, the person in the room who has already thought through three failure modes before the meeting starts.


At Facebook, he worked on the Cassandra team at a moment when the database was being pushed into workloads nobody had designed it for. He was deep in the code. He knew where the bodies were buried. He knew which guarantees Cassandra had compromised in the name of scalability and which of those compromises were permanent architectural decisions rather than temporary limitations.


After Facebook, he did something counterintuitive: instead of immediately founding a startup, he joined Nutanix. He spent two years there working on distributed storage and hyperconverged infrastructure, watching a company scale from small to enormous, learning what building enterprise infrastructure from the inside actually looked like. Nutanix IPO'd. Karthik left having understood, experientially, the gap between building something technically correct and building something the enterprise market will actually buy.


### The Infrastructure Force Multiplier


Kannan Muthukkaruppan came to Facebook with a background that ran from Oracle's database internals to large-scale distributed systems. At Facebook, he led the NoSQL infrastructure initiatives — the layer powering Facebook Messenger, the Operational Data Store, Site Integrity. He was a force multiplier on both Cassandra and HBase, the person who knew how to take academic distributed systems concepts and make them survive contact with production at billions of operations per day.


Kannan also moved through Nutanix after Facebook — working on distributed and hybrid cloud data fabric — before the three engineers found their way back to each other. The Nutanix years were not a detour. They were a calibration: learn what real enterprise scale looks like when you're not inside the most infrastructure-sophisticated company on earth.


### The HBase Architect


Mikhail Bautin held a PhD in Computer Science from Stony Brook University. At Facebook, he was deep in Apache HBase — contributing significant changes to the open-source trunk, earning committer status, running the system for Facebook Messages and Search Indexing. He understood, at a code level, how HBase's architectural choices differed from Cassandra's and where each system broke down under pressure.


Bautin was the quietest of the three founders — less visible externally, present in the architecture. He would spend eight years building YugabyteDB before leaving in November 2024 to join Google Cloud and work on Cloud SQL. The engineer who built the databases powering Facebook Messenger went back into big-company orbit, without a press release, without a farewell blog post. Just a LinkedIn update.


### The Name Itself


In Hindu cosmology, time is not linear. It moves in cycles — vast, recurring epochs called *Yugas* . The four Yugas together span 4.32 million years: the Satya Yuga, the Treta Yuga, the Dvapara Yuga, and the Kali Yuga (the one we're currently in — an age of strife and discord, if you were wondering). A single Yuga is not a unit of measurement. It is a unit of civilizational time.


The founders chose this word deliberately. They combined it with "byte" — the fundamental unit of digital information. *Yugabyte* : data measured against geological time. Data that should outlast the infrastructure it lives on.


It was a quiet act of philosophical positioning disguised as branding. Everyone in the database industry talked about data in terms of *size* — gigabytes, terabytes, petabytes. These three were asking a different question: what about *permanence* ? What about data that needs to survive not just this year's infrastructure choices but the next decade of architectural shifts?


**In February 2016, Karthik Ranganathan, Kannan Muthukkaruppan, and Mikhail Bautin incorporated Yugabyte.**


The product vision had three pillars: high performance, *all* of SQL (not a subset), and geographic distribution without operational nightmare. Nothing in the market offered all three simultaneously.


---


## The Technical Bets That Defined Everything


### The Wrong Path First


The team's first architectural instinct was to build their own PostgreSQL-compatible query layer from scratch. Write something new that looked like Postgres from the outside but was entirely their own code underneath. Full control. Clean slate. No dependencies on upstream Postgres releases.


They tried it. They got deep into it. They wrote real code.


And then they ran into the wall that every team that has tried this eventually runs into: the PostgreSQL query layer — the parser, the planner, the optimizer, the executor, the catalog system, the locking machinery — is the product of decades of engineering by hundreds of contributors across more than thirty years. It is not a component you can reproduce in a startup timeline. It is a civilization.


Rewriting it was not a shortcut. It was a trap.


The pivot was counterintuitive: instead of replacing PostgreSQL's query layer, they would *use* it. Take vanilla Postgres as-is for the SQL processing layer and replace only the storage layer — substitute Postgres's local storage engine with a distributed storage engine of their own design. When an application connects to a YugabyteDB node, it is literally connecting to a Postgres postmaster process. The SQL is processed by a Postgres backend process. The error codes are Postgres error codes. The catalog semantics are Postgres catalog semantics.


Behind that backend, instead of writing to a local disk, the data moves across a distributed cluster.


Karthik would later call this architectural decision "one of the best design decisions made in YugabyteDB." It was also born out of necessity, not purity — a detail the company's marketing has naturally chosen not to emphasize.


### DocDB: Spanner's Ghost in Open-Source Clothing


The distributed storage layer they built is called DocDB. It is, explicitly and without embarrassment, inspired by Google Spanner.


The lineage runs like this: DocDB is built on top of RocksDB — Facebook's LSM-tree key-value store, itself a fork of Google's LevelDB. On top of RocksDB, Yugabyte added the consensus layer, the transaction machinery, and the distributed semantics that transform a fast local key-value store into something that can span multiple availability zones with ACID guarantees.


For consensus, they chose Raft rather than Paxos. Raft is less mathematically elegant than Paxos but meaningfully more understandable — and understandability is a practical advantage when you're debugging a distributed system at 3am. Each data shard maintains its own independent Raft group. Leaders handle writes and consistent reads. Followers maintain replicas.


The harder problem was time.


Google Spanner solves distributed consistency using TrueTime — GPS receivers and atomic clocks installed in every Google data center, measuring clock uncertainty down to single-digit milliseconds. TrueTime lets Spanner make globally consistent reads without coordination overhead because it can *bound* how wrong any clock might be.


Outside Google's infrastructure, you don't have atomic clocks. You have commodity servers with NTP synchronization and clocks that drift.


YugabyteDB's answer was Hybrid Logical Clocks (HLC) — a combination of physical timestamps and logical Raft operation counters that provide linearizable reads without requiring atomic clock hardware. HLC is a research concept from 2014, applied here to production distributed systems. The result: the same architectural guarantees as Spanner (strong consistency, distributed ACID transactions, multi-region replication) on infrastructure that anyone can rent from AWS or Google Cloud or Azure.


### The Dual API: One Database, Two Languages


YugabyteDB ships with two query APIs:


**YSQL** — a distributed SQL API built by reusing PostgreSQL's language layer code. Wire-format compatible with PostgreSQL. Any Postgres driver, ORM, framework, or tool connects to YSQL and works. Not "mostly works." Works — because the query process *is* a Postgres process.


**YCQL** — Cassandra Query Language, adapted for a strongly consistent database. Ports 9042 — the same default port as Cassandra. CQL syntax that Cassandra developers already know. Designed for workloads that think in terms of distributed data placement: IoT telemetry, time-series, messaging infrastructure, anything that previously went to Cassandra specifically for its operational model.


The dual API was a deliberate migration play. When a company runs Cassandra workloads and PostgreSQL workloads, they can migrate both to a single database. The NoSQL teams don't have to learn SQL. The SQL teams don't have to unlearn SQL. Both talk to the same storage layer, with the same ACID guarantees, with the same replication semantics.


The query layer was built with extensibility as its organizing principle — designed to allow new APIs to be added. YSQL and YCQL are ports 5433 and 9042 respectively, running concurrently on the same cluster.


### Stealth, Seed, and the Open-Source Declaration


The company ran in stealth until November 2, 2017, when Karthik published the announcement alongside an $8M seed round led by Lightspeed Venture Partners and Dell Technologies Capital.


In July 2019, Yugabyte made a decision that would define its competitive identity for years: they moved YugabyteDB to 100% Apache 2.0 licensing, open-sourcing previously commercial features including Distributed Backups, Data Encryption, and Read Replicas.


The timing was not accidental. One month earlier, CockroachDB had moved from Apache 2.0 to the Business Source License — a "source available" license that restricts commercial use of the database. Karthik publicly called the move "an example of short-term thinking that can stifle long-term growth."


The distributed SQL market had declared sides. Yugabyte chose the open-source side. CockroachDB chose the commercial license. The consequences of that divergence would compound over years.


---


## Volunteering for the Harshest Correctness Test in Distributed Systems


### What Jepsen Is and Why It Matters


Kyle Kingsbury runs a consultancy called Jepsen. His specialty is breaking distributed databases — finding the gap between what a database claims to guarantee and what it actually delivers under network partitions, clock skew, process crashes, and the full range of real-world failure modes.


Kingsbury's testing methodology, also called Jepsen, is the closest thing the database industry has to independent verification. His analyses have exposed critical safety violations in Redis, Cassandra, MongoDB, Riak, Etcd, VoltDB, and dozens of others. The reports are published in forensic detail — not vendor-marketing language but engineering precision about exactly what broke, exactly how, and under exactly which conditions.


A Jepsen report that says your database is correct is a credential. A Jepsen report that says your database has safety violations is a liability.


Almost every database company that has been Jepsen-tested did not voluntarily commission the analysis. Kingsbury found the problems first and published them, often to the embarrassment of the vendor. The companies that commission Jepsen tests in advance — and accept the public results, good or bad — are a much shorter list.


### YugabyteDB Commissioned Jepsen. Twice.


In March 2019, Yugabyte commissioned Jepsen to evaluate YugabyteDB 1.1.9. The results were brutal.


Kingsbury found three critical safety violations in the initial version. Under normal operating conditions — not failure scenarios, just *normal operation* — healthy clusters exhibited frequent read skew. In the bank transfer simulation, account balances that should have remained constant at $100 were drifting to $85, $102, $180. The numbers were wrong. Not occasionally wrong. Routinely wrong. A race condition in multi-shard transactions was causing transactions to see partial effects of other concurrent transactions — exactly the consistency guarantee the database was supposed to provide.


There were also rare lost writes during network partitions: acknowledged inserts that simply vanished after cluster recovery. A bug in how Raft quorum membership was counted was letting commit majorities form without certain writes. And under large clock offsets, transactions were producing histories that "should never have existed" — corrupted state propagating through the system.


The report was published. Publicly. In full.


Yugabyte fixed the issues and commissioned a second Jepsen test in September 2019, this time focused on YSQL — their PostgreSQL-compatible interface — and the new serializable transaction support they were preparing to release. The second report found additional issues: columns with DEFAULT values could initialize to NULL due to non-transactional schema changes, and G2-item anti-dependency cycles were occurring when masters crashed or paused.


Yugabyte patched those too. Deployed 1.3.1.2-b1 to address the G2-item issue specifically.


The version 1.2.0 outcome from the first round of testing passed for snapshot isolation, linearizable counters, sets, and registers, with synchronized clocks. Not perfect. But verified.


The Jepsen story is not a story about bugs. Every database has bugs. The story is about what you do with the findings. Most database vendors, when presented with correctness failures, minimize them, dispute Kingsbury's methodology, or quietly patch without acknowledgment. Yugabyte commissioned the tests, published the results, fixed the issues in public view, and commissioned a second round. This is unusual behavior in an industry that treats correctness claims as marketing assets.


---


## The Distributed SQL Wars, the Cloud Product, and the Unicorn


### The Funding Trajectory


Round Date Amount Key Investors


Seed Nov 2017 $8M Lightspeed Venture Partners, Dell Technologies Capital


Series A Jun 2018 $16M Lightspeed, Dell Technologies Capital


Series B Jun 2020 $30M 8VC, Wipro Ventures, Lightspeed, Dell Technologies Capital


Series B Extension Mar 2021 $48M Lightspeed, Greenspring Associates


Series C Oct 2021 $188M Sapphire Ventures, Alkeon Capital, Meritech Capital, Wells Fargo Strategic Capital


Total raised: $291M. Valuation at Series C: **$1.3 billion.** YugabyteDB became a unicorn five years after founding.


Wells Fargo showing up as a Series C investor is notable: it's the most visible signal that the financial services sector — the industry with the strictest uptime and consistency requirements in commercial software — had decided that distributed SQL was real infrastructure, not a technology bet.


### The Cloud Product


In 2021, Yugabyte launched YugabyteDB Aeon (previously called Yugabyte Cloud) — a fully managed database-as-a-service platform running on AWS, Google Cloud, and Azure. The managed offering represents the monetization layer: the open-source database builds community and credibility; the cloud service generates revenue.


The architecture of the business mirrors the architecture of the database: the open-source core is the consistency guarantee, and the cloud service is the availability layer on top.


### The CEO Interlude


In May 2020, Yugabyte brought in Bill Cook — former President of Pivotal Software — as CEO, with Kannan shifting to President of Product Development. The move freed Karthik and Kannan to focus on engineering and product. By 2021, the co-CEO structure was restored, reflecting the founders' preference for a technical leadership model.


### The Distributed SQL Wars: Three Horses, One Category


The space YugabyteDB competes in is called "distributed SQL." The term was coined — or at least popularized — partly by Yugabyte itself in 2019 as a way to describe a new category: databases that offer full SQL semantics plus horizontal scalability plus strong consistency, as opposed to the NoSQL systems that sacrificed one for the other.


The three-horse race looks like this:


**YugabyteDB** — PostgreSQL-native, 100% Apache 2.0, hash-partitioned by default, built for write-heavy workloads across multiple availability zones, strong at operational OLTP across global deployments.


**CockroachDB** — Built from scratch, BSL licensed (with commercial restrictions), range-partitioned, PostgreSQL-compatible at the wire level but not at the code level, strong developer experience, larger presence in US-based fintech and gaming.


**TiDB** — MySQL-compatible, open source (Apache 2.0), strong Asia-Pacific presence, built by PingCAP in Beijing and backed by Sequoia Asia and IDG Capital, unique in supporting both transactional and analytical queries (HTAP) from the same storage layer.


The benchmark wars between Yugabyte and CockroachDB are the most visible point of friction. Yugabyte publishes comparisons claiming 3x higher throughput and 4.5x lower latency on YCSB tests. CockroachDB responds. Both companies maintain competitive comparison pages written in the voice of dispassionate analysis with the energy of a cold war.


The deeper competitive difference is licensing. Yugabyte's open-source bet means the community can run the full database — every feature, every enterprise capability — without a commercial license. CockroachDB's BSL means commercial use beyond a certain scale requires a paid agreement. For companies building infrastructure that will outlast the current vendor landscape, the licensing choice is not a secondary detail.


### Who Actually Runs on YugabyteDB


The adoption pattern is less startup-ecosystem and more industrial-grade:


-


**Plume** — 27 billion daily operations powering millions of smart homes across ISPs worldwide, running on a 60-node YugabyteDB cluster on AWS


-


**Kroger** — the largest supermarket chain in the US uses YugabyteDB for critical retail workloads


-


**Hudson River Trading** — the quantitative trading firm uses it for low-latency financial systems


-


**Paramount+** — streaming infrastructure


-


**Wells Fargo** — financial services (also a Series C investor, which says something about confidence levels)


-


**Shopify** — re-architecting for agentic commerce using YugabyteDB at the foundation


The pattern is consistent: companies with strict uptime requirements, global user bases, and transaction-heavy workloads that have outgrown single-region PostgreSQL but cannot afford to abandon SQL semantics. Not experimental deployments. Not proof-of-concepts. Core infrastructure.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## Five Things Most People Don't Know About YugabyteDB


**1. Karthik Ranganathan named Cassandra.**
Before Cassandra was open-sourced, before it was called Cassandra, before it was anything meaningful to the world, Karthik was the third or fourth person to touch the codebase at Facebook. He gave it the name that would go on to become one of the most widely-deployed open-source databases in history — used at Apple, Netflix, eBay, Uber. He then spent a decade building the system designed to transcend its limitations. The man who named one of the most famous databases in the world left to build a better one.


**2. They tried to build their own PostgreSQL query layer first — and abandoned it.**
The public narrative is that YugabyteDB reuses PostgreSQL's query layer by elegant design choice. The real narrative is that they tried to write their own from scratch, got deep enough to understand how unrealistic the timeline was, and pivoted to reuse. The "brilliant" architectural decision to embed vanilla Postgres at the query layer was born from hitting the wall of complexity, not from first-principles genius. The insight was real. The origin was humbling.


**3. "Yuga" is Sanskrit for a civilizational epoch spanning millions of years.**
In Hindu cosmology, the four Yugas together span 4.32 million years. The founders picked this word deliberately, not as a vague reference to "era" but as a specific philosophical statement: while every database vendor was talking about data in terms of *size* , the Yugabyte founders were asking about data in terms of *time* . How long should your data infrastructure last? Their answer, embedded in the name, was: longer than your current architectural decisions. Data measured against geological epochs, not product cycles.


**4. They voluntarily commissioned Jepsen tests — twice — and published the results publicly, including the failures.**
Most database vendors have Jepsen tests done *to* them, without consent, after which they either dispute the methodology or quietly patch the issues. Yugabyte commissioned Jepsen themselves in 2019, discovered three critical safety violations (including read skew under normal healthy cluster conditions), fixed them, then commissioned a second round on their YSQL serializable transaction support. Both sets of findings were published in full. The decision to fund and publicize an adversarial correctness test — including the parts where they failed — is genuinely unusual vendor behavior in an industry that treats correctness claims as marketing assets.


**5. The third co-founder quietly left in 2024 — to go work at Google Cloud, on a competing database product.**
Mikhail Bautin, the PhD engineer who built HBase infrastructure for Facebook Messenger, co-founded Yugabyte, and helped architect DocDB from scratch, departed in November 2024 to work on Cloud SQL at Google. No press release. No departure post. No public acknowledgment from the company. The man who spent eight years building an alternative to cloud-vendor database lock-in went to work inside one of the largest cloud database vendors on earth. There is probably a very honest internal story behind that decision. None of it is public.


---


## YugabyteDB by the Numbers


-


**Founded:** February 2016


-


**Founders:** Karthik Ranganathan (Co-CEO), Kannan Muthukkaruppan (Co-CEO), Mikhail Bautin (Software Architect, departed 2024)


-


**All three:** Former Facebook engineers from the Cassandra and HBase infrastructure teams


-


**Prior employment:** All three passed through Nutanix after Facebook before founding Yugabyte


-


**Name origin:** "Yuga" (Sanskrit for civilizational epoch) + "byte" (unit of data)


-


**Founding moment:** The daily frustration of telling product engineers that Cassandra couldn't do transactions, joins, or consistent multi-row reads


-


**Core product:** YugabyteDB — distributed SQL database supporting YSQL (PostgreSQL-compatible) and YCQL (Cassandra Query Language)


-


**Storage layer:** DocDB — built on RocksDB, with Raft consensus and HybridTime (Hybrid Logical Clocks)


-


**Google Spanner influence:** Explicit and acknowledged — Raft replaces Paxos, HLC replaces TrueTime


-


**Open source:** 100% Apache 2.0 since July 2019


-


**Unicorn:** October 2021, $1.3B valuation


-


**Total funding:** $291M


-


**Jepsen tested:** March 2019 (v1.1.9) and September 2019 (v1.3.1) — both commissioned by Yugabyte


-


**Cloud product:** YugabyteDB Aeon (managed DBaaS on AWS/GCP/Azure)


-


**Key customers:** Plume, Kroger, Hudson River Trading, Paramount+, Wells Fargo, Shopify


-


**Direct competitor:** CockroachDB (range-partitioned, BSL licensed, scratch-built) — benchmark wars ongoing


---


*Sources: Yugabyte About page, YugabyteDB architecture documentation (docs.yugabyte.com), Jepsen analyses of YugabyteDB 1.1.9 (March 2019) and 1.3.1 (September 2019), jepsen.io/analyses, YugabyteDB query layer and DocDB documentation, YugabyteDB replication architecture docs citing Google Spanner influence, Unite.ai Karthik Ranganathan interview, Percona podcast with founders, BusinessWire Series C announcement, Yugabyte blog on Apache 2.0 licensing transition.*
