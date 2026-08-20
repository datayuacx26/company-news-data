---
schema_version: "1.0.0"
document_id: "90553f8a8ac6a9183bec04cb71dc45dcf1c6550ea63f69c25b35c4668062aa73"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/adtech-serving-layer-sprawl/"
published_at: "2026-07-09T11:08:13+00:00"
first_seen_at: "2026-07-22T13:37:02.744795+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:076019285c69350a38e9cf75207f0b41a90ea4bea083e0207f6bce0f5893e0c1"
---

# Why ad platforms end up running six databases to answer one question

# Why ad platforms end up running six databases to answer one question


Jul 9, 2026


•


11 min read


•


[Kevin Tran, Solutions Engineer](https://www.singlestore.com/blog/author/kevin-tran/)


The actual database count on an ad platform rarely matches a simplified architecture diagram. The production environment usually reveals a far more complex reality: separate systems for auction lookups, advertiser dashboards, audience filtering, and various caches, all linked together using a web of event pipelines. This "data sprawl" isn't the result of intentional design but rather a series of individually sensible choices that grow into an inefficient infrastructure, which few would choose to build from the ground up.


The true source of this inefficiency becomes evident when following the lifecycle of a single event. Instead of existing in one place, every event is created once and subsequently duplicated, modified, and redistributed across multiple systems before it ever reaches a decision-making interface. This cumulative redundancy drives up operational costs, complicates data management, and creates hidden architectural friction that even high-performing teams can overlook.


At its core, this problem stems from a growth beyond traditional data tools to serve the daily demands of AdTech. Lakehouses and warehouses excel as batch-oriented online analytical processing systems, or OLAP systems, for scanning historical data alongside transactional systems. But the industry shifted from having only these


**batch OLAP**


and


**individual OLTP**


systems to requiring a


**real-time HTAP system**


. The increased accessibility demands is paramount, especially with the increase in real-time systems over the past half century. Ad platforms need immediate aggregates for bidders and dashboards while marketing data is still actionable, alongside constant transactional point operations on fresh data.


In this context, the traditional


**OLAP & OLTP**


divide is more accurately viewed as a split between batch analytics and real-time serving. Warehouses are often too slow and expensive for these low-latency tasks. Teams compensate by bolting on specialized databases for every new workload. Several iterations later and the gap between the original core architecture and the complex production reality becomes permanent.


Typical Ad Platform Serving Architecture


## **Where do the extra databases come from?**


Trace a single impression through the stack. The bid that won it needed a feature lookup in single-digit milliseconds, so the features require a key-value store. The advertiser who paid for it opens a pacing report and expects the spend to appear within minutes, so recent events stream now require a columnar store that sits apart from the warehouse. The planner who then built the audience filtered it across a dozen attributes, which is what the search index exists for.


Every store is the right tool for the workload in front of it. Collectively they hold five versions of the same event, and keeping those versions in agreement is a standing engineering discipline with its own backlog. The freshness problem this creates is the subject of the next post.


Sort the bolt-ons by the job they do and a short list appears: a key-value store for auction lookups, a real-time analytical store for fresh aggregates, a cache for hot reads, a search index for audience filtering, and wait till you add on a vector database once similarity search enters the picture. Each category is a different vendor, a different operational model, and a different copy of the data.


> This series treats them as categories on purpose. Which specific product wins a benchmark is a separate question, and a direct comparison belongs in its own series rather than here.


The resulting shape starts to resemble a


**lambda architecture**


: a batch path that produces correct numbers slowly, a speed path that produces fresh numbers approximately, and a permanent reconciliation obligation between the two. When running two pipelines was the only way to get freshness and scale together, the trade was rational. The inheritance is less so.


Jay Kreps made the case a decade ago that maintaining the same logic twice, once for batch and once for streaming, is a tax most teams should not still be paying (


[Questioning the Lambda Architecture, O'Reilly Radar](https://www.oreilly.com/radar/questioning-the-lambda-architecture/) ).


Every transformation exists twice. Every new product feature must be built and validated twice. Every discrepancy between the paths becomes somebody's next investigation.


## **Why does the split between batch and real-time cost so much?**


The root of this data sprawl is a fundamental, long-standing rift in data infrastructure. Events are recorded transactionally as they happen, yet analyzed in aggregates only afterwards. Because these two functions have traditionally relied on distinct engines with incompatible storage formats, data must migrate from the write side to the read side before it becomes queryable. This necessary crossing is precisely where the cost accumulates.


**Change data capture, streaming jobs and scheduled loads each add latency, and each leaves another copy behind to be stored, governed and kept in sync.**


While most industries can tolerate a one-hour delay in reporting, advertising cannot. The value of an ad event is concentrated in its


*first few seconds*


, i.e. the narrow window where a budget can be shielded or a bid adjusted. By the time an hour passes, that value has largely evaporated.


Ironically, the most time-sensitive sector of the market operates on an architecture that fundamentally trades data freshness for scale. This isn't a bottleneck that can be tuned away; it is a core characteristic of the system's design mentioned earlier.


## **What does the lag look like from the outside?**


A campaign appears to be underspending on a pacing dashboard powered by a pipeline with a forty-minute lag. Reacting to this, the advertiser increases the budget, unaware that the campaign was actually on track and only the data was delayed. This leads to an overspend that the customer, rather than the platform, eventually uncovers. Conversely, this same latency can mask an overspend until the budget is completely exhausted; in either scenario, the end customer is the one left with the consequences.


Failures in models are often less obvious. When a bidder uses features calculated an hour prior, it optimizes for a market environment that has already shifted. It does so with high confidence despite no visible anomalies in the logs.


This impact is most immediate for platforms operating at the auction's edge: demand-side platforms managing tens of billions of daily bid requests, measurement firms reconciling identity across linear and streaming for attribution, or retail media networks linking an ad view to a physical store purchase made just an hour earlier.


## **What does the sprawl actually cost?**


The impact of architectural sprawl shows up on the balance sheet: a fragmented collection of invoices for warehouse usage, lakehouse processing, key-value stores, columnar databases, search indexes, caches, and the pipeline infrastructure tying them together. Because these costs are distributed across line items, they often escape scrutiny; no single entry appears excessive, even if the total cost for the end-to-end data journey is significant.


Beyond the financial burden, there is a hidden cost to trust and data governance. If an advertiser questions a specific metric, the platform's answer is contingent upon which system generated that data and its latency at that time. For a platform that bills based on its own reports, this level of uncertainty is an unacceptable response to its customers.


The operational toll is equally severe. With data duplicated across multiple systems, discrepancies between reports and dashboards become inevitable, forcing engineers to manually reconcile data in spreadsheets to identify the point of divergence.


Accountability is similarly fragmented; one engineer might manage the warehouse sync while another oversees an index prone to falling behind during backfills. This often leaves the entire data architecture residing in the head of a single senior engineer creating a critical point of failure where the only documentation is an outdated runbook.


Finally, the sprawl creates a directional problem for the product. When a new idea requires a data access pattern that none of the existing six systems can adequately serve, the path of least resistance is to add a seventh. Without architectural pressure to consolidate, the sprawl continues to grow.


## **Why do well-run platforms fail to notice?**


The persistence of architectural sprawl is often aided by the apparent health of individual components. When every database performs its specific role, every pipeline adheres to its SLA, and every monitoring dashboard remains green, the underlying dysfunction stays hidden. This cost is structural, residing in the integration points between systems. Thus, highly capable operations teams can make these issues even harder to identify, as their consistent effort keeps potential symptoms from escalating into detectable incidents.


Rather than a sudden outage, the indicators of sprawl are subtle: monthly reconciliation threads that never disappear, pipeline teams expanding headcount despite a static product surface, or data freshness expectations that have gradually drifted from minutes to nearly an hour through unrecorded renegotiations.


To test for this, a platform should be able to answer a single diagnostic question immediately:


*what is the end-to-end p95 latency from an event occurring to it being queryable on every decision-making surface?*


While most teams understand the latency of their specific segment, few can provide the total figure because no single entity owns the entire journey. This lack of ownership is the core discovery.


## **What does an HTAP serving layer change?**


Instead of addressing workloads in isolation, the architectural alternative is to stop treating high-speed writes and analytical reads as conflicting requirements that necessitate separate systems. Hybrid transactional/analytical processing (HTAP), a


[category established by Gartner in 2014](https://www.gartner.com/en/information-technology/glossary/htap-hybrid-transaction-analytical-processing) , allows both to run against the same data within a single engine.


By storing data in both row-based formats for point lookups and columnar formats for aggregations, an engine can ingest streams directly and make them queryable the moment they land. This eliminates the need for secondary systems and the inherent delays they introduce.


For ad platforms, this consolidation allows a single system to manage the diverse workloads that previously caused sprawl: millisecond-level auction feature lookups, high-concurrency advertiser dashboards, and the complex joins required for attribution. As a multi-modal engine, it stores JSON events, structured aggregates, and vectors together rather than across disconnected stores synced by pipelines.


The result is a single copy of data with a unified freshness profile and a single point of access.


Ad Platform Unified Real-Time Serving Layer


## **Which stores actually move, and in what order**


Avoid the trap of the "big-bang" migration; any vendor suggesting one is revealing their own priorities rather than yours. Effective transitions happen incrementally. Identify the specific workloads causing the most friction like the auction lookups that lag or the advertiser dashboards that stall during peak Monday morning usage. Then, migrate those first. Other systems can remain in place.


The fundamental issue isn't the data itself, but the tools used to manage it. Much like using a flathead driver on a Phillips screw, the mismatch doesn't always cause immediate failure; instead, it manifests as "stripped threads" where architectural damage accumulates quietly over years. Sprawl typically occurs when a workload sits on an engine that fits it poorly.


By categorizing workloads using their actual requirements rather than their current location, teams can pinpoint which two or three stores are prime candidates for consolidation and which can stay exactly where they are. Engineers often overlook this by reflexively tuning an existing, ill-fitting engine instead of questioning if it's the right one for the job; evaluating fit early is the more cost-effective strategy.


## **Determining what remains in place**


It is vital to understand that the serving layer is an


augmentation of, not a replacement for,


the warehouse or lakehouse; any suggestion to "rip out Snowflake" fundamentally misconstrues the architecture. Petabytes of historical archives and intensive model training belong in the lakehouse, where the traditional


**data warehouse vs data lake**


dynamic still applies. The serving layer instead takes ownership of the hot, concurrent, and latency-sensitive customer-facing work that warehouses were never designed to handle. The result is a balanced ecosystem where two different engines manage two distinct workload classes, each optimized for its specific role.


## **The question to ask before the seventh store**


Before the next store comes in, count how many of the existing ones are there solely because the warehouse was the wrong tool for a hot workload. Where the honest count is most of them, the stack is not six unrelated tools. It is one missing layer, patched six ways. The rest of this series takes that layer apart one workload at a time:


-


How fresh ad data really is by the time anyone can query it


-


Identity resolution that cannot wait for a nightly batch


-


Dashboards that thousands of advertisers hit at once


-


The features and context that ad-decisioning models read


-


Graph database or distributed SQL for resolving identity at scale


## On this page


- Where do the extra databases come from?
- Why does the split between batch and real-time cost so much?
- What does the lag look like from the outside?
- What does the sprawl actually cost?
- Why do well-run platforms fail to notice?
- What does an HTAP serving layer change?
- Which stores actually move, and in what order
- Determining what remains in place
- The question to ask before the seventh store


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fadtech-serving-layer-sprawl%2F)


[Engineering](https://www.singlestore.com/blog/category/engineering/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog 4 AI Use Cases Exposing Your Learning Platform's Data Gap Engineering](https://www.singlestore.com/blog/4-ai-use-cases-exposing-your-learning-platform-s-data-gap/)


[Blog I Used to Grow Brain Tumors in a Lab. Now I Think About Databases. Engineering](https://www.singlestore.com/blog/i-used-to-grow-brain-tumors-in-a-lab-now-i-think-about-databases/)


[Blog Retrieval-Augmented Generation (RAG) for Real-World Machine Learning Engineering](https://www.singlestore.com/blog/retrieval-augmented-generation-rag-for-real-world-machine-learning/)


[Blog How Fast Data Ingestion Powers Real-Time AI Engineering](https://www.singlestore.com/blog/how-fast-data-ingestion-powers-real-time-ai/)


[Blog 5 Costs You Can Cut Today to Turn Snowflake Into a Real-Time Powerhouse Engineering](https://www.singlestore.com/blog/5-costs-you-can-cut-today-to-turn-snowflake-into-a-real-time-powerhouse/)


[Blog Pipeline Tuning with Engine Variables Engineering](https://www.singlestore.com/blog/pipeline-tuning-with-engine-variables/)
