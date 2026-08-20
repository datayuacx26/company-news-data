---
schema_version: "1.0.0"
document_id: "9189c7bcd6066d5de6b44e8a946d6373c7a8efc62b5a17acdf45f7a30fd9e4d0"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/the-ram-thesis-that-turned-out-to-be-wrong-the-origin-story-of-singlestore"
published_at: "2026-03-23T14:32:00+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:425a39aacfee304d699e23be5c80c46f0f3398f091e39725384f7aeb4f173678"
---

# The RAM Thesis That Turned Out to Be Wrong: The Origin Story of SingleStore

## A SQL Query That Ran for a Full Day


Two engineers at Facebook watch a single SQL query run for **more than 24 hours** on a billion-row analytical table. Not 45 seconds. Not 3 minutes. A full day. The query finishes before the business can act on the answer.


One of them, Nikita Shamgunov, spent years before Facebook building SQL Server at Microsoft — one of the most ambitious database releases in history, a project that took five straight years. He understands exactly why the query is slow. The database is interpreting every instruction, line by line, like a human reading a recipe aloud while cooking. Nobody compiles it. Nobody turns it into machine code.


He thinks: *if we compiled the query to native machine code at runtime, the way a C++ compiler does, we could skip all of that interpretation overhead entirely.*


They quit Facebook in early 2011. The Monday after Eric Frenkiel's last day, they walked into an investor's office with zero lines of code. They walked out with a $200,000 check.


That thought became a company called MemSQL. The company eventually became SingleStore. And in 2025, after raising $464 million and hitting a $1.3 billion valuation, it sold a majority stake to private equity — not because it failed, but because the venture capital world had moved on to bigger bets.


This is the story nobody tells about real-time databases.


---


## Facebook, the In-Memory Bet, and the SQL Compilation Insight


### The Three Founders


Most people know MemSQL had two founders. It had three.


**Eric Frenkiel** — Stanford engineering graduate (Class of 2008, Management Science and Engineering). Worked at Cooliris and Marin Software before landing at Facebook in 2010 in Partner Engineering, helping companies like CNN and Zynga integrate with Facebook's API. The business-facing co-founder. His insight wasn't technical — it was market: Facebook had solved a problem every enterprise would eventually face.


**Nikita Shamgunov** — Ph.D. in computer science. Spent most of a decade at Microsoft on the SQL Server kernel team before joining Facebook. The technical co-founder. His insight was the opposite of Frenkiel's: he understood databases at the level of memory allocation and compiler design. When he saw Facebook's slow queries, he didn't see a product problem. He saw a computer science problem.


**Adam Prout** — a third co-founder, also with database engineering roots, who joined the venture to build the system out. Less public-facing, but part of the original founding team from 2011.


Frenkiel and Shamgunov met at Facebook orientation in 2010. They left together in early 2011 to apply to Y Combinator. The timing forced a painful decision: leaving two months earlier than planned meant walking away from a meaningful chunk of unvested Facebook equity — but YC has fixed start dates, and they didn't want to wait.


### The Facebook Problem


Facebook in 2010 had solved its own big data problem through brute engineering: a heavily denormalized, distributed architecture that was wickedly fast but utterly unintuitive. To access data, you had to understand a system designed specifically for Facebook's scale. There was no standard SQL interface. Developers had to learn exotic patterns.


The analytical problem was different. Certain analytical workloads at Facebook took more than 24 hours to complete on a single query. Facebook threw more hardware at it. In-memory storage — huge amounts of DRAM — was one key to their approach.


Frenkiel and Shamgunov saw the pattern: as data generation accelerated, every enterprise would eventually face Facebook's problem. But enterprises couldn't rebuild their systems from scratch. They needed something that ran familiar SQL — and ran it fast.


### The Technical Bet: JIT Compilation for SQL


The core MemSQL insight was not just "put data in memory." It was: *compile the query, don't interpret it.*


Every traditional database interprets SQL at runtime through what's called the "volcano model" — a tree of operators, each requesting rows from the one below it, one at a time, with function call overhead at every step. It's flexible. It's also slow.


Shamgunov's insight, rooted in his SQL Server work, was to treat a SQL query the way a compiler treats source code. Take the query, translate it to an imperative execution plan (MemSQL called this MPL — MemSQL Plan Language), then compile it down to native x86_64 machine code using LLVM. The first time a query shape runs, there's compilation overhead. Every subsequent execution of that query shape runs as pre-compiled machine code — no interpretation, no operator overhead, no function call chains.


**The early MemSQL system used GCC (not LLVM initially)** — compiling SQL to C++ at runtime, storing the compiled plan as a shared object. Compilation overhead: 0.5 to 10 seconds per query per thread. A significant cost. But for long-running analytical queries — the kind that used to take 24 hours — irrelevant.


MemSQL 5 later replaced the GCC pipeline with a full LLVM-based compiler, building an entirely new MPL virtual machine. The result: a **20x speedup in code generation itself** , while still producing the same quality native code. The pipeline: SQL → MPL → MemSQL Bytecode (MBC) → LLVM bitcode → x86_64 machine code.


