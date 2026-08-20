---
schema_version: "1.0.0"
document_id: "74a1a66e228d1dd6dc65315453fca396edea721659badfb6fae532979fc601de"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-lambda-prompt-coding-agents/"
published_at: "2026-07-14T20:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:d4adc06382f4fef7353212be0ab2f8ec1b9c435c083ac7d75120b47b5fe9d1f9"
---

# AWS Lambda console provides a one-click setup prompt for coding agents

AWS Lambda console now provides a one-click setup prompt for coding agents that configures your agent with AWS Serverless skills and the Serverless Model Context Protocol (MCP) server, embedding serverless best practices from the start. This setup is available on the Lambda console wherever the developers start their Lambda journey: whether they are getting started with Lambda, exploring its capabilities, or have created their first function.


Developers use coding agents to build, test, and deploy Lambda functions, but setting up an agent for serverless development previously required navigating across multiple documentation pages to find the right configuration. The one-click setup prompt eliminates this friction as it provides a prompt that instructs the agent to install AWS Serverless skills (hosted in[Agent Toolkit for AWS](https://aws.amazon.com/products/developer-tools/agent-toolkit-for-aws/) ) and the Serverless MCP server directly in the developer's preferred coding agent. The prompt references the[Lambda agent setup guide](https://docs.aws.amazon.com/lambda/latest/dg/samples/aws-lambda-agent-setup.md) , which includes installation commands for Claude Code, Kiro, Cursor, GitHub Copilot, Codex, Devin Desktop, and OpenCode, for the AWS Serverless skills, three specialized Lambda skills (MicroVM, Managed Instances, durable functions), and Serverless MCP server configuration. If a developer does not have local AWS authentication configured, the prompt guides them to connect using the signing-in-to-aws skill.


This capability is available in all commercial[AWS Regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/?refid=2abe6167-e3db-40c4-a9fa-b283e7b4d7c8) (except Middle East (Bahrain) and Middle East (UAE)) and AWS GovCloud (US) Regions where Lambda is available. Get started by visiting the[AWS Lambda console](https://console.aws.amazon.com/lambda) or learn more in the[Lambda agent setup guide](https://docs.aws.amazon.com/lambda/latest/dg/samples/aws-lambda-agent-setup.md) .
