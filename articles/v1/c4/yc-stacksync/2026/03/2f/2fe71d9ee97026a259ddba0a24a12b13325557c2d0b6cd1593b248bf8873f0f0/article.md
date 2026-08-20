---
schema_version: "1.0.0"
document_id: "2fe71d9ee97026a259ddba0a24a12b13325557c2d0b6cd1593b248bf8873f0f0"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/the-titanium-database-the-origin-story-of-tidb"
published_at: "2026-03-23T14:32:40+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T22:17:58.599767+00:00"
content_hash: "sha256:d43958052a73de099c170c85c165000f95b4102daa923dffd1e66b97bc7d98f0"
---

# The Titanium Database: The Origin Story of TiDB

## What Happens When Three Engineers Read a Google Paper


It is 2013. Beijing is mid-boom. The Chinese internet is scaling faster than any infrastructure it sits on was designed to handle. And somewhere in that city, three engineers are reading a research paper that has nothing to do with them — geographically, institutionally, or commercially.


The paper is called *F1: A Distributed SQL Database That Scales* . It was published by Google engineers to describe how they had solved one of the hardest problems in database history: running a globally distributed, strongly consistent, SQL-speaking database underneath Google's AdWords product — a system handling billions of dollars in real-time auction transactions across data centers on multiple continents. The database had to be always available. It had to never lose a transaction. It had to speak SQL, because the engineers who built AdWords were SQL engineers. And it had to scale horizontally, because no single machine on earth was big enough to hold Google's business.


F1 ran on top of something even more audacious: Spanner, Google's globally distributed storage system, which used GPS receivers and atomic clocks to synchronize timestamps across data centers with sub-millisecond precision. It was, by any reasonable measure, the most sophisticated database system ever built by a private company.


Ed Huang, Liu Qi (Max Liu), and Dong Yang read that paper and understood two things simultaneously.


First: this was the right answer to a problem that hundreds of millions of developers were going to face. Every company running MySQL at scale was sharding manually, building application-layer routing logic, accepting the agonizing maintenance burden of databases that had no idea they were distributed. F1 and Spanner had proved that this problem was solvable.


Second: nobody outside Google was ever going to run it.


Google didn't release Spanner as open source. They published the academic papers — the Spanner paper in 2012, F1 in 2013 — but the code stayed inside one of the most valuable companies in the world, serving Google's own needs, inaccessible to the engineers at Meituan and ByteDance and every other company in the world that was going to face the same scaling wall.


The three of them looked at each other and asked the question that would cost all of them their jobs, their stability, and the next ten years of their lives.


"Why can't we build this for everyone?"


They quit. In April 2015, they started PingCAP. They opened a GitHub repository in September 2015. They named their database TiDB — Ti for Titanium, the metal that is stronger than steel, harder than iron, lighter than either, and nearly impossible to corrode. MySQL was the steel. They were going to build something that outlasted it.


---


## Who These Three People Were, and What They Already Knew


Ed Huang and Max Liu had been working at Wandou Labs, a Chinese Android app store that was later acquired by Xiaomi. The database pain they lived with there was not exotic. It was ordinary and crushing: sharded MySQL.


When data grows past what one MySQL instance can hold, you shard it. You split the data across multiple instances and write application-layer code — routing logic, shard maps, key distribution schemes — that your application uses to figure out which shard holds which record. This works. It also means that every developer who touches your database has to understand the sharding logic. Joins across shards are expensive or impossible. Transactions that span multiple shards require application-level coordination that is difficult to get right. Migrations — when you need to reshard because your data has grown again — are among the most dangerous operations in engineering, a live surgery on a running system where one mistake causes downtime.


Ed's first answer to this problem was Codis, an open-source Redis cluster proxy that abstracted away Redis sharding from application developers. Codis became one of the most-starred Chinese projects on GitHub. Behind Codis, though, sat a massive sharded MySQL cluster with the exact same problem. Redis sharding could be proxied away. MySQL sharding was structural.


When Ed proposed solving MySQL sharding by building a new distributed database from scratch, his boss at Wandou Labs declined. It was a moon shot. They were an app store. The answer was no.


So Ed and Max quit.


Their Codis reputation in China's open-source community gave them something most infrastructure founders don't have at the starting line: credibility with engineers. They could recruit. They could raise. They got seed money and went full-time in April 2015. Dong Yang, who had deep expertise in distributed systems, joined as the third co-founder.


On April 1st — April Fools' Day — Max Liu sent a founding invitation to Siddon Tang, who would become one of PingCAP's first key engineers. Tang's first reaction was "Are you kidding?" Max replied: "I'm dead serious." They were. Tang joined.


