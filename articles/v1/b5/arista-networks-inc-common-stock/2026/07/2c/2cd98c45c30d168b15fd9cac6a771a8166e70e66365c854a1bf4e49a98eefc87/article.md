---
schema_version: "1.0.0"
document_id: "2cd98c45c30d168b15fd9cac6a771a8166e70e66365c854a1bf4e49a98eefc87"
company_key: "arista-networks-inc-common-stock"
company: "Arista Networks Inc."
source_id: "arista-networks-inc-common-stock-news-import-c50acbb14cd0"
canonical_url: "https://blogs.arista.com/blog/the-many-facets-of-ai-fabrics"
published_at: null
first_seen_at: "2026-07-21T07:42:03.541009+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:07f51df6ea0dd974ad85dcf4f383995ec0f829e0a2e0a022eacb1762af310ceb"
---

# The Many Facets of AI Fabrics

As computational resources scale to meet the demands of large generative artificial intelligence (AI) models, networking plays a crucial role in improving the utilization of precious cycles from accelerator processing units (XPUs). The network has become the governor of AI performance! Every stalled packet, every microsecond of congestion, translates directly to loss of revenue. Further, well-optimized networks can unlock latent AI performance across distributed XPU systems, unleashing productivity and intelligence at massive scale. In a world of trillion-parameter models and real-time inference, the efficiency of the network defines the efficiency of AI itself. The right network topology and design must be implemented as a leaf spine fabric. Scale-up, scale-out, and scale-across are three key strategies used in network infrastructure design to connect and scale AI accelerators. Let us review these three network AI fabrics.


Scale-Up: High-Speed XPU Interconnect Intra Rack


Vertical XPU scaling can be achieved by interconnecting multiple compute nodes within a single rack using non-blocking, low-latency network switches to achieve shared memory coherency. This allows AI workloads, distributed among multiple XPUs in the same rack, to access unified memory as a single giant pool of resources. XPUs can thus coordinate with each other via non-blocking all-to-all communications, using the unified memory to share any data updates in the shared pool with all XPUs simultaneously. The advantage of this approach is its simplicity: it involves localized computations and can lead to significant performance improvements due to higher computational density.


Modern designs improve XPU density through liquid cooling, enabling more AI accelerators per scale-up rack by reducing heat generation and, in turn, power consumption. This is complemented by low-power, high-bandwidth interconnects like co-packaged copper or optics (CPC/CPO), which provide the interconnect. Such close-knit integration results in a significant reliance on individual elements; consequently, issues such as interconnect link failures or memory errors can cause communication stalls across the whole node, necessitating resolution through appropriate collective controls for traffic within the scale-up fabric.


**Scale-Out: High-speed XPU Interconnect Inter Racks**


Scale-out or horizontal scaling involves adding more machines to a system, moving workloads across multiple servers or nodes, or even connecting other elements like storage, general-purpose CPU and WAN connectivity. Scale-out systems can be dual-mode traversing in both east-west and north-south patterns. They are ideal for distributed training and inference, where tasks can be parallelized across multiple nodes to handle massive datasets and model training. Scale-out network efficiency is driven by network topology economics. By leveraging massive radix, operators can maximize the number of XPUs reachable in a flat two-tier leaf-spine network. This maintains the same bisection bandwidth without incurring the penalty of an extra tier of transceiver and fiber counts for power-conscious AI centers.


**Scale Across: AI Performance Across Distance and Locations**


Scale-across enables expansion across multiple datacenters by interconnecting physically separated AI clusters or pods over large distances. This architecture allows training jobs to span a massive number of XPUs, pooling geographically distributed resources to achieve the aggregate capacity necessary for frontier models. This requires a robust infrastructure that integrates internet, storage, WAN, and optical layers through complex routing features and hierarchical deep buffers needed to absorb the transient congestion and micro-bursts inherent in distributed AI workloads. The integration of advanced traffic engineering, robust encryption, and sophisticated routing ensures that the AI compute clusters remain resilient and secure across multi-tenants.


**Networking for the AI Center: Training & Inference**


**The Next Frontier - Introducing AI Fabrics**


