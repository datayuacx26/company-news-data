---
schema_version: "1.0.0"
document_id: "9360bc73f7553ccf96e91ff559d0af1ea6a1ea8e3085db720f02a4570f18637c"
company_key: "rambus-inc-common-stock"
company: "Rambus Inc."
source_id: "rambus-inc-common-stock-news-import-b51b587c8dec"
canonical_url: "https://www.rambus.com/blogs/ultra-ethernet-security-protecting-ai-hpc-at-scale/"
published_at: "2025-09-26T00:09:29+00:00"
first_seen_at: "2026-07-25T20:32:34.030231+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:b7e849a2db625d35da30c4e6cdaa41ff6deb5ddd2e4cd8cdc749426aad9c2039"
---

# Ultra Ethernet Security: Protecting AI/HPC at Scale

## **The Evolving Landscape of AI/HPC Connectivity**


As artificial intelligence and high-performance computing (AI/HPC) reshape industries, the need for robust, scalable, and secure connectivity has never been greater. Built from tightly integrated CPUs, GPUs, and SmartNICs, today’s compute clusters demand high-throughput, low-latency networks that can scale from die-to-die to multi-rack deployments.


### **Why Network Security Matters More Than Ever**


AI/HPC clusters process vast amounts of sensitive data, making network security a top priority. Effective solutions must deliver access control, data confidentiality, and threat detection, without sacrificing performance or scalability. Protocols like MACsec and IPsec have long protected data in transit, but new use cases are pushing the limits of these technologies.


### **MACsec and IPsec: Proven, But Ready for Evolution**


MACsec and IPsec are trusted standards for securing Ethernet and IP traffic, respectively. Their use of AES-GCM enables terabit-per-second throughput, but feature scaling to the demands of modern AI/HPC clusters exposes limitations in flexibility and domain isolation. The industry is now looking to the Ultra Ethernet Consortium (UEC) for answers.


### **Ultra Ethernet Consortium: Purpose-Built for AI/HPC**


UEC’s new specification introduces a high-performance Ethernet stack tailored for AI/HPC, with a Transport Security Sub-layer (TSS) that draws on the strengths of IPsec and Google’s PSP. UEC is designed for scale-out networks, enabling secure, efficient data delivery directly to application memory, minus the overhead of legacy protocols.


### **Looking Ahead: Integrating Security at Terabit Speeds**


As SmartNICs and DPUs evolve to support 800G and 1.6T Ethernet, integrating UEC TSS will be key to protecting AI/HPC workloads at scale. IPsec remains to be used for RoCEv2, an industry-wide transport protocol as well as for securing virtual networks and management traffic. MACsec will continue to secure DCI and long-haul links. The future of network security is purpose-built, high-speed, and ready for the next wave of innovation.


**Additional Resources:**


Webinar:[Network Security at Terabit-per-second Rates with MACsec, IPsec and UEC](https://go.rambus.com/network-security-at-terabit-per-second-rates-with-macsec-ipsec-uec)
Ask the Experts Video:[MACsec at Terabit Line Rates](https://www.youtube.com/watch?v=zkalga1UNdE)
SemiEngineering.com:[Network Security For AI/HPC: From MACsec/IPsec Towards Ultra Ethernet](https://semiengineering.com/network-security-for-ai-hpc-from-macsec-ipsec-towards-ultra-ethernet/)
