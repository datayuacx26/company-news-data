---
schema_version: "1.0.0"
document_id: "052cc3cd59203ec299d7103a30e4803dc32503f550fe994a7ad8de6034df0168"
company_key: "rambus-inc-common-stock"
company: "Rambus Inc."
source_id: "rambus-inc-common-stock-news-import-b51b587c8dec"
canonical_url: "https://www.rambus.com/blogs/the-ultimate-guide-to-pci-express/"
published_at: "2026-06-11T16:52:31+00:00"
first_seen_at: "2026-07-25T20:32:34.030231+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:3dac5911e40fff7eeaff303215c54e13d75054c345116efc828c68db4d91eaff"
---

# The Ultimate Guide to PCI Express: Specs, Features, Use Cases and PCIe 7.0

PCI Express, commonly called PCIe, is the high-speed interconnect standard used to move data between key system components inside modern electronic systems. It connects processors, CPUs, GPUs, storage devices, accelerators and networking interfaces, enabling fast and efficient communication across servers, PCs, embedded platforms and cloud infrastructure. Over time, PCIe has become the industry’s default choice for high-bandwidth, low-latency I/O because it continues to scale performance while preserving backward compatibility across generations.


That scalability matters more than ever as AI infrastructure, cloud platforms, storage systems and high-speed networks continue to drive higher data movement requirements. The latest specification, PCIe 7.0, was officially[released](https://pcisig.com/specifications/pcie-70-specification-version-03-now-available-members) to PCI-SIG members on June 11, 2025, and doubles the raw data rate of PCIe 6.x from 64.0 GT/s to 128.0 GT/s, delivering up to 512 GB/s of bidirectional bandwidth in a x16 configuration.


PCIe 7.0 is designed for data-intensive applications including AI and machine learning, hyperscale data centers, high-performance computing, high-speed networking, cloud infrastructure, automotive, military and aerospace, and quantum computing. It also continues the architectural transition introduced in PCIe 6.0, including PAM4 signaling and FLIT-based encoding, while improving power efficiency and supporting emerging optical interconnect use cases.


## PCIe 7.0 at a glance


- Latest specification: PCIe 7.0, officially released to PCI-SIG members on June 11, 2025.
- Maximum raw data rate: 128.0 GT/s
- Maximum bidirectional bandwidth: Up to 512 GB/s in a x16 configuration
- Core technologies: PAM4 signaling, FLIT-based encoding and FEC
- Key target markets: AI/ML, HPC, data centers, cloud, networking, automotive, military/aerospace and quantum computing
- Compatibility: PCIe 7.0 maintains backward compatibility with previous generations


##### **Table of Contents:**


-


- What is PCI Express?
- Why PCIe matters in modern systems
- PCIe evolution: from earlier generations to PCIe 7.0
- What is new in PCIe 7.0
- Key PCIe terms explained: PAM4, FEC, FLIT mode, PIPE and AXI
- What markets and verticals does PCIe 7.0 serve?
- How does PCIe 7.0 compare with PCIe 6.0 and PCIe 5.0?
- Why PCIe controllers matter in chip design
- What are PCIe switches, DMA and USB4 in the PCIe ecosystem?
- How Rambus PCIe IP helps accelerate PCIe design
- Conclusion
- Frequently asked questions about PCI Express


## What is PCI Express?


PCI Express is a **high-speed serial, point-to-point interconnect standard** that enables data transfer between system components. Instead of sharing a common parallel bus like older interfaces, PCIe creates dedicated links between components, which helps improve throughput, reduce contention and support scalable lane widths such as x1, x2, x4, x8 and x16. Modern PCIe controller IP supports these common lane configurations across multiple generations.


PCIe is used throughout modern computing platforms, from enterprise servers and accelerator cards to storage devices, embedded systems and communications infrastructure. Its long-standing strength is that each new generation typically increases available bandwidth while maintaining compatibility with earlier generations. PCIe has served as the de facto high-bandwidth interconnect for more than two decades and continues to evolve to meet growing compute demands.


## Why PCIe matters in modern systems


Modern compute platforms rely on moving massive amounts of data quickly and efficiently. For example:


- AI training systems move large datasets between CPUs, GPUs and accelerators
- Data centers handle high-throughput networking and distributed workloads
- Storage systems require low-latency access to large data volumes


PCIe sits at the center of these data paths. As performance demands grow, PCIe must scale bandwidth without significantly increasing cost, power consumption or system complexity.


PCIe has historically achieved this through a predictable cadence of **doubling bandwidth each generation** . PCIe 7.0 continues that pattern by doubling the raw bit rate relative to PCIe 6.x while targeting better power efficiency and broader deployment flexibility, including optical interconnect support.


## How has PCIe evolved from earlier generations to PCIe 7.0?


PCIe has advanced steadily, increasing throughput with each generation:


PCIe Generation Max Data Rate Notable characteristics


PCIe 2.1 5 GT/s Early high-speed serial PCIe generation


PCIe 3.1 8 GT/s Higher bandwidth for mainstream compute and storage.


PCIe 4.0 16 GT/s Doubled throughput for faster accelerators, SSDs and networking.


PCIe 5.0 32 GT/s Expanded bandwidth for advanced servers and high-speed peripherals.


PCIe 6.x 64 GT/s Introduced PAM4 signaling, FLIT mode and FEC to continue scaling without simply doubling fundamental frequency in the same way as earlier NRZ generations.


PCIe 7.0 128 GT/s Doubles PCIe 6.x bandwidth, keeps PAM4 and FLIT-based encoding, improves power efficiency and supports optical interconnect-related advancements.


PCIe 6.0 marked a major transition because it introduced PAM4 signaling, FLIT mode and FEC to keep scaling bandwidth without relying on the same signaling approach used in earlier NRZ-based generations. PCIe 7.0 builds on that same architectural foundation at a higher data rate.


## What is new in PCIe 7.0?


The headline improvement in PCIe 7.0 is speed. The specification reaches 128.0 GT/s, double the data rate of PCIe 6.x, and delivers up to 512 GB/s bidirectional throughput in a x16 link. That increase is aimed squarely at systems that need more bandwidth density, such as AI clusters, HPC systems and hyperscale infrastructure.


The video below outlines what’s new in PCIe 7.0:


PCIe 7.0 also continues the use of PAM4 signaling and FLIT-based encoding, building on the major architectural change introduced in PCIe 6.0. PCI-SIG also highlights improved power efficiency, backward compatibility with previous generations, and support for optical interconnect solutions that can extend reach across racks and pods while enabling data mapping across electrical and optical domains.


In practical terms, PCIe 7.0 is not just a faster version of PCIe. It is part of a broader interconnect strategy for environments where bandwidth, scale, reach and efficiency all matter at once. These improvements are specifically designed for bandwidth-intensive applications such as AI, hyperscale infrastructure and high-speed networking.


## What do key PCIe terms mean: PAM4, FEC, FLIT mode, PIPE and AXI?


#### What is PAM4 signaling?


PAM4 is a signaling technique that uses four voltage levels to encode two bits per symbol, enabling higher data rates without proportionally increasing clock frequency. PCI-SIG identifies PAM4 as a key enabler of both PCIe 6.0 and PCIe 7.0 bandwidth scaling.


PAM4 lets PCIe send more information in each signaling interval than traditional NRZ signaling, which is why it became necessary as the standard moved to 64 GT/s and then 128 GT/s.


#### What is FEC in PCIe?


Forward Error Correction, or FEC, is a low-latency error correction method used to improve data reliability at very high signaling rates. PCIe 6.0 introduced FEC to address the higher bit error rate associated with PAM4 signaling, and PCIe 7.0 continues using that same approach.


FLIT mode was adopted in part because error correction needs to operate on fixed-size packets, and that the combination of FEC and CRC helps maintain high reliability with low overhead.


#### What is FLIT mode?


FLIT mode organizes PCIe traffic into fixed-size Flow Control Units rather than variable-size packet handling used in older PCIe generations. In PCIe 6.0 and PCIe 7.0, FLIT-based encoding supports efficient transport at very high speeds and works together with FEC and CRC.


FLIT structure remains 256 bytes in both PCIe 6.0 and 7.0, with the same basic layout for Transaction Layer Packets, Data Link Protocol content, CRC and FEC bytes.


#### What is PIPE?


PIPE stands for PHY Interface for PCI Express and defines the interface between the PCIe controller and the physical layer implementation. This matters because a standards-aligned PIPE interface helps SoC designers combine controller IP with PIPE-compliant SerDes implementations.


#### What is AXI?


AXI, or Advanced eXtensible Interface, is part of Arm’s AMBA specification and is widely used for high-performance on-chip communication in SoCs. Arm’s AMBA AXI Protocol Specification documents AXI as part of the AMBA family and describes its role in supporting modern on-chip data movement.


In PCIe-based SoCs, AXI is important because it provides an efficient internal interface for connecting processors, memory and accelerators. That makes AXI support especially relevant when integrating PCIe controller IP into complex chip architectures.


## What markets and verticals does PCIe 7.0 serve?


PCIe 7.0 is aimed at data-intensive markets. These include hyperscale data centers, high-performance computing, automotive, military and aerospace, plus emerging applications such as AI and machine learning, 800G Ethernet, cloud computing and quantum computing.


Here is how that maps to real-world needs:


- AI and machine learning need fast movement of data among CPUs, GPUs and accelerators. Higher PCIe bandwidth helps feed compute resources more effectively.
- Hyperscale data centers and cloud platforms benefit from greater interconnect density, improved efficiency and the ability to scale traffic across disaggregated resources.
- HPC systems require very high throughput and low-latency connectivity between compute nodes, storage and networking elements.
- 800G Ethernet and high-speed networking depend on host and accelerator interfaces that can keep pace with ever-faster network pipes.
- Automotive and aerospace/defense increasingly rely on high-bandwidth internal connectivity as systems integrate more sensors, compute and real-time data processing.
- Quantum and emerging compute architectures need scalable interconnect fabrics that can grow with new compute models.


## How does PCIe 7.0 compare with PCIe 6.0 and PCIe 5.0?


**PCIe 7.0 vs PCIe 6.0**


PCIe 7.0 doubles the raw data rate of PCIe 6.0 from 64 GT/s to 128 GT/s, while continuing to use PAM4 signaling, FLIT-based encoding and FEC. According to PCI-SIG’s PCIe 7.0 webinar Q&A, the FLIT-based encoding and FEC model remain effectively the same between PCIe 6.0 and 7.0, with the bandwidth increase coming from higher clocking.


**PCIe 7.0 vs PCIe 5.0**


PCIe 7.0 delivers four times the raw data rate of PCIe 5.0, moving from 32 GT/s to 128 GT/s. It also reflects a major architectural change relative to PCIe 5.0 because PCIe 7.0 relies on PAM4 signaling, FLIT-based encoding and FEC rather than the earlier NRZ-based approach used before PCIe 6.0.


## Why PCIe controllers matter in SoC and ASIC design


A PCIe specification defines the standard, but designers still need implementation IP that can turn that standard into a production-ready subsystem. That is where PCIe controllers come in. Controllers handle protocol compliance, data packet management, link initialization, link initialization and training, error handling and reliability features, and integration with SoC fabrics and memory systems.


For advanced SoCs, controller IP must do more than support the headline data rate. It must also support the right topologies, lane configurations, virtual channels, reliability features, security capabilities and integration interfaces.


As PCIe moves to more advanced signaling and encoding methods, controller design becomes increasingly complex and critical to system performance.


## What are PCIe switches, DMA and USB4 in the PCIe ecosystem?


PCI Express is often described as a point-to-point interconnect between a processor and a device, but modern systems rely on a broader set of technologies to fully realize its performance and scalability. Components such as PCIe switches, Direct Memory Access (DMA) engines and USB4 integration extend PCIe beyond simple device connectivity, enabling more flexible architectures and more efficient data movement across increasingly complex systems.


### What is a PCIe switch?


PCIe switch is a connectivity device that enables multiple endpoints to share a single upstream PCIe link. Rather than connecting devices directly to the host processor, a switch allows designers to expand the number of available connections by creating multiple downstream ports from one upstream interface.


This capability is especially important in systems that require high device density. In AI and hyperscale environments, for example, a PCIe switch allows multiple GPUs, accelerators or SSDs to communicate with a host processor without requiring a dedicated PCIe link for each device. The switch manages traffic between endpoints and ensures efficient data flow across the system.


As system architectures continue to evolve toward disaggregation and composability, PCIe switches also play a critical role in enabling more flexible topologies. They allow resources such as storage and compute accelerators to be pooled and dynamically shared, which improves utilization and scalability without significantly increasing system complexity.


#### Where are PCIe Switches implemented?


With PCIe 6 & 7, PCIe switches are primarily implemented in advanced networking devices like SmartNICs and SuperNICs. These devices sit at the intersection of compute, networking, and acceleration, and they rely on high-performance PCIe fabrics to move massive amounts of data efficiently between CPUs, GPUs, storage devices, and the network.


A SmartNIC, or smart network interface card, goes beyond traditional NIC functionality. Instead of simply moving packets between the server and the network, a SmartNIC offloads infrastructure tasks from the CPU. This can include things like network virtualization, storage processing, security functions, and telemetry. By offloading these tasks, SmartNICs free up CPU cycles for application workloads, which is especially important in cloud and hyperscale environments.


The SuperNIC is essentially an evolution of the SmartNIC concept, but purpose-built for the demands of large-scale AI and accelerated computing environments. While SmartNICs focus on offloading infrastructure services, SuperNICs are designed to support extreme data movement and coordination across clusters of GPUs and accelerators.


In AI systems, SuperNICs are architected to handle orchestrating communication across thousands of GPUs, often in tightly synchronized training workloads. They incorporate advanced networking capabilities, high radix connectivity, and deep integration with accelerator fabrics.


### What is DMA in PCIe?


Direct Memory Access, or DMA, is a fundamental mechanism that enables high-speed data transfers between devices and system memory without continuous intervention from the CPU. In PCIe-based systems, DMA engines allow peripherals such as GPUs, network adapters and storage devices to read from and write directly to system memory.


This approach significantly improves system efficiency. Without DMA, the processor would need to manage each data transfer, which would introduce overhead and reduce the amount of compute resources available for applications. By offloading these transfers to DMA engines, systems can maintain high throughput while allowing the CPU to focus on higher-level tasks.


DMA is particularly important in data-intensive workloads such as AI training, high-performance computing and networking. These applications often involve moving large volumes of data at very high speeds, and DMA enables that data movement to occur efficiently in parallel with compute operations.


### What is USB4 and how does it relate to PCIe?


USB4 represents an important extension of PCIe beyond internal system connectivity. It is a high-speed interface that supports the tunneling of PCIe traffic over a USB Type-C connection, alongside other protocols such as DisplayPort and traditional USB data.
This capability allows PCIe to be used in external applications, not just inside a system. Devices such as external GPUs, high-performance storage solutions and advanced docking stations can leverage PCIe through USB4 to deliver high-bandwidth connectivity outside the chassis.


From an architectural perspective, PCIe continues to serve as the underlying protocol for many high-performance data transfers, even when those transfers occur over a USB4 connection. USB4 effectively acts as a transport layer that carries PCIe traffic, enabling a unified interface for multiple data types while preserving PCIe’s performance characteristics.
Why these components matter for PCIe 7.0


As PCIe 7.0 increases available bandwidth to unprecedented levels, the surrounding ecosystem becomes even more important. Greater bandwidth on its own does not guarantee better system performance unless it can be efficiently distributed, managed and utilized.
PCIe switches help ensure that bandwidth can scale across multiple devices in dense configurations. DMA engines ensure that large volumes of data can be moved without overwhelming the processor. USB4 extends PCIe’s reach beyond the system, enabling new use cases that rely on high-speed external connectivity.


Together, these technologies allow PCIe 7.0 to deliver meaningful system-level benefits across AI, cloud, HPC and next-generation computing platforms, translating raw bandwidth improvements into real-world performance gains.


## How Rambus PCIe IP helps accelerate PCIe design


For teams designing next-generation SoCs, Rambus offers a broad PCIe controller portfolio spanning from PCIe 2.1 through PCIe 7.0, with the latest generation optimized for data rates up to 128 GT/s. Rambus positions these controllers for SoCs, ASICs and FPGAs targeting demanding AI/ML, data center and edge applications, where bandwidth, latency and scalability are critical design factors.


The Rambus PCIe 7.0 Controller is designed as a highly configurable and scalable solution for ASIC implementations, optimized for high bandwidth efficiency at 128 GT/s. The controller supports a wide range of deployment models, including endpoint, root port, switch port and dual-mode configurations. It incorporates advanced PIPE interface options, port bifurcation, and support for multiple virtual channels, allowing designers to tailor architectures for diverse workloads. The controller can be delivered standalone or integrated with a customer’s choice of PIPE-compliant SerDes, enabling flexibility across different process nodes and system requirements.


Rambus also offers a PCIe 7.0 Controller with AXI, which is especially relevant for SoC designers building around AMBA-based internal fabrics. Native support for AMBA AXI interconnect simplifies integration between PCIe and on-chip compute, memory and accelerator subsystems, helping to reduce development complexity while improving data movement efficiency across increasingly heterogeneous designs.


Extending beyond controller IP, Rambus provides a portfolio of PCIe switch and retimer solutions that address the scaling challenges inherent in modern AI infrastructure. PCIe switches play a central role in both scale-up and scale-out architectures, enabling efficient fan-out and connectivity between CPUs, GPUs, accelerators and high-speed storage within a node, as well as across server clusters. Retimers complement these deployments by preserving signal integrity over longer channel reaches and more complex topologies, ensuring reliable high-speed operation at Gen6 and Gen7 data rates. Together, these components enable system designers to build disaggregated and composable architectures required for large-scale AI training and inference, where bandwidth density, low latency and system flexibility are essential.


Beyond raw performance, Rambus emphasizes features that matter in real-world deployments. These include advanced RAS capabilities to improve system reliability and availability, optional Integrity and Data Encryption (IDE) security with AES-GCM encryption, decryption and authentication, and support for key PCIe 6.0/7.0 architectural advancements such as forward error correction (FEC), FLIT-based encoding and L0p low-power mode. These features help ensure that Rambus PCIe solutions are not only high-performance, but also production-ready for the evolving requirements of AI-driven data center and edge systems.


That makes Rambus relevant not just for “supporting PCIe 7.0,” but for helping customers build practical, high-performance interconnect subsystems for data center, edge, AI/ML and HPC designs.


### Conclusion


PCI Express remains one of the foundational technologies of modern computing because it provides the bandwidth and flexibility needed to move data across increasingly complex systems. With PCIe 7.0, the industry takes another major step forward by doubling the data rate to 128 GT/s, reaching up to 512 GB/s bidirectional bandwidth in x16 configurations, and continuing the use of PAM4, FLIT-based encoding and FEC to support next-generation workloads.


As AI, cloud, HPC and high-speed networking continue to evolve, PCIe will remain central to system architecture and performance. For design teams moving from specification to implementation, controller IP will continue to play a critical role, and solutions such as Rambus PCIe Controllers can help accelerate the path to robust, standards-aligned PCIe 7.0 designs.


---


###


### Frequently Asked Questions About PCI Express


**What is PCI Express used for?**
PCI Express is used to connect high-speed devices such as GPUs, SSDs, NICs and accelerators to processors and system fabrics inside modern computing platforms.


**What is the latest PCIe specification?**
The latest PCIe specification is PCIe 7.0, which was officially released to PCI-SIG members on June 11, 2025


**How fast is PCIe 7.0?**
PCIe 7.0 supports a 128.0 GT/s raw bit rate and up to 512 GB/s of bidirectional bandwidth in a x16 configuration.


**What does GT/s mean in PCIe?**
GT/s means gigatransfers per second, which refers to the signaling transfer rate of a PCIe link. PCIe generation speeds such as 32 GT/s, 64 GT/s and 128 GT/s are commonly used to describe the maximum link rate for each generation.


**Why did PCIe switch to PAM4?**
PCIe switched to PAM4 beginning with PCIe 6.0 to continue doubling bandwidth while transmitting more information per symbol than earlier NRZ signaling allowed.


**Is PCIe 7.0 backward compatible?**
Yes. PCI-SIG states that PCIe 7.0 maintains backward compatibility with previous generations of PCIe technology.


Need more details?[Contact us here](https://www.rambus.com/contact/) and request a meeting with one of our dedicated sales specialists.


**More on PCI Express:**


–[Scaling AI Infrastructure with PCIe 7 and CXL 3](https://event.on24.com/wcc/r/5051048/9DDC583F5FEB476AB8DF5CF39F634FB7)
–[Revolutionizing Power Efficiency in PCIe 6.x: L0p and FLIT Mode in Action](https://www.rambus.com/blogs/revolutionizing-power-efficiency-in-pcie-6x-l0p-and-flit-mode-in-action/)
