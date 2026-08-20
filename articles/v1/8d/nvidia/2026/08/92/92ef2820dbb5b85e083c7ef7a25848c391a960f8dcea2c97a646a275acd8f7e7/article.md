---
schema_version: "1.0.0"
document_id: "92ef2820dbb5b85e083c7ef7a25848c391a960f8dcea2c97a646a275acd8f7e7"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/nemotron-lightning-switchyard-rtx-dgx/"
published_at: "2026-08-11T13:00:05+00:00"
first_seen_at: "2026-08-11T13:50:14.179640+00:00"
fetched_at: "2026-08-11T13:50:16.738157+00:00"
content_hash: "sha256:896547b85425afc12ec613a4d8378de6cea0a77e7688d3064d5da550594adef5"
---

# NVIDIA Nemotron 3.5 Lightning and NeMo Switchyard Deliver Faster, Smarter, More Efficient Agentic AI

As AI shifts from chatbots to autonomous agents, open models are serving market demands for full control over where AI runs and how it’s deployed and evolves.


Today, NVIDIA is expanding its Nemotron 3 model family with Nemotron 3.5 Lightning, the highest-efficiency model in its class for long-running agentic AI workloads. This release follows Nemotron 3 Nano and reflects NVIDIA’s commitment to continually improving open models for greater accuracy and speed.


Built for specialized tasks within larger multi-agent systems, Nemotron 3.5 Lightning, a 30-billion-parameter


