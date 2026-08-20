---
schema_version: "1.0.0"
document_id: "fe3b6ad76e437d08b302a3e6af90515216cf234b2dfdfdd3e8f68a667ca8afa6"
company_key: "rambus-inc-common-stock"
company: "Rambus Inc."
source_id: "rambus-inc-common-stock-news-import-b51b587c8dec"
canonical_url: "https://www.rambus.com/blogs/rambus-announces-industry-leading-ultra-ethernet-security-ip-solutions-for-ai-and-hpc/"
published_at: "2026-02-18T22:00:48+00:00"
first_seen_at: "2026-07-25T20:32:34.030231+00:00"
fetched_at: "2026-07-28T22:19:40.776398+00:00"
content_hash: "sha256:1cfb483f82a2fbc6af67f66b88910770c5f807b68a61735d4bd63fa114abf384"
---

# Rambus Announces Industry-Leading Ultra Ethernet Security IP Solutions for AI and HPC

AI/HPC clusters process a tremendous volume of valuable data and have already become a critical element of modern infrastructure and therefore must be protected at all levels that are exposed to potential threats. Network security is one of the key components aiming to provide:


- Access control – allowing only the authorized nodes or users to access the compute systems
- Data confidentiality – encrypting data on the wire and protecting the encryption keys from being recovered
- Data isolation – using different secure domains and encryption keys for data belonging to different applications, jobs, or users
- Threat detection – ability to identify manipulations with the data and critical network headers, identify packets being replayed, delayed, etc.


Because of the scale and cost of each AI/HPC system, there is a need to support a high level of utilization for return on investment. This requires the network security feature to keep up with the overall AI/HPC system goals:


- High throughput, ideally full line rate
- Minimal impact on latency
- Efficiently support the system scale (number of ports, nodes, secure domains)
- Have reasonable cost of silicon area and power


# Ultra Ethernet Tailored for AI and HPC


The Ultra Ethernet Consortium ([Ultra Ethernet Consortium](https://ultraethernet.org/) ) was set up in 2023 by leading companies in HPC and connectivity with the goal of enhancing Ethernet’s capabilities through standardizing a high-performance Ethernet stack purpose-built for the unique demands of AI/HPC.


UEC specification v1.0 that was publicly released in June 2025 defines a transport protocol (UET) that provides the ability to deliver data straight from the network and into application memory and vice versa, without software involvement. This method is known as “Remote DMA”. The new transport protocol brings numerous enhancements to the current, widely used RoCE v2 (RDMA over converged network) and both are going to co-exist for some time. For transport protocol protection, UEC defined a Transport Security Sub-layer (TSS), which leverages concepts from IPsec and PSP (Google’s open-source security protocol) to efficiently support multiple use cases at scale. The TSS is a new protocol, therefore it is not compatible with any of the previously defined protocols while the implementation is going to have a lot in common with line-rate MACsec and IPsec solutions.


TSS protocol is defined in such a way that it is applied to protect the payload and optionally authenticate network headers, leaving the routing and load balancing information accessible. TSS also allows leaving parts of the UEC transport headers in clear for ease of inspection and debugging. This means that fabric switches do not need to deal with the TSS layer, and its presence in the packet is transparent for the network operation. Therefore, TSS is going to be added only to SmartNICs, and the market is going to have two flavors of SmartNICs: UEC-only (requiring only TSS for security) and UEC+RoCE (requiring TSS along with IPsec and if required, MACsec).


# Rambus Security IP for Ultra Ethernet TSS


Rambus, having successfully served the data center, enterprise and infrastructure markets with line rate MACsec and IPsec products ([Secure Networking](https://www.rambus.com/security/protocol-engines/) ), adds two new solutions for securing UET transport protocol with TSS:


- [UET-TSS-IP-69](https://www.rambus.com/security/protocol-engines/uet-tss-ip-69/) , an inline, multi-channel TSS transformation engine at rates up to 1.6Tbps for customers who already have the classification and policy databases in place.
- [UET-TSS-IP-369](https://www.rambus.com/security/protocol-engines/uet-tss-ip-369/) , an inline, high-performance, multi-channel packet engine that provides a complete TSS layer at rates up to 1.6Tbps. It embeds the UET-TSS-IP-69 as processing data path and complements it with classification, SDKDB, key management and statistics.


*UET-TSS-IP-369 Block Diagram*


Both of these[UET-TSS IP solutions](https://www.rambus.com/security/protocol-engines/) are expected to be used in SmartNICs and NIC chiplets, and in the future to be part of UET-based NVMe-oF storage controllers. As IP blocks that can be instantiated independent of the ethernet controller, the Rambus UET-TSS IP solutions can support customers’ own tunneling protocols while providing flexibility and scalability. With over three decades of state-of-the-art security expertise, Rambus offers the industry’s broadest and most performant portfolio of security IP solutions backed by world-class support from Rambus security experts. For complete details, please visit Rambus[Security IP.](https://www.rambus.com/security/)
