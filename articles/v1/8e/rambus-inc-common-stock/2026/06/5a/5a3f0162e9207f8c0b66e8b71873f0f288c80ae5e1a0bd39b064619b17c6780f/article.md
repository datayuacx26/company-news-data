---
schema_version: "1.0.0"
document_id: "5a3f0162e9207f8c0b66e8b71873f0f288c80ae5e1a0bd39b064619b17c6780f"
company_key: "rambus-inc-common-stock"
company: "Rambus Inc."
source_id: "rambus-inc-common-stock-rss-ae9cc712c4a3"
canonical_url: "https://www.rambus.com/ask-the-experts-pcie-7-switch-with-tdm/"
published_at: "2026-06-30T18:52:16+00:00"
first_seen_at: "2026-07-20T03:32:34.663136+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:2466d2579f579aa38f823875bd1dc95f5a7d517a5a60c578c8b4cc4bed3bb956"
---

# Ask the Experts: PCIe 7 Switch with TDM

*Video: How PCIe 7.0 Switches and Time Division Multiplexing Enable Scalable AI Systems*


As AI infrastructure grows from hundreds to thousands of accelerators, connectivity has become just as important as compute and memory. In this Ask the Experts video, Lou Ternullo, Sr. Director of Product Management for Interconnect IP at Rambus, explains how PCIe 7.0 switch IP and Time Division Multiplexing (TDM) help solve the bandwidth, scalability, and flexibility challenges of next-generation AI architectures.


Watch the video to learn how PCIe 7 switching enables efficient communication between GPUs, SmartNICs, SuperNICs, storage, and memory resources in large-scale AI deployments.


## What This Video Covers


In this Ask the Experts, Rambus explores:


- Why connectivity has become critical for AI infrastructure
- How PCIe 7.0 increases bandwidth to support next-generation workloads
- Why PCIe switches are essential for scalable and disaggregated architectures
- The role of SmartNICs and SuperNICs in modern data centers
- How Time Division Multiplexing (TDM) improves bandwidth utilization
- Why Rambus PCIe 7 switch IP is optimized for complex AI networking SoCs


## Why Connectivity Matters in AI Infrastructure


As AI systems continue to scale, adding more accelerators alone is not enough. GPUs, SmartNICs, storage devices, memory expanders, and networking resources must communicate efficiently across increasingly complex system architectures.
While AI discussions often focus on compute and memory, the interconnect fabric plays an equally important role. If data cannot move quickly between resources, overall system performance suffers regardless of how powerful the underlying processors may be.


## What Is New in PCIe 7.0?


The headline advancement in **PCIe 7.0** is the doubling of bandwidth to **128 GT/s** , delivering significantly higher throughput for AI training, inferencing, and large-scale data movement.


However, increasing data rates introduces new challenges, including:


- Signal integrity at extreme speeds
- Power consumption optimization
- Efficient bandwidth sharing across many devices
- Scalable connectivity architectures


As systems become larger and more distributed, efficiently utilizing available bandwidth becomes just as important as increasing raw bandwidth.


## Why PCIe Switches are Essential for AI Systems


In large AI deployments, direct connections between every device are impractical. PCIe switches enable multiple devices to communicate efficiently through a shared fabric.


PCIe switches provide:


- High-density connectivity
- Flexible system design
- Support for composable infrastructure
- Resource sharing across accelerators, storage, and networking devices


They are a foundational technology for modern scale-up and scale-out AI architectures.


## Where are PCIe Switches Used?


According to Rambus, PCIe 6.0 and PCIe 7.0 switches are increasingly being implemented in advanced networking devices such as **SmartNICs** and **SuperNICs** .


These devices sit at the intersection of:


- Compute
- Networking
- Storage
- AI acceleration


Their role is to efficiently move data between CPUs, GPUs, accelerators, storage subsystems, and the network.


## What is a SmartNIC?


A **SmartNIC (Smart Network Interface Card)** extends traditional network functionality by offloading infrastructure services from the CPU. Instead of simply moving packets, SmartNICs can handle:


- Network virtualization
- Security processing
- Storage acceleration
- Telemetry and monitoring


By offloading these functions, SmartNICs free CPU resources for application workloads, improving efficiency in cloud and hyperscale environments.


## What is a SuperNIC?


A **SuperNIC** is an advanced evolution of the SmartNIC designed specifically for AI and accelerated computing environments.


Unlike traditional SmartNICs, SuperNICs focus on enabling communication across large clusters of GPUs and accelerators by providing:


- Extreme data movement capabilities
- High-radix connectivity
- Integration with accelerator fabrics
- Support for tightly synchronized AI workloads


These capabilities are critical for AI training environments that rely on efficient communication among thousands of GPUs.


### **What Is Time Division Multiplexing (TDM)?**


**Time Division Multiplexing (TDM)** is a technique that dynamically allocates bandwidth among connected endpoints using time slots rather than dedicating fixed bandwidth to each device.


For PCIe switching, TDM provides several advantages:


- Dynamic bandwidth allocation based on demand
- Improved link utilization
- More efficient sharing of switch resources
- Greater flexibility in system configuration


TDM also allows system designers to configure PCIe lane groupings in multiple ways, such as:


- 1 × 16
- 2 × 8
- 4 × 4


This flexibility enables a single switch architecture to support different combinations of GPUs, SSDs, and other endpoint devices.


### **Why Is TDM Important for AI Workloads?**


AI workloads rarely generate consistent traffic patterns. Different accelerators, storage devices, and memory resources may require dramatically different bandwidth levels over time.


TDM helps by:


- Allocating bandwidth where it is needed most
- Reducing wasted bandwidth
- Improving overall utilization
- Allowing systems to scale more efficiently


This dynamic approach helps maintain high performance without excessive overprovisioning.


### **How TDM Improves PCIe 7.0 Performance**


At PCIe 7.0 speeds, inefficiencies become increasingly costly. Underutilized links and bandwidth contention can significantly impact overall system performance.


TDM helps address these challenges by:


- Optimizing bandwidth allocation
- Reducing bottlenecks
- Managing traffic more predictably
- Supporting deterministic system behavior


For large AI systems, predictability and efficiency are essential for maintaining performance at scale.


### **Rambus PCIe 7 Switch IP with TDM**


Rambus offers advanced **PCIe 7 switch IP** with integrated TDM capabilities for AI networking SoCs, including those used in SmartNIC and SuperNIC designs.


Key capabilities include:


- PCIe 7.0 support at 128 GT/s
- Advanced switching architectures
- Integrated TDM functionality
- Optimization for AI networking applications
- Physically aware implementation for large SoCs


The architecture is designed to address the routing, latency, and implementation challenges associated with large, high-performance networking devices.


### **Why Choose Rambus for PCIe 7 Switch IP?**


Rambus brings decades of expertise in:


- High-speed interface design
- Signal integrity
- PCIe technology
- Complex SoC implementation


The company’s approach focuses on real-world deployment challenges, enabling customers to implement highly scalable PCIe switching solutions for AI infrastructure.


### Watch the Ask the Experts Video


Learn how PCIe 7.0 switch IP and Time Division Multiplexing help eliminate connectivity bottlenecks and enable scalable AI infrastructure.


#### Frequently Asked Questions about PCIe 7.0


**What is PCI Express 7.0?**
PCIe 7.0 is the seventh generation of the PCI Express specification, doubling data rates to 128 GT/s to support AI, accelerated computing, and disaggregated system architectures.


**Why are PCIe switches important for AI systems?**
PCIe switches enable efficient communication among GPUs, storage, networking resources, and accelerators, making large-scale AI infrastructures possible.


**How do SmartNICs and SuperNICs differ?**
SmartNICs primarily offload infrastructure functions from CPUs, while SuperNICs are optimized for large-scale AI environments that require communication among thousands of accelerators.


**What does Rambus provide for PCIe 7 switching?**
Rambus offers PCIe 7 switch IP with TDM support, designed for AI networking SoCs, SmartNICs, and SuperNICs.
