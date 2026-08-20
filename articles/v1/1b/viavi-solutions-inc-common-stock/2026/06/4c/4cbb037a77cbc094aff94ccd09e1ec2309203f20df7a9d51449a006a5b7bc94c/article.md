---
schema_version: "1.0.0"
document_id: "4cbb037a77cbc094aff94ccd09e1ec2309203f20df7a9d51449a006a5b7bc94c"
company_key: "viavi-solutions-inc-common-stock"
company: "Viavi Solutions Inc."
source_id: "viavi-solutions-inc-common-stock-rss-02890e6b52bf"
canonical_url: "https://blog.viavisolutions.com/2026/06/24/graph-neural-networks-and-digital-twins-the-path-to-autonomous-ip-transport/"
published_at: "2026-06-24T15:04:41+00:00"
first_seen_at: "2026-07-20T03:31:17.383489+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:8eb5181664fa0e747398e55fc0cabf72f8ff6cce0ed813b2c813d2d7ed429bdb"
---

# Graph Neural Networks and Digital Twins: The Path to Autonomous IP Transport

**


# Graph Neural Networks and Digital Twins: The Path to Autonomous IP Transport


- **Manish Shrivastava
- ** June 24, 2026
- 1


**VIAVI** **powered by NVIDIA accelerates the path to autonomous networks**


Ahead of DTW Ignite 2026, Copenhagen


Transport and IP networks are the connective tissue of the entire service stack. The packets that move between a cell site and a datacenter, an enterprise and the cloud, a subscriber and a CDN node, traverse this layer. Think every video, instant message, email, or website.


Transport networks are complex to automate. A modern IP/MPLS core runs BGP, IGP, SR-TE, and RSVP simultaneously, with routing policies layered across hundreds of nodes. Service paths are load-balanced, protected, and constrained by Traffic Engineering (TE) policies that interact in ways that are difficult to reason about even with full visibility. When something changes, the consequences propagate through the topology in non-obvious ways.


The traditional approach, where engineers make a change, watch what happens, and react, is no longer viable. In a network with millisecond SLA commitments and premium services riding transport paths, that is too risky.


The autonomous networks conversation has concentrated on the RAN. But the RAN does not operate in isolation. A route withdrawal at the IP layer can black-hole traffic for premium services that have nothing to do with the change that triggered it. Transport is where failures reach services and outages can truly go global.


### **Transport, AI, and the Validation Gap**


Transport networks are graph-based systems: topology, adjacency, path state. The Machine Learning (ML) architectures that work well on tabular or time-series data perform poorly on graph-structured problems. Graph Neural Networks (GNNs) are the right tool, but they require training data that represents the real topology and traffic state of the network under evaluation.


This is why most “AI for transport” efforts produce dashboards, not decisions. The gap between observability and autonomous action is a validation gap. You need a system that can simulate what will happen before it happens and produce a validated and trustworthy recommendation that an operator or an autonomous agent can trust.


Autonomous operation demands a validation environment that is always current, available, and fast enough to process a change request before the maintenance window closes. That means an underlying **Digital Twin** continuously synchronized with the live network, and simulation infrastructure that can evaluate relevant changes to the network and return a confidence scored recommendation in seconds, not hours.


### [NVIDIA Accelerated Computing](https://blogs.nvidia.com/blog/telecom-ai-agents-dtw-ignite-2026/)


BGP convergence simulation at full topology scale is computationally expensive. SR-TE path computation across thousands of nodes and tens of thousands of policy constraints is worse.


Simulating failure propagation and blast radius across both simultaneously requires compute that CPU-based platforms cannot deliver at operational speed with reasonable costs.


Transport networks are graph-structured at every level: topology graphs, routing tables as directed graphs, traffic matrices as weighted edge sets. GNNs operating on these structures can reason about path behavior, predict congestion under rerouting, and estimate failure impact in ways that rule-based simulators cannot. Their training and inference workloads map directly onto accelerated computing infrastructure.


A validation system that runs at the speed required by autonomous transport operation should be an accelerated compute-native one.


**The VIAVI IP Network Configuration Blueprint**


VIAVI built the IP Network Configuration Blueprint to make pre-deployment validation for transport operations more reliable. The system combines two complementary digital twins (an emulation and an algorithmic one), a federation layer that reconciles their outputs, and an AI-driven intent interface.


### **Two twins, purpose-built for transport**


The algorithmic twin computes what the network will do: routing table state, traffic engineering decisions, MPLS and SR-TE path selection, utilization projections, and policy enforcement outcomes. It operates on the current Routing Information Base (RIB) and Forwarding Information Base (FIB), continuously synchronized from the live network.


The emulation twin recreates how the network will behave: actual session behavior and protocol convergence timing for BGP, IS-IS SR-MPLS and SR-TE state machines, alongside real traffic patterns under redistribution. It runs in either a virtual environment or physical routing hardware using real protocol stacks and the actual routing software images from the relevant vendors.


Algorithmic twins are fast and analytically precise but abstract away protocol timing, software bugs etc. Emulation twins are behaviorally accurate but expensive at full scale. Together, they cover failure modes that either would miss by itself.


When the two twins produce different predictions, the Federation Agent reconciles them through a three-layer hierarchy, starting from the cheapest:


- Deterministic rules handle exact-match conditions: RIB withdrawals, service path divergence, convergence direction. This resolves approximately 70% of cases at zero model inference cost.
- Severity-weighted mathematical aggregation handles the next 20%, scoring discrepancies by service impact.
- Frontier model arbitration via NVIDIA Nemotron is invoked only when rules and math cannot reconcile — approximately 10% of cases.


