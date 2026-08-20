---
schema_version: "1.0.0"
document_id: "0e1e0634fbce9d0632ff38e24b6277438c6faf5b6830bef8e1af0aca45f552f2"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-bedrock-web/"
published_at: "2026-08-04T20:37:00+00:00"
first_seen_at: "2026-08-04T23:02:13.139824+00:00"
fetched_at: "2026-08-04T23:10:21.652752+00:00"
content_hash: "sha256:22f0e7288de4a9200405973f35a50a807dcabf8123d593d8d459825616198ad3"
---

# Amazon Bedrock launches Web Search for OpenAI GPT models

Today, we are announcing the general availability of Web Search on Amazon Bedrock, a built-in server side tool that performs web search entirely within AWS, enabling OpenAI models (GPT-5.4, GPT-5.5, and GPT-5.6 Sol/Terra/Luna) to ground responses with current web knowledge while maintaining data residency within your secured AWS environment with zero data egress. Previously, adding web grounding required onboarding a third-party search provider, managing separate API keys and billing, building custom orchestration, and conducting additional compliance reviews for each external vendor. Web Search removes this heavy lifting by enable grounding with a single parameter in an existing API call, with no vendor onboarding, no external APIs to orchestrate, and no additional vendor security reviews to conduct.


Web Search is built by Amazon, informed by years of experience across Alexa+, Amazon Quick and Kiro. It combines a web index operated by Amazon, spanning tens of billions of documents refreshed continually, with a built-in knowledge graph that provides verified facts. Rather than returning raw pages, Web Search performs semantic snippet extraction, delivering context-efficient results optimized for the model's context window with low latency. Web Search integrates through a standardized tool-use interface, compatible with the OpenAI Responses API. Simply add the web search tool to your API call, and Bedrock handles the entire search lifecycle server-side; a single API call returns a grounded response with citations.


Web Search on Amazon Bedrock is generally available today in US East (N. Virginia), US East (Ohio), and US West (Oregon). To get started, read our blog post[Introducing Web Search on Amazon Bedrock for foundation model grounding](https://aws.amazon.com/blogs/machine-learning/introducing-web-search-on-amazon-bedrock-for-foundation-model-grounding/) , review the[Web Search section in the Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/web-search.html) for technical documentation, and visit the[Amazon Bedrock pricing page](https://aws.amazon.com/bedrock/pricing/) for cost details.
