---
schema_version: "1.0.0"
document_id: "0376df6cdaf825325d137e473f6835e67eb638d80d5644ef8259b2c161d87be0"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/06/22/building-the-gpu-accelerated-ran-digital-twins-that-will-run-tomorrows-networks/"
published_at: "2026-06-22T15:27:24+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:320d3a182db6aba4a60dcafc66f1b5052a92ae6ce25a1dc9250fd91491e114c8"
---

# Building the GPU-Accelerated RAN Digital Twins That Will Run Tomorrow’s Networks

**


# Building the GPU-Accelerated RAN Digital Twins That Will Run Tomorrow’s Networks


- **Fransiscus Asisi Bimo
- ** June 22, 2026
- Like


Telecom operators already know the problem. Networks have grown too complex, too layered, and too interdependent to manage through observation and reaction alone. The window for human intervention, between a condition emerging and a service degrading, has narrowed to the point where manual operations are no longer a viable model at scale. The industry has been converging on the same conclusion for several years: the network has to manage itself.


An autonomous network can reason about its own state, validate its own decisions, and act with enough confidence to execute without a human in the loop. But autonomy creates a problem of its own. An autonomous system proposes changes in a volume and variety that no human change management process can keep pace with, and that human review quickly becomes the qualitative and quantitative bottleneck on the whole system. The way past it is to validate every proposed change at machine speed. That demands something the industry has long talked about: **a high-fidelity, near-real-time digital twin.**


A good simulation predicts how the network will behave under a proposed change before you make it. But a simulation is only ever as accurate as what it is calibrated against, and a model left to run on its own drifts from the network it is meant to represent.


The differentiator is the data. Simulation becomes accurate when fueled by real field measurements, live telemetry, and environmental conditions fed continuously back into the model. That feed of reality is what turns simulation into a true digital twin, and it’s the principle **VIAVI Generative Reality Digital Twin™** (GRDT) is built on.


GRDT is a federated, high-fidelity, generative digital twin of a live network. It encapsulates domain-specific twins (RAN, IP/transport, and more) that validate AI and algorithms before and during deployment. Calibrated with real network data from VIAVI field solutions and third-party live sources, the same data trains the AI engine, so the model replicates real behavior as conditions evolve. AI trains and tests AI inside GRDT before any change touches the live network.


Domains differ, and each comes with their own problems and requirements. RAN behaves nothing like IP and transport, and the particularities of each must be validated in a model built for them. That is the strength of the federated approach: VIAVI already offers domain-specific blueprints, each the right tool for its own job. What you need in any given use case follows the problem you have, and VIAVI can help with all of it.


### **NVIDIA Accelerated Computing**


