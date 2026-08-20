---
schema_version: "1.0.0"
document_id: "ae06703309cc64b7f1168a678776801a3dce1168c1010f24842654f28d51e8af"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/the-broken-ui-filter-the-origin-story-of-timescaledb"
published_at: "2026-03-23T14:32:44+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:ed61d4605951f5cebc4ce7eaf96bc279b6eab694bf44795eeddcb138e5d65876"
---

# The Broken UI Filter: The Origin Story of TimescaleDB

## The Filter That Cost Them Their Company


Somewhere in a New York office in early 2016, two engineers sat in front of a product requirements document and had a realization that would cost them their company — and birth a better one.


They were building Iobeam, an IoT data platform. The pitch was straightforward: industrial sensors generate enormous streams of time-stamped readings, and most companies have no infrastructure to handle it. Iobeam would be the infrastructure layer. They had funding, a team, customers to talk to.


And then a product manager wrote down a simple UI feature: let users filter their connected devices by` device_type` AND by` uptime` . A dropdown. A filter. The kind of thing a junior developer builds in an afternoon.


The engineering estimate came back: weeks. Maybe a full sprint.


The reason was embarrassing to explain. Iobeam had been built on two separate databases — one relational database for device metadata, one time-series database for sensor readings. The metadata lived in one system. The uptime data lived in another. To build a single filter that touched both, you needed to write custom application-layer glue code to JOIN across two completely different query languages, two different connection pools, two different failure modes. Every new feature that crossed the boundary between "device context" and "sensor data" was a week-long engineering project.


An engineer on the team said something out loud that day. Simple. Almost throwaway: *What if we just put everything in PostgreSQL?*


What if instead of managing two databases, they extended PostgreSQL — the database they already ran, the database their team already knew, the database every tooling vendor in the world already supported — to handle time-series data natively?


Nobody had done this for time-series data before. Most people in the industry thought it was a bad idea.


They built it anyway.


The IoT startup didn't survive. The database they built to save it raised $180 million and became one of the most influential database projects of the past decade. The company that built it eventually renamed itself after a tiger.


---


## Two MIT Freshmen, Eighteen Years Later


### September 1997: Room 312, MIT


Ajay Kulkarni and Mike Freedman met during freshman week at MIT in September 1997. They were eighteen. They became roommates. In 1998 — their freshman year — they ran the Boston Marathon together.


For most of the next eighteen years, they built things separately.


### Mike Freedman: The Academic Who Never Left


Mike Freedman went deep into academia in a way that is unusual even among serious computer scientists. After his bachelor's and master's degrees at MIT, he completed his PhD at NYU's Courant Institute of Mathematical Sciences in 2007. His dissertation examined content delivery networks and peer-to-peer distributed systems — consistent hashing, distributed lookup protocols, the mathematics of getting data from one machine to many others efficiently. He was not a database researcher. He was a systems thinker: how do you build distributed infrastructure that doesn't fall apart under real-world conditions?


His academic trajectory after NYU was steep. He joined Princeton University as a faculty member and eventually became the **Robert E. Kahn Professor of Computer Science** — a named endowed chair. Named chairs at Princeton are not given to people who publish occasionally. They are among the highest distinctions in academic computer science, carrying the name of Robert Kahn, one of the architects of TCP/IP and the modern internet.


Freedman earned ACM Fellowship, PECASE designation (the highest honor the US federal government gives to early-career scientists), and multiple Test of Time Awards from top computing conferences. Before TimescaleDB, he had already founded or co-founded companies alongside his faculty role: CoralCDN, a decentralized content delivery network that predated modern CDN architecture; Ethane, whose ideas became foundational to OpenFlow and software-defined networking; and Illuminics Systems, an IP intelligence company acquired by Quova, which was later acquired by Neustar.


Mike Freedman was not a researcher who had never shipped. He was a serial founder who had never left Princeton.


He was also, by his own admission in a 2026 blog post, someone who had watched the PostgreSQL ecosystem for years and believed something that the industry hadn't fully accepted yet: that Postgres wasn't just a reliable general-purpose database. It was a platform. A foundation you could build entire categories on top of.


### Ajay Kulkarni: The IoT Engineer Before IoT Was a Word