The company's name carried a philosophy encoded into it. PingCAP: "Ping" for connectivity, for the network command that tests reachability, for the idea of communication across a distributed system. "CAP" for the CAP theorem — the foundational constraint in distributed systems, proven by Eric Brewer in 2000, stating that a distributed system can only guarantee two of three properties: Consistency, Availability, and Partition tolerance. Traditional databases chose consistency. NoSQL databases chose availability. The CAP theorem was supposed to make this a permanent tradeoff.


PingCAP was named after the theorem it intended to push against.


The broader Chinese context matters here. In 2015, China's technology industry was in the middle of an aggressive campaign called "de-IOE" — a deliberate drive to eliminate Western enterprise infrastructure, specifically IBM servers, Oracle databases, and EMC storage. Alibaba had launched the campaign around 2008, giving it 1 billion RMB per year and eventually decommissioning Oracle from Taobao's core systems entirely by 2013. By the time PingCAP launched, every major Chinese internet company was looking for something to replace Oracle. The timing was not coincidental. Ed and Max understood that their market existed, was growing, and desperately needed what they intended to build.


---


## Building a Distributed Database From Three Google Papers


The first decision PingCAP made was architectural, and it was correct.


They split the problem in two.


A database has two fundamental jobs: store data durably, and let you query it. Most databases couple these tightly — the storage layer and the query layer are the same system. PingCAP decided to build them separately. TiDB would be the SQL processing layer: the thing applications talked to, the thing that parsed queries, planned executions, managed transactions at the SQL level. TiKV would be the storage layer: the distributed key-value store that actually held data, managed replication, and enforced consistency.


The two systems would communicate over a clean interface. Either could be improved independently. TiKV could be used by other databases that needed a distributed storage layer. The modularity was philosophically consistent with open source — you build components that can be composed — and practically important for a small team trying to build something that Google had needed hundreds of engineers to construct.


TiDB, the SQL layer, they wrote in Go. TiKV, the storage layer, they wrote in Rust.


The Rust decision deserves attention. In late 2015, Rust was barely a year old as a stable language. Almost no production systems ran it. The choice looked reckless from the outside. From the inside, it was the only option that solved all their constraints simultaneously.


Go was ruled out first. Not because the team didn't know Go — they did. But TiKV needed RocksDB for the underlying on-disk storage, and RocksDB is written in C++. Go's Cgo bridge introduces overhead that becomes unacceptable when a storage engine is making thousands of calls per second into a C++ library. The performance cost was too high.


C++ was ruled out next. Despite team expertise, they feared the class of bugs that destroy databases: dangling pointers, memory leaks, data races, use-after-free errors. In a storage engine, these bugs don't crash loudly. They corrupt data silently, in ways that don't surface until months later when a transaction's integrity check fails or a replica diverges from its primary. C++ could produce fast code that they couldn't prove was safe.


Rust was the answer: memory safety enforced at compile time, performance comparable to C++, and clean FFI to RocksDB without Cgo overhead. The team estimated about one month to learn Rust for experienced systems engineers. That was the cost.


The unintended consequence: Western engineers who would never have contributed to a Chinese database project in Go or C++ filed pull requests against TiKV because they cared about Rust. The Rust community in 2015 was young, idealistic, and global — distributed across Europe, North America, and Asia, held together by a shared belief that systems programming didn't have to sacrifice safety for performance. Those engineers filed issues, submitted patches, helped TiKV mature faster than it would have through Chinese contributors alone. The language choice became an accidental trust-building mechanism across geopolitical lines.


The architecture borrowed directly from the three Google papers.


From Spanner: the idea of dividing data into Regions — contiguous chunks of the keyspace, each 64MB by default — that could be distributed across nodes and moved automatically as the cluster grew. No manual sharding. No routing tables written by application developers. The database managed its own data distribution.


From Percolator: the transaction model. Spanner used GPS clocks and atomic clocks to achieve globally synchronized timestamps. PingCAP couldn't afford GPS clocks. They built a Placement Driver — a central timestamp oracle that assigned globally monotonically increasing timestamps without hardware dependencies. This was the practical compromise that made Spanner-style transactions accessible to everyone.


From F1: the SQL interface. TiDB spoke MySQL's wire protocol. Any application that could connect to MySQL could connect to TiDB without changing a line of code. This was not a coincidence. It was the strategy.