Operators state what they need in plain language:


- *“Withdraw this route without impacting premium services”*
- *“Reroute traffic around this failed node”*
- *“Validate this maintenance action before deployment”*


The Intent Agent translates operator request into structured validation workflows. At the core, VIAVI’s telecom-domain foundation model, trained on Telco Architectural knowledge, operational workflows, and 3GPP intelligence — converts operator language into validated network actions and executable validation plans. Low-confidence responses or ambiguous intents escalate automatically to[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) Nano-9B-v2, delivered as an[NVIDIA NIM](https://www.nvidia.com/en-us/ai-data-science/products/nim-microservices/) microservice, ensuring frontier grade reasoning where it matters. A lightweight deterministic layer handles routine edge cases at zero model cost, delivering precision, scale, and efficiency in one closed-loop system. A pre-LLM deterministic preprocessor catches approximately 30% of edge cases such as out-of-domain verbs and under-specified intents, at zero model cost.


Today an operator can ask advanced AI assistants or an in-house copilot to validate a proposed change — query the algorithmic twin for a predicted path, ask the emulation twin for convergence behavior, compare alternative levers, find a convergence threshold, return a reasoned verdict— without any of these assistants knowing anything BP3-specific.


BP3 ships its capabilities as Model Context Protocol (MCP) servers, the emerging open standard for AI-tool interoperability that leading model providers have converged on. The implications for autonomous transport operations are direct. First, operators are not locked to a single vendor’s copilot. They bring the AI they already trust while the network infrastructure adapts. Second, the validation infrastructure stays stable as frontier models evolve underneath: a new model plugs in, BP3 keeps running. The reasoning layer can be swapped, upgraded, or composed, while the twin underneath remains the ground truth.


### **What the system validates**


Every change proposal is validated across:


- BGP and IGP convergence behavior, including timing and path stability
- MPLS and SR-TE path selection and failover under the proposed change
- Traffic redistribution and congestion buildup under rerouting
- Per-service SLA exposure and service impact breadth
- Failure propagation and blast radius across the affected topology


The output is an approve or reject recommendation with the full reasoning: how traffic redistributes, where congestion builds, which services are exposed, and how far a failure propagates under the worst-case scenario.


Approved changes are dispatched to lab or production via NETCONF and gNMI. A 60-second post deployment monitoring window watches per-service KPIs against SLA targets.


Every step is written to a cryptographically signed audit envelope using HMAC-SHA256.


The blueprint is available for trial on GitHub:[https://github.com/VIAVI-AIOPS/closed-loop-intent-assurance](https://github.com/VIAVI-AIOPS/closed-loop-intent-assurance)


### **VIAVI and NVIDIA: Accelerated Computing for Transport**


Transport validation is more computationally demanding than RAN, but accelerated computing changes the equation in the same two ways it changes it in RAN.


**Time to result** . Transport changes often require validation inside narrow maintenance windows. A cycle that takes minutes on CPU takes seconds on NVIDIA accelerated computing infrastructure. That gap determines whether pre-deployment validation is operationally viable, and whether an autonomous system can act on a validated decision before the window closes.


**Cost of outcome** . Meeting near-real-time latency on CPU means holding large, expensive compute resources through every scenario. NVIDIA accelerated computing reaches the same result far more efficiently. At the volume of decisions an autonomous transport network generates, that efficiency is what makes continuous validation economically viable.


The specific mechanism is Graph Neural Networks. Transport networks are graph-structured at every level: topology, routing tables, traffic matrices. GNNs trained on real network topology can reason about path behavior, predict congestion under rerouting, and estimate failure impact in ways that rule-based simulators cannot match. They are inherently parallelizable, and their workloads map directly onto accelerated computing infrastructure.


NVIDIA contributes accelerated computing infrastructure and the CUDA ecosystem. VIAVI contributes the continuously calibrated, multi-protocol transport twin that knows the current state of the network. Together, they produce a validation environment that operates at the speed demanded by transport automation.


### **VIAVI Generative Reality Digital Twin™ (GRDT)**


GRDT is a federated, high-fidelity, generative digital twin of a live network. It encapsulates domain-specific twins — RAN, IP/transport, and more — that validate AI and algorithms before and during deployment. Calibrated with real network data from VIAVI field solutions and third-party live sources, the same data trains the AI engine, so the model replicates real behavior as conditions evolve. AI tests AI inside GRDT before any change touches the live network.


Domains differ, and each comes with its own problems and requirements. RAN behaves nothing like IP and transport, and the particularities of each must be validated in a model built for them. That is the strength of the federated approach: VIAVI offers domain-specific blueprints, each the right tool for its own job. What you need follows the problem you have, and VIAVI addresses all of it.


### **VIAVI at DTW Ignite 2026**


Join VIAVI at DTW Ignite 2026 in Copenhagen from June 23-25, where we will showcase a portfolio that supports the model of a live network, continuously synchronized in real-time, feeding into the synthetic RAN digital twin. Visit us at booth 236 where VIAVI experts will demonstrate a range of solutions that provide a clear path to Level 4 autonomous operations without the usual risk. VIAVI is also participating in multiple focused roundtables and **Catalyst** events. Find out more at the event site:[https://tmforum.org/events/dtw](https://tmforum.org/events/dtw)


**DTW Ignite 2026 | June 23–25 | Booth 236 | Bella Center, Copenhagen**


- ** Categories:[AI](https://blog.viavisolutions.com/category/ai/)
- ** Tags:[AIOps](https://blog.viavisolutions.com/tag/aiops/) ,[Digital Twin](https://blog.viavisolutions.com/tag/digital-twin/)


-
-
-
-
-
-
