---
schema_version: "1.0.0"
document_id: "0f28f3f3a19813ca2af493010b49caa12bdc8aba75b45c6130fdf01e70b63d88"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/open-world-models-physical-ai/"
published_at: "2026-08-06T13:00:00+00:00"
first_seen_at: "2026-08-06T13:34:42.276854+00:00"
fetched_at: "2026-08-06T13:34:43.157467+00:00"
content_hash: "sha256:88416cf0f5e2925569e891825c284dffdd1fad011537b7ce7c7c6d255a3db8f2"
---

# Into the Omniverse: How Open World Models Push the Frontier of Physical AI

*Editor’s note: This post is part of*[Into the Omniverse](https://www.nvidia.com/en-us/omniverse/news/) *, a series focused on how developers, 3D practitioners and enterprises can transform their workflows using the latest advancements in*[OpenUSD](https://www.nvidia.com/en-us/omniverse/usd/) *and*[NVIDIA Omniverse](https://www.nvidia.com/en-us/omniverse/) *.*


In July, NVIDIA joined more than 200 companies and organizations in signing “[Open Weights and American AI Leadership](https://images.nvidia.com/pdf/Open-Weights-and-American-AI-Leadership.pdf) ,” an open letter arguing that AI leadership will be measured not by any single frontier model but by whether an open ecosystem reaches every sector.


[Open models](https://www.nvidia.com/en-us/glossary/open-models/) , which anyone can download, inspect, modify and run on their own infrastructure, are what make that possible. Nowhere is that more crucial than in[physical AI](https://www.nvidia.com/en-us/glossary/generative-physical-ai/) , where every deployment is a specialization problem.


Physical AI has to understand and predict consequences, not just appearances.


To make this possible, w[orld models](https://www.nvidia.com/en-us/glossary/world-models/) learn how physical environments behave, what may happen next and which following actions make sense. They can generate physically grounded world and action data, simulate future states and provide a foundation that teams can specialize for a robot, autonomous vehicle or vision AI system.


Open world models are already being used to generate training data, test policies and specialize physical AI systems.[NVIDIA Cosmos 3](https://www.nvidia.com/en-us/ai/cosmos/) brings these capabilities together in an open model family, with leading benchmark results and adoption across robotics, autonomous vehicles and vision AI.


And[NVIDIA Omniverse](https://www.nvidia.com/en-us/omniverse/) libraries, part of NVIDIA Agent Toolkit, provides prebuilt capabilities for building simulation-ready worlds that physical AI teams can use to train, test and validate systems before real-world deployment.


## ****World Models Are the Foundation of Physical AI****


The data behind physical AI is difficult and expensive to collect at the scale required. Rare events and long-tail scenarios can be especially difficult to reproduce safely and repeatedly.


World models enable:


-


More useful data by learning physical relationships from large-scale multimodal scenarios.


-


More diverse environments that vary in weather, lighting, objects and trajectories.


-


A better foundation to build on and adapt to a particular robot, vehicle, sensor configuration, task or operating environment.


[https://blogs.nvidia.com/wp-content/uploads/2026/08/cosmos-corp-blog-promo-1920x1080-1.mp4](https://blogs.nvidia.com/wp-content/uploads/2026/08/cosmos-corp-blog-promo-1920x1080-1.mp4)


A general model hasn’t seen a team’s particular robot, sensors or operating environment. Closing that gap requires access to model weights, a license that permits adaptation and the tools needed for post-training.


NVIDIA Cosmos world foundation models are available under the Linux Foundation’s OpenMDW 1.1 license, enabling teams to post-train models on their own data and hardware. Specialization is where openness becomes a practical technical requirement.


Specializing a model is only part of the workflow. Teams also need environments to generate data, run simulations and test behavior.


Omniverse libraries help developers build simulation-ready environments, while[OpenUSD](https://www.nvidia.com/en-us/glossary/openusd/) provides the open framework for composing, reusing and exchanging complex 3D data across[digital twins](https://www.nvidia.com/en-us/glossary/digital-twin/) , simulations and[synthetic data generation](https://www.nvidia.com/en-us/glossary/synthetic-data-generation/) workflows. Together, Omniverse and OpenUSD cut the duplicated work that can otherwise pile up every time assets, sensor configurations or environmental conditions change.


## ****Cosmos 3: The Frontier Model****


NVIDIA Cosmos 3 — a frontier open physical AI foundation[omni-model](https://www.nvidia.com/en-us/glossary/omni-model/) built on a[mixture-of-transformers](https://www.nvidia.com/en-us/glossary/mixture-of-transformers/) architecture — combines vision reasoning, world generation and action prediction, letting developers use one model family to understand scenes, generate synthetic data, simulate future states and build specialized[world action models](https://www.nvidia.com/en-us/glossary/world-action-model/) .


Developers can use Cosmos 3 as a[vision language model](https://www.nvidia.com/en-us/glossary/vision-language-models/) , as a physics-grounded world simulator that predicts future world states and generates large-scale synthetic data, or as the backbone for world action models, instead of assembling and maintaining a separate model for each capability.


The family includes Cosmos 3 Super (64B) for high-fidelity world modeling, Cosmos 3 Nano (16B) for efficient reasoning and post-training, and[Cosmos 3 Edge](https://huggingface.co/nvidia/Cosmos3-Edge) (4B) for on-device vision reasoning and robot policy deployment. Lightweight enough to run on edge GPUs, Cosmos 3 Edge can be deployed across NVIDIA RTX GPUs, NVIDIA DGX systems and NVIDIA Jetson, including Jetson Thor platforms.


Across benchmark evaluations, Cosmos 3 ranks No. 1 on[Artificial Analysis](https://artificialanalysis.ai/image/leaderboard/text-to-image/open-weights) for open weights text-to-image and image-to-video generation, on[PAI-Bench](https://huggingface.co/spaces/shi-labs/physical-ai-bench-leaderboard) for world generation and in the image-to-video category of[Physics-IQ](https://physics-iq.github.io/) . For robot policy, it ranks No. 1 on[RoboLab](https://research.nvidia.com/labs/srl/projects/robolab/leaderboard.html) . Cosmos 3 Super is also the highest-ranked open model on[VANTAGE-Bench](https://huggingface.co/spaces/clemson-computing/VANTAGE-Bench-Leaderboard) for vision understanding.


In addition to Cosmos, NVIDIA’s physical AI stack includes[Isaac GR00T](https://developer.nvidia.com/isaac/gr00t) for robotics,[Alpamayo](https://www.nvidia.com/en-us/solutions/autonomous-vehicles/alpamayo/) for autonomous vehicles and[Metropolis](https://www.nvidia.com/en-us/autonomous-machines/intelligent-video-analytics-platform/) for vision AI.


## ****How Developers Are Putting Cosmos 3 to Work****


Across industries, developers are building on NVIDIA Cosmos for physical AI applications: Doosan Robotics, LG Electronics, Samsung Electronics and Skild AI in robotics; Li Auto, Xiaomi and Afari in autonomous vehicles; and[Centific](https://www.centific.com/blog/centific-brings-last-mile-physical-ai-to-production-with-nvidia-cosmos-3) ,[Fogsphere](https://fogsphere.com/fogsphere-announces-cosmos-3-support/) ,[Linker Vision](https://www.linkervision.com/post/linker-vision-unveils-application-driven-ai-grid-for-agentic-video-reasoning-at-scale) ,[Milestone Systems](https://www.milestonesys.com/resources/content/articles/milestone-hafnia-nvidia-cosmos-3/) and[Yuan](https://www.yuan.com.tw/news/preview-news?id=336&t=d74eb353274d4fd78e460600ae11a561) for[vision AI agents](https://www.nvidia.com/en-us/use-cases/video-analytics-ai-agents/) powering industrial AI and smart spaces applications.


The[NVIDIA Cosmos Coalition](https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-3-the-open-frontier-foundation-model-for-physical-ai) extends this work by bringing together world model builders, AI developers and physical AI leaders to contribute models, research and evaluation methods. NVIDIA recently[expanded the coalition to Japan](https://nvidianews.nvidia.com/news/japans-robotics-and-manufacturing-leaders-build-on-nvidia-cosmos-to-advance-physical-ai-frontier) , where robotics and manufacturing leaders intend to join and develop open world models for factories, logistics, agriculture, construction, healthcare and transportation.


Together, these implementations and collaborations are establishing open world models as an adaptable foundation for physical AI across robots, autonomous vehicles and vision AI systems.


## ****Get Plugged In****


Learn more about world models, OpenUSD and physical AI development by exploring these resources:


-


**Explore** the open[Cosmos 3 model collection](https://huggingface.co/collections/nvidia/cosmos3) and datasets on[Hugging Face](https://huggingface.co/nvidia) and[GitHub](https://github.com/nvidia-cosmos) .


-


**Read** the[Cosmos 3 technical report](https://research.nvidia.com/labs/cosmos-lab/cosmos3/technical-report.pdf) for full architecture details and evaluations.


-


**Read** the[Cosmos 3 technical blog](https://developer.nvidia.com/blog/) .


-


**Tune in** to the[Cosmos Labs livestreams](https://www.addevent.com/calendar/ss55fmjpm04t) .


-


**Learn** about the[NVIDIA Cosmos Coalition](https://nvidianews.nvidia.com/news/nvidia-launches-cosmos-3-the-open-frontier-foundation-model-for-physical-ai) .
