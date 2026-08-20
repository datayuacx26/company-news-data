---
schema_version: "1.0.0"
document_id: "b49b984b9338017df87495844ab0bfc2dde5e090ccf1921ca6dff73e61499392"
company_key: "yc-metorial"
company: "Metorial"
source_id: "yc-metorial-news-import-9c457a3d81d4"
canonical_url: "https://metorial.com/blog/mcp-role-based-access"
published_at: "2026-08-19T09:00:00+00:00"
first_seen_at: "2026-08-19T18:53:41.634509+00:00"
fetched_at: "2026-08-19T18:53:42.631478+00:00"
content_hash: "sha256:b1c14ce3f846e8d59e0acf2e059517d05caebbd644c2e9da847b8f76158c3858"
---

# One MCP URL for Your Org: Role-Based Access via SSO

TL;DR


Every AI client an employee might use (Claude, Cursor, Codex, etc.) needs its own tool connections and its own access rules, unless there's one shared entry point underneath all of them. Metorial's Magic MCP is a single URL that you can paste into any MCP-compatible client. You sign in with the company's existing SSO and an agent reaches only the tools and providers that person is allowed to use, with nothing to install and no separate setup per tool. Access is set once, per user and group, and it applies the same way across every client, with every call logged and tied to a real identity rather than a shared key. Most AI agent deployments today aren't run this way yet. A 2026 industry survey of more than 900 executives and technical practitioners found only 14.4% of organizations have full security approval for their entire AI agent fleet, and fewer than a quarter treat agents as independent, identity-bearing accounts creating real security threat for the whole company.


## Why does "which AI client does IT support" become the wrong question?


Ask an engineering team which AI client to standardize on and different rooms give different answers. Claude, Cursor, and Codex are all reasonable choices for different work, and forcing everyone onto one of them is usually where a rollout stalls. The actual problem underneath that question isn't the client. Instead, it's the fact that each client is left to configure its own connections, ends up with its own credentials, its own access rules, and its own blind spot for security. Fix the layer underneath the client instead, and the client someone prefers stops being a decision that matters.


## What is Metorial Magic MCP?


Magic MCP is our single MCP (Model Context Protocol) URL. Paste it into any MCP-compatible client, sign in once with the login a company already uses, and an agent reaches every application and tool that person is allowed to use. Metorial offers more than 1,000 verified integrations plus any custom or internal MCP servers a company has added. There's nothing to install beyond pasting the URL, and no separate setup for each tool behind it. Every call the agent makes runs on the real identity of the person who connected it, through the[Metorial Gateway](https://metorial.com/access-control) , and gets logged.


## How does role-based access work when the entry point is one URL for everyone?


Access starts with sign-in, not with the URL itself. A person connects through their company's existing SSO or SAML identity provider, the same login they already use for email, and Metorial imports the groups that identity provider already has. From there, an administrator sets per-user and per-group policies once: which tools, which providers, which actions. Because Magic MCP is the one path every client uses to reach a tool, setting that policy once makes it apply everywhere at once, not once per client.


## Does access follow the person if they switch from Claude to Cursor to Codex?


Yes, because the policy lives on the identity, not on the client. Magic MCP is standards-based: any MCP-compatible client works the same way, paste the URL, sign in with OAuth, done. A person approved to use a CRM integration through Claude has the exact same access if they open Cursor or Codex instead, because the access decision was never made by the client in the first place. That's also what makes a shared skill reliable: a skill that works fine in one client and behaves differently in another usually means the access underneath it wasn't uniform. At Metorial we solve that for every team member, not only your engineering team.


## What happens to someone's access the day they leave, or the day their role changes?


Access follows the account in the identity provider, so it changes the moment that account changes there, not on a separate schedule someone has to remember to run. Offboarding a person in the company's identity provider removes their agent access at the same time it removes everything else. There's no separate list of AI tool grants to clean up by hand, because there was never a separate list to begin with.


