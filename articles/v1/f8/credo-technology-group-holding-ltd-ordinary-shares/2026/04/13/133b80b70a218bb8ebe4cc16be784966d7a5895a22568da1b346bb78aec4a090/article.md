---
schema_version: "1.0.0"
document_id: "133b80b70a218bb8ebe4cc16be784966d7a5895a22568da1b346bb78aec4a090"
company_key: "credo-technology-group-holding-ltd-ordinary-shares"
company: "Credo Technology Group Holding Ltd"
source_id: "credo-technology-group-holding-ltd-ordinary-shares-news-import-f27688828eed"
canonical_url: "https://credosemi.com/blogs/microled-technology-bringing-reliability-longer-reach-and-lower-power-to-high-throughput-ai-fabrics/"
published_at: "2026-04-22T18:35:12+00:00"
first_seen_at: "2026-08-09T21:09:53.172171+00:00"
fetched_at: "2026-08-09T21:09:54.481524+00:00"
content_hash: "sha256:8e401928f2f02ee638587036e512f0a3e42d4f2e6bda7d5935859b7864f72779"
---

# MicroLED Technology: Bringing Reliability, Longer Reach, and Lower Power to High-Throughput AI Fabrics

By LK Bhupathi, AVP of Product


AI is pushing the boundaries of computing infrastructure to new levels as data centers scale to millions of GPUs. High-speed interconnects play a critical role in enabling this expansion with high-throughput and reliable data movement between compute, memory, and storage systems. New photonic connectivity technologies are emerging to meet the higher bandwidth requirements of GPU-dense architectures.


Credo is innovating across its product portfolio that spans diverse physical mediums, distances, and protocols to meet customer needs. Credo began investing in microLED technology in 2023, and in September of 2025, Credo announced that it had acquired Hyperlume, Inc, a startup based in Ottawa. MicroLED technology addresses the connectivity requirements for high-reliability, power-efficient, high-throughput, and scalable solutions for massive AI data center buildouts.


MicroLED-based interconnects feature links made of wide-parallel channels and utilize a dense array of emitters on one end and photodetectors (PDs) on the other, bridged by a fiber bundle. For example, a 200G link might be split into dozens of optical channels, each consisting of an LED and PD pair operating at less than 10Gbps. With this wide-parallel approach to realize optical links, the marginal cost of additional channels is small and modulation complexity of each channel is significantly reduced.


MicroLED technologies deliver key benefits compared to other optical approaches. Hundreds of parallel LED channels enable higher link reliability through redundancy and failover in the optical path. Link deterioration on the channels can be monitored, predicted, and managed without data loss. MicroLED-based interconnects also minimize complex compensation schemes and reduce power consumption significantly. Using NRZ (non-return-to-zero) modulation at a few Gbps on each channels results in a simplified transmit and receive circuit with minimal or no signal processing required. In addition, micro-emitter-based arrays can scale to higher data-rates and to smaller sizes to deliver higher total throughput and bandwidth density as AI applications place greater demands.


**Networking and Interconnect Applications for AI Infrastructure**


AI infrastructure utilizes three different connectivity fabrics: front-end, scale-out and scale-up networks – all of which are getting faster and denser but have different sensitivities to reliability, latency, and bit error rate (BER). In addition, a new category of point-to-point connectivity application—called scale-in interconnects—demands even higher throughput to connect compute and memory subsystems.


**Front-end networks** process data in and out of the data center and are primarily handling user data. Despite growth of the internet and increasing consumption of video, this part of the data center network is scaling relatively slowly and is the least demanding. Front-end networks run a protocol called TCP which is relatively resilient against instabilities such as link flaps.


**Scale-out networks** connect GPU clusters within a modern data center. They enable many GPU clusters to collaborate on a single model through a process called pipeline parallelism. Scale-out networks are roughly ten times as dense as front-end networks. They use a protocol called UDP which relies on a low-loss interconnect fabric. Packet losses in this network will cause productivity losses in the compute infrastructure.


High reliability and low power have enabled Credo’s ZeroFlap (ZF) active electrical cables (AECs) to become the industry-standard for short-reach communications up to 7m in scale-out networks with full host-to-switch connectivity in leading GPU clusters.


Credo’s microLED solutions will extend that reach, reduce cable-bulk, and enable lower power while still maintaining the reliability for which Credo ZF solutions are known.


**Scale-up networks** connect GPUs within a cluster. They are ten times as dense as scale-out networks or 100x as dense as front-end networks. Today, scale-up networks are implemented only in passive copper inside of an appliance or a rack such as Nvidia’s NVL72. In the future, scale-up networks will expand to row scale (10-30m reach) and need a highly reliable, low power interconnect that microLED technology is perfectly suited for. Optical solutions in various forms are being proposed to make this transition.


**Scale-in interconnects** enable very high throughput, point-to-point connectivity between compute and memory chips. Significantly denser than scale-up interconnects, scale-in interconnects require practically lossless physical layer connectivity. They make off-chip memory behave as if it were near-chip, essentially pushing memory physically and logically “inside” the compute domain. Scale-in interconnects rely on massive concurrency, an attribute native to microLED architecture. The resulting high-throughput, low latency, and power-efficient interconnects deliver higher sustained compute utilization for AI workloads.


*Part two of this blog series will focus on new interconnect form factors and solutions utilizing microLED technology to address the need for bandwidth, optimized power, and low-latency as AI infrastructure scales.*