A key architectural decision: rather than generate code only for parts of the query (the partial approach), MemSQL generated code for the *entire* query, including error handling and corner cases. More complex to build. Massively better performance for complex, long-running analytical queries.


**Also notable:** instead of B-trees for range queries, MemSQL used skip lists — lock-free, more scalable, but requiring separate indexes for ascending and descending queries. And the entire system was built on lock-free data structures throughout: linked lists, queues, stacks, hash tables. The only locks acquired were on write-write conflicts on the same row. This was unusual in 2011.


### The Launch and the Controversy


MemSQL launched publicly in June 2012 and released a benchmark showing 80,000 queries per second versus MySQL's 3,500 — a roughly **23x advantage** . The headline: "the world's fastest database."


The database community was not impressed.


Domas Mituzas, a MySQL performance expert, published a detailed rebuttal. The benchmark compared default-configured MySQL against default-configured MemSQL — but the defaults weren't equivalent. MemSQL's "full durability mode" was asynchronously buffering transactions up to 128MB before flushing, while InnoDB was configured with just 10MB transaction logs. MemSQL also only supported single-query transactions at READ COMMITTED isolation level at launch.


The conclusion: MemSQL was measuring different things and calling it a fair fight.


MemSQL published a response blog post. The controversy blew over. Within six months of the (quiet) initial release, 10,000 developers were using it. The performance claims weren't wrong — they were just narrowly scoped.


---


## The MemSQL Years, the HTAP Pivot, and the Name Change


### Scaling Slowly


The early years were measured. They raised:
- **$2.1M seed** (First Round Capital, SV Angel, YC, Paul Buchheit, Ashton Kutcher) — yes, Ashton Kutcher was an early investor
- **$5M** at launch from IA Ventures, Max Levchin, Aaron Levie, Data Collective
- **$35M Series B** (2013) led by Accel Partners


By 2018, the company had reached eight-figure ARR with 200% year-over-year growth. The "land and expand" model worked: initial $100,000 contracts grew into million-dollar deals. But the name "MemSQL" was becoming a liability.


### The Name Was a Lie


By the time Raj Verma joined as Co-CEO in mid-2019, "in-memory" represented just **1% of SingleStore's revenue** . One percent. The company had evolved far beyond its original architectural bet — it stored most data on disk, with hot data in memory, and the product had expanded to handle both transactional and analytical workloads. But the name "MemSQL" kept sending the wrong signal, especially in international markets where it didn't translate well.


Verma later said the rename was one of the first things he asked about when he joined: "99% of our revenue doesn't come from in-memory use cases. Why are we called MemSQL?"


On **October 27, 2020** , MemSQL officially became **SingleStore** . The name reflected the thesis: one database that handles everything — transactions, analytics, operational data — so companies don't need 20+ different database systems, each a silo.


The "Mem" was gone. The product had been growing past it for years.


### Nikita Leaves, Then Eric


Nikita Shamgunov transitioned to CEO in 2017 (when ARR was roughly $7M) but departed in 2022, joining Khosla Ventures as an investing partner. He already had a new database idea he "couldn't act on while at SingleStore because you can only have one engine in the company" — he went on to co-found **Neon** , a serverless Postgres platform, which raised over $100 million and runs 700,000+ databases.


Eric Frenkiel had left earlier, in 2017. He eventually founded **Pomelo** , a fintech company.


By 2022, neither original CEO nor CTO was at the company. It was fully Raj Verma's SingleStore.


---


## Real-Time as a Product Category


### What HTAP Actually Means


HTAP — Hybrid Transactional/Analytical Processing — is a term from the analyst community (Gartner coined it around 2014) describing systems that can handle both write-heavy transactional workloads and read-heavy analytical workloads simultaneously, without requiring separate systems or ETL pipelines.


Traditional architecture: a transactional database (Postgres, MySQL) handles writes; a warehouse (Snowflake, Redshift) handles analytics; ETL runs every night to sync them. Data latency: hours to days.


SingleStore's claim: eliminate the ETL layer entirely. Writes and reads in one system, with data available for analytics within milliseconds of the write.


The architecture that makes this work: SingleStore separates compute and storage at the workspace level. A primary workspace handles writes, logging transactions to shared storage, then replicating logs to read-only workspaces asynchronously — in near real-time. The transaction log tail is applied at the replica almost instantly. This means a read-only analytics workspace can serve queries on data that's seconds old, not hours old.


The competitive claim: for use cases where data freshness matters, SingleStore beats any ETL-based architecture by definition. The tradeoff: for pure historical analytics at petabyte scale, Snowflake or Databricks remain better choices.


### Fraud Detection at 50 Milliseconds


The most visceral proof of the SingleStore model: a major US bank replaced overnight batch fraud detection with real-time fraud scoring.


Before SingleStore: fraud models ran on yesterday's transactions. The bank could detect a fraud pattern on Tuesday that happened on Monday.


After SingleStore: from the moment a card is swiped to the system's approval or denial decision — **50 milliseconds** . The system scores against 70+ queries simultaneously within that window. Estimated value: tens to hundreds of millions of dollars in prevented fraud events per year.