## Can an agent reach more than the person behind it is allowed to reach?


No. Every agent gets its own identity, and every action ties back to that agent and the person it's acting for. An agent can be restricted further than the person behind it, limited to specific tools, providers, or actions on a per-agent basis, but it can never be granted more access than the person already has. Widening what one agent can do never quietly widens what the person behind it can do, and the reverse holds too.


## What does a security team get to see once this is in place?


Every call through Magic MCP, and every interaction inside Metorial's[Portals](https://metorial.com/portals) , is recorded: which agent made the call, on whose behalf, which tool it reached, and how the request was handled. That record splits into two related but distinct views. Session logs carry the full technical detail of an individual connection, every tool call, message, and execution in order, useful for debugging a specific issue. Audit logs are the accountability record across every actor, people, agents, and service accounts alike, filterable by user, agent, or integration, exportable for an auditor, with retention configurable to whatever a company's compliance rules require. Neither view depends on someone remembering to turn logging on for a specific tool. It's the same for all of them, because it all runs through the same gateway.


## Where do most AI agent deployments stand on this right now?


Most companies rolling out AI agents this year are further along on adoption than on governance. Gravitee's 2026 State of AI Agent Security report, based on a survey of more than 900 executives and technical practitioners, found 80.9% of technical teams have moved past planning into active testing or production, but only 14.4% report their entire AI agent fleet running with full security and IT approval. The same report found just 21.9% of teams treat agents as independent, identity-bearing accounts; the rest lean on shared API keys or custom, hardcoded authorization logic, and only 47.1% of an organization's AI agents are actively monitored at all. On top of that, 88% of organizations in the same survey had already had a confirmed or suspected AI agent security incident in the past year.


That gap is exactly the one a single governed entry point closes: an agent identity for every agent instead of a shared key, one access policy instead of one per client, and a log of every call instead of coverage for less than half of them.


## What does setting this up look like?


Connecting an existing identity provider is the first step, and it's the one everything else depends on. From there, importing groups, setting per-group policy, and generating each person's Magic MCP URL is largely one-time setup rather than ongoing maintenance, because new integrations get added to the shared catalog centrally instead of wired up client by client. The heaviest lift is usually deciding what the access policy should actually say, not configuring the mechanism that enforces it.


## FAQ


**Which AI clients and agents work with Magic MCP?**


Any MCP-compatible client. It's standards-based, so there's no custom client to build, just a URL to paste and an OAuth sign-in.


**Do people need API keys or tokens to use it?**


No. Magic MCP is tokenless. People sign in with the SSO identity they already have, so there are no keys to create, copy, or leak.


**Does this work with our existing identity provider?**


Yes. Magic MCP connects to existing SSO and SAML identity providers and imports groups automatically, so access maps onto accounts a company already manages.


**Is every action through Magic MCP logged?**


Yes. Every call is recorded, tied to the person behind it, and available through session logs for technical detail and audit logs for accountability and compliance export.


**Can we expose our own internal tools through the same URL, not just the verified integrations?**


Yes. Custom and remote MCP servers for internal systems reach the same Magic MCP URL as every verified integration.


**How is this different from a basic MCP server someone stands up themselves?**


A single self-hosted MCP server usually bundles a handful of tools with no organization-wide identity or logging behind it. Magic MCP reaches 1,000+ integrations plus a company's own servers, runs every call through one governed gateway on real identity, and records it.


## Sources


1. [Gravitee, "State of AI Agent Security 2026 Report: When Adoption Outpaces Control"](https://www.gravitee.io/blog/state-of-ai-agent-security-2026-report-when-adoption-outpaces-control)
2. [Metorial Magic MCP](https://metorial.com/magic-mcp)
3. [Metorial Access Control](https://metorial.com/access-control)
4. [Metorial Tracing](https://metorial.com/tracing)
5. [Metorial Portals](https://metorial.com/portals)