[mixture-of-experts](https://www.nvidia.com/en-us/glossary/mixture-of-experts/) model, helps create smarter and more efficient agentic applications.


Also, NVIDIA is releasing NeMo Switchyard, an open source library for smart routing inside popular agent tools. Enterprises can use it to build a router based on their specific needs. When deployed, NeMo Switchyard can intelligently direct each request to the most capable and suitable model for the job without requiring developers to rewrite their applications.


Together, Nemotron 3.5 Lightning and NeMo Switchyard deliver greater control over how AI is deployed, where it runs and how efficiently it operates — across PCs, workstations, data centers and the cloud.


Nemotron 3.5 Lightning delivers frontier-level intelligence in a small, customizable open model built for high-volume agentic workflows.


## **Always-On Agents Need a System of Models**


Modern agentic systems — always-on agents — increasingly operate as


[systems of models](https://youtu.be/Np0afRWtdp8?si=om8lMihXpHOTXc3e) , or model ensembles, with different models specialized for different tasks.


[NVIDIA Nemotron](https://www.nvidia.com/en-us/ai-data-science/foundation-models/nemotron/) open models are designed for this architecture. A frontier reasoning model such as Nemotron 3 Ultra or GPT-5.6 may plan and orchestrate a workflow, while smaller specialized models like Nemotron 3.5 Lightning can perform targeted tasks such as code review, tool use, security alert monitoring and answering billing questions.


## **Powering High-Volume Specialized Tasks With Nemotron 3.5 Lightning**


[NVIDIA Nemotron 3.5 Lightning](https://developer.nvidia.com/blog/nvidia-nemotron-3-5-lightning-delivers-fast-accurate-specialized-task-execution-for-long-running-agents/) is a fully customizable open model built for high-volume tasks powering always-on agents. It was developed with contributions from the Nemotron Coalition, whose members provided evaluation methodologies, inference software and datasets to help advance the model.


The model delivers up to 4x faster output speed, leading to 30% faster agentic task completion compared with other models in its class. And because it’s open and customizable, Nemotron 3.5 Lightning can be easily post-trained with NVIDIA NeMo on an organization’s own domain data, tools and workflows to improve accuracy for specialized tasks.


PinchBench benchmarks demonstrate that Nemotron 3.5 Lightning delivers faster agentic task completion with frontier-level accuracy compared to other models in its class.


AI leaders across industries are customizing Nemotron 3.5 Lightning for their workloads, including


CrowdStrike


for cybersecurity,


Harvey


with


Trajectory


for legal services and


[CodeRabbit](https://www.coderabbit.ai/blog/teaching-nvidia-nemotron-3-5-lightning-to-route-code-reviews) with


Baseten


for code review, helping improve accuracy for domain-specific agentic tasks. Additionally


,


[Lila Sciences](https://www.lila.ai/news/building-the-agent-driven-era-of-science-with-nvidia-bionemo-agent-toolkit) i


s helping to improve reasoning capabilities for agentic tasks across physical and life sciences, and


[Fastino Labs](http://fastino.ai/blog/fastino-nemotron-3-5-lightning-finance-and-healthcare) customized the model and is seeing leading accuracies for software development, finance and healthcare workloads.


Enterprises have customized Nemotron 3.5 Lightning to achieve leading accuracy for their specialized task in their agentic workflows.


Nemotron 3.5 Lightning also gives organizations control over privacy and deployment. It can run on local AI systems — including


[NVIDIA RTX PCs](https://www.nvidia.com/en-us/ai-on-rtx/) ,


[NVIDIA DGX Spark](https://www.nvidia.com/en-us/products/workstations/dgx-spark/) ,


[NVIDIA DGX Station](https://www.nvidia.com/en-us/products/workstations/dgx-station/) and


[NVIDIA Jetson](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/) — to help users maximize existing infrastructure investments, or scale across edge AI devices,


[NVIDIA RTX PRO](https://www.nvidia.com/en-us/products/workstations/) workstations, data centers and cloud environments for enterprise use cases. And Nemotron 3.5 Lightning can run locally or on premises for high-volume, specialized tasks that require fast responses.


Also, as with every Nemotron launch, NVIDIA publishes as much of the training data and techniques as licensing permits, which allows for traceability, auditing and training of other models. Alongside Lightning, NVIDIA is releasing


[Nemotron-RL-Agentic-Terminal-Pivot](https://huggingface.co/datasets/nvidia/Nemotron-RL-Agentic-Terminal-Pivot-v1-nano35-release) , an agentic reinforcement learning dataset used to post-train it for coding agent capabilities.


## **More Efficient AI Apps With Model Routing**


Some models are better for coding, some for reasoning, some for lightweight tasks and some are optimized to run locally for greater privacy and efficiency. If customers rely on one default model, they might either overspend or lose quality; if they manage routing manually, it becomes integration work that can slow down a deployment.


[NVIDIA NeMo Switchyard](https://developer.nvidia.com/blog/route-ai-agent-workloads-across-models-with-nvidia-nemo-switchyard) is an open source model routing library for AI agents. The technology routes prompts to the most capable and efficient model for each step of an agent workflow automatically, based on specific needs. Agent application developers can tune or modify the router with different routing algorithms to match their priorities, such as quality, latency and cost requirements. In a system of models, enterprises can create powerful AI agents with improved tokenomics.


Internal benchmarks show that


NeMo Switchyard maintains frontier-level accuracy while reducing task completion cost to nearly one-third of Opus 4.8 alone.


NVIDIA internal benchmarks show that NeMo Switchyard maintains frontier-level accuracy while reducing task completion cost to nearly one-third of Opus 4.8 alone.


NVIDIA is working with partners across the AI ecosystem to bring intelligent model routing into the tools and platforms developers already use.


- [Boomi](https://boomi.com/blog/why-open-model-routing-matters) **:** Evaluated Switchyard across five routing capabilities, achieving 100% domain-routing accuracy, sending 59% of traffic to a 5x faster fine-tuned model and reducing later-turn latency by 21%.


- **Cadence** : Improved efficiency by 9.9% by using the


[ChipStack AI Super Agent](https://www.cadence.com/en_US/home/company/newsroom/press-releases/pr/2026/cadence-unveils-industrys-first-fully-autonomous-virtual.html) for a formal verification use case.


- [Classmethod](https://dev.classmethod.jp/en/articles/nvidia-nemo-switchyard-first-touch/) : Is running opencode and Fireworks workloads using NeMo Switchyard internally, with initial testing showing a 27% cost reduction while maintaining quality.


- **Cognition** :


​Integrated the NVIDIA NeMo Switchyard staged router into Devin Desktop for NVIDIA internal use, achieving near-frontier performance on


[FrontierCode Main](https://cognition.com/frontiercode) while reducing mean cost by 28% relative to routing all requests to a single underlying frontier model.


- [Kong](https://konghq.com/blog/preview/6a6cd1d40364e90001c019a5) :


Delivers routing with NeMo Switchyard natively through Kong AI Gateway.


- [LangChain](http://www.langchain.com/blog/switchyard-agent-routing-benchmark) : With NeMo Switchyard, achieved 74% lower cost in 145 multi-turn Deep Agents tasks by routing only 7% of calls to a frontier model, at a 6% accuracy tradeoff.


- **LiteLLM** : Is adding NeMo Switchyard as a plug-in into its proxy layer so developers can access these benefits without changing their existing stack.


- **Nous Research** : Integrated NeMo Switchyard into Hermes to provide developers with an easy-to-configure routing system to improve agent efficiency.


- **Ramp** : Used NeMo Switchyard to match a frontier model’s performance while cutting costs by 58% and runtime by 33% in


[Ramp SWE-Bench](https://labs.ramp.com/swebench) .


- **Siemens** : Is benchmarking to improve efficiency in its


[Fuse EDA AI Agent](https://www.siemens.com/en-us/products/fuse-eda-ai-system/agent/) .


*Nemotron 3.5 Lightning is available on*[Hugging Face](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4) *,*[ModelScope](https://modelscope.ai/collections/nv-community/Nemotron-35-Lightning) *,*[OpenRouter](https://openrouter.ai/nvidia/nemotron-3.5-lightning:free) *and*[build.nvidia.com](https://build.nvidia.com/nvidia/nemotron-3.5-lightning-30b-a3b) *as an NVIDIA NIM microservice as well as through a broad ecosystem of NVIDIA Cloud Partners, post-training platforms, inference platforms and cloud service providers. NeMo Switchyard is available on*[GitHub](https://github.com/NVIDIA-NeMo/Switchyard) *and coming to partner platforms soon.*