Kulkarni's trajectory was less academic but no less prescient. While Freedman was building distributed systems theory at Princeton, Kulkarni was building intelligent environments at MIT's Media Lab and adjacent research groups. In 2002, he wrote a thesis titled *"A Reactive Behavioral System for the Intelligent Room"* — a context-aware system for natural human-computer interaction in smart spaces. He was building IoT applications in 2002. The term IoT would not enter mainstream usage for another decade.


After MIT, Kulkarni attended MIT Sloan School of Management from 2006 to 2008. His career after Sloan traced through ambient computing, smart devices, connected hardware — the early fragmented world of connected sensors before anyone had agreed on protocols, standards, or what to call the category.


By 2015, when he and Freedman co-founded Timescale, Kulkarni's LinkedIn bio included the line: "into Internet of Things before it was cool." It was accurate. He had spent thirteen years in that world. He knew exactly what broke when you tried to build IoT applications at scale.


The thing that broke was always the database.


### The Iobeam Years: When Everything Failed


The founding premise of Iobeam was sound. Industrial operators — factories, utilities, logistics companies — had thousands of sensors generating continuous data. None of their existing database infrastructure was designed for this. You'd end up with one database for the relational context (which machine, which location, which operator, which product line) and a completely different database for the time-series readings (temperature at 14:32:07, vibration at 14:32:08, pressure at 14:32:09).


Iobeam built a data platform to bridge this gap. But to bridge the gap, they had to live in it. And living in the gap between two databases meant that every interesting product feature — anything that crossed the boundary between context and measurement — required engineering glue code that was painful to write, painful to maintain, and painful to debug.


The breaking moment was that UI filter. But it was also every feature before it and every feature they could see ahead of them. The two-database architecture was not a technical debt problem they could pay down over time. It was a structural tax on every new feature they would ever build.


One engineer's question — *what if we just put everything in PostgreSQL?* — was not a naive question. It was a diagnosis. The actual product they needed didn't exist. So they built it themselves.


---


## Building on a 35-Year-Old Database


### The Counterintuitive Choice


In 2015, the consensus in the database industry was pointing in one direction: purpose-built is the future. MongoDB was becoming ubiquitous. Cassandra was what serious scale looked like. InfluxDB, founded in 2013, was building a time-series database from scratch in Go, designing every layer of the stack specifically for time-series workloads. The narrative was that general-purpose relational databases like PostgreSQL were the past. Specialized systems were the future.


Kulkarni and Freedman went the other way.


Their reasoning was not contrarian for its own sake. It was practical. PostgreSQL had thirty-five years of engineering in it: thirty-five years of careful transaction handling, thirty-five years of query planner optimization, thirty-five years of reliability engineering that every cloud provider, every ORM, every BI tool, every backup system in the world already knew how to operate. The ecosystem around PostgreSQL wasn't a feature. It was an asset class.


And PostgreSQL had a secret that not everyone had noticed: it was deeply, genuinely extensible. The extension API allowed external code to add custom data types, custom operators, custom index access methods, and custom storage strategies — all without touching the core database codebase. PostGIS had demonstrated this as early as 2001: a geospatial extension that turned PostgreSQL into a world-class Geographic Information System. The PostGIS bet proved that PostgreSQL extensions could add entirely new capabilities that outperformed purpose-built alternatives.


The bet Kulkarni and Freedman were making: we can do for time-series what PostGIS did for geospatial. We can extend PostgreSQL to handle time-stamped data at scale without replacing any of the infrastructure that already works.


This was the bet that the top HackerNews comment on their 2017 launch called "a rather bad idea."


### The Technical Problem Nobody Had Solved


Standard PostgreSQL fails at time-series workloads in a specific, predictable way. The core issue is table bloat. Every row in a standard PostgreSQL table gets indexed together. As the table grows, every insert requires updating increasingly large indexes. Every query requires scanning increasingly large page ranges. At tens of millions of rows, the degradation is tolerable. At hundreds of millions of rows, it becomes painful. At tens of billions of rows — which IoT deployments routinely reach — it becomes catastrophic.


TimescaleDB's core innovation was **hypertables** : a PostgreSQL table that looks completely normal from the outside — same SQL syntax, same query patterns, same tooling compatibility — but is automatically partitioned into time-based "chunks" behind the scenes. Each chunk covers a specific time window (one day, one week, configurable). Each chunk is its own physical table with its own indexes. When you query for last week's data, the query planner touches only the chunks that contain that time range. Old chunks can be compressed independently as they age. The index for each chunk stays small and fast.


