---
schema_version: "1.0.0"
document_id: "671b6c5d45a547f0e2051be5a279dfcd58745b38c64bc9c6c37e3e25879644c8"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/01/locateAnything-3B-qwen-agentworld-35B-A3B-qwen3.5-122B-A10B-on-sagemaker-jumpstart/"
published_at: "2026-08-11T15:22:00+00:00"
first_seen_at: "2026-08-11T17:03:23.922129+00:00"
fetched_at: "2026-08-11T17:03:27.183313+00:00"
content_hash: "sha256:a0508d13e95b922f310f3b3070c4d011f1b9a66a111562df975048ca8556432a"
---

# LocateAnything-3B, Qwen-AgentWorld-35B-A3B, and Qwen3.5-122B-A10B models now available on Amazon SageMaker JumpStart

NVIDIA's LocateAnything-3B, Qwen's Qwen-AgentWorld-35B-A3B, and Qwen's Qwen3.5-122B-A10B models are now available on Amazon SageMaker JumpStart, expanding the portfolio of foundation models available to AWS customers. These three models bring specialized capabilities spanning visual grounding, agent environment simulation, and large-scale multimodal reasoning, enabling customers to deploy high-performance, scalable AI solutions on AWS infrastructure.


These models address different enterprise AI challenges with specialized capabilities:


LocateAnything-3B is optimized for fast, high-quality visual grounding and object localization from natural language instructions. It uses a Parallel Box Decoding (PBD) framework that decodes bounding boxes and points as atomic units in a single step, preserving geometric coherence and unlocking substantial parallelism. It enables precise object localization, dense detection, and point-based localization across diverse domains in both Enterprise Intelligence and Physical AI applications.


Qwen-AgentWorld-35B-A3B excels in simulating agent environments across seven interaction domains: tool calling, search, terminal, software engineering, Android, web, and OS interaction. It is the first language world model to cover all seven domains within a single model, predicting next environment states given an agent's action and interaction history via long chain-of-thought reasoning—trained on over 10 million real-world interaction trajectories.


Qwen3.5-122B-A10B provides high-performance multimodal reasoning with production-friendly efficiency. It features 122B total parameters with only 10B activated per token through a hybrid architecture integrating Gated Delta Networks with sparse Mixture-of-Experts (256 experts), delivering strong reasoning, coding, agents, and visual understanding performance with a native 262K context window and minimal latency overhead.


With SageMaker JumpStart, customers can deploy any of these models with just a few clicks to address their specific AI use cases.


To get started with these models, navigate to the SageMaker JumpStart model catalog in the SageMaker console or use the SageMaker Python SDK to deploy the models to your AWS account. For more information about deploying and using foundation models in SageMaker JumpStart, see the[Amazon SageMaker JumpStart documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) .


###
