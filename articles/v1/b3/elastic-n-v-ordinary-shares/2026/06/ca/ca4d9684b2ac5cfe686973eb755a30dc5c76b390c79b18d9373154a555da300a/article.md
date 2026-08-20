---
schema_version: "1.0.0"
document_id: "ca4d9684b2ac5cfe686973eb755a30dc5c76b390c79b18d9373154a555da300a"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/liveperson-observability"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-08-13T21:20:39.411797+00:00"
fetched_at: "2026-08-13T21:20:42.205031+00:00"
content_hash: "sha256:cf46c5bec681cd845a777ca2545964e98e31fc94efa124600f8d2a040330fd1f"
---

# How LivePerson optimized Logstash and Kafka performance on Google Cloud through benchmarking

# How LivePerson optimized Logstash and Kafka performance on Google Cloud through benchmarking


By benchmarking five Google Cloud machine types across both Logstash and Kafka, LivePerson's observability team found that infrastructure selection (not just pipeline configuration) is one of the highest-leverage cost optimization decisions at scale.


By


[Emily Chioconi](https://www.elastic.co/blog/author/emily-chioconi)[Kiril Karamanolev](https://www.elastic.co/blog/author/kiril-karamanolev)[Strahil Nikolov](https://www.elastic.co/blog/author/strahil-nikolov)


June 16, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


###### Summary


- LivePerson benchmarked Logstash and Kafka across five


Google Cloud


machine type configurations, finding the optimal cost-per-event ratio for a high-volume logging pipeline.


- n4d-standard-2 (AMD Milan) delivered 100%+ throughput improvement on Logstash over the e2-standard baseline.


- Similar gains were observed on Kafka with compression codec selection (LZ4 vs. GZIP), adding a further meaningful performance delta.


-


At ~25% higher monthly cost, n4d-standard-2 processed events at $2.70 per 1,000 EPS versus $5.95 for the least efficient instance tested — less than half the cost per event for the best performer


- Fewer high-throughput instances are required, resulting in a smaller Kafka cluster and lower overall infrastructure overhead.


## The challenge: A pipeline growing faster than its infrastructure was optimized for


As LivePerson expanded its Google Kubernetes Engine (GKE) footprint and migrated workloads from on-premises to Google Cloud, its logging pipeline scaled with it, but the underlying infrastructure choices hadn't kept pace.


The pipeline runs Filebeat as the log producer, Kafka as the message broker, and Logstash as the consumer and processor before data lands in Elastic. As the system grew, Kafka scaled with it. Because each Logstash instance requires a dedicated partition to consume from, Logstash instances had to scale proportionally. More instances meant more partitions. More partitions meant a larger, more expensive Kafka cluster. The overhead was compounding.


The infrastructure was running on e2-standard instances, Google's general-purpose option, and a common default for logging workloads. Kiril Karamanolev and Strahil Nikolov, DevOps engineers on LivePerson's observability team, suspected there was room to improve. But the team had inherited the stack and never had a baseline to compare against. Without data, there was no way to know whether the current setup represented good value or a significant missed opportunity.


There was also a subtler problem: e2-standard instances are not tied to a single CPU platform, which means the same machine type can run on AMD or Intel hardware depending on availability. For compute-intensive workloads like Logstash, that distinction can have real performance implications, but only benchmarking would reveal how large the gap actually was.


## The approach: Benchmark both ends of the pipeline


Rather than making assumptions, the team ran structured benchmarks across both Logstash and Kafka, treating each as a separate workstream.


**Logstash benchmarking**


used the built-in Logstash benchmarking framework with standardized test data and configurations, keeping results consistent across instance types. The machine types evaluated were those realistically considered for high-volume logging on


Google Cloud


:


-


e2-standard-2 (AMD)


-


e2-standard-2 (Intel)


-


n2-standard-2


-


t2d-standard-2


-


n4d-standard-2


AMD and Intel variants of e2-standard-2 were treated as separate test cases, given the potential for meaningful CPU-level performance differences.


**Kafka benchmarking**


used Kafka's native producer and consumer benchmark scripts with a custom wrapper to streamline execution. The team also tested the impact of compression codec selection, specifically GZIP (Filebeat’s default compression codec) against LZ4, on both write throughput (Filebeat > Kafka) and read throughput (Kafka > Logstash).


AI-assisted analysis was used to process and interpret these performance metrics, ensuring that no personal or customer data was involved.


The evaluation metric throughout was not monthly instance cost but throughput per dollar (events per second per unit of spend) — the figure that actually matters at production scale.


## Results


### Logstash performance


The n4d-standard-2, backed by AMD Milan architecture, delivered a decisive result:


-


100%+ throughput improvement over the e2-standard-2 baseline


-


370 EPS per dollar, the strongest cost efficiency of any instance tested


-


$2.70 per 1,000 EPS, compared to $5.95 for the least efficient configuration tested, over 50% reduction in processing cost


-


t2d-standard-2 ranked second, offering strong value for regions where n4d availability is limited


-


The performance gap between AMD and Intel variants of e2-standard-2 was large enough to be operationally significant


-


n2-standard-2 ranked last on efficiency despite carrying essentially the same monthly price as t2d-standard-2


The clearest takeaway: the cheapest monthly instance is rarely the most cost-efficient once actual throughput is accounted for.


### Kafka performance


Benchmarking Kafka produced two key findings:


-


Machine type selection had a significant impact on Kafka throughput; consistent with the Logstash results


-


Compression codec choice was an independent variable with meaningful impact on both write and read performance. Switching from GZIP to LZ4 delivered throughput gains on both the producer and consumer side.


Testing both halves of the pipeline together was essential. A Kafka bottleneck throttles the entire system: If logs can't be written fast enough, Filebeat backs up; if they can't be read fast enough, Logstash goes underutilized. Optimizing only one side would have given an incomplete picture.


### Infrastructure footprint reduction


The most consequential outcome was structural. Higher per-instance throughput directly reduces the number of Logstash instances required to handle the same volume. Fewer Logstash instances means fewer required Kafka partitions. Fewer partitions means a smaller Kafka cluster with lower overhead across the board.


The optimization compounds: Better instance selection reduces component count, which reduces operational complexity and cost while improving performance at the same time.


## The broader lesson


Cloud providers release new instance families continuously. What represented good value at initial deployment may no longer be competitive one or two years later. LivePerson's team recommends treating infrastructure benchmarking as a recurring practice rather than a one-time decision, particularly for teams running high-volume observability workloads where compute efficiency directly affects both cost and pipeline stability.


The specific findings here reflect


Google Cloud


. Equivalent benchmarking on AWS or Azure would likely surface different winners. The recommendation is not a machine type, it is a methodology: Measure before you commit, and revisit the question periodically as the cloud landscape changes.


## Measuring efficiency at scale


For a company like LivePerson — whose platform supports millions of enterprise customer conversations daily and processes tens of terabytes of log data across distributed GKE infrastructure — the difference between a well-optimized pipeline and a default one isn't abstract. It shows up in infrastructure spend, in system headroom, and in the team's capacity to focus on higher-value work instead of managing an oversized cluster.


The benchmarking work described here is one part of a broader effort to bring LivePerson's observability platform fully up to date as its cloud migration matures. The methodology, however, applies well beyond any single migration; in an environment where cloud providers continuously release new instance generations, the cost of never re-evaluating your infrastructure choices compounds quietly over time.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
