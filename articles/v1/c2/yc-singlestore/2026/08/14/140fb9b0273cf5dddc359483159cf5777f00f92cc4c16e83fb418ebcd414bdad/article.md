---
schema_version: "1.0.0"
document_id: "140fb9b0273cf5dddc359483159cf5777f00f92cc4c16e83fb418ebcd414bdad"
company_key: "yc-singlestore"
company: "SingleStore"
source_id: "yc-singlestore-news-import-7744c8297ba3"
canonical_url: "https://www.singlestore.com/blog/identity-resolution-without-nightly-batch/"
published_at: "2026-08-03T13:42:51+00:00"
first_seen_at: "2026-08-03T20:25:04.686479+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:44f4faed2a1c02d48ccaac5ac8e7ed0ee2f53bad51b91480d782ff85e9731d00"
---

# Identity resolution without the nightly batch

# Resolving identity across a billion devices without a nightly batch


Aug 3, 2026


•


6 min read


•


[Kevin Tran, Solutions Engineer](https://www.singlestore.com/blog/author/kevin-tran/)


The foundation of every advertising platform is its identity graph. Critical functions like cross-screen frequency capping, attribution from initial exposure to final purchase, and audience reach metrics all rely on joining against this graph. The caveat is its overall value is limited to the freshness of the most recent rebuild. For many platforms, this process is still a legacy nightly batch job established when data volumes were lower and the demands for real-time insights were less intense.


Achieving identity resolution at AdTech scale requires continuous merging of device signals, logins, and household identifiers into a persistent, current structure; however, the conventional approach still involves recomputing the entire graph within a lakehouse on a fixed nightly schedule. Every hour that passes between these rebuilds forces the platform to target, cap, and attribute using outdated connections. With graphs encompassing billions of devices, the time required for the rebuild itself has become more of an engineering bottleneck that expands alongside the data it serves.


## **The Impact of Inter-Rebuild Latency**


Consider a typical scenario: a household views a CTV advertisement at 9:02 AM, visits the site via phone at 9:03 AM, and completes a purchase at 9:15 AM on the computer. If the system only links these devices during a 2:00 AM rebuild, the resulting attribution is either delayed by a full day or credited to the wrong channel. This leads to inaccurate reporting of the conversion journey, resulting in discrepancies to an advertiser's own data.


Diagnosing these issues is exceptionally difficult because data staleness provides no clear evidence; the graph lacks a historical record of its knowledge state at specific times. Without a way to trace discrepancies back to a pending rebuild, this delay becomes a silent source of error. Later down the road, it is mistaken for other technical failures, leading engineering teams on weeks-long investigations before the root cause is identified.


Frequency capping is another problem that often fails because caps are tracked per device until the graph eventually merges them. Take for example, an individual who receives the same ad a dozen times across their TV, smartphone, and laptop in a single afternoon. Each device is staying within policy, but the platform remains unaware until an advertiser presents screenshots in an independent audit highlighting the failure.


The implications for consent management are even more severe. When a consumer opts out at 9:00 AM, a graph that only processes updates during a nightly rebuild will leave the profile active and targetable for many hours afterward. These hours of processing despite withdrawn consent are still recorded and timestamped within the platform's own logs. Under GDPR, several critical mandates apply: consent withdrawal must be as straightforward as providing it (


[Article 7](https://gdpr-info.org/art-7-gdpr/) ), the right to object is effective the moment it is exercised (


[Article 21](https://gdpr-info.org/art-21-gdpr/) ), and the right to erasure does not allow for a "best-effort" eventual update (


[Article 17](https://gdpr-info.org/art-17-gdpr/) ).


Because regulators have become adept at auditing logs, the gap between consent withdrawal and processing serves as a definitive fact for building enforcement actions. Relying on batch processing transforms a strict legal compliance deadline into a best-effort update, documenting the platform's own failure in its audit trail.


## **The circularity of match rates**


Match rate, the industry's standard quality metric for identity graphs, remains inherently circular because it is measured against the very graph it evaluates. It reports the degree of agreement with the existing graph, which makes staleness invisible already with its design. An edge missing today that will not be established until a nightly rebuild does not impact current match rates because the denominator has no knowledge of its absence. While expensive ground-truth panels would reveal these gaps, the healthy figures on a standard dashboard often mask decaying quality. This allows platforms with stale graphs to report match rates identical to those with current data, despite providing vastly different attribution.


## **Why does the batch window keep growing?**


As deterministic identifiers diminish, there is an increase in the proportion of probabilistic edges within the identity graph due to signal losses compounding annually. Because matching requires more compute per decision and the graph expands exponentially relative to its inputs, processing demands surge. This leads to a gradual degradation of the asset: a nightly job stretches to six hours, eventually failing to conclude before the next business day begins; therefore, architectural proposals to rebuild reflect a quiet devaluation of a key differentiator.


The challenge of scale makes architectural choices critical. Distributing batch recomputation of a billion-node graph across a lakehouse is the process that creates the processing window. An alternative approach is


**data sharding**


within a distributed data intelligence engine, where the graph is partitioned using a stable key such as a household.


In this model, incoming signals only impact a segmented shard rather than recomputing everything. This makes incremental resolution feasible with the ability for the workload of the new edge to remain proportional to the edge itself rather than the entire graph.


Success depends on an engine like SingleStore capable of handling transactional updates while simultaneously serving analytical joins, a concept explored further


[in the first post of the series](https://www.singlestore.com/blog/adtech-serving-layer-sprawl) . The trade-offs between utilizing a purpose-built graph database or a distributed SQL engine for this purpose are detailed in a separate deep dive.


## **What does continuous resolution change?**


An incoming signal now resolves incrementally against a live graph, ensuring that new edges are ready for attribution, targeting, and frequency capping within seconds. For instance, a site visit at 9:03 AM connects a mobile device to a household within minutes before a purchase occurs, allowing a 9:15 AM conversion attribution to occur the moment it is recorded.


Compliance follows the same accelerated path; opt-out requests that once took seventeen hours to process now resolve in seconds, providing teams with a defensible audit trail using the same logs that previously documented non-compliance.


The lakehouse remains essential for its intended purpose. Tasks such as offline match-quality research, historical graph analysis, and the development of embedding models are batch-oriented workloads that stay in the lakehouse; however, the serving copy of the graph (aka the real-time decision-making) is a critical component that will no longer be delayed and limited to nightly processing cycles.


## **The obsolescence of the nightly schedule**


The decision to rely on nightly resolution was originally a scale-based compromise from a time when identity graphs were small enough to process within a single window without downstream disruption. Most platforms outpaced that window years ago; however, the practice persists through institutional habit and a lack of defined metrics to quantify the resulting architectural debt. While


[a delayed event simply results in a missed data point](https://www.singlestore.com/blog/ad-data-freshness) , a stale identity edge is more damaging: every subsequent join against it is fundamentally inaccurate.


Beyond the write path, the graph must support heavy read traffic for thousands of concurrent advertiser users every morning on dashboards. The operational impact and financial cost of this concurrency on a traditional warehouse is explored in the next entry.


## On this page


- The Impact of Inter-Rebuild Latency
- The circularity of match rates
- Why does the batch window keep growing?
- What does continuous resolution change?
- The obsolescence of the nightly schedule


## Start building now


Get started with SingleStore Helios today and receive $500 in credits.


[Start free](https://portal.singlestore.com/intention/cloud#UA.utm_ref=%2Fblog%2Fidentity-resolution-without-nightly-batch%2F)


[Engineering](https://www.singlestore.com/blog/category/engineering/)


---


Share


### Don’t miss a thing.
Get the SingleStore newsletter.


## Related reading


[Blog One Copy, One Engine, No Seams Engineering](https://www.singlestore.com/blog/one-copy-one-engine-no-seams/)


[Blog What Happens When Something Goes Wrong Engineering](https://www.singlestore.com/blog/cloud-database-disaster-recovery-rpo-rto/)


[Blog Trust Over Passwords: Mutual TLS in SingleStore Engineering](https://www.singlestore.com/blog/trust-over-passwords-mutual-tls-in-singlestore/)


[Blog Unlocking Serverless MySQL: How Custom Handshake Interception Solved the Multi-Cluster Routing Problem Engineering](https://www.singlestore.com/blog/unlocking-serverless-mysql-how-custom-handshake-interception-solved-the-multi-cluster-routing-problem/)


[Blog Context Engineering: A Definitive Guide Engineering](https://www.singlestore.com/blog/context-engineering-a-definitive-guide/)


[Blog Scaling Time-Series Data for AI Models Product](https://www.singlestore.com/blog/scaling-time-series-data-for-ai-models/)
