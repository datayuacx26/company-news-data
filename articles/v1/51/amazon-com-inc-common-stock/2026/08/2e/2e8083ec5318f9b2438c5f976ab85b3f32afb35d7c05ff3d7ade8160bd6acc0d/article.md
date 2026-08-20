---
schema_version: "1.0.0"
document_id: "2e8083ec5318f9b2438c5f976ab85b3f32afb35d7c05ff3d7ade8160bd6acc0d"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-sagemaker-fft"
published_at: "2026-08-03T17:00:00+00:00"
first_seen_at: "2026-08-03T21:24:28.279899+00:00"
fetched_at: "2026-08-03T21:33:45.218976+00:00"
content_hash: "sha256:119d05eb0e818b4cd4d486e18fd439c8db76fa31ffb3a424102e79582307c7b5"
---

# Amazon SageMaker AI serverless model customization now supports full fine-tuning

Amazon SageMaker AI serverless model customization now supports full fine-tuning for over 25 open-source models. These include popular models from gpt-oss, Gemma, Llama, Nemotron, and Qwen model families. In addition to parameter-efficient methods such as LoRA, which update a small subset of model weights, you can now update all parameters in the model for deeper adaptation when your use case requires it.


Full fine-tuning allows the model to more thoroughly learn your domain-specific patterns, terminology, and task structure. This is particularly valuable when you need the model to acquire capabilities beyond surface-level style adjustments, such as learning specialized reasoning patterns, adopting complex output formats, or internalizing domain knowledge from large proprietary datasets. With serverless model customization, SageMaker manages all infrastructure provisioning and training orchestration, so you can run full fine-tuning jobs without provisioning or managing any infrastructure and you pay only for what you use.


Serverless full fine-tuning on SageMaker is available in US East (N. Virginia), US West (Oregon), Asia Pacific (Tokyo), and Europe (Ireland). To get started, navigate to the JumpStart and Models page in Amazon SageMaker Studio to launch a customization job, or use the[SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/model_customization/index.html) . To learn more, and see the supported list of models, see the Amazon SageMaker AI[model customization documentation.](https://docs.aws.amazon.com/sagemaker/latest/dg/model-customize-open-weight.html)
