---
schema_version: "1.0.0"
document_id: "03dc29aff541601874977be5fad40049275e0bea13f1ce2c4afb0f8882ea179a"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/run-claude-code-safely-with-blaxel-sandboxes"
published_at: "2026-02-02T17:53:27+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T22:22:21.045319+00:00"
content_hash: "sha256:8dfcf162d25a4eb74ea2634c8141f540ba5eb0fdaa88cc34cd220dd5c287b182"
---

# Run Claude Code safely with Blaxel sandboxes

A few months ago, Anthropic[released](https://claude.com/blog/building-agents-with-the-claude-agent-sdk) the[Claude Agent SDK](https://platform.claude.com/docs/en/agent-sdk/overview) (formerly *Claude Code SDK* ), a library that allows developers to build their own agents, powered by the same technology behind[Claude Code](https://claude.com/product/claude-code) . This SDK includes powerful[capabilities](https://platform.claude.com/docs/en/agent-sdk/overview#capabilities) , such as support for MCP, event hooks and sub-agents, human-in-the-loop approval and the ability to maintain context across sessions. As a result, it's quickly become[one of the most popular frameworks](https://claude.com/blog/building-agents-with-the-claude-agent-sdk) for agent development.


Blaxel's[Agents Hosting](https://docs.blaxel.ai/Agents/Overview) lets you deploy agents developed with any framework or SDK on Blaxel, so long as the agent exposes an HTTP endpoint for requests. We fully support all agentic frameworks like AI SDK, Mastra...and, of course, Claude Agent SDK. Once deployed, your agents are[globally accessible](https://docs.blaxel.ai/Infrastructure/Global-Inference-Network) ,[deeply observable with full logging, telemetry and tracing](https://docs.blaxel.ai/Observability/Overview) , and customizable using[fine-grained deployment rules](https://docs.blaxel.ai/Model-Governance/Policies) .


In addition to hosting agents, every Blaxel[sandbox](https://docs.blaxel.ai/Sandboxes/Overview) exposes a[Model Context Protocol (MCP) server](https://docs.blaxel.ai/Sandboxes/MCP) that allows agents to operate the sandbox using tool calls. These include[tools for process management, filesystem operations, and code generation](https://docs.blaxel.ai/Sandboxes/MCP#tools-available-in-the-mcp-server) .


You're probably starting to see where this is headed...what if you give access to remote, secure sandboxes to your Claude Agent, and co-host it next to the sandbox so your users can experience local-like latency even at scale? Blaxel lets you do both.


To help you get started with Claude agents on Blaxel, we've put together two guides:


- A starter guide, showing you how to[connect your Claude agent to a Blaxel sandbox (through its MCP server) and have it "do something" with the sandbox](https://docs.blaxel.ai/Tutorials/Claude-Agent-SDK-MCP) .
- A more advanced guide, explaining how to[deploy the agent itself to Blaxel as a serverless auto-scalable API](https://docs.blaxel.ai/Tutorials/Claude-Agent-SDK) .


All you need is your[Anthropic API key](https://platform.claude.com/) , a[Blaxel account](https://blaxel.ai/) , and the[Blaxel CLI](https://docs.blaxel.ai/cli-reference/introduction) .


Start building with Blaxel


Run Claude Code in sandboxes with near-instant latency. Sign up and get started for free in minutes.


[Get started free](https://app.blaxel.ai/)


Check it out and see how easy it is to deploy your Claude Agent SDK projects to Blaxel - with almost no configuration required! Have feedback?[Tell us in Discord](https://discord.gg/enAfyZFWHW) .
