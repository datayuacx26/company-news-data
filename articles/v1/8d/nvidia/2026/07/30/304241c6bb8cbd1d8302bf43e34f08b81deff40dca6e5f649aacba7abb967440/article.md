---
schema_version: "1.0.0"
document_id: "304241c6bb8cbd1d8302bf43e34f08b81deff40dca6e5f649aacba7abb967440"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/performance-per-watt-ai-infrastructure-efficiency/"
published_at: "2026-07-14T15:00:20+00:00"
first_seen_at: "2026-07-19T06:54:21.753056+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:77e9d84bf127d77935e08d2d2e65e932f00d9f5ad642971fb73ab8ce0b0ca10b"
---

# Why Performance per Watt Is the Ultimate Metric for AI Infrastructure Efficiency

Power is AI infrastructure’s inescapable constraint. How many


[tokens](https://blogs.nvidia.com/blog/ai-tokens-explained/) an AI factory can generate within a fixed power budget determines its revenue and profitability. Because of this, performance per watt — a metric that can’t be gamed, only earned through real-world results — is the foundation for AI factories.


As agentic AI drives token demand higher, the infrastructure decisions organizations make today will determine who scales and who doesn’t in a power-constrained world.


Virtually every frontier AI model today runs on a


[mixture-of-experts](https://blogs.nvidia.com/blog/mixture-of-experts-frontier-models/) (MoE) architecture.


Serving these large-scale models efficiently means GPU domain size — the number of GPUs connected over an ultrafast, scale-up interconnect — matters, and bigger is better.


While the NVIDIA Hopper generation set the standard with an eight-GPU domain, the scale of frontier AI today has outgrown it. Serving MoE with a 72-GPU domain demands full-stack codesign and the operational depth earned from running these models under real production load.


With the


[NVIDIA Blackwell NVL72 platform](https://www.nvidia.com/en-us/data-center/technologies/blackwell-architecture/) , that


foundation is already built and proven, delivering the highest performance per watt to maximize revenues and the lowest token cost to maximize profit margins. It’s this foundation that the


[NVIDIA Vera Rubin](https://www.nvidia.com/en-us/data-center/technologies/rubin/) platform builds upon next to further elevate rack-scale energy efficiency.


## **Maximizing Performance per Watt for Frontier AI**


Each new generation of frontier models brings architectural changes that unlock greater intelligence while demanding new optimizations to run efficiently at scale.


Across the newest generation of leading open models, NVIDIA GB300 NVL72 delivers up to 25x performance per watt compared with the NVIDIA Hopper generation — showcasing that MoE performance improves when moving from an 8-GPU to 72-GPU domain size. These numbers reflect where Blackwell stands today, a starting point that continues to improve.


Any single number only tells part of the story. Different workloads demand different operating points: some optimize for latency, others for throughput and cost — and most need to move between the two.


To best represent these operating points, NVIDIA showcases Pareto curves for each model rather than a single point and provides tools such as


[DynoSim](https://developer.nvidia.com/blog/dynosim-simulating-the-pareto-frontier/) to help teams find their optimal point on the Pareto frontier before spending a single GPU-hour on validation.


NVIDIA GB300 NVL72 systems deliver up to 25x performance per watt over NVIDIA Hopper on DeepSeek V4 Pro. Source: SemiAnalysis InferenceX On GLM5.1 NVIDIA GB300 NVL72 systems deliver up to 20x performance per watt over NVIDIA Hopper. Source: SemiAnalysis InferenceX NVIDIA GB300 NVL72 systems deliver up to 10x performance per watt over NVIDIA Hopper for Kimi K2.6, a model purpose-built for long-horizon agentic tasks. Source: SemiAnalysis InferenceX


The performance per watt NVIDIA Blackwell delivers is a result of extreme codesign: every component of the rack-scale system, from silicon to


[software](https://blogs.nvidia.com/blog/inference-software-lowest-token-cost/) , designed together to maximize token throughput for AI inference workloads. That codesign touches every layer of the stack.


For example,


[NVIDIA NVLink Switch](https://www.nvidia.com/en-us/data-center/nvlink/) , critical for rack-scale performance, is purpose-built to unlock massive scale-up GPU domains, not adapted from general-purpose networking. Now in its sixth generation with the Vera Rubin platform, its capabilities are designed specifically for AI workloads such as SHARP, which performs in-network computing directly in the switch, offloading work from the GPUs themselves.


NVIDIA’s


[inference software stack](https://blogs.nvidia.com/blog/inference-software-lowest-token-cost/) , including NVIDIA Dynamo and TensorRT LLM, as well as SGLang and vLLM, is built to run the full range of optimizations: NVFP4 quantization, disaggregated serving, large-scale expert parallelism, KV-aware routing, KV cache offloading and more. These stack together to multiply the performance each GPU delivers. Moreover, software keeps improving performance over time: On DeepSeek V4, performance per watt improved by up to 5x in a single month.


In AI factories, power lost to cooling and rack-level inefficiencies can mean only about 60% of the electricity pulled from the grid turns into useful AI work. NVIDIA DSX MaxLPS, the power-and-efficiency software in the


[NVIDIA DSX](https://www.nvidia.com/en-us/data-center/products/dsx/) platform, closes that gap by shifting power between GPUs and racks in real time, supporting warm-water liquid cooling and using techniques like power steering to wring more performance. This enables operators to run up to 40% more GPUs within the same power budget.


## **Production Is Where It Counts**


Rack-scale reliability at AI factory scale is hard-won. Rack-scale systems introduce failure modes that single-node deployments never encounter, and handling them requires engineering rigor and time in production.


NVIDIA Blackwell NVL72 systems continues to set the standard across a diverse range of models and production use cases delivering sustained performance, rack-level reliability and economics that hold under real traffic day after day.


That’s why leading AI labs such as


Anthropic, OpenAI and SpaceXAI


use NVIDIA Blackwell NVL72 systems to run inference.


In addition, a variety of inference service providers and AI natives use the Blackwell platform to deploy open models in production.


[CoreWeave has deployed Kimi K2.6](https://www.coreweave.com/blog/coreweave-is-now-the-fastest-at-inference-on-the-best-open-source-model-kimi-k2-6) on NVIDIA GB300 NVL72, combining NVFP4 quantization and EAGLE3 speculative decoding to maximize inference performance.


Perplexity


runs


[Qwen3 235B](https://research.perplexity.ai/articles/advancing-search-augmented-language-models) and post-trained Qwen3.5-397B-A17B


on NVIDIA GB200 NVL72 for its AI agent platform, serving millions of queries daily with the latency and reliability that consumers need.


Fireworks AI


deploys GLM 5.2 on the NVIDIA Blackwell platform, enabling production deployments for customers including Cursor and Factory AI.


This accumulated production experience, built across generations of frontier models and real-world deployments, is what gives NVIDIA Vera Rubin its head start.


*Learn more about the NVIDIA Vera Rubin platform in this*[technical blog](https://developer.nvidia.com/blog/inside-the-nvidia-rubin-platform-six-new-chips-one-ai-supercomputer/) *and find details on the*[NVIDIA DSX AI factory-scale platform and DSX MaxLPS](https://docs.nvidia.com/dsx) *.*
