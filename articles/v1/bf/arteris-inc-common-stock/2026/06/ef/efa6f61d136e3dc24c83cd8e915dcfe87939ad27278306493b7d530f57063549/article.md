---
schema_version: "1.0.0"
document_id: "efa6f61d136e3dc24c83cd8e915dcfe87939ad27278306493b7d530f57063549"
company_key: "arteris-inc-common-stock"
company: "Arteris Inc."
source_id: "arteris-inc-common-stock-news-import-b55a70a8b442"
canonical_url: "https://www.arteris.com/blog/achieving-asil-compliance-in-automotive-ai-systems/"
published_at: "2026-06-11T09:41:09+00:00"
first_seen_at: "2026-07-23T02:24:35.785039+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:c0ba7dcad237e6df39dc42e760efeeb269236f1440c09b3441ceb40be1cc288f"
---

# Achieving ASIL Compliance in Automotive AI Systems

[Home](https://www.arteris.com/)


»[Newsroom](https://www.arteris.com/newsroom/)


»[Blog](https://www.arteris.com/newsroom/blog/)


» Achieving ASIL Compliance in Automotive AI Systems


# Achieving ASIL Compliance in Automotive AI Systems


- Jun 11, 2026


- Arteris


-


5


min read


Key Takeaways


- There is currently no single industry-standard approach to achieving ASIL-compliant AI in automotive systems.
- Most automotive architectures today keep AI accelerators outside the highest safety boundary and rely on ASIL D safety islands to validate outputs.
- AI accelerator vendors are increasingly adding ASIL A, B, or C capabilities directly into their products, but approaches vary widely.
- Redundancyremains a foundational safety strategy, whether through duplicate processors, multiple sensors, or independent validation paths.
- Regardless of the approach, safe and deterministic communication between AI, compute, memory, and safety domains is essential.


## The Challenge of Making AI Safe Enough for Cars


Artificial intelligence is rapidly becoming a foundational technologyin modern vehicles. Advanced driver assistance systems (ADAS), automated parking, driver monitoring, sensor fusion, and autonomous driving all depend on AI models that can process enormous volumes of data from cameras, radar, LiDAR, and other sensors in real time. Delivering this level of performance requires specialized AI accelerators capable of executing complex neural networks efficiently within the power, thermal, and cost constraints of an automotive system.


As AI takes on a larger role in vehicle decision-making, however, a new challenge emerges: functional safety. Automotive systems must meet stringent safety requirements defined by ISO 26262, with many ADAS and autonomous driving functions targeting ASIL D, the highest Automotive Safety Integrity Level. This creates a unique engineering challenge because AI accelerators are designed to perform probabilistic inference, while functional safety frameworks are built around deterministic behavior that can be verified and validated.


The challenge is that AI and functional safety were never designed toworktogether, and AI inference engines are fundamentally probabilistic. They are often described as “black boxes” because understanding exactly how a neural network arrived at a specific conclusion can be difficult, even for the engineers who designed it.


This means that as AI becomes more deeply integrated into vehicle operation, the industry faces a critical question:How do you build an automotive system that meets ASIL D safety goals when part of that system relies on non-deterministic AI?


The answer today is not a single architecture, as the industry continues to explore several different approaches.


## The Industry Has Not Yet Converged on a Single Solution


Unlike CPUs, microcontrollers, and other established automotive processing elements, AI accelerators are still relatively new to the functional safety landscape.As a result, there is currently no universally accepted model for how AI should operate in an ASIL-compliant automotive system. Different semiconductor companies, OEMs, and Tier 1 suppliers are pursuing strategies tailored to their performance goals, risk tolerance, system architecture, and safety philosophy.


While some vendors emphasize external validation mechanisms, others focus on redundancy, and some are beginning to build functional safety features directly into their AI accelerators. This lack of consensus suggests the industry is still in an exploratory phase, where multiple architectural approaches are being evaluated in parallel.


## The Established Model: Keep AI Outside the Safety Boundary


Today, the most common approach treats the AI accelerator primarily as a high-performance compute engine, rather than a safety-certified decision-making element.


In this architecture, AI processes sensor data and generates outputs, but those outputs are not trusted blindly. Instead, they are passed to an independent safety processor or safety island that performs deterministic validation before any vehicle action is taken.


However, the safety island does not attempt to understand the neural network’s internal reasoning. Instead, it evaluates whether the resulting action is reasonable and safe.For example, if an AI system recommends an aggressive steering maneuver while the vehicle is traveling at highway speed, a safety validator can determine whether that action falls within acceptable operating parameters before allowing it to influence vehicle behavior.


This approach aligns well with traditional functional safety principles because it avoids the challenge of certifying the neural network itself, while still providing a mechanism for detecting potentially unsafe outputs.Many automotive architects continue to view this safety-island model as the most mature and practical solution available today.


## Functional Safety Is Gradually Moving Closer to the AI


While the safety-island approach remains common, the market is beginning to evolve, and a growing number of AI accelerator vendors are introducing functional safety capabilities directly into their products. Rather than positioning AI as entirely outside the safety architecture, these vendors are incorporating varying levels of safety functionality within the accelerator itself.


Some solutions target ASIL A, others ASIL B, and some ASIL C. Each approach reflects a different balance between safety coverage, silicon area, power consumption, performance, and implementation cost.


To this point, the economicsare important. Functional safety mechanisms consume silicon area and add complexity, while higher ASIL targets require increasing levels of fault detection, monitoring, and redundancy. Moving all the way to ASIL D within an AI accelerator would be extremely expensive, which may explain why the industry has largely focused on lower ASIL levels within the accelerator itself and why it faces a growing diversity of approaches rather than convergence around a single model.


## Redundancy Remains a Powerful Safety Strategy


When dealing with systems that cannot always be analyzed deterministically, redundancy becomes particularly valuable.Instead of relying on a single processing path, redundancy allows systems to compare multiple independent results and identify inconsistencies that may indicate faults.This can take several forms:


- Duplicate AI accelerators processing the same workload
- Independent compute paths comparing results
- Safety processors performing cross-checks
- Diverse neural network implementations
- Multiple sensor modalities validating one another


Sensor fusionis particularly important in automotive systems. While some autonomous driving strategies rely heavily on cameras, many architectures fuse cameras, radar, and LiDAR to provide multiple perspectives of the vehicle’s environment. If one sensor experiences degradation or produces an unexpected result, other sensors can help validate the situation. Rather than attempting to prove the correctness of a single processing engine, redundancy provides a practical way to improve overall system robustness.


As automotive AI systems become increasingly complex, redundancy is likely to remain a central component of functional safety strategies.


## Safety Is Ultimately a System-Level Problem


One of the most important observations emerging from the industry is that functional safety cannot be viewed solely through the lens of an individual accelerator.Safety exists at multiple levels simultaneously: An AI accelerator may have its own safety mechanisms. A CPU cluster may have a different safety target. A safety island may operate at ASIL D. Ultimately, however, regulators, OEMs, and consumers care about the safety of the complete vehicle.


This creates a hierarchical safety model where the requirements of each subsystem are determined by its contribution to overall vehicle behavior, and viewed through this lens, the question may not be whether an AI accelerator itself can achieve ASIL D. The more relevant question may be how AI accelerators, safety processors, CPUs, sensors, memory systems, and software work together to achieve ASIL D objectives at the vehicle level.


## Why Data Movement Matters


Although safety discussions often focus on processors and algorithms, every architectural approach discussed above depends on one common requirement: trusted communication.As automotive AI systems become more heterogeneous, maintaining safe, deterministic, and observable communication between these domains becomes increasingly important.


AI accelerators must exchange data with sensors, CPUs, memory subsystems, safety islands, and other processing domains; safety validators must receive data reliably and within deterministic timing constraints; redundant processing paths must remain synchronized, and safety mechanisms must be able to communicate fault conditions throughout the system.


Whether a vehicle relies on external validation, embedded safety capabilities, redundancy, or a combination of all three, the movement of data between these domains ultimately determines how effectively the overall safety architecture functions.


The industry’s search for an ASIL D AI solution may ultimately reveal that safety cannot be achieved solely within the AI accelerator. As automotive architectures grow more complex, functional safety is increasingly a system-level challenge spanning AI engines, compute domains, safety islands, sensors, memory systems, and software. From independent safety validation to embedded ASIL capabilities and redundant processing paths, leading companies are pursuing different strategies, suggestingthe industry is still determining what “safe AI” should look like in practice.


The eventual winners may not be those with the most powerful AI accelerators, but those that build the most effective architecture for making AI trustworthy at vehicle scale.


### Frequently Asked Questions


**Can an AI accelerator be ASIL D compliant?**


Today, most automotive architectures do not attempt to make the AI accelerator itself ASIL D. Instead, ASIL D objectives are typically achieved through system-level architectures that combine AI processing with safety islands, validation mechanisms, redundancy, and deterministic control systems.


**Why is AI difficult to certify for functional safety?**


AI inference engines are probabilistic rather than deterministic. Traditional functional safety methodologies were developed around systems whose behavior could be fully analyzed and verified, making AI a fundamentally different challenge.


**What is a safety island?**


A safety island is an independent safety processor that validates system behavior and performs safety-critical checks. In many automotive AI architectures, AI outputs are evaluated before they can influence vehicle operation.


**Why do some AI accelerators claim ASIL A, B, or C?**


Vendors are increasingly integrating functional safety mechanisms into AI accelerators. Different ASIL targets represent different levels of safety coverage and implementation complexity, allowing architects to balance safety, performance, area, and cost requirements.


**Is redundancy still important in automotive AI?**


Yes. Redundancy remains one of the most effective ways to improve system robustness. It can be implemented through duplicate processors, diverse sensors, independent processing chains, or validation mechanisms that cross-check results.


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