[VIAVI AI RAN Scenario Generator (AI RSG)](https://www.viavisolutions.com/en-us/products/teravm-ai-rsg) is a RAN digital twin that develops, validates, and stress-tests network applications across 4G, 5G, and future 6G networks — open and purpose-built alike — before they touch a live network. Rooted in synthetic data generation, it simulates city-scale RAN environments across terrestrial and non-terrestrial networks, and continuously ingests real-world data to calibrate its model, enabling accurate closed-loop testing of AI/ML applications at any scale. With a long-term vision of a real-time, city-scale digital twin driven by live field data, AI RSG accelerates operators’ journey from manual network management to full network autonomy.


The NVIDIA Blackwell platform extends the AI RSG proven capability into two areas that autonomous operations increasingly demand: time to result and cost of outcome.


**Time to result** . Some autonomous use cases need feedback near real-time, inside the decision loop, sometimes within seconds. Early results using NVIDIA accelerated computing show order-of-magnitude improvements in simulation throughput, enough to run full-fidelity Massive MIMO scenarios at real deployment scale.


**Cost of outcome** . At the scale of continuous autonomous validation (thousands of decisions per day) GPU acceleration delivers lower cost and energy per validated scenario than CPU, despite higher upfront resource cost. Short-lived inference tasks keep utilization efficient, and on-demand readiness is cheaper to maintain than equivalent CPU capacity scaled for peak throughput.


The value shows up at distinct points in the ecosystem:


- For application developers and research organizations: early algorithm design, training AI models, and validating inferencing.
- For operators working pre-deployment in the lab: validating concepts, benchmarking vendors, and tuning configurations before anything reaches production.
- For semi-autonomous and fully autonomous systems in operations: automated impact analysis, what-if analysis, and the risk assessment that lets the network approve or reject its own changes.


Acceleration is what lets the most realistic, scalable scenarios run at each of these touchpoints in a time- and resource-efficient way, so autonomous functions can manage the risk of change properly.


The[VIAVI-NVIDIA collaboration](https://blogs.nvidia.com/blog/telecom-ai-agents-dtw-ignite-2026/) is structured around complementarity. NVIDIA contributes the accelerated computing infrastructure and the CUDA ecosystem. VIAVI contributes the O-RAN-compliant, CI/CD-ready RAN digital twin that operators, RAN vendors, and rApp developers rely on for pre-production validation, and now within the operational network itself.


The result is a RAN simulation that keeps pace with the speed of AI development and stays operationally useful as network complexity grows.


### **Intent-Based RAN Optimization Blueprint**


Simulation speed is critical. The operational value sits a layer above it, in what the twin does with that capability.


The **Intent-based RAN Optimization Blueprint** is a GRDT use case where[NITRO®](https://www.viavisolutions.com/en-us/solutions/nitro-aiops)[AIOps](https://www.viavisolutions.com/en-us/solutions/nitro-aiops) and[NITRO Location Intelligence](https://www.viavisolutions.com/en-us/solutions/nitro-location-intelligence) maintain a continuously synchronized model of the live network, feeding AI RSG as the RAN synthetic digital twin.


It interprets operator intent (e.g. enhance coverage while maintaining throughput, increase capacity with minimum additional sites, improve SLA by x%, etc.) and a TLM based optimizer recommends the network changes required to accomplish business or network optimization objectives.


Every recommendation is validated inside the twin against real coverage, real subscriber location, real environmental data, and returned with a confidence score before anything touches the live network.


It scales from scheduled maintenance windows and investment planning decisions up to TM Forum L4 autonomous execution, where the network acts on validated intent without human intervention.


The autonomous network is being built now, in the validation environments where every AI decision gets tested before it acts on the live network.


GRDT keeps a calibrated, federated twin of the live network. AI RSG, accelerated on NVIDIA Blackwell, generates full-scale RAN scenarios fast enough to sit inside the decision loop. And the Intent-based RAN Optimization Blueprint turns a business intent, a coverage target, an SLA commitment, or a CapEx case, into a validated, confidence-scored change the network can act on, up to TM Forum L4 autonomous execution.


**Platform, speed, and intent, working as one system. That is what operators will see in Copenhagen.**


### **VIAVI at DTW Ignite 2026**


Join VIAVI at DTW Ignite 2026 in Copenhagen from June 23-25, where we will showcase a portfolio that supports the model of a live network, continuously synchronized in real-time, feeding into the synthetic RAN digital twin. Visit us at booth 236 where VIAVI experts will demonstrate a range of solutions that provide a clear path to Level 4 autonomous operations without the usual risk. VIAVI is also participating in multiple focused roundtables and **Catalyst** events. Find out more at the event site: **https://tmforum.org/events/dtw**


**DTW Ignite 2026 | June 23–25 | Booth 236 | Bella Center, Copenhagen**


Transport is the next frontier. A carrier running thousands of interconnected routers faces failure propagation, rerouting, and topology changes that are graph-computation problems at a scale that makes RAN look simple. NVIDIA GNN acceleration applied to the transport digital twin is what makes that problem tractable.


That is what VIAVI and NVIDIA are building, and what we will cover in the next blog.


- ** Categories:[AI](https://blog.viavisolutions.com/category/ai/)
- ** Tags:[Generative Reality Digital Twin](https://blog.viavisolutions.com/tag/generative-reality-digital-twin/)


-
-
-
-
-
-
