---
schema_version: "1.0.0"
document_id: "d3bb9047a89c19ee912d694c4c92337cfa047c8e4fe90768937a98aaf361496c"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/retool-mcp-server"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:c672230af4dd904f4e46782fe9c33769b3825b4a6aa11404780e659ce277d7f8"
---

# Retool MCP Server: Manage Retool from Any AI Agent | Retool Blog

01 Available tool calls02 Where you build shouldn’t determine your security standards03 Getting started with MCP in Retool


You’re already[building in Claude](https://retool.com/blog/retool-vs-claude-code) , Cursor, Codex, or Kiro. When you receive a request to check on a connected resource in your Retool environment, leaving your preferred workspace is a disruption to your workflow.


Our MCP server, now available in public beta, brings Retool to wherever you work.


## Available tool calls


Instead of opening a browser, admins and builders can interact with Retool directly through AI coding environments to:


- Build and edit apps, deploy them onto Retool’s platform.
- Write queries against resources to power data analysis.
- Bulk-invite users or manage pending invites.
- Audit the users who have access to their organization.
- Inspect organization and resource environment configurations.
- List all connected resources and see their configurations.


You can view the full list of supported tools in the[MCP tools reference](https://docs.retool.com/org-users/reference/mcp-tools) .


## Where you build shouldn’t determine your security standards


AI coding tools have given builders incredible optionality in how and where they work.[Model Context Protocol (MCP)](https://retool.com/blog/what-is-model-context-protocol) makes these tools even more powerful by connecting them to other agents, databases, and SaaS platforms.


As the building surface expands, the security perimeter must grow with it. When builders can ship from anywhere, organizations need to ensure that “anywhere” still means “securely,” which is difficult when model behavior fluctuates from agent to agent.


You can prompt “enforce SSO for my app” and “create an audit log feature within the admin dashboard,” but what happens when you ship multiple apps like this? And your coworker does the same. Your team *feels* productive, but what you’re actually doing is adding tech debt and security risk. The freedom to choose your own tools shouldn’t come with a tradeoff on governance.


This is the problem Retool is built to solve.


Whether you’re building in a terminal or visual UI, Retool ensures every builder’s output follows their organization’s security standards. Guardrails like access controls, audit trails, and data permissions aren’t configured per app or per prompt. Instead, they’re enforced at the platform level. A data connection updated for one app is updated for all. A bug fixed once is fixed everywhere.


Retool’s MCP server extends this security model to connected coding agents. Whether you’re improving an existing app or starting from scratch, your changes will inherit the security standards your organization has already approved and implemented.


## Getting started with MCP in Retool


Connect Retool’s MCP server to any MCP-compatible client using:


- **URL** : https://<your-retool-instance>/mcp
- **Transport** : HTTP
- **Authentication** : OAuth 2.0


After authenticating, you may need to restart your coding agent. Setup instructions for Claude, Cursor, Codex, ChatGPT, and Kiro[are in the docs.](https://docs.retool.com/org-users/guides/mcp#connect-mcp-server)


Retool’s MCP server is currently available for cloud and self-hosted customers.


light Reader
