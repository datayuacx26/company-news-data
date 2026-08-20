---
schema_version: "1.0.0"
document_id: "9caf79ba2c889a398b17cfe154fbf5cf857ceadff4705d6025c549164425921b"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/remote-mcp-server"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:28ff8f7eea69db8b4b227e3efdda090fbdb7f4176ca2a1455d201fa739760296"
---

# Remote MCP Server

Agents are everywhere: your terminal, browser, and automation tools. We've been hard at work to bring our MCP server to everywhere your agents live.


Today, we're launching our hosted, **remote MCP server** at[mcp.resend.com/mcp](https://mcp.resend.com/mcp) with full OAuth support for authentication. Use it with all your agents today.


With the remote MCP server:


- Review request logs during incident response
- Trigger transactional sends from CI/CD pipelines
- Sync contact segments from Zapier or n8n
- Draft support replies inside helpdesk workflows
- Audit webhook deliveries from monitoring dashboards


## Install it everywhere


Hosting the server enables a single, trusted endpoint for all MCP clients. Streamable HTTP transport works anywhere remote MCP servers are supported.


Connect Resend from the[connector directory](https://claude.ai/directory/connectors/resend) . The Connector bundles the MCP server and every Resend skill.


Our local[resend-mcp](https://github.com/resend/resend-mcp) server stays open source for self-hosting, air-gapped setups, and stdio clients. For all other MCP clients and surfaces, see the[MCP server docs](https://resend.com/docs/mcp-server) .


## Secure authentication


We've added OAuth authentication to the MCP server, so you can sign in with your Resend account when you connect. Choose your account and grant the necessary permissions.


You can view or revoke your MCP server's OAuth permissions from your Resend account's Team settings.


If your client runs somewhere a browser login isn’t possible (a server, CI, scripts, or a headless agent), pass a Resend API key as a Bearer token instead of using OAuth.


```text
claude mcp   add     --transport   http resend https://mcp.resend.com/mcp   --header     "Authorization: Bearer re_xxxxxxxxx"
```


## Full coverage


The remote MCP server gives your AI agent native access to the full Resend platform through a single integration: emails, templates, broadcasts, contacts, logs, webhooks, and more.


[View a full list](https://resend.com/docs/mcp-server) of all features and supported clients.


## Log visibility


Each logged action includes a` user agent` trace, so you have full visibility on each logged action your agent takes with MCP.


## What's next


The remote MCP server opens up a new way to interact with Resend, allowing you to control your email campaigns and contacts from anywhere, to build custom integrations, and more.


It's also the foundation for one-click integrations across your favorite agents: the[Resend connector for Claude](https://resend.com/changelog/claude-connector) and the[Resend plugin for Codex](https://resend.com/changelog/codex-plugin) .


The applications are nearly endless and we can't wait to see what you build with it.
