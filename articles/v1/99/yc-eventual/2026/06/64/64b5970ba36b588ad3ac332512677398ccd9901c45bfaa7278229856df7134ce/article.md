---
schema_version: "1.0.0"
document_id: "64b5970ba36b588ad3ac332512677398ccd9901c45bfaa7278229856df7134ce"
company_key: "yc-eventual"
company: "Eventual"
source_id: "yc-eventual-news-import-1ca85739ec11"
canonical_url: "https://www.eventual.ai/blog/the-data-wall-in-physical-ai"
published_at: "2026-06-02T08:00:00+00:00"
first_seen_at: "2026-07-25T03:47:26.048705+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:1073204448cc104fadc6f9ca00f6fd12f634d906300852a0f625017f703e42b6"
---

# The limiting factor in physical AI isn't compute or architecture - it's data

> "The core of the problem is still ... robot data, this physical interaction data ... it's just limited - it's not as big as the internet."
>
>
> --[Kanishka Rao, Director of Robotics at Google DeepMind](https://www.youtube.com/watch?v=UALxgn1MnZo&t=972)


In our[last post](https://www.daft.ai/blog/vlas-are-dead-long-live-world-action-models) , we covered Jim Fan's argument that VLAs are architecturally wrong - that video world models are the future of robotics. It's a compelling thesis. But there's a quieter, more fundamental problem that doesn't care which architecture wins.


Robotics is hitting a data wall.


## The compute overhang


[Epoch AI analyzed](https://epoch.ai/data-insights/compute-for-robotic-manipulation) the training compute used by the largest robotic manipulation models and found that they typically use about 1% of the compute used by frontier AI models in other domains. Not because labs can't afford more GPUs - many of these models come from the same labs that train the biggest language models on the planet. The gap exists largely because there's not enough data to feed them.


The implication: there's a massive compute overhang sitting idle, waiting for data to catch up. If the data constraint eases, capability gains could come fast.


## How big is the gap?


To put the scale problem in perspective:


- **Language models** train on trillions of tokens scraped from the internet
- **Open X-Embodiment** , the largest open robotics dataset, has[1M+ episodes across 22 robot types](https://robotics-transformer-x.github.io/)
- **DROID** , the most consistent single-embodiment dataset, has[76K episodes](https://arxiv.org/html/2403.12945v2) across 564 real scenes
- **Scale AI's Physical AI Data Engine** has collected[100,000+ production hours](https://scale.com/blog/physical-ai) - impressive, but still orders of magnitude below what language models consume


The internet gave language models their training data. Robotics has no equivalent. You can't simply scrape physical interaction data from the web. Every trajectory requires a real robot (or a real human wearing sensors) doing a real thing in a real environment.


## Why this bottleneck is architecture-agnostic


The data wall hits regardless of which architecture you pick.


VLAs rely primarily on teleoperation data today, which is limited and expensive to collect. World action models still need real-world data for the last mile - Ego-Scale needs[50 hours of motion-capture glove data and 4 hours of teleop](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=699) even after 21,000 hours of video pre-training. Simulation can multiply[a single demonstration into thousands of synthetic variations](https://www.notateslaapp.com/news/2998/an-in-depth-look-at-how-teslas-optimus-learns-digital-dreams-and-ai-simulation) , but someone still has to do that first demonstration, and the sim-to-real gap remains an active research problem.


## Four strategies to break through


### 1. Brute-force teleoperation


A human operates a robot remotely, and the robot learns from that demonstration. High-quality data, but teleoperation is[upper bounded by 24 hours per robot per day, and in practice it's much lower](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=504) .


### 2. Egocentric human video


Learn from first-person human video - YouTube tutorials, head-mounted cameras - and transfer that knowledge to robots. The data exists at massive scale, but the embodiment gap is real: a video doesn't record force or joint angles.


[EgoMimic](https://arxiv.org/abs/2410.24221) found that human egocentric data from Meta's Project Aria glasses can contribute more to policy performance than equivalent teleoperation data. But the[Data Utilization Law paper](https://openreview.net/forum?id=m6mRGNO7Yj) found the exchange rate is harsh: roughly 10 human video samples can negate the benefit of a single teleoperated data point for in-domain performance. Human video helps generalization but can hurt precision.


NVIDIA's Ego-Scale has shown[neural scaling laws for dexterity](https://www.youtube.com/watch?v=3Y8aq_ofEVs&t=755) using egocentric pre-training - the strongest signal yet that this approach has legs.


### 3. Synthetic data and world models


Instead of collecting more real data, generate it.[NVIDIA's Cosmos](https://www.nvidia.com/en-us/ai/cosmos/) can generate physically plausible synthetic training scenarios. Tesla's[neural world simulator](https://www.humanoidsdaily.com/news/tesla-ai-chief-details-unified-world-simulator-for-fsd-and-optimus) uses the same architecture behind FSD to train Optimus.


### 4. Real-world partnerships at scale


Instead of simulating environments, get access to real ones.[Figure AI partnered with Brookfield](https://www.therobotreport.com/brookfield-partners-figure-ai-develop-humanoid-pre-training-dataset/) (100,000 residential units) for manipulation data in real homes.[Scale AI partnered with Universal Robots](https://www.universal-robots.com/news-and-media/news-center/universal-robots-scale-ai-launch-imitation-learning-system-accelerate-ai-training-lab-to-factory/) to embed data collection directly into industrial arms.


All four strategies are happening simultaneously - nobody is betting on just one. The data problem will most likely be solved by hybrid pipelines mixing real and synthetic, teleop and egocentric.


## Learn more


If you want to learn more about physical AI, feel free to check out our[newsletter](https://topicqueue.substack.com/) . If you're a machine learning engineer getting started with physical AI, we're building a tool for multimodal model training called[MultiBase](https://www.eventual.ai/multibase) .
