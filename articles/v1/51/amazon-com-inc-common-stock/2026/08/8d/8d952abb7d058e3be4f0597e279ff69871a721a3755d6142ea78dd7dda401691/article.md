---
schema_version: "1.0.0"
document_id: "8d952abb7d058e3be4f0597e279ff69871a721a3755d6142ea78dd7dda401691"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/01/glm-5.2-fp8-nemotron-nano-12b-v2-glm-ocr-on-sagemaker-jumpstart/"
published_at: "2026-08-10T23:02:00+00:00"
first_seen_at: "2026-08-11T02:12:04.648727+00:00"
fetched_at: "2026-08-11T02:12:06.305464+00:00"
content_hash: "sha256:c78ed01d0fd501192cf2d16510c968ac82a6a0c2d1ccc99641861b8d3637f303"
---

# GLM-5.2 FP8, NVIDIA-Nemotron-Nano-12B-v2 and GLM-OCR models now available on Amazon SageMaker JumpStart

Z.ai's GLM-5.2 FP8, NVIDIA's Nemotron-Nano-12B-v2, and Z.ai's GLM-OCR models are now available on Amazon SageMaker JumpStart, expanding the portfolio of foundation models available to AWS customers. These three models bring specialized capabilities spanning long-horizon agentic engineering, efficient hybrid reasoning, and advanced document understanding, enabling customers to deploy high-performance, scalable AI solutions on AWS infrastructure.


GLM-5.2 FP8 is optimized for long-horizon tasks and agentic engineering workflows such as full-cycle software development from requirements to deployment. It delivers a substantial leap in long-horizon task capability over its predecessor GLM-5.1 and, for the first time, provides a truly usable 1M-token context window, enabling it to handle project-level engineering context, execute long-running tasks reliably, follow engineering standards consistently, and complete full development workflows in a single task.


NVIDIA-Nemotron-Nano-12B-v2 excels in unified reasoning and non-reasoning tasks with high inference throughput, making it ideal for enterprise applications requiring both accuracy and efficiency. It uses a hybrid Mamba-2 and Transformer architecture with a 128K context length, generating reasoning traces before concluding with final responses. Its compact 12B parameter design achieves comparable or better accuracy than leading open models while delivering up to 6x higher inference throughput.


GLM-OCR provides accurate, fast, and comprehensive document understanding for complex real-world materials including scanned PDFs, handwritten notes, dense academic papers with formulas, multi-column tables, code documentation, and multilingual text. This 0.9B-parameter multimodal model reconstructs structure, tables, and formulas into clean Markdown, JSON, or LaTeX, with latency low enough for real-time services and edge devices—ideal for large-scale document processing and invoice extraction workflows.


With SageMaker JumpStart, customers can deploy any of these models with just a few clicks to address their specific AI use cases.


To get started with these models, navigate to the SageMaker JumpStart model catalog in the SageMaker console or use the SageMaker Python SDK to deploy the models to your AWS account. For more information about deploying and using foundation models in SageMaker JumpStart, see the[Amazon SageMaker JumpStart documentation](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-jumpstart.html) .
