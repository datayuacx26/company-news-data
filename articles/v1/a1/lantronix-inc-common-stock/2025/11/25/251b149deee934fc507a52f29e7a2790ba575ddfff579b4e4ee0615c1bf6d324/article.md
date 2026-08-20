---
schema_version: "1.0.0"
document_id: "251b149deee934fc507a52f29e7a2790ba575ddfff579b4e4ee0615c1bf6d324"
company_key: "lantronix-inc-common-stock"
company: "Lantronix Inc."
source_id: "lantronix-inc-common-stock-rss-a450c006f3f3"
canonical_url: "https://www.lantronix.com/blog/gateway-to-the-future-how-lantronix-is-unlocking-drone-autonomy/"
published_at: "2025-11-13T20:38:56+00:00"
first_seen_at: "2026-07-20T23:18:54.707261+00:00"
fetched_at: "2026-07-28T22:25:14.090185+00:00"
content_hash: "sha256:917388214db94c2aaf5e53c548283392e948399b6fd3e70a4e95047caa0425a6"
---

# Gateway To The Future: How Lantronix is Unlocking Drone Autonomy

November 13, 2025


- [Coverage](https://www.lantronix.com/blog/category/coverage/)
- [Executive Rant & Rave](https://www.lantronix.com/blog/category/executive-rant-rave/)
- [Industry Trends](https://www.lantronix.com/blog/category/industry-trends/)
- [New Technology](https://www.lantronix.com/blog/category/new-technology/)


### Gateway To The Future: How Lantronix is Unlocking Drone Autonomy


#### Drones World Editor Kartikeya In conversation with Mr. Mathi Gurusamy, Chief Strategy Officer, Lantronix Inc.


**Q: The U.S. Army’s recent approval of Teal Drones, which use your technology, is a significant endorsement. What specific feature of your Edge IoT solution was most critical in meeting the Army’s stringent requirements for security and performance?**


A: Our Open-Q™ 5165N SOM helped solve both the security and performance requirements for Teal. On the security front, our tightly controlled supply and production processes mean the SOM meets both the NDAA and TAA requirements, which are fundamental to delivering a drone to the U.S. Army. On the performance size, the low SWaP (Size, Weight and Power) of our SOM enables Teal to deliver a high-performance solution that offers cutting-edge features to meet operational performance targets as well as extended flight times.


**Q: Your solution is often described as a critical “gateway” for drones. In simple terms, what is the core function of this gateway, and why is it a better approach than how drones traditionally managed data?**


A: Traditionally, a drone is constructed of multiple separate sub-systems, each with their own processing node to manage flight control, navigation, computer vision and communication as well as perform individual tasks. We call it a gateway because it serves as the intelligent bridge between sensors, flight systems and command infrastructure. Eliminating the separate sub-systems reduces weight and power draw, which extends battery life and flight time while simplifying development and maintenance. The single gateway processor can also “share” information between different SW functions, which reduces costs and enables new capabilities.


**Q: Your technology integrates components from various partners, like GREMSY gimbals and Teledyne FLIR thermal cameras. How does your platform act as the “glue” that makes these different specialized components work together more seamlessly?**


A: The Open-Q 5165N SOM is the main computing node or “gateway” in a drone design. It processes the data coming from the various peripherals integrated in the design. By processing the data within a single computing node (our SOM), performance is optimized and decisions made based on a complete understanding of the drone’s environment. The flight controller can also control navigation or drone orientation based on objects “seen” by the camera sub-systems without requiring input from the controller. This intelligent system operation is enabled by integrating the sub-systems in the SW running on the SOM, which is not possible when sub-systems operate independently.


**Q: A key benefit you highlight is enabling “longer and more efficient flight times.” How does processing data directly on the drone, rather than streaming it all away, directly contribute to conserving the aircraft’s battery life?**


A: Communication sub-systems are a significant consumer of power on a drone, especially those offering extended operating range or even BVLOS (Beyond Visual Line of Sight) operation. Modern drones are becoming flying sensor networks, often capturing feeds from multiple visible, thermal and depth cameras at once. Streaming this data to the ground in real time simply isn’t practical; the bandwidth and latency requirements overwhelm most links and drain valuable power. Lantronix systems use Edge AI to process and interpret data locally, directly on the drone, considerably increasing efficiency. The system delivers advanced AI performance while drawing single-digit watt power levels at full operation, making it one of the most energy-efficient embedded compute solutions available for small UAVs. This ultra-low-power design allows drones to run complex perception and navigation models without sacrificing flight time. The resulting longer missions, cooler operation and higher reliability, enable smaller aircraft to perform more sophisticated tasks on a single charge.


**Q: Beyond military use, where do you see the most promising commercial or industrial applications for this integrated drone and Edge AI technology?**


A: We see strong traction in infrastructure inspection, public safety and smart agriculture industries. Edge AI drones are now being deployed for powerline and pipeline monitoring, first-responder support and precision agriculture. Edge AI allows drones to instantly detect faults in power lines, pipelines or solar arrays rather than relying on ground-based post-processing. In agriculture, multispectral analysis performed onboard can track crop health and irrigation needs with greater accuracy. For emergency response, drones with onboard intelligence can navigate complex environments safely, even without continuous network connectivity. The same compute modules and AI pipelines that power defense drones are being used to enable BVLOS operations as new FAA Part 108 regulations come online.


**Q: How does the ability to run A models directly on the drone during flight create new, immediate possibilities that aren’t available with drones that only capture and stream video?**


A: Running AI models on the drone transforms it from a passive camera into an autonomous decision-maker. Instead of simply capturing footage, the system can identify and classify what it sees — such as people, vehicles, heat signatures or anomalies — and take action in real time. This is especially powerful for search-and-rescue, perimeter security and reconnaissance where speed and accuracy are critical. With Edge AI, the drone can perform multi-camera fusion, combining visual, thermal and depth data for a more complete understanding of its environment. It reacts faster, requires less operator input and maintains full capability even when communication links are limited, transforming a drone from a sensor platform into a self-reliant asset.


**Q: Looking at your partnerships with companies like Teal, GREMSY, and FLIR, how important is building this ecosystem of hardware and software partners to your overall strategy in the advanced drone market?**


A: Building a strong ecosystem of partners is fundamental to our strategy. The next generation of drones will depend on tight integration between flight systems, payloads and AI compute. Lantronix provides the common Edge AI foundation that brings those elements together into one cohesive architecture. Lantronix is expanding that ecosystem every quarter. Its collaboration with leading gimbal, camera and communications vendors delivers pre-integrated, fully validated solutions that enable OEMs to bring their solutions to market faster. By maximizing interoperability and simplifying development, our customers can focus on innovation rather than integration. Ultimately, this approach provides drone manufacturers and service providers with a unified, trusted platform that delivers the best possible user and mission experience, whether the application is defense, public safety or industrial inspection.
