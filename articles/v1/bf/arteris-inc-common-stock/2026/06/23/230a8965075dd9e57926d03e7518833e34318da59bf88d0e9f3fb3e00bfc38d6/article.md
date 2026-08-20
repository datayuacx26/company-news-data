---
schema_version: "1.0.0"
document_id: "230a8965075dd9e57926d03e7518833e34318da59bf88d0e9f3fb3e00bfc38d6"
company_key: "arteris-inc-common-stock"
company: "Arteris Inc."
source_id: "arteris-inc-common-stock-rss-6a3bbc376771"
canonical_url: "https://www.arteris.com/blog/model-your-ips-and-your-nocs-edn/"
published_at: "2026-06-17T11:40:36+00:00"
first_seen_at: "2026-07-20T23:22:10.429186+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:6b449651e9a9dcf7a372b3ab763aac17072676b04a63c7ad27c837fc1ec5cf24"
---

# EDN: Model your IPs and your NoCs

[Home](https://www.arteris.com/)


»[Newsroom](https://www.arteris.com/newsroom/)


»[Blog](https://www.arteris.com/newsroom/blog/)


» Semiconductor Engineering: Using SystemC TLM Modeling To Solve AI Data Movement Challenges


# Semiconductor Engineering: Using SystemC TLM Modeling To Solve AI Data Movement Challenges


- May 28, 2026


- Andy Nightingale


-


2


min read


In AI silicon, the performance numbers tell only part of the story. Marketing claims often highlight headline metrics such as trillions of operations per second, tensor throughput, matrix dimensions, and accelerator density. But engineers building these systems understand the harder truth. Compute performance matters only when data arrives at the right rate, with the right latency, and without starving the engines that process it.


At the beginning of an AI design project, early exploration is critical. Before teams commit to an architecture, they need to understand how the system behaves under real workload conditions. A block diagram is only a starting point. Engineers need to change assumptions, run use-case-based simulations, and quickly compare architectures. They need to find where bottlenecks start. Teams need to see which paths need more bandwidth, which flows need priority, and whether the system can keep latency-sensitive transactions moving while bulk data transfers continue.


Managing data movement has become a central focus in AI chip design. That is where SystemC transaction-level modeling (TLM) becomes useful. This gives teams a way to study packetized data as it moves through the network-on-chip (NoC), explore behavior, test performance assumptions, and understand whether the interconnect can support the workload before the design reaches RTL.


## AI data movement


AI workloads are not uniform. High-volume data flows depend on sustained bandwidth, while control traffic is smaller and more latency-sensitive. Activation movement, intermediate writebacks, and new reads can overlap, creating changing demand on the NoC interconnect. Some transfers must stay coherent so processors and accelerators see the correct version of shared data. In contrast, high-bandwidth accelerator data movement may be better kept outside the coherent domain, depending on the architecture. At the same time, multiple engines may read from memory while other blocks update control structures or move completed results.


To read the full article on[Semiconductor Engineering](https://semiengineering.com/using-systemc-tlm-modeling-to-solve-ai-data-movement-challenges/) , click[here](https://semiengineering.com/using-systemc-tlm-modeling-to-solve-ai-data-movement-challenges/) .


- [AI / Machine Learning](https://www.arteris.com/tag/ai-machine-learning/) ,[FlexGen](https://www.arteris.com/tag/flexgen/) ,[FlexNoC](https://www.arteris.com/tag/flexnoc/) ,[Network-on-Chip (NoC)](https://www.arteris.com/tag/network-on-chip-noc/)


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


Semiconductor Engineering: Reducing Avoidable Memory Trips In HBM Systems


As AI and high-performance SoCs increasingly rely on HBM, memory bandwidth alone is no longer enough to maximize performance. This article discusses why the intelligent data movement and cache efficiency are critical to unlocking the full benefits of HBM-based architectures.


[Learn more about this](https://www.arteris.com/blog/semiconductor-engineering-reducing-avoidable-memory-trips-in-hbm-systems/)


Blog


Beyond Moore’s Law: Heterogeneous Computing and AI SoCs


As Moore’s Law slows, heterogeneous computing is driving AI, automotive, and data center innovation through specialized compute, chiplets, and advanced interconnects.


[Learn more about this](https://www.arteris.com/blog/beyond-moores-law-heterogeneous-computing-and-ai-socs/)


Blog


EDN: Model your IPs and your NoCs


As SoC and chiplet architectures become increasingly complex, effective modeling must extend beyond functional IP blocks to include the NoC interconnect fabric itself. This article highlights why NoCs have become critical determinants of system performance and explains the value of


[Learn more about this](https://www.arteris.com/blog/model-your-ips-and-your-nocs-edn/)


[View All Blogs](https://www.arteris.com/newsroom/blogs/)
