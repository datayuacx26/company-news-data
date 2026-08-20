---
schema_version: "1.0.0"
document_id: "55dd66f9acee33b4f305d8e64d041314fb2b102228f4e43c87b6f23f60bca717"
company_key: "arteris-inc-common-stock"
company: "Arteris Inc."
source_id: "arteris-inc-common-stock-news-import-b55a70a8b442"
canonical_url: "https://www.arteris.com/blog/semiconductor-engineering-reducing-avoidable-memory-trips-in-hbm-systems/"
published_at: "2026-06-25T08:22:27+00:00"
first_seen_at: "2026-07-23T02:24:35.785039+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:e2aa3b300e58a687609bc6acb9c4f6713c6dfad81fea10490d3299e0a829d75b"
---

# Semiconductor Engineering: Reducing Avoidable Memory Trips In HBM Systems

[Home](https://www.arteris.com/)


»[Newsroom](https://www.arteris.com/newsroom/)


»[Blog](https://www.arteris.com/newsroom/blog/)


» Semiconductor Engineering: Reducing Avoidable Memory Trips In HBM Systems


# Semiconductor Engineering: Reducing Avoidable Memory Trips In HBM Systems


- Jun 25, 2026


- André Bonnardot


-


< 1


min read


Picture a highway during rush hour. When a road has limited capacity, traffic backs up quickly because only so many cars can move through at once. Adding more lanes increases capacity, but it does not always guarantee a smoother commute. If cars keep flooding onto the highway, if exits are poorly placed, or if drivers have to stay on the road for long distances, congestion can still build. More lanes help, but the system still depends on how efficiently traffic moves.


Memory systems face many of the same challenges. High-bandwidth memory (HBM) enables advanced AI accelerators and high-performance systems-on-chip (SoCs) to move large data sets quickly.


## When bandwidth is not enough


This is where memory hierarchy becomes important. Even when total throughput is high, bandwidth determines how much data can move, while latency determines how quickly the system can respond. However, increased memory bandwidth does not eliminate delays. Each round trip to external memory adds time before the compute engine can continue, creating idle cycles that can become a performance bottleneck. When data is fetched suboptimally, HBM systems can hide inefficiencies in bandwidth headroom while still suffering from poor data reuse, unpredictable access patterns, and repeated trips outside the compute die.


A practical answer is to keep more reusable data on chip. A last-level cache (LLC) provides a solution because it sits between compute engines and external memory, as shown in Figure 1. CPUs, GPUs, NPUs, and other accelerators typically include their own local caches to reduce access latency for frequently used data. However, when data must be shared across engines or exceeds the capacity of the smaller caches, the LLC provides a common cache layer that can satisfy those requests before they reach external memory.


To read the full article on[Semiconductor Engineering](https://semiengineering.com/reducing-avoidable-memory-trips-in-hbm-systems/) , click[here](https://semiengineering.com/reducing-avoidable-memory-trips-in-hbm-systems/) .


- [AI / Machine Learning](https://www.arteris.com/tag/ai-machine-learning/) ,[CodaCache](https://www.arteris.com/tag/codacache/) ,[FlexGen](https://www.arteris.com/tag/flexgen/) ,[Low Power](https://www.arteris.com/tag/low-power/) ,[Ncore](https://www.arteris.com/tag/ncore/)


Blogs


## **Latest** Blogs


[View All Blogs](https://www.arteris.com/newsroom/blogs/)


Blog


Semiconductor Engineering: Avoid The Hidden Bottleneck Of Integration At Scale


This Semiconductor Engineering article examines how SoC integration has become a major bottleneck as designs scale in complexity, with growing numbers of IP blocks, registers, and hardware/software interfaces.


[Learn more about this](https://www.arteris.com/blog/semiconductor-engineering-avoid-the-hidden-bottleneck-of-integration-at-scale/)


Blog


The AI Journal: Making chiplets work for AI requires more than connectivity


This article explains why building successful AI chiplet architectures requires more than high-speed die-to-die connectivity. It explores how efficient data movement, protocol selection, coherency, and intelligent NoC architecture are critical to maximizing performance, scalability, and energy efficiency in next-generation AI


[Learn more about this](https://www.arteris.com/blog/the-ai-journal-making-chiplets-work-for-ai-requires-more-than-connectivity/)


Blog


What the Cyber Resilience Act means for the future of chip design


The EU Cyber Resilience Act is reshaping semiconductor security, making cybersecurity, compliance, and lifecycle management core design priorities.


[Learn more about this](https://www.arteris.com/blog/what-the-cyber-resilience-act-means-for-the-future-of-chip-design/)


Blog


Beyond Moore’s Law: Heterogeneous Computing and AI SoCs


As Moore’s Law slows, heterogeneous computing is driving AI, automotive, and data center innovation through specialized compute, chiplets, and advanced interconnects.


[Learn more about this](https://www.arteris.com/blog/beyond-moores-law-heterogeneous-computing-and-ai-socs/)


Blog


EDN: Model your IPs and your NoCs


As SoC and chiplet architectures become increasingly complex, effective modeling must extend beyond functional IP blocks to include the NoC interconnect fabric itself. This article highlights why NoCs have become critical determinants of system performance and explains the value of


[Learn more about this](https://www.arteris.com/blog/model-your-ips-and-your-nocs-edn/)


Blog


Cyber Defense Magazine: Securing The Unseen Why Data in Motion is the Next Cybersecurity Frontier


Securing the unseen: As AI drives massive data movement inside chips, organizations must secure and monitor data in motion to close hidden hardware attack surfaces.


[Learn more about this](https://www.arteris.com/blog/cyber-defense-magazine-securing-the-unseen-why-data-in-motion-is-the-next-cybersecurity-frontier/)


[View All Blogs](https://www.arteris.com/newsroom/blogs/)
