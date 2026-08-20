---
schema_version: "1.0.0"
document_id: "2d913bc60aaea0604d32577b6690f8694484bf6af49d8438d847dba87bdffd98"
company_key: "yc-superb-ai"
company: "Superb AI"
source_id: "yc-superb-ai-news-import-8e5d04580d69"
canonical_url: "https://superb-ai.com/en/resources/blog/physical-ai-data-factory-dokpamo-phase-2-results-en"
published_at: null
first_seen_at: "2026-08-04T19:45:31.056647+00:00"
fetched_at: "2026-08-04T20:28:10.240209+00:00"
content_hash: "sha256:8fcb64a0b53af1b8b87ab6c64cc966db06c8da9a074f2fc2bdc0928bcb59c8de"
---

# 50 Korean Homes Became Robot Training Data: Building 50 3D Spaces, 5,000 Behaviors, and 10,000 Object Assets

Tech


50 Korean Homes Became Robot Training Data: Building 50 3D Spaces, 5,000 Behaviors, and 10,000 Object Assets


Hyun Kim


Co-Founder & CEO | 2026/08/04 | 7 min read


[Linkedin](https://www.linkedin.com/company/superb-ai/)[X(twitter)](https://x.com/superb_hq)


This past January, as we kicked off Phase 2 of Korea’s Sovereign AI Foundation Model Project, known in Korean as **Dokpamo** , we shared our plan: to build the 3D spaces, 4D behaviors, and intelligent objects that serve as the raw materials for virtual physical environments robots can learn from.


Dokpamo is a Korean government-led initiative to build sovereign AI foundation model capabilities and the data infrastructure behind them. While the project is national in scope and focuses on Korean living environments, the approach is relevant far beyond Korea. Every country deploying home and service robots faces the same underlying challenge: robots trained on data from elsewhere may underperform in local environments, and the physical-world data needed to close that gap does not exist on the web.


🔗 Related post:[Superb AI “Proprietary AI Foundation Model Project” Phase 2: Key Takeaways on Digital Twin Assetization](https://superb-ai.com/en/resources/blog/superb-ai-national-foundation-model-project-phase-2-en)


**What Is a Physical AI Data Factory?**


A Physical AI Data Factory is a data production system that takes data collected in the real world as raw material, converts it into 3D and 4D digital assets that simulators can understand, and generates large volumes of labeled training data in virtual environments.


The concept has become a key agenda item this year. In Korea’s Strategy for Securing Core Competitiveness in Physical AI, announced in July 2026, the Korean government identified data acquisition as the first priority area for development. Across the industry, companies and institutions are also discussing the buildout of regional data factories nationwide. Through Dokpamo, Superb AI has been actively operating this system in Korea.


Figure 1. The structure of a Physical AI Data Factory: raw materials, processing, mass production, and quality control


# **Summary (TL;DR)**


- **50 high-precision 3D space assets replicating Korean home environments:** A hybrid structure combining 3D Gaussian Splatting backgrounds with physical meshes for interactive objects, aligned to real-world scale and floor geometry
- **5,000 4D behavior assets covering household activities such as dishwashing, cleaning, and organizing:** Quality-selected from 7,500 recorded sequences, with body shape and pose separated using SMPL and a pipeline processing success rate above 98%
- **10,000 intelligent object assets focused on household objects frequently manipulated by robots:** Pixel-level segmentation and articulated modeling for eight object types, including refrigerators, washing machines, and drawers
- **A proof-of-concept run of the synthetic data generation pipeline, integrating these assets into NVIDIA Isaac Sim:** Generation of 10,000 synthetic images is currently in progress
- **Next step:** Phase 3 in the second half of 2026, focused on multimodal integration of language and behavior


# **Plan vs. Results**


In the upcoming deep-dive series, we will walk through how each asset type was built.


Across the following four articles, we will explain how the behavior, space, and object assets were created, and how these three asset types come together.


Figure 2. Preview of the four-part deep-dive series


# **Why Behaviors, Spaces, and Objects?**


The bottleneck in robot learning is not the model. It is the data, and that data does not exist on the web.


Spaces, behaviors, and objects are the raw materials that help robots understand where they are, what they are seeing, and how they should move. When these three asset types are combined inside a simulator, they form a structure that can reproduce training data at scale.


In the asset-specific deep dives, we will cover how each asset was built, including the trial and error we encountered throughout the process.


# **Global Trends**


Similar efforts are moving quickly in other markets. In China, more than 40 robot training centers were established over the past year with government support, and AgiBot released an open-source dataset containing more than one million real-robot manipulation data points. In Germany, the Technical University of Munich and NEURA Robotics are opening Europe’s largest Physical AI training center this year, backed by an investment of €17 million.


The race to accumulate learning data from the physical world has already begun. Superb AI’s project is Korea’s working example of that race in action.


# **Frequently Asked Questions**


### **Q. Why Korean home environments?**


For robots trained on overseas datasets, Korean residential spaces are almost edge cases, with scarce examples. Training data for robots deployed in Korea needs to come from Korean environments.


### **Q. Does synthetic data replace real data?**


No. It amplifies real data. High-quality real-world data assets are required to generate synthetic data that remains aligned with reality. We will cover this in Deep Dive Part 4.


For Physical AI data-building partnerships, leave your information below and Superb AI’s experts will contact you shortly.


Want to explore more?


Sign up for an account to get started. No credit card required.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)


Related Posts


[Tech How ZERO Won the CVPR 2026 Foundational Few-Shot Object Detection Challenge: A Technical Walkthrough of the Winning Solution Hyun Kim Co-Founder & CEO | 10 min read](https://superb-ai.com/en/resources/blog/cvpr-challenge-win-technical-walkthrough-en)[Tech Building Superb AI’s Synthetic Data Pipeline with NVIDIA Isaac Sim Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/synthetic-data-pipeline-using-nvidia-isaac-sim-en)[Tech Superb AI “Proprietary AI Foundation Model Project” Phase 2: Key Takeaways on Digital Twin Assetization Hyun Kim Co-Founder & CEO | 7 min read](https://superb-ai.com/en/resources/blog/superb-ai-national-foundation-model-project-phase-2-en)


About Superb AI


Superb AI is an enterprise-level training data platform that is reinventing the way ML teams manage and deliver training data within organizations. Launched in 2018, the Superb AI Suite provides a unique blend of automation, collaboration and plug-and-play modularity, helping teams drastically reduce the time it takes to prepare high quality training datasets. If you want to experience the transformation, sign up for free today.


[Sign Up](https://platform.superb-ai.com/auth/sign_up)
