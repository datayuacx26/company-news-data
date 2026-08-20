---
schema_version: "1.0.0"
document_id: "7b4ac16c3ba5f5263ddf1796ceb71a57d0befa663b0d67d49fbfabcead9d12a9"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-bedrock-agentcore-runtime-instances-generally-available/"
published_at: "2026-08-06T20:00:00+00:00"
first_seen_at: "2026-08-07T03:27:14.540712+00:00"
fetched_at: "2026-08-07T03:27:16.648914+00:00"
content_hash: "sha256:e5863d35a076dff28f41b843a34810ed322fafacbd1d915231c2819a1f29b616"
---

# AgentCore runtime instances are now generally available

Today, AWS announces runtime instances, a new feature in Amazon Bedrock AgentCore that lets you run agents on your own Amazon EC2 instances without managing infrastructure. AgentCore runtime provides purpose-built infrastructure to deploy and operate AI agents securely at scale. Runtime instances complement the existing microVM-based option in AgentCore runtime and give teams running sustained, resource-intensive, or specialized-hardware agents access to the breadth of EC2 instance types, while AgentCore handles provisioning, patching, scaling, and lifecycle management.


Using the AWS Management Console, CLI, SDKs, or APIs, you create a capacity provider that specifies the EC2 instance types your agents need, including GPU-accelerated, memory-optimized, and compute-optimized families, and attach your agents to it. Runtime instances support long-running agent sessions of up to 14 days, while the default serverless, microVM-based runtime is designed for sessions of up to 8 hours that need fast startup . You can choose the right compute for each agent, or run a mix, without changing how you deploy or invoke your agents.


You can use runtime instances in the following AWS Regions: US East (N. Virginia), US East (Ohio), US West (Oregon), Asia Pacific (Mumbai), Asia Pacific (Singapore), Asia Pacific (Sydney), Asia Pacific (Tokyo), Europe (Frankfurt), and Europe (Ireland). You are charged for the management of the compute provisioned, in addition to your Amazon EC2 costs. To get started, visit[AWS News Blog](https://aws.amazon.com/blogs/aws/runtime-instances-persistent-compute-for-production-ai-agents-on-amazon-bedrock-agentcore/) or[AgentCore documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-instances-how-it-works.html) . To learn about pricing, visit[AgentCore pricing](https://aws.amazon.com/bedrock/agentcore/pricing/) .