The hypertable abstraction was the core insight: make time-series data management invisible to the application. You write SQL. You query SQL. The database does the hard work of keeping your data performant at any scale.


On top of hypertables, they added a series of innovations that compounded the advantage:


**Columnar compression** achieving 90%+ storage reduction on older time chunks. A dataset that would take 350 GB in raw form compresses to 3 GB — a compression ratio that changes the economics of long-term time-series retention entirely.


**Continuous Aggregates** : the innovation that made real-time dashboards over billions of rows practical. A continuous aggregate is an incremental materialized view that refreshes only the data that has changed since the last refresh. Where a standard PostgreSQL materialized view requires rescanning the entire dataset on each refresh, a continuous aggregate tracks only newly completed time buckets. The result: query response times of 18 milliseconds on aggregations that would take 15 seconds on a regular view. Approximately 1,000 times faster — on the same underlying data.


**Hyperfunctions** : SQL extensions that make complex time-series analytics expressible in pure SQL without custom application code. Functions for approximate counts, percentiles, statistical aggregations, and time-bucket operations that would otherwise require pulling data into application memory and computing manually.


**Direct columnar insert** : data that writes directly to compressed columnar format on ingestion, skipping the intermediate row-store staging step that was a performance bottleneck in earlier versions.


### The Launch — April 4, 2017


They open-sourced TimescaleDB on April 4, 2017. They posted to Hacker News.


The top comment was not congratulatory: *"While I appreciate PostgreSQL every day, am I the only one who thinks this is a rather bad idea?"*


The comment was not wrong to be skeptical. The idea seemed to violate the consensus. Time-series databases needed to be purpose-built, or so the story went. You couldn't retrofit a 35-year-old general-purpose relational database into a high-performance time-series system. You couldn't compete with InfluxDB, which had been designed from the first commit specifically for this workload.


The comment became their unofficial origin myth. Building on PostgreSQL was "a rather bad idea." That skepticism pushed them to prove the alternative.


### The Open Source Strategy


From the beginning, open source was not a marketing decision. It was an architectural one. Kulkarni and Freedman had noticed that adoption in the database world followed a specific pattern: developers try it for free, put it in production, and eventually pay for the managed version. You don't pay for the database. You pay for not having to run the database yourself.


The open-source approach also forced technical discipline. If your database was open and inspectable, every performance claim had to survive public scrutiny. Every optimization had to be real. The community that forms around an open-source project becomes both your best beta testing environment and your most credible marketing channel.


They launched open source. The community grew. The claims survived scrutiny.


---


## When the Postgres Bet Started Winning


### The InfluxDB Rivalry in Concrete Numbers


The rivalry between TimescaleDB and InfluxDB was never primarily a marketing war. It was an architectural argument, and the argument eventually got answered with data.


InfluxDB was built from scratch in Go, with a custom query language (InfluxQL, later replaced by Flux, later replaced again by SQL — a migration odyssey that became its own cautionary tale). InfluxDB used a tagset data model where tags were indexed and fields were not, restricting the types of queries you could efficiently run. At low cardinality — a few dozen devices — InfluxDB was slightly faster. At high cardinality — millions of unique devices or measurement streams — TimescaleDB's B-tree indexing on per-chunk partitions scaled in ways InfluxDB's architecture could not match.


The Plexigrid case study became the canonical demonstration. Plexigrid, a smart grid energy company, had built their infrastructure on four separate databases: InfluxDB for time-series sensor data, TigerGraph for graph relationships, MySQL for operational data, and PostgreSQL for relational queries. When they consolidated onto TimescaleDB, the results were:


-


**350x faster queries** : analytical workloads that took 5 minutes dropped to under 0.5 seconds


-


**95% storage reduction** : 350 GB compressed to 3 GB


-


**Memory efficiency** : InfluxDB required approximately 8 GB RAM for a dataset that TimescaleDB handled in 300 MB


More significant than any benchmark: they went from four databases to one. The glue code problem — the original problem Kulkarni and Freedman had encountered at Iobeam — disappeared entirely.


### The Two Waves That Arrived Simultaneously


TimescaleDB's timing was fortunate in a specific way. Two massive technology trends converged in 2017-2018, and both of them needed exactly what TimescaleDB provided.


The **industrial IoT wave** : factories were instrumenting everything. Smart grids. Logistics networks. Connected vehicles. Every physical process in the world was beginning to emit continuous sensor streams, and every one of those streams needed to be stored, queried, and correlated with context data about the things being monitored. The "dual query problem" — deep time queries for individual sensor history, wide context queries across many sensors simultaneously — was universal across IoT. TimescaleDB's architecture was designed exactly for this.


