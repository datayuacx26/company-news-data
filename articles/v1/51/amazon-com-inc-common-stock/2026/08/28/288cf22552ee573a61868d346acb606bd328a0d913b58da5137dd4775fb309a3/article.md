---
schema_version: "1.0.0"
document_id: "288cf22552ee573a61868d346acb606bd328a0d913b58da5137dd4775fb309a3"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-bedrock-cross-region-openai-v2/"
published_at: "2026-08-17T20:29:00+00:00"
first_seen_at: "2026-08-17T22:18:50.550803+00:00"
fetched_at: "2026-08-17T22:18:53.535075+00:00"
content_hash: "sha256:ba19ab07fe6c2cb0fba9fad0985fb8b3ee5a5dd7ba730e178cb6cd1a8f0fea28"
---

# Amazon Bedrock expands API support and introduces Cross Region Inferencing for OpenAI models

Amazon Bedrock now supports the OpenAI GPT-5.6 models (Sol, Terra, and Luna) on the bedrock-runtime endpoint, with support for the Responses, Converse, and Chat Completions APIs. It also adds support for cross-Region inference, allowing customers to use Global and Geo cross-Region inference to access higher throughput and lower inference costs.


Cross-Region inference automatically routes inference requests across multiple AWS Regions to give you higher throughput, without you needing to manage capacity across multiple Regions. Geo cross region inference routes requests within a predefined geography—including new US Geo (US CRIS) support with this launch—so you can scale while keeping data processed within that geography, while Global cross region inference serve requests from any commercial AWS Region where the model is available, giving you the broadest access to Bedrock capacity and the highest throughput during demand spikes. With Global cross-Region inference you also get lower costs as Global inferencing is priced lower per token for OpenAI models than in-Region and Geo inferencing. This launch also expands API support—you can also use OpenAI GPT models with the Responses API, Chat Completions API, and the Converse API on the bedrock-runtime endpoint. Because these native OpenAI APIs now run on bedrock-runtime, the models work with the same account-level controls you already use for other models on Bedrock: usage appears in Bedrock model invocation logging (deliverable to Amazon S3 or Amazon CloudWatch Logs) and in Amazon CloudWatch metrics covering invocation counts, token counts, latency, throttles, and errors, and it is itemized in AWS Cost Explorer and the AWS Cost and Usage Report so you can attribute spend by model.


Cross-Region inference for OpenAI models is available in all AWS Regions where OpenAI models on Amazon Bedrock are offered. To get started, review the model cards for GPT 5.6 (Sol, Tera and Luna) in the[Amazon Bedrock User Guide.](https://docs.aws.amazon.com/bedrock/)
