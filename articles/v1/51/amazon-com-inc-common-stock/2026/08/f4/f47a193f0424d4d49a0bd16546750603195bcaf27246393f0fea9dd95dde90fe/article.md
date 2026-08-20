---
schema_version: "1.0.0"
document_id: "f47a193f0424d4d49a0bd16546750603195bcaf27246393f0fea9dd95dde90fe"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-bedrock-web-access-web-search/"
published_at: "2026-08-19T23:00:00+00:00"
first_seen_at: "2026-08-20T00:12:43.329206+00:00"
fetched_at: "2026-08-20T00:12:46.759003+00:00"
content_hash: "sha256:86a6e120ff452009a0ed200868e79ffd197cad79d830e3aadd038090c00dff9d"
---

# Launching External Web Access for Web Search on Amazon Bedrock

Earlier this month, we announced[Web Search on Amazon Bedrock](https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-bedrock-web/) , a built-in server-side tool that allows you to ground model responses with current web knowledge, while maintaining data within your secured AWS environment with zero data egress. Today, we are expanding Web Search to enable the external_web_access parameter allowing Web Search to retrieve content directly from the public web so models can ground responses in the latest information.


To enable external_web_access, grant the bedrock-websearch:ExternalWebAccess IAM permission to the request identity and leave the external_web_access parameter at its default of true. In doing so, Web Search can then fetch content live from the public web for use cases that need the freshest possible information, such as latest sports score, live pricing, or newly released documentation. If handling sensitive data, to keep retrieval entirely within your AWS boundary, set external_web_access: false. By setting it false, Web Search serves results only from Amazon's in-AWS web index and knowledge graph, with no request data leaving the AWS boundary.


Enabling External Web Access is available in the following AWS Regions: US East (N. Virginia), US East (Ohio), and US West (Oregon). To learn more, read our blog post[Introducing Web Search on Amazon Bedrock for foundation model grounding,](https://aws.amazon.com/blogs/machine-learning/introducing-web-search-on-amazon-bedrock-for-foundation-model-grounding/) review[Controlling external web access](https://docs.aws.amazon.com/bedrock/latest/userguide/web-search.html#web-search-controlling-external) in the Amazon Bedrock User Guide, and visit the[Amazon Bedrock pricing page](https://aws.amazon.com/bedrock/pricing/) for cost details.