The **DevOps observability wave** : Kubernetes adoption was exploding. Every microservice produced metrics. Every container needed to be monitored. CPU usage, memory consumption, request latency, error rates — all time-stamped data, all needing dashboards and alerts. Tools like Grafana and Prometheus were building ecosystems around time-series storage, and PostgreSQL compatibility meant TimescaleDB worked with all of them immediately. The same extension that served factory floors served SRE teams.


The customer portfolio that accumulated over the next six years reflected both waves simultaneously: industrial companies (Bosch, Caterpillar, Schneider Electric), technology companies (Cisco, Akamai, Nvidia), financial firms (JP Morgan Chase, NYSE), automotive (Tesla, Toyota, Lucid Motors), AI labs (OpenAI, HuggingFace, Mistral), and space agencies (NASA). This is not a coherent industry vertical. It is the profile of a horizontal infrastructure tool — a database — that works for any domain where things happen over time and need to be measured.


### The Postgres Bet Wins the Culture War


The most significant moment in TimescaleDB's history was not a product release or a funding announcement. It was a cultural shift in how the database industry thought about PostgreSQL.


In 2015, PostgreSQL was widely seen as a solid, reliable, "boring" database — the responsible choice for data that mattered, but not the frontier. NoSQL was the narrative. Purpose-built databases were the growth market. "Use Postgres" was advice you gave to people who didn't want to think too hard about their database.


By 2023, that narrative had inverted. PostgreSQL was the fastest-growing database in the world. The open-source extensions ecosystem — TimescaleDB for time-series, PostGIS for geospatial, pgvector for vector embeddings, Citus for distributed SQL — had turned PostgreSQL into a platform. The "boring" bet had won every category it competed in.


TimescaleDB didn't cause this shift. But it validated it early and contributed to it directly. When they launched in April 2017 and the top comment called it a bad idea, they were betting on an outcome the industry hadn't priced in yet. By 2023, the industry had come around.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## Cloud, Licensing, and a New Name


### The Funding History


Timescale raised $180 million in total: an initial seed round, a $31 million Series A from NEA in April 2018, a $40 million Series B led by Redpoint Ventures (with Benchmark, NEA, Icon Ventures, and Two Sigma Ventures participating) in 2021, and subsequent rounds that brought total capital to $180 million from investors including Tiger Global.


They went through Y Combinator's Winter 2017 batch. The YC network gave them early validation and — more practically — a framework for thinking about open-source monetization. The playbook was: open source the database, build a community, offer the managed cloud service to the companies that would rather pay than operate.


The Series A pitch in 2018 was essentially: every company is generating time-series data, and most of them have no idea how to handle it. The IoT wave, the monitoring wave, the financial data wave — all converging. The time-series database category was growing faster than any other database segment. NEA wrote the check.


### The Open-Source Licensing Evolution


TimescaleDB's licensing history is a case study in open-source business model evolution. The company started with Apache 2.0 for the core functionality and a proprietary Timescale License (TSL) for enterprise features — a model designed to prevent cloud providers from running TimescaleDB as a managed service without paying for the commercial license.


In 2023, they made an unusual move: full Apache 2.0. Every feature, including the enterprise ones, released under permissive open-source licensing. The reasoning was counterintuitive: a larger open-source community creates more enterprise prospects for the managed cloud product. Removing all friction to adoption would grow the community faster, and a larger community would create more enterprise customers even if each individual deal took longer to close.


The approach worked. Cloud revenue grew 5x in 18 months following the full open-source shift.


### The Cloud Product


Timescale Cloud — now Tiger Cloud after the 2024 rebrand — became the primary revenue engine. The managed offering includes:


-


Disaggregated compute and storage (scale each independently)


-


Automatic failover and multi-AZ clustering


-


Tiered storage: hot data on SSD, cold data on S3-compatible object storage, transparent to applications


-


Point-in-time recovery and cross-region backups


-


Compliance certifications: SOC 2, HIPAA, GDPR


-


Native pgvector support for AI and machine learning workloads


The AI workload angle became increasingly important from 2023 onward. PostgreSQL's support for vector embeddings via pgvector — combined with TimescaleDB's time-series capabilities — created a database that could handle both time-stamped sensor data and vector similarity search in a single system. For AI applications that needed to store embeddings alongside timestamped events, the combination was uniquely practical.


