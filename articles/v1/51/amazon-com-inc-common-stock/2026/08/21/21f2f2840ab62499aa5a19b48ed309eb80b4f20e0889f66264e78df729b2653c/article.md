---
schema_version: "1.0.0"
document_id: "21f2f2840ab62499aa5a19b48ed309eb80b4f20e0889f66264e78df729b2653c"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-bedrock-openai-india-v1/"
published_at: "2026-08-18T23:30:00+00:00"
first_seen_at: "2026-08-19T00:16:28.993170+00:00"
fetched_at: "2026-08-19T00:16:30.402166+00:00"
content_hash: "sha256:08d57dcf236cc589896fdba2c6c00d73210f970fc722bcf8f5798ac649abbbdb"
---

# Amazon Bedrock now supports OpenAI models in India

Amazon Bedrock now supports the OpenAI GPT-5.6 models (Terra and Luna) in India, with India Geo cross-Region inference. Customers with regulatory requirements of in-country inferencing can now use OpenAI models at scale ensuring that inferencing is processed within India.


Cross-Region inference automatically routes inference requests across multiple AWS Regions to give you higher throughput, without you needing to manage capacity across multiple Regions. The new India Geo inference profiles—in.openai.gpt-5.6-terra for Terra and in.openai.gpt-5.6-luna for Luna—route requests only within the India geography, across AWS Regions such as Asia Pacific (Mumbai) and Asia Pacific (Hyderabad), so you can scale to meet demand while keeping data processed within India to meet data residency requirements. The models run on the bedrock-runtime endpoint with support for the Responses, Chat Completions, and Converse APIs, and work with the same account-level controls you already use for other models on Bedrock, including model invocation logging (deliverable to Amazon S3 or Amazon CloudWatch Logs), Amazon CloudWatch metrics, and cost itemization in AWS Cost Explorer and the AWS Cost and Usage Report.


OpenAI models with India cross-Region inference are available in the Asia Pacific (Mumbai) and Asia Pacific (Hyderabad) Regions. To get started, review the model cards for GPT-5.6 (Terra and Luna) and the Cross-Region inference section in the[Amazon Bedrock User Guide.](https://docs.aws.amazon.com/bedrock/latest/userguide/getting-started.html)