Raft, the consensus algorithm that makes distributed nodes agree on the state of the world, was ported from etcd, the distributed key-value store that Kubernetes uses for its cluster state. Using Raft rather than Paxos — the algorithm Google used — was a deliberate choice for understandability. Raft had been designed specifically to be easier to comprehend and implement correctly. For a small team building a database they intended to open-source and invite contributions to, legibility in the core consensus mechanism mattered.


The GitHub repository went live in September 2015. TiDB 1.0 shipped on October 16, 2017 — 771 days later, by Max Liu's count. It was already running in production at more than thirty companies across the Asia-Pacific region.


In 2019, the Jepsen test — the notorious distributed systems correctness evaluation run by researcher Kyle Kingsbury, which had embarrassed dozens of databases that claimed stronger guarantees than they delivered — published a report on TiDB 2.1.7. The findings were serious: lost updates, read skew, two separate auto-retry mechanisms that blindly re-applied updates during conflicts, and in one 30-second test, 64 insertions lost out of 378. PingCAP's default configuration was providing weaker guarantees than the documentation promised.


PingCAP published a detailed technical response. They fixed every issue identified. They did not litigate, PR-spin, or suppress. In the database engineering community, where trust is built on transparency about failure modes, that response became a case study in how to handle a Jepsen result. Engineers who might have dismissed TiDB after the initial report came back after seeing how the team responded. The failure, owned completely, became proof of character.


---


## CNCF, Chinese Unicorns, and the Open-Source Wager


The moment that changed PingCAP's trajectory didn't happen in a server room. It happened in a vote.


In August 2018, the Cloud Native Computing Foundation accepted TiKV as an incubating project. In September 2020, TiKV graduated to full CNCF status — one of very few projects from Chinese companies to achieve it, earning the same institutional standing as Kubernetes, Prometheus, and Envoy.


This is not a technical milestone. It is a trust milestone.


The CNCF is the governing body of the infrastructure that Fortune 500 companies build on. When TiKV graduated, it meant that engineers from Google, Microsoft, Amazon, Red Hat, and IBM had reviewed the code and voted it production-worthy by the standards they applied to their own critical systems. A Chinese company's storage engine now lived under the same neutral governance umbrella as the tools running most of the modern cloud. An enterprise CIO evaluating TiKV after the graduation could point to that status in a procurement discussion and have it mean something.


PingCAP also donated Chaos Mesh — their chaos engineering platform for testing distributed systems — to CNCF. This made them one of a very small number of organizations to graduate multiple CNCF projects. The donations were not charity. They were a deliberate strategy: move your most critical infrastructure into a neutral home where contributors from competing companies could participate without feeling like they were building someone else's commercial moat.


The Chinese internet companies had already arrived by the time CNCF graduated TiKV. The scale numbers are extraordinary.


Meituan — the food delivery and lifestyle platform that is China's equivalent of DoorDash, Yelp, and Instacart simultaneously — ran more than 1,700 TiDB nodes across hundreds of clusters, with peak query rates exceeding 100,000 per second in a single cluster and a largest table holding over 100 billion records. Zhihu, China's Quora, ran 1.3 trillion rows of data in TiDB production with millisecond response times, importing 1.1 trillion records in four days using TiDB Lightning. Xiaomi ran 20+ clusters, 100+ TiKV nodes, and approximately 100 million read/write queries daily.


These deployments were not experiments. They were load-bearing. Companies with hundreds of millions of daily active users were trusting their most critical data to a database that a group of three engineers had started building in Beijing five years earlier because they had read a Google paper and decided it wasn't fair that only Google could use it.


The funding reflected the traction. A $50 million Series C in September 2018, with GGV Capital — a US-China crossover fund — leading. A $270 million Series D in November 2020, led by GGV and Coatue. A $300 million Series D+ in 2021, led by Sequoia Capital China and GIC, Singapore's sovereign wealth fund, at a $3 billion valuation.


That $3 billion valuation made PingCAP China's first open-source unicorn. Western capital — Sequoia, GGV, Coatue — backed a Beijing-headquartered open-source database company to unicorn status at the peak of US-China technology tensions. The bet was not on geopolitics. It was on the size of the problem TiDB was solving.


---


## HTAP and the Column That Changed Everything


In 2019, PingCAP introduced TiFlash. And with it, they made a claim that almost every database company has tried and almost none have delivered on.


A single database that handles both your transactional workload and your analytical workload, simultaneously, without interference, without a nightly ETL pipeline, without the data latency that comes from synchronizing two separate systems.


