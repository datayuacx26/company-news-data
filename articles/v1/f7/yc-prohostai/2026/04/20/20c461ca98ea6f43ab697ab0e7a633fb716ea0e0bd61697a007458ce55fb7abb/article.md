---
schema_version: "1.0.0"
document_id: "20c461ca98ea6f43ab697ab0e7a633fb716ea0e0bd61697a007458ce55fb7abb"
company_key: "yc-prohostai"
company: "ProhostAI"
source_id: "yc-prohostai-rss-f015bbed11b4"
canonical_url: "https://www.prohost.ai/blog/developers-api-and-mcp"
published_at: "2026-04-22T19:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.827263+00:00"
fetched_at: "2026-07-28T22:15:40.956663+00:00"
content_hash: "sha256:bcaf905b7fba7be9c9ed067d9ea8991525dae84c312a68bf550354beebe0d52a"
---

# Build on ProhostAI: API & MCP for Developers

**ProhostAI now offers a developer platform at connect.prohost.ai: one REST API and one MCP server that expose the full platform to your tools, scripts, and AI agents. Use REST anywhere HTTPS works, or connect an AI agent over MCP to build automations and integrations on top of ProhostAI's data.**


Until now, ProhostAI has been the system your team logs into. Starting today, it's also the system your tools, scripts, and AI agents can build on top of. We're launching` connect.prohost.ai` — one REST API and one MCP server that expose the full ProhostAI platform to developers.


## Two ways to build


` connect.prohost.ai` speaks two protocols over the same data:


- **REST API** — works anywhere HTTPS does. Use it from a backend job, a Zapier-style automation, a custom dashboard, or any existing integration.
- **MCP Server** — plugs directly into[Claude](https://claude.ai/download) ,[Cursor](https://cursor.com/) , and any other Model Context Protocol-aware client so an AI agent can read and act on your hosting data in the same authenticated session.


Same data, same auth, same audit log. You pick the shape that fits the job.


## What you can build


The API and MCP cover the core of your vacation rental business end-to-end:


- **Sync listings** — create, read, and update listings, photos, amenities, and pricing rules across every connected channel.
- **Read reservations** — pull reservations, guest details, check-in codes, and payment status with filters and webhooks for state changes.
- **Send guest messages** — message guests programmatically across Airbnb, VRBO, WhatsApp, email, and SMS through one unified endpoint.
- **Pull analytics** — query occupancy, revenue, ADR, RevPAR, and cleaning metrics, filtered and aggregated and ready to chart.
- **Create tasks** — spin up tasks and cleanings, assign teammates, and sync status back to your ops dashboard in real time.
- **Manage calendars** — block dates, edit availability rules, and push calendar updates to every connected channel at once.


If it's in ProhostAI, it's in the API.


## The MCP moment


The MCP piece is the part we're most excited about. Model Context Protocol is the emerging standard for giving AI agents first-class access to live business data, and the ProhostAI MCP server lets Claude Desktop, Cursor, VS Code, and any other MCP-aware client read and act on your PMS over a single authenticated connection.


The install is deliberately short: one` npx` command in your AI client of choice, and your agent has live access to listings, reservations, messages, and analytics — with the same OAuth-scoped permissions as your own account.


That turns "can you ask Claude to summarize last month's reservations?" from a copy-paste exercise into a single sentence.


## Built on the platform you already trust


Every API call and every MCP tool invocation flows through the same authorization, auditing, and rate-limiting layer as the rest of ProhostAI. OAuth 2.0 with scoped keys. Full audit log. Sub-100ms p50 latency. 99.99% uptime target.


You get the ergonomics of a modern API and the operational guarantees you already rely on for the rest of your hosting stack.


## Get started


[Explore the developers page](https://www.prohost.ai/developers) ·[Read the docs](https://connect.prohost.ai/docs) ·[Install the MCP server](https://connect.prohost.ai/docs/mcp) ·[Sign up for free](https://app.prohost.ai/signup)


If you build something cool on top of ProhostAI, we'd love to see it — drop it in our[Discord](https://discord.gg/7sUnGXJxWg) .
