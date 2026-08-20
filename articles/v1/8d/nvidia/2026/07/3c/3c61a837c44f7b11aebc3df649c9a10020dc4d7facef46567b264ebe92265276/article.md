---
schema_version: "1.0.0"
document_id: "3c61a837c44f7b11aebc3df649c9a10020dc4d7facef46567b264ebe92265276"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/open-models-icml-2026/"
published_at: "2026-07-06T16:00:00+00:00"
first_seen_at: "2026-07-19T06:54:21.753056+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:0de8218f389b593bb51d780f0154c8839d82db23eee5b43600eaa98f213cd401"
---

# How Open Models Are Driving AI Research

Every year, the International Conference on Machine Learning (ICML) reveals where thousands of AI researchers have decided to put their work.


This year’s accepted papers reveal a clear direction: open


[frontier models](https://www.nvidia.com/en-us/glossary/frontier-models/) and open


[AI infrastructure](https://www.nvidia.com/en-us/glossary/ai-infrastructure/) have become foundational to how modern AI science gets done.


NVIDIA had 74 papers accepted at ICML 2026. Approximately 2,000 accepted papers cite NVIDIA GPUs, and 145 cite


[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) — a family of[open models](https://www.nvidia.com/en-us/glossary/open-models/) , including[open datasets](https://huggingface.co/blog/nvidia/open-data-for-agents) — as the foundation for new research. Hundreds more draw on NVIDIA


[Cosmos](https://www.nvidia.com/en-us/ai/cosmos/) , NVIDIA


[Isaac GR00T](https://developer.nvidia.com/isaac/gr00t) ,


[BioNeMo](https://nvidianews.nvidia.com/news/nvidia-launches-bionemo-agent-toolkit-giving-ai-agents-the-tools-to-accelerate-scientific-discovery) and other NVIDIA open model families, spanning physical AI, robotics, autonomous vehicles and biomedical research.


## **The Themes Defining This Year’s Research**


Areas including vision and video generation,


[reinforcement learning](https://www.nvidia.com/en-us/glossary/reinforcement-learning/) for large language models (


[LLMs](https://www.nvidia.com/en-us/glossary/large-language-models/) ) and


[agent](https://www.nvidia.com/en-us/glossary/ai-agents/) training as well as


[AI inference](https://www.nvidia.com/en-us/glossary/ai-inference/) remained prominent themes across this year’s papers, reflecting sustained investment these fields command — while several new areas also broke through.


**Robot**[world models](https://www.nvidia.com/en-us/glossary/world-models/) drew significant attention, with papers like


[DreamDojo](https://arxiv.org/abs/2602.06949) pushing the boundary of how AI systems learn to reason about and act in physical environments. DreamDojo, for example, learns how the physical world behaves from human video and builds on NVIDIA Cosmos open frontier models to predict how a robot would handle objects and operate in environments it was never trained on. It lets researchers evaluate policies, plan actions and teleoperate a virtual robot, accelerating development without the costs and risks of physical deployment.


**AI for life sciences** was fueled by NVIDIA BioNeMo open models and research contributions that help researchers understand protein function, molecular behavior and genetic code. Papers like


[FLIP2](https://www.biorxiv.org/content/10.64898/2026.02.23.707496v4) introduce public benchmarks for testing how well AI predicts the effects of protein mutations.


[KERMT](https://github.com/NVIDIA-BioNeMo/KERMT) is a new BioNeMo open model for predicting molecular properties important to drug discovery.


[Synthetic data generation](https://www.nvidia.com/en-us/glossary/synthetic-data-generation/) (SDG) drew particular interest at ICML this year with several Nemotron and


[physical AI](https://huggingface.co/collections/nvidia/physical-ai) open datasets, reflecting a broader shift in how researchers are thinking about training at scale without relying solely on human-labeled data.


## **The Open Research Stack**


Open infrastructure gives researchers the tools to accelerate breakthroughs.


The papers show Nemotron being used less like a single model release and more like a research stack: open weights to evaluate against, open datasets to train and adapt with, and open recipes for reasoning, tool use, safety, data curation and efficient inference.


Alongside the models, NeMo Curator and the open datasets it supports


[gives researchers a reproducible foundation for training data curation](https://blogs.nvidia.com/blog/nemotron-open-source-ai/) . SDG tools enable creating high-quality training sets at a scale and speed that would’ve been impractical just a few years ago.


The


[Cosmos 3](https://www.nvidia.com/en-us/ai/cosmos/) family of open, frontier


[omnimodels](https://www.nvidia.com/en-us/glossary/omni-model/) gives researchers and developers a generational leap in the ability to build robots, autonomous vehicles and vision AI that perceive, reason, plan and act in the physical world.


In addition, the


[NVIDIA Alpamayo](https://www.nvidia.com/en-us/solutions/autonomous-vehicles/alpamayo/) open model family for autonomous vehicle development,


[NVIDIA Isaac GR00T](https://developer.nvidia.com/isaac/gr00t) for robotics and


[NVIDIA BioNeMo](https://github.com/NVIDIA-BioNeMo) for biomedical help accelerate research and development across industries.


## **The Ecosystem Building on Top**


The momentum extends beyond NVIDIA’s own


[research labs](https://research.nvidia.com/research-labs) .


Basecamp Research


developed a new DNA foundation model,[EDEN](https://basecamp-research.com/wp-content/uploads/2026/01/BCR_Designing-programmable-therapeutics-with-the-EDEN-family-of-foundation-models.pdf) , that helps researchers interpret and design genetic sequences.


Merck & Co.


, uses[KERMT](https://www.merck.com/stories/our-ai-model-kermt-is-helping-to-advance-drug-discovery/) to predict how potential drug molecules may behave in the body, including whether they are likely to be effective, safe and developable.


Sakana AI


— attending ICML this year — built its[Fugu](https://sakana.ai/fugu/) and Fugu-Ultra models directly on Nemotron 3 Ultra, using the open foundation to push forward its work on AI research automation.


[KiloCode](https://kilo.ai/models/by/nvidia) integrated Nemotron into its code-routing architecture, reporting token cost reductions of up to 90% — a result with real implications for the economics of deploying AI in production.


[NAVER](https://nvidianews.nvidia.com/news/naver-ai-infrastructure) developed its own model using the Nemotron architecture, extending the foundation for Korean-language AI research.


[Together AI](https://www.together.ai/models/nvidia-nemotron-3-ultra) is hosting Nemotron models on its platform, making them more accessible to researchers who need reliable, seamless access to open inference.


Humanoid


,


[LG Electronics](https://blogs.nvidia.com/blog/nvidia-and-lg-group-ai-factory/) ,


NEURA Robotics


and


Noble Machines


are adopting NVIDIA Isaac GR00T models to accelerate industrial deployments of their humanoids, while


1X


,


Agility


,


Agile Robots


,


Boston Dynamics


,


Hexagon Robotics


, and


Mentee


are building the next generation of humanoids using Cosmos world models, Isaac Sim and Isaac Lab to accelerate the development and validation of their robots.


Explore NVIDIA’s open models on


[Hugging Face](https://huggingface.co/nvidia) .


Explore genomics and biology research at ICML’s


[GenBio Workshop](https://genbio-workshop.github.io/2026/) on Friday, July 10.
