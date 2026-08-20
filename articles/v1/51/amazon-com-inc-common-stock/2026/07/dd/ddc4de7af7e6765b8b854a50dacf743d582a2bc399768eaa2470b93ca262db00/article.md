---
schema_version: "1.0.0"
document_id: "ddc4de7af7e6765b8b854a50dacf743d582a2bc399768eaa2470b93ca262db00"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-sagemaker-ai-g7/"
published_at: "2026-07-22T15:23:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:211f2ff3154b10a8011d0d7d3a8aebb0ad809708cda5e51eeec78a15c4076e5b"
---

# Amazon SageMaker AI inference now supports G7 instances

Amazon SageMaker AI inference now supports[G7 instances](https://aws.amazon.com/ec2/instance-types/g7/) powered by NVIDIA RTX PRO 4500 Blackwell Server Edition GPUs, enabling you to deploy machine learning models with up to 4.6x AI inference performance compared to previous-generation G6 instances. Customers deploying generative AI models for production inference need high GPU throughput and memory capacity to serve medium-to-large models cost-effectively, but previous-generation instances often required over-provisioning expensive compute or quantizing models to fit within memory constraints.


G7 instances provide 32 GB of GPU memory per GPU with 5th Generation Tensor Cores, up to 700 Gbps of EFA-enabled networking (7x compared to G6), and up to 7.6 TB of local NVMe SSD storage for keeping large models close to compute. These capabilities make G7 instances well suited for serving models in the 7B–30B parameter range, image and video generation workloads, and multi-model inference endpoints that benefit from higher memory bandwidth and throughput. You can deploy models on G7 instances using the SageMaker AI Inference console, API, or SDK by specifying G7 instance types (such as ml.g7.xlarge through ml.g7.48xlarge) in your endpoint configuration.


G7 instances for SageMaker AI inference are available in US East (N. Virginia, Ohio) and US West (Oregon). For pricing information on these instances, please visit our[pricing page](https://aws.amazon.com/sagemaker/ai/pricing/?refid=ft_sagemaker) .
