---
schema_version: "1.0.0"
document_id: "40770855e0c2c5e6b698b270194a7042ae062600507b74128309a80be7d4add1"
company_key: "nvidia"
company: "NVIDIA"
source_id: "co-nvda-newsroom-rss"
canonical_url: "https://blogs.nvidia.com/blog/local-ai-open-source-models-agents-nemotron/"
published_at: "2026-08-11T13:00:25+00:00"
first_seen_at: "2026-08-11T13:50:14.179640+00:00"
fetched_at: "2026-08-11T13:50:16.738157+00:00"
content_hash: "sha256:cf178cad700ff7cd2c620b019cc016907db04d50e0525215568eab925c0cd94b"
---

# NVIDIA and Local AI Community Fuel Open Source Models and Intelligent Agents

The open source ecosystem is making it easier for AI enthusiasts and developers to build, customize and run increasingly capable agents locally.


Throughout August, NVIDIA is celebrating the partners and open source communities moving local AI forward, along with the models, applications and tools emerging across the ecosystem. That includes NVIDIA’s latest open models, software and developer tools, plus the accelerated computing, libraries and educational resources that help users get started.


It’s shaping up to be a big month for agents. Follow along for the latest developments in this special-edition NVIDIA Local AI blog series, with new updates added over the coming weeks.


Follow NVIDIA RTX Spark on


[X](https://x.com/NVIDIARTXSpark) ,


[Instagram](https://www.instagram.com/nvidiartxspark/) ,


[TikTok](https://www.tiktok.com/@nvidiartxspark) and


[Facebook](https://www.facebook.com/NVIDIARTXSpark) — and stay informed by subscribing to the


[RTX AI PC newsletter](https://www.nvidia.com/en-us/ai-on-rtx/?modal=subscribe-ai) . Follow NVIDIA Workstation on


[LinkedIn](https://www.linkedin.com/showcase/3761136/) and


[X](https://x.com/NVIDIAworkstatn) .


---


*Tuesday, Aug. 11, 6:00 a.m. PT **[🔗](https://blogs.nvidia.com/blog/local-ai-open-source-models-agents-nemotron/#nemotron-switchyard)***


## NVIDIA Introduces Nemotron 3.5 Lightning for Fast, Specialized Agentic Tasks


Today, NVIDIA expanded its Nemotron 3 model family with


[Nemotron 3.5 Lightning](https://blogs.nvidia.com/blog/nemotron-lightning-switchyard-rtx-dgx) , a customizable open 30B


[mixture-of-experts (MoE)](https://www.nvidia.com/en-us/glossary/mixture-of-experts/) model for always-on agents.


Nemotron 3.5 Lightning delivers up to 4x faster token generation and 30% faster time to completion compared to open models in its class.


And because Nemotron 3.5 Lightning is open weights, AI enthusiasts and developers can fine-tune it with their own examples to better match specific tasks, interests and workflows. For example, they could train the model to:


- **Write in a preferred style:** Follow established tones, formats and terminology when writing emails, reports or other documents.


- **Learn a specialty:** Better understand the language and common tasks associated with areas such as photography, gaming or 3D design.


- **Code a certain way:** Follow preferred coding conventions, frameworks and testing approaches when writing, reviewing or refactoring code.


Paired with access to apps, files and other tools, these fine-tuned models can power more personalized local agentic AI experiences — from an assistant that helps manage email and calendars, to a smart-home agent that handles everyday routines, to a coding companion that works alongside developers on a local codebase.


NVIDIA collaborated with vLLM, Ollama, llama.cpp and LM Studio to provide the best local deployment experience for Nemotron 3.5 Lightning models — offering developers choice of NVFP4 and GGUF format of models. Unsloth also provides day-one support with optimized and quantized models for efficient local deployment via


[Unsloth Studio](https://unsloth.ai/docs/models/gemma-4) .


Nemotron 3.5 Lightning runs locally on


[NVIDIA RTX PCs](https://www.nvidia.com/en-us/ai-on-rtx/) ,


[NVIDIA DGX Spark](https://www.nvidia.com/en-us/products/workstations/dgx-spark/) and


[OEM GB10 systems](https://marketplace.nvidia.com/en-us/enterprise/personal-ai-supercomputers/?manufacturer=Acer%2CASUS%2CDell%2CGigabyte%2CHP%2CLenovo%2CMSI%2CSupermicro&superchip=GB10&page=1&limit=15) , and


[NVIDIA Jetson](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/) , and scales up to


[RTX PRO workstations](https://www.nvidia.com/en-us/products/workstations/) ,


[NVIDIA DGX Station and GB300 deskside](https://www.nvidia.com/en-us/products/workstations/dgx-station/) systems, data centers and cloud environments. With NVIDIA Blackwell systems available from Acer, ASUS,


[Dell Technologies](https://www.dell.com/en-us/blog/dell-expands-enterprise-agentic-ai-with-nvidia/) , Exxact, GIGABYTE, HP, Lenovo, MSI and


[Supermicro](https://learn-more.supermicro.com/data-center-stories/enterprise-agentic-ai-nvidia-nemotron-3-5-lightning-supermicro) , users can choose from a wide range of devices and form factors to fit their needs.


As generative AI adoption grows, enterprises are looking for ways to keep rising token costs in check without sacrificing access to frontier intelligence.


[NVIDIA NeMo Switchyard](https://github.com/NVIDIA-NeMo/Switchyard#2-run-a-standalone-profile-config-server) , an open source routing library, automatically directs each step of an agent workflow to the best-fit model based on accuracy, speed and cost. It also gives developers the flexibility to work across models and providers for different tasks.


Internal benchmarks show that NeMo Switchyard, by routing each step across a system of models, helped maintain frontier-level task completion while reducing benchmark completion cost to roughly one-third of Opus 4.8 alone. NeMo Switchyard is available on


[GitHub](https://github.com/NVIDIA-NeMo/Switchyard) .


Visit the


[Nemotron 3.5 Lightning](https://blogs.nvidia.com/blog/nemotron-lightning-switchyard-rtx-dgx) ,


[NeMo Switchyard](https://developer.nvidia.com/blog/route-ai-agent-workloads-across-models-with-nvidia-nemo-switchyard) and


[Jetson AI](http://jetson-ai-lab.com/models/nemotron3-5-lightning/) technical blogs to get started. And to build at the edge, start with


[Jetson AI Lab tutorials](https://www.jetson-ai-lab.com/) and discover


[real-world Jetson projects](https://blogs.nvidia.com/blog/build-ai-with-nvidia-jetson/) . Nemotron 3.5 Lightning is also available through OpenRouter, on


[build.nvidia.com](http://build.nvidia.com/) as an NVIDIA NIM microservice, and through a broad ecosystem of NVIDIA Cloud Partners, post-training platforms, inference platforms and cloud service providers. NeMo Switchyard is available on


[GitHub](https://github.com/NVIDIA-NeMo/Switchyard) .