This is the HTAP thesis in its clearest form: the latency between data creation and decision is the product.


### Uber's Apollo Platform


Uber built its internal real-time analytics platform — called Apollo — on SingleStore. Before SingleStore, Uber's P99 ingest latency ranged from 1.5 seconds on the low end to **3 minutes** on the high end, depending on the data source.


After embedding SingleStore at Apollo's core: ingest latency reduced from minutes to seconds across all sources. Uber uses SingleStore to monitor and react to live marketplace metrics — demand patterns, driver positioning, dynamic pricing — in real time.


The irony Raj Verma tells as a story: he took an Uber after a cricket match in London. The post-match surge pushed his £18 ride to £42. "That's SingleStore working as designed," he says.


### Comcast, Hulu, Disney


Comcast deployed SingleStore to aggregate statistics across its entire US footprint for real-time video quality analysis — diagnosing issues before customers report them. Their words: "We're now able to aggregate statistics across the entire US as well as glue on to complex streams for complex analysis."


The pattern across all these customers: the value isn't just speed. It's **collapsing the stack** . One system instead of three. No ETL job to maintain. No stale data problem.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## $1.3 Billion, Private Equity, and an Unsatisfying Ending


### The Funding Arc


Total raised: **$464 million** across 14 rounds spanning 2011–2022.


Key rounds:
- **Series B (2013):** $35M — Accel Partners
- **Series C (2015):** $50M
- **Series D (2017):** $36M
- **Series E (Dec 2020):** $80M — Insight Partners (alongside the rebrand)
- **Series F (2021):** $80M
- **Series F extension (July 2022):** $116M — pushed valuation to $1.3B ("unicorn" status)
- **Series F-2 (Oct 2022):** $146M — led by Goldman Sachs


Peak valuation: **$1.3 billion** (2022).


### The Unsatisfying Exit


By 2023, Raj Verma was reporting $40M ARR growing to over $100M ARR by 2023. 350 customers, 50% from Fortune 500. Strong unit economics. The company maintained **$150M+ in cash with zero debt** by 2025.


But the venture capital window for database IPOs had effectively closed. Snowflake, Databricks — these had captured all the oxygen. Databricks was valued at $100 billion. In that context, a $1.3B database company starts to look like a rounding error to a late-stage investor hoping for a 10x return.


In September 2025, SingleStore sold a majority stake to **Vector Capital** , a private equity firm, at a valuation estimated around **$500 million** — a significant step down from the 2022 peak. Google Ventures, Dell Technologies Capital, IBM, and REV Venture Partners retained minority stakes.


CEO Raj Verma framed it as a strategic long-term move: "Building a database natively engineered for the AI era, well before the moment arrived, has been the work of more than a decade."


The company didn't fail. It built genuinely novel technology, proved it at scale with real enterprise customers, and generated over $100M in ARR. But in the venture math of 2025 — where "AI database" means Databricks or Snowflake — it wasn't a story big enough for an IPO.


The two engineers who watched a Facebook query run for 24 hours built a company that ran queries in 50 milliseconds. One went on to build another database company. The company they left behind was absorbed into private equity.


---


## Five Things Most People Don't Know About SingleStore


**1. The query that took 24 hours.**
The founding insight isn't a cute benchmark. At Facebook in 2010, certain analytical queries took more than 24 hours to complete. The entire MemSQL project was born from one question: what if we compile the SQL query instead of interpreting it? The answer was to build a JIT compiler for SQL — not in academia, but as a product.


**2. In-memory was only ever 1% of revenue.**
The company was called MemSQL for nine years. By the time of the rebrand, in-memory storage represented just 1% of actual customer revenue. The name had been misleading for most of the company's existence. CEO Raj Verma said fixing the name was the first thing he asked about when he joined.


**3. The benchmark controversy almost killed launch credibility.**
When MemSQL launched in 2012 claiming to be 23x faster than MySQL, a MySQL expert immediately published a detailed rebuttal showing the benchmark was rigged (intentionally or not): different durability settings, different log configurations, different transaction isolation levels. The community was brutal. MemSQL survived it — but it's a reminder that "world's fastest database" is almost always a lie of context.


**4. The co-founder left to build a competitor.**
Nikita Shamgunov — the Ph.D. who wrote the original MemSQL code — departed SingleStore in 2022, joined Khosla Ventures, and then founded **Neon** , a serverless Postgres platform. His explicit reason: there was a database idea he couldn't pursue at SingleStore because "you can only have one engine in a company." He's now running a product that competes in the same general space he spent a decade building. He said he was three years late to start it.


**5. The unicorn peak was $1.3B. The exit was ~$500M.**
After raising $464 million total and peaking at a $1.3 billion valuation in 2022, SingleStore sold a majority stake to private equity (Vector Capital) in 2025 at approximately $500 million — less than half the peak valuation, despite generating over $100M in ARR with no debt. The company didn't fail. It just wasn't big enough for venture math in the AI era.


---
