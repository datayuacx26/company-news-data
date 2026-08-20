---
schema_version: "1.0.0"
document_id: "3dfdc2b38748ceefea97547c9595f12f5a5799c2724bb0d4970747ca796946b7"
company_key: "corvex-inc-common-stock"
company: "Corvex Inc."
source_id: "corvex-inc-common-stock-rss-5d3752083407"
canonical_url: "https://www.corvex.ai/blog/liquid-cooling-nvidia-b200-ai-infrastructure"
published_at: "2025-07-01T00:00:00+00:00"
first_seen_at: "2026-08-07T19:33:03.363948+00:00"
fetched_at: "2026-08-07T19:33:04.062086+00:00"
content_hash: "sha256:66814000f0610b934bbd268c73f4d94a77870eb8182123902e6016c80620f245"
---

# Cool New Reality: The Advantage of Liquid Cooling for NVIDIA B200s

As AI models scale into the trillions of parameters and GPU density increases, as we’re seeing with NVIDIA’s Blackwell architecture and, in particular, B200s, the reliability and energy efficiency of air cooling has definite limitations.


When we were designing our Blackwell deployment at Corvex, we faced a choice when it came to cooling: air versus liquid. We chose liquid cooling as the best way to ensure the best reliability in our data center and to keep our fleet of B200s running at peak performance. Here’s how we approached the problem.


### **Air Cooling Has Hit Its Limits**


The NVIDIA B200 is a beast. But with great power comes great heat—and with that heat comes risk. Under sustained workloads, B200s push thermal output to levels that air cooling struggles to handle. Some OEMs have already scrapped air-cooled B200 variants entirely, citing reliability and stability concerns. Air-cooled systems have a lot of moving parts, primarily fans, with high failure rates. That can lead to increased downtime. Those fans also require a lot more power than liquid cooled systems.


Liquid cooling emerged with two distinct advantages: reliability and energy efficiency. Fewer moving parts mean lower failure rates, which means less downtime. Greater energy efficiency means lower operating costs that we can pass on to customers. For our B200s, liquid cooling was clearly the better way to deliver reliability and value.


### **Liquid-Cooled B200s: The Gold Standard for Reliable AI Compute**


To put the thermal profile in perspective: while a typical CPU server rack draws 10–15 kW, modern GPU racks can exceed 100 kW, especially when loaded with B200s. Because they’re loaded onto servers twice as densely as their predecessors (H200s), Blackwell chips pose a new challenge when it comes to cooling. Liquid cooling meets that challenge by removing heat directly at the source—fewer airflow bottlenecks, fewer thermal hotspots, no mid-job throttling.


Lenovo’s ThinkSystem servers, used in Corvex’s infrastructure, are purpose-built for high-density GPU deployments and include advanced direct-to-chip liquid cooling support. These systems are engineered to dissipate over 100 kW per rack while maintaining thermal uniformity and performance stability—even during sustained, full-rack AI training operations. Combined with Corvex’s data center-level thermal management, these platforms help us deliver B200 performance at scale, without compromising reliability or uptime.


At Corvex, our liquid-cooled B200 clusters operate at peak performance under sustained 100% load—delivering better throughput, lower total energy consumption per FLOP, and dramatically higher system uptime. We’ve seen:


- **Double-digit improvements in sustained performance** in multi-GPU training workloads


- **Near elimination of thermal throttling** , even under constant full-load scenarios


- **More efficient power usage efficiency** , thanks to reduced reliance on oversized air-handling systems


### **Liquid-Cooling is the Future, and the Future is Now**


The thermal requirements of AI are only going up. As chips get more powerful, models get larger, and density demands increase, the choice between air and liquid isn’t really a choice at all. In an environment where a mid-training interruption can cost thousands in wasted compute – and a lot of precious time – liquid cooling is a key component in optimizing your AI computing spend.


‍


‍