The industry term is HTAP — Hybrid Transactional/Analytical Processing. The reason it's hard: transactional databases and analytical databases have opposite physical needs. OLTP — online transactional processing — handles small, fast writes and point reads. "Record this order. Fetch this customer's current balance." These queries touch one or a few rows. Row-oriented storage — where all the columns for a single row are stored together — is ideal for this.


OLAP — online analytical processing — handles large aggregations across millions or billions of rows. "What was the average order value by region last month, segmented by product category and customer tenure?" These queries scan enormous amounts of data but touch only a few columns. Columnar storage — where all values for a single column are stored together, compressible and scannable sequentially — is ideal for this.


You can't optimize one physical layout for both access patterns. For decades, the industry's answer was: run two systems. One OLTP database for live operations. One OLAP database (a data warehouse, usually) for analytics. Synchronize data between them on a schedule — usually nightly. Accept that your analytics are always hours or days old.


TiFlash is a columnar storage engine that lives inside TiDB alongside TiKV. The same data that is written to TiKV in row format is asynchronously replicated to TiFlash in columnar format using the Raft Learner mechanism — TiFlash nodes receive data from TiKV nodes as non-voting Raft members, so they stay synchronized without participating in the consensus protocol and adding latency to transactional writes.


When a query arrives, TiDB's optimizer evaluates whether to route it to TiKV (row access, transactional queries), TiFlash (columnar scan, analytical queries), or both simultaneously through a massively parallel processing engine. The data doesn't move. No ETL. No pipeline. No staleness.


The technical implementation of TiFlash embedded the columnar execution engine from ClickHouse — one of the fastest analytical query engines in existence — giving TiDB world-class analytical performance without building it from scratch. TiDB ships two world-class storage engines simultaneously, keeps them synchronized via Raft, and routes queries between them in real time. That is the HTAP claim. It is not marketing.


The VLDB 2020 paper — *"TiDB: A Raft-based HTAP Database"* — appeared in one of the most prestigious database research venues in the world. A Beijing startup publishing in VLDB, describing an architecture that the academic database community considered genuinely novel. Pinterest reduced infrastructure costs 80% by consolidating onto TiDB. Plaid cut database maintenance burden 96% with zero-downtime upgrades. WeBank scaled to petabyte-level operations while cutting costs 30%.


---


See real-time two-way sync in action


Book a demo with real engineers, no sales script.


