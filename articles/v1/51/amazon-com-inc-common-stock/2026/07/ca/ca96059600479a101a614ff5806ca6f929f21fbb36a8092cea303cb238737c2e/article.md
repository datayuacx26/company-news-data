---
schema_version: "1.0.0"
document_id: "ca96059600479a101a614ff5806ca6f929f21fbb36a8092cea303cb238737c2e"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/amazon-bedrock-agentcore-unified-observability-single-log-group/"
published_at: "2026-07-23T17:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:4dad343739b02460af9b232d9ba673dd58f790d58b1f339af56de5dad55e8b7a"
---

# Amazon Bedrock AgentCore now delivers unified observability with traces and logs in a single log group

Amazon Bedrock AgentCore now delivers agent traces and prompts to the same log group as your agent's logs, giving you unified observability for AI agents in a single Amazon CloudWatch log group.


Previously, AgentCore split agent telemetry across multiple destinations trace spans went to the shared \`aws/spans\` log group while event logs containing prompts, inputs, and outputs went to a separate resource-specific log group. This meant debugging an agent invocation required searching across multiple log groups, and customers could not apply fine-grained access control or customer-managed key (CMK) encryption at the individual agent level. With today's launch, all of an agent's telemetry traces, prompts, structured logs, and standard output is delivered to a single per-agent log group (\`/aws/bedrock-agentcore/runtimes/<agent_id>-<endpoint_name>\`). You can now correlate traces and logs in one place, scope IAM policies and CMK encryption to individual agents, and export all telemetry by subscribing to a single log group. For multi-agent systems, each agent's complete execution history stays together, making end-to-end debugging straightforward.


All newly created agents starting July 20, 2026 in supported AWS Regions use unified observability by default starting no configuration needed. For existing agents, set the \`UNIFIED_TRACES_DESTINATION_ENABLED=true\` environment variable on your agent runtime and upgrade ADOT to version 0.17.1 or later. This feature is available in all AWS commercial regions where AgentCore runtime is supported. Learn more in the[AgentCore Developer Guide](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability-configure.html) .
