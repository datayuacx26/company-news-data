---
schema_version: "1.0.0"
document_id: "ab7d003b543616f19af115ea4a937695af8edb41f1e5a445f9a8181564d1bfc1"
company_key: "yc-roboflow"
company: "Roboflow"
source_id: "yc-roboflow-rss-9175e36df81e"
canonical_url: "https://blog.roboflow.com/ai-on-prem-manufacturing-it-ot/"
published_at: "2026-08-07T14:02:19+00:00"
first_seen_at: "2026-08-07T14:02:39.077575+00:00"
fetched_at: "2026-08-07T14:02:40.206381+00:00"
content_hash: "sha256:9144c05c12921dfce9545fa10e6fe269f9d9dc5eeb6916c553ba1ecae0dcf790"
---

# How Cloud Connected AI Products Run On-Prem

[Daniel Reiff](https://blog.roboflow.com/author/daniel-reiff/)


Published Aug 7, 2026 • 7 min read


Summary


**The Purdue Model splits an industrial network into levels (physical process, PLCs, SCADA, MES, the DMZ, then enterprise IT) so a breach in business software never reaches the machinery, and it still underpins ISA-95, IEC 62443, and most heavy-industry networks today. A cloud-native AI platform can deploy into it without breaking segmentation: Roboflow runs inference at the edge, fully air-gapped, through a brokered DMZ gateway, or over the API, matching whichever posture the network enforces.**


In[manufacturing](https://roboflow.com/industries/manufacturing?ref=blog.roboflow.com) , if you are operating an IT/OT network, you’ve probably heard of the *Purdue Model* . Providing a reference model for how sensors, controllers, and systems all connect, it ensures the plant runs smoothly.


In doing so, it institutes a separation between IT and OT, isolating controls. This separation causes every enterprise OT buyer to eventually ask the same question:


> How does a cloud-native AI platform deploy into a network built to never phone home?


It's a fair question. The plant floor wasn't built to trust the internet, and for good reason.


Working alongside Fortune 500 partners, Roboflow has developed a flexible deployment model for AI products. We deploy systems across levels of OT, in the DMZ, and air gapped to meet the requirements of today’s[industrial](https://roboflow.com/industries/industrial-manufacturing?ref=blog.roboflow.com) network architecture. Before we dive in depth on how it works, let’s start with the model most industrial networks are still built on.


## What Is the Purdue Model?


The Purdue Model (formally the Purdue Enterprise Reference Architecture) is the mental map industrial security has run on for thirty years. Its job is simple to state and hard to do: keep the systems that run physical machinery separated from the systems that run the business, so a bad day in IT never becomes a dangerous day in OT.


It does this by slicing the network into layers, each with its own level of operational and informational control. The key idea is "adjacency": a level only talks to the layer directly above and below it. If an attacker lands on the corporate email server up top, strict separation means they're several controlled hops away from the controller that opens a valve. Segmentation contains the blast radius.


What lives at each level:


- **Level 0, the physical process.** The machinery itself: sensors, actuators, motors, valves, the equipment doing the real work. This is where bits meet the physical world.
- **Level 1, basic control.** The[PLCs](https://blog.roboflow.com/programmable-logic-controller/) and RTUs that read Level 0 sensors and send back commands. This is the reflex layer, translating a signal into an action thousands of times a second.
- **Level 2, supervisory control.**[SCADA](https://blog.roboflow.com/what-is-scada/) , DCS, and the[HMIs](https://blog.roboflow.com/hmi-design/) where an operator watches and steers the process. If Level 1 is reflexes, Level 2 is the cockpit.
- **Level 3, manufacturing operations.**[MES](https://blog.roboflow.com/manufacturing-execution-system/) and historians, the systems that coordinate production rather than individual machines and store the record of what happened. This is the top of the OT world.
- **Level 3.5, the DMZ.** The demilitarized zone: firewalls, proxies, jump servers, and data brokers that sit between OT and IT. Nothing crosses from the plant to the enterprise, or back, without passing through here and being inspected. This is the border checkpoint.
- **Level 4, enterprise network.** Business applications, logistics, email, the site's IT infrastructure. Corporate, but plant-adjacent.
- **Level 5, corporate IT.**[ERP](https://blog.roboflow.com/what-is-erp-in-manufacturing/) , directory services, data centers, the internet. The outside world.


The dividing line that matters most runs between Level 3 and Level 4: the IT/OT boundary, enforced through the DMZ. Above it is business software, where a breach costs money and downtime. Below it is a physical process, where a breach can cost a limb or a life. That asymmetry is why the model exists.


## Who Runs on This Model?


Almost everyone in heavy industry, but very few say so on the record, and that's by design. Publishing your exact level structure is handing an attacker a map to your DMZ. So the honest answer comes in two shapes.


The institutional answer: if a company follows ISA-95, IEC 62443, NIST SP 800-82, or the Cisco/Rockwell Converged Plantwide Ethernet (CPwE) architecture, it's running a Purdue-derived network whether it uses the word or not. ISA-95 says outright that it "relies on the well-known Purdue Reference Model."


The named answer: a smaller set has put Purdue in writing.[ExxonMobil has publicly](https://www.arcweb.com/blog/how-exxonmobil-balances-itot-convergence-business-value-cybersecurity?ref=blog.roboflow.com) described challenging the model's historical assumptions, which is itself proof their installed base is Purdue-structured. Georgia-Pacific's[own patents](https://patents.google.com/patent/US20220342401A1/en?ref=blog.roboflow.com) reference the "Purdue Model for ICS security" as a required protocol.[Phillips 66 and Honeywell](https://patents.google.com/patent/US20210329455A1/en?ref=blog.roboflow.com) wrote Purdue levels directly into granted patent claims. Even the modern players lean on it: Anduril hires OT network engineers to "design segmented architectures based on ISA-95 and Purdue Model principles," and Microsoft's guidance for extending OT to Azure is built around Purdue's six levels. It shows up everywhere from oil refineries to hyperscale data-center mechanical systems.


## Is It Outdated, Or Still Relevant?


The original model, drawn up in 1990, assumed OT would stay air-gapped forever, physically walled off from IT and the internet. That assumption has collapsed. Plants now want their operational data in the cloud for analytics and their AI on the floor for efficiency. IT and OT are converging whether the security architecture likes it or not. The[Colonial Pipeline attack](https://en.wikipedia.org/wiki/Colonial_Pipeline_ransomware_attack?ref=blog.roboflow.com) made the stakes concrete: the intrusion was in IT, but the impact landed on operations.


So is Purdue obsolete? No, but the way you enforce it has to evolve. The consensus keeps the layers and stops trusting position alone. The reimagined model puts modern controls on top of the old hierarchy:


- **Zero-trust segmentation** : instead of assuming a device is safe because of where it sits, verify what it's doing. Allow only the transactions that are needed, and monitor everything.
- **Visibility first:** you can't secure what you can't see. As plants add IoT and edge devices, knowing each device's normal behavior is what lets you catch the abnormal one.
- **Deliberate convergence:** cross-layer communication is now necessary, but it should be brokered and logged, never a flat network that happens to work.


The layers still describe where things are. Zero trust describes whether they're allowed to talk. You need both now.


## How Does It Coexist with AI?


AI is where there is tension, because AI breaks the adjacency rule by nature. An edge inference model has to act down at Level 1 or 2, right next to the machinery, but it also needs to learn, which means sending data upstream to retrain and improve. A copilot reads from the cloud data lake and writes suggestions back to the SCADA console. A vision model runs inference at the edge and ships flagged images out for review. That's cross-layer data movement in real time, as a core function of the product, exactly what Purdue was built to prevent.


Deploy it carelessly and you've drilled a hole straight through your segmentation. Deploy it thoughtfully and you get the efficiency without giving up the boundary. The difference is in how the AI system moves data and models across the layers, which makes it a product-architecture question more than a security-policy one.


## What a Purdue-Friendly AI System Requires


If you take the model seriously, it becomes a checklist. Any AI or computer vision system that wants to live in a segmented industrial network has to be capable of these things, not all at once, but able to, so each site can pick the shape that fits its posture:


1. **Run where the work is:**[inference](https://inference.roboflow.com/?ref=blog.roboflow.com) has to execute inside the OT zone, at the[edge](https://roboflow.com/ai/edge?ref=blog.roboflow.com) , next to the process, not only in a distant cloud. Latency and segmentation both demand it.
2. **Keep runtime data in-zone by default:** live process data (camera feeds, PLC signals, inference results) should never have to cross the IT/OT boundary just to keep the system running. A plant that allows no outbound internet at all should still get a fully functional system.
3. **Move models across the boundary with flexibility:** new models and workflows have to be able to enter the OT zone in whatever form the network trusts, as reviewable, injectable packages where a live API dependency isn't welcome, or over the API where it is. Either way, the crossing adapts to the network's rules instead of asking the network to adapt to it.
4. **Be observable:** operators and security teams need visibility into every edge device (what it's running, whether it's healthy, what it's doing) to support a zero-trust posture.


## How Roboflow Delivers Secure AI for Enterprises


We're an end-to-end computer vision platform for building the applications that run on a plant floor: defect detection, visual inspection,[assembly verification](https://roboflow.com/ai/assembly-verification?ref=blog.roboflow.com) , process monitoring, and measurement. These are the problems process-analytics AI can't touch, because they require seeing. A vision model is fast becoming a standard tool in the process engineer's kit.


We fit a Purdue network because we don't insist on one deployment shape. We offer a range of postures that map onto where a customer sits, and onto the requirements above:


- **IT and cloud batch:** for workloads that live above the boundary, push image batches to our managed cloud and pull predictions back. No on-prem computer required.
- **OT with API:** pull models and workflows down through the API, run inference on-prem, and send images back out when the network permits. Inference happens where the work is, and egress is allowed but not required.
- **OT air-gapped:** for sites that strictly enforce the boundary, inference runs with no internet at all. Models and workflows are injected as artifacts, over a GitOps pipeline or physical media, and runtime data (camera to inference to PLC) never crosses the gap.
- **DMZ and secure gateway:** pull models and return data through one brokered proxy in the DMZ, a single logged egress point instead of scattered connections. The same gateway gives security teams remote visibility into edge devices.


The through-line is deployment flexibility as a security feature. A customer that won't allow a single outbound packet gets air-gapped inference. A customer that wants centralized model management with controlled data flow gets the secure gateway. A customer comfortable with the cloud gets the API. Same platform, same models, different posture, chosen to match their segmentation instead of fighting it. The Purdue Model isn't an obstacle to deploying AI. It's a specification, and we built Roboflow to meet it.


**Sources:**


- [Industrial Cyber](https://industrialcyber.co/features/how-ai-is-quietly-rewiring-purdue-model-forcing-industrial-defenders-to-rethink-trust-across-operational-environments/?ref=blog.roboflow.com)
- [Forbes](https://www.forbes.com/councils/forbestechcouncil/2022/01/18/a-reimagined-purdue-model-for-industrial-security-is-possible/?streamIndex=0&ref=blog.roboflow.com)
- [Palo Alto Networks](https://www.paloaltonetworks.com/cyberpedia/what-is-it-ot-convergence?ref=blog.roboflow.com)
- [ZScaler](https://www.zscaler.com/resources/security-terms-glossary/what-is-purdue-model-ics-security?ref=blog.roboflow.com)
- [Fortinet](https://www.fortinet.com/resources/cyberglossary/purdue-model?ref=blog.roboflow.com)


### **Cite this Post**


Use the following entry to cite this post in your research:


*[Daniel Reiff](https://blog.roboflow.com/author/daniel-reiff/) . (Aug 7, 2026). How Cloud Connected AI Products Run On-Prem. Roboflow Blog: https://blog.roboflow.com/ai-on-prem-manufacturing-it-ot/*


### Written by


Daniel Reiff


Field Engineer at Roboflow! Passionate about pushing boundaries in applied computer vision.


[View more posts](https://blog.roboflow.com/author/daniel-reiff/)


### Topics


- [Computer Vision](https://blog.roboflow.com/tag/computer-vision/)
- [Deployment](https://blog.roboflow.com/tag/deployment/)
- [Roboflow Deploy](https://blog.roboflow.com/tag/roboflow-deploy/)
