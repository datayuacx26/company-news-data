---
schema_version: "1.0.0"
document_id: "a67c3736c379c9a9d8f89135f0196d23adc3afbd895f5d253f5e0273484a8ff7"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/01/langcache-embed-v3-small-mellum2-12B-A2.5B-thinking-lightOnOCR-2-1B-on-sagemaker-jumpstart/"
published_at: "2026-08-10T23:05:00+00:00"
first_seen_at: "2026-08-11T02:12:04.648727+00:00"
fetched_at: "2026-08-11T02:12:06.305464+00:00"
content_hash: "sha256:402e6763f53b2d22ed789c445db47d127147b50368b328e1ad854bb3d56984e3"
---

# langcache-embed-v3-small, Mellum2-12B-A2.5B-Thinking, and LightOnOCR-2-1B models now available on Amazon SageMaker JumpStart

Redis's langcache-embed-v3-small, JetBrains' Mellum2-12B-A2.5B-Thinking, and LightOn's LightOnOCR-2-1B models are now available on Amazon SageMaker JumpStart, expanding the portfolio of foundation models available to AWS customers. These three models bring specialized capabilities spanning semantic caching optimization, code-focused reasoning, and end-to-end document OCR, enabling customers to deploy high-performance, scalable AI solutions on AWS infrastructure.


langcache-embed-v3-small is optimized for semantic caching in LLM applications. It maps sentences and paragraphs into a dense vector space purpose-built for identifying semantically equivalent queries regardless of phrasing, enabling intelligent cache hits that reduce redundant LLM calls and accelerate response times in high-volume inference workloads.


Mellum2-12B-A2.5B-Thinking excels in code generation, debugging, multi-step reasoning, and agentic coding workflows. It uses a Mixture-of-Experts architecture (64 experts, 8 activated per token), activating only 2.5B of its 12B total parameters per forward pass with a 131,072-token context length. It emits explicit chain-of-thought reasoning traces before final answers, delivering high-throughput, low-latency inference ideal for routing, RAG, sub-agents, and private deployments.


LightOnOCR-2-1B provides end-to-end multilingual document-to-text conversion for PDFs, scans, and images without brittle OCR pipelines. This 1B-parameter vision-language model directly transduces page images into clean, naturally ordered text, achieving state-of-the-art performance on OlmOCR-Bench while being ~9× smaller and significantly faster than competing approaches.


With SageMaker JumpStart, customers can deploy any of these models with just a few clicks to address their specific AI use cases.


To get started with these models, navigate to the SageMaker JumpStart model catalog in the SageMaker console or use the SageMaker Python SDK to deploy the models to your AWS account. For more information about deploying and using foundation models in SageMaker JumpStart, see the[Amazon SageMaker JumpStart documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) .
