---
schema_version: "1.0.0"
document_id: "0e1b242e7d77f37f6985ebdb34505b2cb31993c91239fc32b521dfe07fcb395c"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-news-import-6460f63f60af"
canonical_url: "https://investors.digitalocean.com/news/news-details/2026/DigitalOcean-Launches-Inference-Engine-with-New-Capabilities-for-Production-AI-Including-Inference-Router-for-Efficient-Scaling-of-Agentic-Workloads/default.aspx"
published_at: "2026-04-28T08:00:00+00:00"
first_seen_at: "2026-07-21T16:29:05.431888+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:8cde69c7f4d271addea3aecc430090170b9c27de2175acfc3f3510e11144ce99"
---

# DigitalOcean Launches Inference Engine with New Capabilities for Production AI, Including Inference Router for Efficient Scaling of Agentic Workloads

[View All News](https://investors.digitalocean.com/news/default.aspx)


### DigitalOcean Launches Inference Engine with New Capabilities for Production AI, Including Inference Router for Efficient Scaling of Agentic Workloads


April 28, 2026


*Built alongside early design partners, the Inference Engine gives AI developers unified control over performance, cost, and scale — with customers reporting up to 67% lower inference costs.*


BROOMFIELD, Colo.--(BUSINESS WIRE)--


[DigitalOcean](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2F&esheet=54524125&newsitemid=20260428279648&lan=en-US&anchor=DigitalOcean&index=1&md5=e46a0af9107f401b59cc3e81c1de7b29) (NYSE: DOCN) today announced the launch of its Inference Engine, a set of new production capabilities that give AI builders exceptional performance and unified control over how they run, scale, and optimize inference workloads. The announcement comes ahead of[DigitalOcean Deploy](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2Fdeploy&esheet=54524125&newsitemid=20260428279648&lan=en-US&anchor=DigitalOcean+Deploy&index=2&md5=3fd1c4b79c046844c0bb38867c078369) , the company's conference for AI builders, where it will unveil their full, integrated platform and new capabilities live.


[DigitalOcean’s Inference Engine](https://cts.businesswire.com/ct/CT?id=smartlink&url=http%3A%2F%2Fdigitalocean.com%2Fproducts%2Finference-engine&esheet=54524125&newsitemid=20260428279648&lan=en-US&anchor=DigitalOcean%26%238217%3Bs+Inference+Engine&index=3&md5=c897d92e2dfbde82def2caed5421fcf0) is built around four core capabilities: **Inference Router** , **Batch Inference, Serverless Inference** , and **Dedicated Inference,** giving development teams a single engine to match every workload type to the right performance and cost profile, without stitching together separate providers.


**New Capabilities: Built for How AI Actually Runs in Production**


**Inference Router** is designed to solve one of the biggest inefficiencies in agentic AI: sending every request to the most expensive model. With Inference Router, AI builders can define a model pool, describe tasks and priorities in natural language mapped to that model, and optimize each request for cost and latency. Powered by DigitalOcean’s purpose-built MoE (Mixture of Expert) router model, Inference Router matches each request to the right model, helping teams improve performance and unit economics without the need to build or manage routing infrastructure themselves. Customers like LawVo are already benefitting from this new capability:


"DigitalOcean's Inference Router gives us the kind of intelligent model selection we would otherwise have had to build ourselves. It routes each request to the right model based on complexity, helping us reduce inference costs by more than 40% while maintaining the accuracy, speed, and reliability our users expect." — Hovsep Seraydarian, Co-Founder and CTO, LawVo


**Dedicated Inference** delivers predictable performance and exceptional unit economics for teams running high-scale, sustained workloads, with reserved capacity that eliminates the variability of shared infrastructure.


**Serverless Inference** provides a single API key to access dozens of models, with scale-to-zero elasticity and the industry’s first off-peak pricing, giving teams instant access to leading open-source models without managing infrastructure or paying for idle capacity.


**Batch Inference** reduces the cost of offline AI workloads by 50% through asynchronous execution, built-in retries, and a guaranteed 24-hour completion window. Batch Inference is purpose-built for workloads where real-time response isn't required but reliability is critical.


“Most teams building agentic systems today make a single model decision and apply it uniformly across their agentic workflows. They default to a frontier model and pay the generalization tax: premium prices and higher latency for work that often does not require the most expensive closed source model. Inference Router **** is the essential AI middleware that removes that tax by intelligently matching requests to the right model based on task, context, and developer-defined preferences. The result is a smarter operating model for inference - one that gives developers more control over quality, speed, and cost while helping AI-native builders move faster and build more durable businesses on DigitalOcean.” — Vinay Kumar, CPTO, DigitalOcean


**Performance Benchmarks: Independent Validation**


The new Inference Engine was built around three core advances: hardware and software integrations, including vLLM, TensorRT, and SGLang to maximize token throughput; request-path and model-level optimizations that improve unit economics without compromising quality; and distributed scaling designed for the bursty, uneven demands of production AI applications.


According to Artificial Analysis, an independent AI inference benchmarking platform, the[results demonstrate DigitalOcean leading](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2Fblog%2Fhow-we-built-fastest-deepseek-minimax-qwen-on-blackwell-ultra&esheet=54524125&newsitemid=20260428279648&lan=en-US&anchor=results+demonstrate+DigitalOcean+leading&index=4&md5=cd3744ba74a85a3d69a6bbdacb2ce9dd) across key inference performance metrics, including 3x faster time-to-first-answer-token and 3x higher output speed than Amazon Bedrock on DeepSeek V3.2 at 10,000 input tokens. DigitalOcean also delivers stronger performance across output speed and latency consistency compared with most hyperscaler and neo-cloud providers, and is one of only three providers ranked in the Most Favorable Quadrant on Artificial Analysis's Latency vs. Output Speed chart, with Amazon, SambaNova, Nebius, and six others falling outside it.


**Customers Report Significant Cost and Performance Gains**


The Inference Engine was co-developed alongside early design partners running real production workloads, and the results are already showing at scale.


Hippocratic AI, which runs safety-critical healthcare agents on the platform, achieved 2x production throughput and 40% lower P99 latency across more than 20 million patient interactions.


"In healthcare AI, a node going down isn't just an SLA issue, it impacts patient experience. We've pressed DigitalOcean hard on reliability, access to the newest hardware, and the ability to scale efficiently. They've delivered." — Debajyoti Datta, Co-Founder, Hippocratic AI


Workato's Research Lab, which processes over 1 trillion automated workloads, saw meaningful performance and cost improvements, achieving 77% faster time-to-first-token, 79% lower end-to-end latency, and 67% lower inference costs on DigitalOcean.


"Through close collaboration on performance optimization, DigitalOcean helped us accelerate our inference performance and overall progress by two to three times." — Oscar Wu, AI Research Scientist, Technical Lead, Workato


At Deploy in San Francisco, DigitalOcean will also unveil new products that show how it has built a five-layer stack purpose-built for the Inference Era. Hovsep Seraydarian of LawVo, Debajyoti Datta of Hippocratic AI, and Oscar Wu of Workato will share stories live at Deploy about how their teams are building and scaling real-world AI applications on DigitalOcean. In-person attendance is full; sign up to[watch the keynote live stream](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fwww.digitalocean.com%2Fdeploy%23register&esheet=54524125&newsitemid=20260428279648&lan=en-US&anchor=watch+the+keynote+live+stream&index=5&md5=0d398b78a9fa61aa47f219502b02faac) at 12pm Pacific on April 28.


**About DigitalOcean**


DigitalOcean is the Agentic Inference Cloud built for AI-native and digital-native enterprises scaling production workloads. The platform combines production-ready GPU infrastructure with a full-stack cloud — all built on open source at every layer — to deliver operational simplicity and predictable economics at scale. More than 640,000 customers trust DigitalOcean to power their cloud and AI infrastructure. Learn more at[digitalocean.com](https://cts.businesswire.com/ct/CT?id=smartlink&url=https%3A%2F%2Fdigitalocean.com&esheet=54524125&newsitemid=20260428279648&lan=en-US&anchor=digitalocean.com&index=6&md5=760f376627f9be48c511f7eb03c62a26) .


**Investor Relations**
Radu Patrichi, CFA
investors@digitalocean.com


**Media Relations**
Meghan Grady
press@digitalocean.com


Source: DigitalOcean


[View All News](https://investors.digitalocean.com/news/default.aspx)
