---
schema_version: "1.0.0"
document_id: "0d04018fc6a90b6c8cbec3a661208080c4202460c27dfe61daf80f60a8115596"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/temporal-policies-agentcore/"
published_at: "2026-08-06T04:00:00+00:00"
first_seen_at: "2026-08-06T21:16:52.186535+00:00"
fetched_at: "2026-08-06T21:16:53.695254+00:00"
content_hash: "sha256:ff078c5eb13852a6bb3a2f96e36ef0db6cae738bc16dc81f75de41dc5bf4b79a"
---

# Announcing temporal policies and rate limiting in Amazon Bedrock AgentCore

Amazon Bedrock AgentCore announces two new controls: temporal policies for stateful agent authorization and rate limiting for AI traffic.


Temporal policies let you define stateful authorization rules that evaluate each request in the context of an agent's prior actions within a session, because a single tool call can be safe in isolation yet harmful given what preceded it. With temporal policies you can enforce workflow sequencing, require that a tool argument exactly matches the output of a prior call, require human approval before taking privileged actions, and enforce data freshness.


Rate limiting enables per-user or per-group controls over how much traffic flows to the tools, models, and agents connected to your gateway. Using rules scoped by OAuth or AWS IAM, you can set rate limits on requests across all target types, tokens for inference targets, and concurrent connections to cap long-lived concurrent sessions, aiding downstream service availability and enforcing fair limit distribution.


For regional availability and to learn more, see the[documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html) , read the[announcement blog,](https://aws.amazon.com/blogs/machine-learning/control-agent-behaviors-and-cost-beyond-a-single-action-new-capabilities-in-amazon-bedrock-agentcore/) and explore the[Dogwood reference implementation](https://github.com/dogwood-policy) .