With the relentless growth of AI workloads and demands for performance, the industry is moving beyond isolated, single-purpose networks toward unified AI fabrics. This transforms classic leaf-spine architectures into intelligent, multi-fabric systems that synchronize scale-up, scale-out, and scale-across capabilities as shown below. In this fabric paradigm, the network converges the deterministic RDMA-driven performance required for scale-out clusters with the advanced metro-scale traffic steering needed for distributed deployments. By harmonizing hardware and software networking, customers can get both the economic simplicity of a two-tier design while scaling from thousands to millions of AI accelerators.


Arista’s AI Etherlink™ platforms optimize the Multipath Reliable Connection (MRC) protocol via hardware-accelerated packet trimming and intelligent buffering to minimize tail latency. Multi-planar orchestration isolates traffic across independent fabric planes for deterministic performance and increased resiliency. At this massive scale, the flagship 7800 AI Spine introduces a vital high-radix spine layer, in metro mesh topologies, offloading inter-cluster traffic and enabling seamless routing with uncompromised performance.


**AI Fabrics Deliver One Consistent, Resilient Architecture**


More than a decade ago, Arista pioneered the Universal leaf-spine (CLOS) architecture to replace the rigid, oversubscribed 3-tier legacy data center networks. Traffic patterns have shifted from strictly east-west to massive, synchronized all-to-all or all-reduce bursts of collective communication for AI training and inference. At the same time, bandwidth capacity demands are exploding- from 112G SerDes to 224G and soon 448G per lane, driving exponential terabits of performance in both scale and throughput.


Modern AI centers must adapt to variable traffic patterns. One must simultaneously cope with both the synchronous elephant flows of massive training and the low-latency, concurrent swarms of real-time inference. Traditional, static topologies often struggle with this unpredictability, leading to hotspot jitter that slows job completion time (JCT) or increases Time to First Token (TTFT) for inference. AI fabrics that are adaptive across L1/L2/L3 with data control and management network designs overcome these slowdowns. AI fabrics can be implemented as multi-planar designs to further increase resilience and scale. An example of this is the optimization of the Multipath Reliable Connection (MRC) protocol through hardware-accelerated packet trimming and intelligent buffering, combined with SRv6 micro-segment identifier (uSID) support in EOSⓇ, to minimize tail latency, enabling fine-grained, source-routed steering. Multi-planar orchestration isolates traffic across independent fabric planes for deterministic performance and increased resiliency.


As agentic AI models increase parallelism, and accelerator (XPU) density continues to rise, the AI Fabric removes the rigidity between specialized compute and high-scale cluster networks. In scale-up, the 200G SerDes foundation enables Ethernet Scale-Up Networking (ESUN), providing a memory-semantic, sub-microsecond interconnect that serves as an open-standard alternative to proprietary interconnects. Simultaneously, in scale-out, the fabric moves beyond static routing to topology-aware cluster load balancing (CLB).


In Scale-Across environments, the 7800 AI Spine also leverages SRv6 to unify the data plane across geographically dispersed sites, providing a stateless, end-to-end routing architecture. The result is a unified AI fabric that optimizes AI workloads dynamically as they run. **** Arista EOS differentiators provide the operational elasticity needed to move from siloed and proprietary networks to a truly open, performant AI fabric.


**** As we enter the generative AI era, the network has become the inherent fabric **,** or the elastic **** backplane of the AI infrastructure. The introduction of this architecture is a force multiplier for AI workloads generating billions of parameters and millions of tokens per second. Arista is raising the bar again with a pioneering class of leaf-spine fabric designed for the age of agentic AI, enabling higher utilization, faster training, and lower inference latency for seamless scale-up, scale-out, and across AI fabrics with Arista’s Etherlink portfolio. Welcome to the new era of AI centers!


**References**


[Etherlink Products](https://www.arista.com/assets/data/pdf/Arista-Etherlink-Smart-Networking-Portfolio-Solution-Brief.pdf)


[AI Networking White Paper](https://www.arista.com/assets/data/pdf/Whitepapers/AI-Network-WP.pdf)


[AI Ecosystem Video](https://www.youtube.com/watch?v=FzwydqLKHxI)


[Project Glasswing/Mythos](https://www.arista.com/en/support/advisories-notices/glasswing)