[Book a demo →](https://www.stacksync.com/book-a-demo)


## The China Tension That Product Quality Can't Fix


PingCAP solved the global headquarters problem the way ambitious Chinese tech companies often solve it: by making it not visibly a Chinese company.


Legal headquarters: Sunnyvale, California. Offices in Singapore, Kuala Lumpur, and Tokyo. Website in English. Engineering team distributed globally. The company presents as a global infrastructure company whose founders happen to be Chinese — not as a Chinese company trying to expand globally.


This positioning is both true and strategic. The engineering team is genuinely international. The open-source community contributes from every timezone. The product has customers across six continents. It is also strategic because enterprise buyers in regulated industries carry historical caution about infrastructure that originates in China. Banking, healthcare, government procurement — these sectors carry questions about data sovereignty, government access, and where data actually lives that are not hypothetical. They decide deals.


The CNCF graduation was a partial answer. Code reviewed by engineers at Google, Microsoft, and Red Hat, running under an open-source Apache 2.0 license visible to every security auditor alive, is harder to characterize as an opacity risk than a proprietary black box. Open source was not just a distribution strategy for PingCAP. It was a trust strategy — and the CNCF donations were the most expensive and deliberate version of that strategy.


The TiDB Cloud launch on AWS Marketplace in February 2022 was another answer. By running as a fully managed service on AWS infrastructure — infrastructure that Western enterprise buyers already had compliance frameworks around — PingCAP gave enterprise customers an adoption path that didn't require them to have an opinion about where the founders were born. You were running on AWS. Your data was in AWS regions you chose. The database was open source. The CNCF had blessed the storage layer. The procurement question became answerable.


China's "xinchuang" push — the government-driven information technology application innovation campaign requiring Chinese state enterprises and government agencies to replace Western software — created a paradox. It accelerated TiDB's domestic growth dramatically, as Meituan, ByteDance, Bilibili, and iQIYI all migrated Oracle and IBM workloads onto TiDB. It simultaneously made it harder to sell internationally, because being the preferred database of China's state-adjacent enterprises is a two-edged characterization in Western procurement discussions.


TiDB ended up with exactly the market position the founders designed for, and a harder geopolitical ceiling than they anticipated. Over 3,000 enterprise customers. 39,900+ GitHub stars. TiKV at 31,330 GitHub stars with a $301.6 million estimated software value according to CNCF's own calculations. A contributor community measured in the tens of thousands. TiFlash research published in VLDB. The product is unambiguously world-class.


And yet Max Liu still talks in interviews about the distance between the quality of TiDB and its adoption in Western enterprise markets. The distance is real. It is not a product problem. It is a decade of geopolitical friction that no engineering team can commit its way out of.


The three engineers who quit their jobs in Beijing to build the open-source version of Google Spanner have shipped a database running at petabyte scale across hundreds of millions of daily active users, published research in VLDB, graduated two projects from CNCF, raised $641 million, and reached a $3 billion valuation. They read a Google paper and decided it wasn't fair that only Google could have what it described.


They built it for everyone. Getting everyone to use it turned out to be a different problem entirely.


---


## Five Things Most People Don't Know About TiDB


**1. TiDB is three Google papers welded together, not one.**


Everyone says "TiDB was inspired by Google Spanner." The full story: TiDB recombined three separate Google research systems. F1 (2013) provided the SQL interface model — a distributed SQL query engine sitting above a distributed storage layer. Spanner (2012) provided the architectural philosophy — globally distributed Regions, automatic data rebalancing, strong consistency. Percolator (2010) provided the transaction model — because PingCAP couldn't use atomic clocks like Spanner did, they implemented Percolator's two-phase commit with a central Timestamp Oracle instead. No single Google team was building the combination of all three. PingCAP's original contribution was the synthesis.


**2. The "Ti = Titanium" name was a direct declaration of superiority over MySQL.**


MySQL is the steel that built the web. Titanium is the metal that is stronger than steel, lighter than iron, and nearly impossible to corrode. The founders named their database after Titanium specifically and deliberately — in the chemical symbol Ti, which also stands for the Titanium in the periodic table. When MySQL engineers eventually encountered TiDB in competitive discussions, they understood immediately what the name meant. The naming was not modest.


**3. TiKV's CNCF graduation was as much a geopolitical event as a technical one.**


When TiKV graduated from CNCF in September 2020, the technical community noticed a well-built distributed key-value store reaching maturity. The strategic significance was different: a project from a Chinese company now carried the institutional endorsement of the organization that stewards Kubernetes, certified by engineers from Google, Microsoft, Amazon, and IBM. In enterprise procurement, where "where is this company from" is a question that determines whether a conversation continues, CNCF graduation was worth more than any marketing campaign. It was a form of geopolitical neutrality that PingCAP had deliberately purchased by donating their most valuable infrastructure.


**4. The Jepsen failure in 2019 made TiDB more trustworthy, not less.**


Kyle Kingsbury's Jepsen analysis found that TiDB 2.1.7 through 3.0.0-beta lost 64 of 378 insertions in a 30-second test and violated snapshot isolation guarantees in default configuration. This was a damaging finding. PingCAP published a detailed technical response, fixed every issue, and did not suppress, spin, or litigate the results. In the database engineering community — where trust is built on honesty about failure modes, not claims of perfection — that response became a case study. Engineers who had dismissed TiDB after the initial report came back after seeing how the team handled it. The failure, owned completely, became proof that the engineering team had more integrity than most.


**5. TiFlash is powered by ClickHouse internals — meaning TiDB ships two world-class analytical engines simultaneously.**


TiDB's HTAP capability — the ability to handle both transactional and analytical workloads simultaneously — depends on TiFlash, a columnar storage engine whose vectorized query execution is based on ClickHouse's engine, one of the fastest columnar analytical systems ever built. TiDB keeps TiFlash nodes synchronized with TiKV nodes via Raft Learner replication and routes queries between them using a cost-based optimizer. A single TiDB cluster contains both a world-class transactional engine (TiKV, written in Rust, inspired by Spanner) and a world-class analytical engine (TiFlash, powered by ClickHouse internals) synchronized in real time. The HTAP claim is not a feature flag. It is an architectural commitment that no major incumbent database has matched.


---


*Sources: PingCAP engineering blog ("How We Build TiDB"), CNCF TiKV project page, Jepsen analysis (Kyle Kingsbury, 2019), VLDB 2020 paper "TiDB: A Raft-based HTAP Database," TechCrunch Series C and Series D coverage, tikv.org, PingCAP about/careers pages, TiDB GitHub repository (est. September 6, 2015 — 39,900+ stars as of 2026), PingCAP case studies (Pinterest, Plaid, WeBank, Meituan, Zhihu, Xiaomi), AWS Marketplace launch announcement (February 2022).*