MarketReader, a fintech company, became a demonstration of the combined use case: ingesting 3 million trades per minute across approximately 26,000 securities, running vector search for semantic relevance in AI-generated market commentary, and storing all timestamped market data — all in a single TimescaleDB instance. No glue code. One system.


### The Rebrand — Timescale Becomes Tiger Data


In September 2024, Timescale rebranded to **Tiger Data** .


The reason wasn't marketing. It was customer feedback. Enterprise CTOs kept telling the team the same thing: "You keep positioning yourself as the best time-series database, but I see you as the best PostgreSQL platform."


The company had outgrown its founding label. They started as an IoT startup that built a time-series database extension. Ten years later, they were a PostgreSQL platform company that happened to start with time-series. The category they had been filed under — "time-series database" — had become too narrow for what they actually were.


At the time of the rebrand:
- 2,000+ paying customers across 25 countries
- Mid-8-digit annual recurring revenue, growing over 100% year-over-year
- 200 employees
- 60%+ gross margins
- $180 million raised from top-tier investors
- A cloud product growing 5x in 18 months


Mike Freedman — the Robert E. Kahn Professor of Computer Science who had maintained his Princeton faculty position while co-founding and CTOing a company through a decade of growth — became CTO of Tiger Data. He also updated his Princeton faculty page.


The "boring" bet had won.


---


## Five Things Most People Don't Know About TimescaleDB


**1. The company was born from a broken UI filter, not a grand vision.**


The founding moment was not a whiteboard session about the future of time-series databases. It was a product manager's simple feature request: let users filter devices by` device_type` and` uptime` simultaneously. Because Iobeam ran on two separate databases — one for device metadata, one for sensor readings — this single filter required weeks of engineering glue code. One engineer's frustrated question — *what if we just put everything in PostgreSQL?* — was the founding insight. TimescaleDB was not born from a market thesis. It was born from a sprint estimation that was too depressing to accept.


**2. Mike Freedman is one of the most decorated computer scientists to co-found a venture-backed startup without leaving his faculty role.**


The Robert E. Kahn Professor of Computer Science at Princeton University is not a typical startup CTO. Freedman holds ACM Fellowship, PECASE designation (the US government's highest honor for early-career scientists), and multiple Test of Time Awards from top computing venues. His PhD examined content delivery networks and peer-to-peer distributed systems at NYU Courant. He co-founded three other companies alongside his Princeton faculty role before Timescale. He built a database company to $180M in funding while holding one of the most prestigious named chairs in American computer science. He is simultaneously a serious academic and a serial entrepreneur — a combination that is genuinely rare.


**3. The founders met as MIT freshmen in September 1997 and ran the Boston Marathon together in 1998.**


The company that raised $180 million from Benchmark, Tiger Global, and NEA was built by two people who became friends before they could legally drink. Eighteen years of friendship preceded the company. Kulkarni and Freedman ran the Boston Marathon during their freshman year. The co-founder relationship that produced TimescaleDB is one of the longest-tenured in the database industry.


**4. Their open-source launch on Hacker News was greeted with "this is a rather bad idea" — and they turned that comment into their origin myth.**


When they posted TimescaleDB on April 4, 2017, the highest-voted response questioned the fundamental premise: building a time-series database on top of PostgreSQL, a 35-year-old general-purpose relational system, seemed backwards in an era of purpose-built NoSQL. That comment became the company's unofficial founding story. They proved it wrong not through argument but through adoption: a 10-20x open-source community larger than their paying customer base, enterprise deployments at NASA, NYSE, and Toyota, and a rebrand in 2024 that acknowledged they had grown beyond even the category they'd been filed under.


**5. TimescaleDB gave away all its enterprise features for free in 2023 as a deliberate growth strategy — and it worked.**


Most database companies tighten their licensing as they grow. Timescale did the opposite: they moved from a proprietary Timescale License (TSL) that restricted cloud competitors from offering their software as a managed service, to fully permissive Apache 2.0 licensing covering every feature, including the enterprise ones. The calculation was counterintuitive: remove all friction to adoption, grow the open-source community faster, and let the managed cloud product monetize the larger resulting funnel. Cloud revenue grew 5x in 18 months after the shift. The "give it all away" move paid off more directly than the "protect the enterprise tier" strategy had.


---
