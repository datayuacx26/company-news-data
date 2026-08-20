---
schema_version: "1.0.0"
document_id: "e695baf13bdacb0b7d0cb66666e4019e875053ed9b2dad11cd12f2723cbd23e2"
company_key: "ambarella-inc-ordinary-shares"
company: "Ambarella Inc."
source_id: "ambarella-inc-ordinary-shares-rss-4b0311b4df52"
canonical_url: "https://www.ambarella.com/blog/building-for-the-real-world-at-embedded-vision-summit-2026/"
published_at: "2026-05-21T20:53:34+00:00"
first_seen_at: "2026-07-20T23:21:42.727700+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:7bb0507528e7f8246e0f95e254f6f55fba1d8d1fa712d3ce46529400c00c77b0"
---

# Building for the Real World at Embedded Vision Summit 2026

The Ambarella team is back from a strong week at this year’s Embedded Vision Summit in Santa Clara. A speaking session, panel discussion, and a partner demonstration on the show floor made for a full week. The conversations across all three pointed in the same direction: edge AI and agentic systems have moved past the proof-of-concept stage and the questions now are about deployment at scale.


## **Robotics from Prototype to Production**


On Monday, Chief Architect Bob Kunz joined the plenary panel “Edge AI and Vision in Robotics: From Benchmarks to Fleet-Scale Reality,” moderated by Dave Tokic of Torc Robotics, alongside panelists from Agility Robotics, Qualcomm, and Simbe Robotics.


The panel examined what separates a functioning robotics demo from a system that scales reliably in the field. Sending trade-offs, compute constraints, data pipelines, and safety requirements all compound as deployments grow from 10 robots to 1,000. A key takeaway that Bob emphasized throughout was that the architecture that produces a compelling demo is almost never the same one that gets deployed in production.


## **Edge-First Coding Agents**


On Tuesday, Pietro Antonio Cicalese, Sr. Technical Marketing Engineer presented “Edge-First Coding Agents: Trustworthy Agentic Development for Real Devices.” The session drew strong attendance and a lively Q&A, centered on what agentic AI looks like when it has to run under embedded hardware constraints (fixed power budgets, intermittent connectivity, hard latency requirements) rather than the cloud-native assumptions baked into most agentic frameworks.


Pietro walked through how coding agents need to be structured for edge deployment: separating reasoning from execution, validating against actual device behavior, and producing systems that remain auditable in production. He also showed how Ambarella’s[Cooper Developer Platform](https://www.ambarella.com/cooper/) supports this approach, showcasing agentic blueprints that translate intent into deployable edge workflows.


A consistent theme in the discussion was the role of the cloud. It remains relevant in edge AI architectures, but only for tasks where latency and data residency constraints don’t apply. Real-time perception and time-critical control will need to happen at the edge.


## **Partner Demonstrations**


On the show floor, Ambarella partner iENSO demonstrated edge AI systems built on Ambarella technology. Their demos featured a hybrid architecture combining a centralized AI compute platform utilizing our[N1-655](https://www.ambarella.com/products/aiot-industrial-robotics/#:~:text=CV7%20Product%20Brief-,N1%2D655,-Ambarella%E2%80%99s%20N1%2D655) AI SoC with a distributed CV75M-based vision processing nodes, running multi-stage decision pipelines and multi-camera scene analysis in a fully on-premises configuration


## **What’s Next**


Thank you to everyone who attended the speaking sessions and stopped by the iENSO demos. For a deeper look at the ideas covered by Pietro and Bob, the articles “[Everything Is Going to Be Driven by Algorithms](https://www.ambarella.com/blog/everything-is-going-to-be-driven-by-algorithms/) ” and “[The AI That Runs the Physical World Looks Nothing Like Your Favorite Chatbot](https://www.ambarella.com/blog/the-ai-that-runs-the-physical-world-looks-nothing-like-your-favorite-chatbot/) ” are available on the Ambarella[blog](https://www.ambarella.com/blog/) . Developers interested in the Cooper Developer Platform, the Cooper Model Garden, or hardware-specific performance benchmarks can get started at[developer.ambarella.com](https://www.ambarella.com/developer/) .
