---
schema_version: "1.0.0"
document_id: "b221cbd28a6b8e380d06847a93e3cbe49378468fb4524e75e23eeeefb8d3e504"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2025/08/13/inside-ue-1-0-what-ultra-ethernet-means-for-ai-and-hpc-networks/"
published_at: "2025-08-13T12:46:14+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T20:56:33.291545+00:00"
content_hash: "sha256:1d218caccc90152644f0de4c4254994350908e9e490c9ba89d845f7f53c86e8a"
---

# Inside UE 1.0: What Ultra Ethernet Means for AI and HPC Networks

**


# Inside UE 1.0: What Ultra Ethernet Means for AI and HPC Networks


- **[VIAVI Staff](https://blog.viavisolutions.com/author/viavi-staff/)
- ** August 13, 2025
- Like


##### Ultra Ethernet is here. With UE 1.0 released, the focus shifts to testing and compliance. Learn how the first public UET test and evolving assurance approaches are enabling performance, interoperability, future-ready infrastructure for AI and HPC networks.


---


After intense and dedicated work, the[Ultra Ethernet Consortium (UEC)](https://ultraethernet.org/)


has released its highly anticipated[UE Specification 1.0](https://ultraethernet.org/uec-1-0-spec)


to meet workload requirements posed by data-intensive AI and high-performance computing (HPC) data centers and networks. These requirements span increased scale and speed, higher bandwidth density, low latency, no or minimal packet loss and multipathing.


UE Specification 1.0 is up to the challenge.


Building on widely deployed Ethernet and IP, UE 1.0 efficiently optimizes Ethernet for AI and HPC networking and targets network scale-out that distributes workloads across multiple devices. The specification delivers a high-performance, scalable, and interoperable solution across all layers of the networking stack, including physical, link, network, transport, and software layers.


UEC open standard specifications leverage the ubiquity and flexibility of Ethernet, with the UE 1.0 suite of protocols and technology (UET) running on top of Ethernet and IP without changing the core Ethernet layer. New APIs are optimized to future workloads and compute architectures while remaining backward-compatible with popular APIs. UE powers horizontal scalability via a new Ultra Ethernet Transport protocol (UET) that builds on RDMA (Remote Direct Memory Access) to deliver data directly between the network and application memory without CPU involvement.


This blog captures our latest perspective and takeaways on the newly released UE specification and the importance of testing in ensuring high-performance, interoperable implementations that meet the real-world demands of AI and HPC networks. It follows insights from[our previous blog](https://blog.viavisolutions.com/2025/02/21/ultra-ethernet-stack-1-0-a-game-changer-but-proper-assurance-is-key/) written as the spec emerged.


### UET Innovation Highlights


The UE Transport (UET) protocol incorporates multiple elements that go beyond RDMA, such as multipathing, relaxed delivery ordering, rapid loss recovery, modern data center congestion control, ordered and unordered delivery, and built-in security.


UET also supports **packet spraying,** in which packets from a single transfer are carried over all the viable paths from source to destination. By ensuring all paths get used equally, fabric hot spots caused by imperfect load balancing of very large flows—a major problem today—are avoided.


A challenge with packet spraying is accurately and rapidly detecting when and which packets are lost. To **overcome packet loss** , UET leverages packet trimming when a packet arrives at a congested switch. Rather than dropping it, the packet is truncated and placed into a higher priority queue. This relieves the congestion and delivers a fast and precise signal as rapidly as possible to the receiver. The trimmed packet alerts the receiver to both reduce transmission speed and precisely identify which packets require retransmission.


**Congestion control** is also an important issue for a packet-sprayed environment that UET addresses. UET defines new sender and receiver-based congestion control algorithms while also maintaining stability. With high bandwidth HPC and AI traffic, the transport protocol needs to start off at wire rate because an entire transfer might only last a few round trips. UET supports rapid connection startup enabling data to be transmitted before a handshake completes. This optimizes performance for short transfers and minimizes state cost by letting idle connections get torn down without a restart penalty.


**Security** is table stakes for UET applications. It provides end-to-end encryption and authentication, leveraging proven technologies, key derivation functions, and replay prevention. UET adds a new group keying scheme for the group computations that are common to AI and HPC.


### UEC Progress on Compliance


The UEC specifications are a major evolution in Ethernet, making it essential that test and assurance methodologies also evolve to realize UE’s full potential. High-level performance, scalability, interoperability, and additional UE features need to be validated. The UEC intends to provide information so that equipment makers and networks understand what they must do to be compliant and to provide clarity to test and assurance providers on what needs to be evaluated.


The overarching goal of Ultra Ethernet is to support AI and HPC with low latency and lossless transmission, compared to traditional Ethernet that allows high latency and loss.


Traditional test approaches are not powerful enough to adequately validate Ultra Ethernet. New assurance models, multi-KPI validation, and real-world traffic generation and modeling are essential for success.


The UEC is including compliance criteria in their specifications so technology implementers can learn what they need to do to ensure their implementation is UE compliant.


The UE 1.0 specifications include PHY and Link-Layer profile matrices and compliance checklists. Progress has been made on UET compliance. The packet delivery sub-layer (PDS) compliance checklist has been released. The trimming and semantic sub-layer (SES) compliance checklists will be released soon. Congestion management compliance will be addressed next.


The UEC Performance and Debug Working Group is also creating performance test criteria including tests and test scenarios for different types of networks. These will provide common performance methods and measurements. The compliance page will continue to be updated as the UEC releases new features in future specifications.


### The First Public UET Test


Network equipment makers are already creating new equipment to carry UE traffic. At Interop25 Tokyo, we showcased the first public UET test with an interoperability with Juniper Networks


. At the event, a new Juniper switch carried Ultra Ethernet traffic via a preliminary UET interconnection.


VIAVI, Juniper, and TOYO Corporation jointly generated and forwarded UET traffic in a live network environment utilizing the[B3 800G Appliance](https://www.viavisolutions.com/en-us/products/800g-ethernet-testing)


and the[Juniper QFX 5240-64OD Switch](https://www.juniper.net/us/en/products/switches/qfx-series/qfx5240-data-center-switches.html)


. The test topology also included RoCEv2 traffic, demonstrating seamless coexistence and interoperability, and earned the companies a coveted Interop ShowNet Special Prize. The Juniper switch with 800G interfaces successfully recognized and forwarded all traffic types, validating its readiness for UET-based deployments.


The successful real-world validation highlights our ability to emulate and test emerging UET transport standards, and Juniper’s capability to support evolving Ethernet technologies, ensuring customers can validate and future-proof their infrastructure for AI scalability and Ultra Ethernet performance.


### Solutions for Ultra Ethernet Use Cases


Partners in the UE ecosystem may have varying testing needs, from needing to validate high-speed Ethernet deployments to ensuring their UE stack’s compliance.


To support this range of requirements, we’ve developed a flexible, phased approach to align with the typical stages of product development. This approach enables targeted testing and validation at each layer of the UE stack—helping teams ensure performance, interoperability, and readiness without overcommitting resources early in the cycle.


**1. Transport Layer Validation:** for initial UE testing, our 800G Ethernet test and validation platform determines whether the UE transport layers are communicating with each other properly. Since UE is fundamentally built on Ethernet, no additional equipment is needed. This helps ensure seamless integration at the foundational level before moving forward.


**2. Physical and Link Layer Testing:** Once transport functionality is confirmed, the next step is to validate the UE stack on networks that support physical and link layer functions. This phase focuses on functional interoperability, evaluating how well the system handles packet generation, analysis, filtering, and Layer 2-3 traffic patterns. Real-world emulation of various device types, users, and protocols helps assess connectivity, communication efficiency, and congestion control.


**3. Full-Stack Performance Assessment:** The final stage expands to a comprehensive performance test across the full UE stack, encompassing physical, link, network, transport, and software layers (real AI workloads). The aim is to validate performance under real-world conditions, including variable packet sizes, higher speeds, and complex traffic scenarios. This stage will provide assurance that the UE can operate reliably at scale and under demanding conditions.


### What’s Next


AI will continue to evolve alongside Ultra Ethernet specifications. The UEC is working on additional capabilities to meet the needs of future Ethernet-based AI and HPC networks. The next UE specification will focus on scale-up networks and incorporate new ideas such as improved telemetry and congestion control, UE bindings for storage protocols, in-network compute, and new file input formats.


Testing will be required to measure and ensure compliance as the network specifications evolve.


We are committed to supporting emerging standards with comprehensive testing solutions that enable the transition to next-generation networking, while maintaining performance and reliability in increasingly demanding environments.


Learn more about Ultra Ethernet test and assurance in our earlier[Ultra Ethernet Stack 1.0: A Game Changer, But Proper Assurance is Key](https://blog.viavisolutions.com/2025/02/21/ultra-ethernet-stack-1-0-a-game-changer-but-proper-assurance-is-key/)


blog.


- ** Categories:[AI](https://blog.viavisolutions.com/category/ai/) ,[Channel Emulation](https://blog.viavisolutions.com/category/hse-ce/channel-emulation/) ,[High-Speed Ethernet](https://blog.viavisolutions.com/category/hse-ce/high-speed-ethernet/) ,[HSE and CE](https://blog.viavisolutions.com/category/hse-ce/)
- ** Tags:[CE](https://blog.viavisolutions.com/tag/ce/) ,[Channel Emulation](https://blog.viavisolutions.com/tag/channel-emulation/) ,[High-Speed Ethernet](https://blog.viavisolutions.com/tag/high-speed-ethernet/) ,[HSE](https://blog.viavisolutions.com/tag/hse/)


-
-
-
-
-
-
