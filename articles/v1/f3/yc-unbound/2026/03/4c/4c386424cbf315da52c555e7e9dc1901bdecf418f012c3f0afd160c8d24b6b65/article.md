---
schema_version: "1.0.0"
document_id: "4c386424cbf315da52c555e7e9dc1901bdecf418f012c3f0afd160c8d24b6b65"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/mcp-server-governance"
published_at: "2026-03-08T00:00:00+00:00"
first_seen_at: "2026-07-22T17:49:49.784561+00:00"
fetched_at: "2026-07-28T21:58:49.530734+00:00"
content_hash: "sha256:a6c8cacce666fc97aaba08d2457ef20cf351cf808823547823f1fa314af835cc"
---

# Your Team Adopted Cursor. Do You Know Which MCP Servers It Can Reach?

Your engineering team is probably already using Cursor, Claude Code, or Windsurf. These AI coding agents have become the fastest-growing category of developer tools in history. But there is a dimension of their capabilities that rarely shows up in procurement reviews or security assessments: **MCP servers** .


## What Is MCP?


The Model Context Protocol (MCP) is an open standard, originally developed by Anthropic, that lets AI agents connect to external tools and data sources. Think of it as a plugin system for AI coding agents. An MCP server exposes a set of "tools" — functions the agent can call — along with access to resources like files, databases, or APIs.


When a developer configures an MCP server in Cursor or Claude Code, the AI agent gains the ability to call those tools autonomously. (For a closer look at how this works across Claude's multiple surfaces, see[Governing Claude Across Surfaces](https://getunbound.ai/blog/governing-claude-across-surfaces) .) A single MCP server might expose tools to query a production database, create Jira tickets, push to a Git repository, or read secrets from a vault.


## The Permission Problem


Here is where it gets concerning. MCP servers typically run with the permissions of the developer who configured them. If a developer has read access to a production database, and they configure an MCP server that wraps that database, the AI agent now has the same access — with no additional authentication, no audit trail, and no policy boundary.


Most MCP servers are configured locally in a JSON file on the developer's machine. There is no centralized registry. No approval workflow. No visibility for security teams. It is shadow IT, but instead of an unapproved SaaS app, it is an unapproved bridge between an AI agent and your production infrastructure.


## What Can Go Wrong?


The attack surface is not theoretical:


- **Data exfiltration** : An MCP server connected to a database could allow an AI agent to read and surface sensitive customer data in a chat response, which might then be sent to an LLM provider's API.
- **Prompt injection via tools** : A malicious or compromised MCP server could return crafted responses that manipulate the agent into executing harmful actions — a technique known as tool-mediated prompt injection.
- **Supply-chain risk** : Public MCP server registries are emerging, and developers are installing community-built servers with minimal vetting. A trojaned MCP server could exfiltrate environment variables, inject backdoors into code, or establish persistence on a developer workstation.
- **Lateral movement** : An agent with access to multiple MCP servers can chain tool calls across systems. A query to a documentation server might reveal credentials that the agent then uses via a different MCP server to access infrastructure.


## What Security Teams Need


The first step is visibility. Security teams need answers to basic questions:


1. **Which AI coding agents** are in use across the organization?
2. **Which MCP servers** are configured on developer machines?
3. **What tools and permissions** does each MCP server expose?
4. **What actions** are agents actually taking through these servers?


The second step is governance. Organizations need the ability to define policies around MCP server usage — allow-listing approved servers, blocking connections to untrusted ones, and enforcing least-privilege principles on tool access. This is one of the core functions of an[Agent Access Security Broker (AASB)](https://getunbound.ai/blog/what-is-aasb) .


The third step is monitoring. Every tool call an agent makes through an MCP server should be logged, attributed, and available for audit. When an agent queries a production database at 2 AM, someone should know.


## The Clock Is Ticking


MCP adoption is not slowing down. New servers are being published to registries daily. Developers are configuring them without security review because no review process exists. Every week that passes without governance is another week of unmonitored agent access to your infrastructure.


---


**Unbound AI provides full visibility into MCP server usage across your organization.** We discover every configured server, map its permissions, and give security teams the controls they need.


**Start free.** Sign up at[getunbound.ai/free](https://www.getunbound.ai/free) and discover which MCP servers are configured across your engineering organization today.


**Book a demo.** See MCP governance, terminal policy enforcement, and approval workflows in action at[getunbound.ai/book-demo](https://www.getunbound.ai/book-demo) .
