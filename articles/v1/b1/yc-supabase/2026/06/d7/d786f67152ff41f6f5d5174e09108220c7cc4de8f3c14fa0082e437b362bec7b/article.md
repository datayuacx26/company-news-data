---
schema_version: "1.0.0"
document_id: "d786f67152ff41f6f5d5174e09108220c7cc4de8f3c14fa0082e437b362bec7b"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/46689-developer-update-june-2026"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:acafb1ef099097915411cec5036ec82a9cbdbe420656821f3f83c4878b45367c"
---

# Developer Update - June 2026

Here's everything that happened with Supabase in the last month:


## Supabase Series F#


Supabase has raised a $500M Series F at a $10B pre-money valuation. It’s led by GIC.


[Read the blog →](https://supabase.com/blog/supabase-series-f)


## Multigres 0.1 alpha released#


Multigres is a scalable operating system for Postgres: it holistically manages your Postgres instances and gives you sharding, connection pooling, automatic failover, and backup orchestration. Multigres v0.1 alpha is an open-source-only release. Multigres for Supabase is coming soon.


[Read the blog →](https://supabase.com/blog/multigres-v0-1-alpha)


## Passkey Sign-in for Supabase Auth#


Users can now sign in with biometrics (Face ID, Touch ID, Windows Hello), a device PIN, or a hardware security key. Built on the WebAuthn standard, passkeys are passwordless and phishing-resistant. Supabase Auth stores only the public key for verification, while private key material stays on the user's device. Available to all projects in beta today.


[Read the docs →](https://supabase.com/docs/guides/auth/passkeys)


## Supabase is now an official ChatGPT app#


Connect your Supabase projects to ChatGPT and manage your database infrastructure conversationally. The integration includes 29 tools covering SQL execution, schema changes, branching, edge function deployment, and live logs, all without leaving ChatGPT. Works on all Supabase plans with paid ChatGPT plans (Plus, Pro, Team, Enterprise).


[Read the blog →](https://supabase.com/blog/supabase-is-now-an-official-chatgpt-app)


## Supabase Plugin for AI Coding Agents#


A single install that gives your AI coding agent everything it needs to build on Supabase. The plugin bundles the Supabase MCP server and agent skills so your agent can query databases, manage migrations, deploy Edge Functions, and follow Supabase and Postgres best practices out of the box. Supports Claude Code, Cursor, Codex, and Gemini CLI.


[Read the docs →](https://supabase.com/docs/guides/ai-tools/plugins)


## Feature preview: Temporary token-based database access#


Grant developers direct database access using Personal Access Tokens with; no passwords required. Project admins assign a specific database role and expiry window (up to 90 days) per user. Revoking project access immediately cuts database access too. Fully supported in branch projects, and available on Postgres 17+.


[Join the GitHub discussion →](https://github.com/orgs/supabase/discussions/46346)


## Quick Product Announcements#


- Guide to securing your app against npm supply-chain attacks. \[[Docs](https://supabase.com/docs/guides/security/npm-security) \]
- Supabase client libraries now support traces following the W3C standard, compatible with any compliant tracing SDK including OpenTelemetry, Sentry, Datadog, and Honeycomb. \[[Docs](https://supabase.com/docs/guides/telemetry/client-side-tracing) \]
- The Schema Visualizer now supports editing tables directly on the page, making it easier to design your database while viewing the full picture. \[[Twitter](https://x.com/dshukertjr/status/2057808949282267275?s=20) \]
- Long text columns can now be expanded in the sidebar to view full content, with support for rendering as Markdown. \[[Twitter](https://x.com/dshukertjr/status/2056722037159362966?s=20) \]
- The RLS Tester lets you run queries as another user, see which RLS policy took effect, and test via client libraries with AI-assisted SQL conversion. Enable it from the feature previews section in the dashboard. \[[Twitter](https://x.com/dshukertjr/status/2056359648937333009?s=20) \]
- Navigation keyboard shortcuts are available in the dashboard. Press cmd+k and open "Show all keyboard shortcuts" to see the full list. \[[Twitter](https://x.com/supabase/status/2054933539645272225?s=20) \]
- pg-delta is a new schema-diffing engine built from scratch at Supabase to handle the full range of Postgres objects, including tables, columns, RLS policies, functions, triggers, indexes, and extensions. \[[Discussions](https://github.com/orgs/supabase/discussions/44938) \]
- Logs usage is now metered. Pro and Team plans include 5 GB ingest and 1,000 GB query per month, with overage at $0.50/GB and $0.002/GB, respectively. New organizations may move to the new pricing as early as June; existing organizations begin migrating July 1. \[[Docs](https://supabase.com/docs/guides/platform/manage-your-usage/logs) \]
- Supabase is now available as a connector on Perplexity Computer. With Supabase as the persistent data layer, Computer can read from and write back to your Postgres tables, keeping state across runs without custom glue code. \[Perplexity blog\]


## Meet the Supabase team#


- **Webinar:** From scattered tools to workflows that scale: Supabase + Perplexity Computer for Small Businesses. June 25 at 10:00 am PT. \[Register\]
- **Webinar:** From shipped app to production-ready insight with Supabase + Hex Agent. July 8 at 9:00am PT. \[[Register](https://events.hex.tech/hex-agent-supabase-webinar/) \]
- Hangout with the Supabase team during Casual Wednesdays on Discord at 10:00 am PT. \[[Join](https://discord.supabase.com/) \]
- DASH by Datadog, June 9-10, New York \[[Register](https://dash.datadoghq.com/) \]
- Figma Config 2026, June 23-25, San Francisco \[[Register](https://config.figma.com/san-francisco/register/) \]
- Vercel Ship 26, June 30, New York \[[Register](https://vercel.com/ship/nyc) \]


## Made with Supabase#


Join our Discord, showcase your app, and maybe you’ll get featured.


- Gather is a simple way to stay connected with your family, friends, and activity groups. \[[Download](https://zuccigather.com/welcome) \]
- 021 helps you make better product decisions with clarity, context, and conviction. \[[Sign up](https://from021.io/) \]
- Nover is the unified visual generation suite that gives designers pixel-perfect precision over every render \[[Sign up](https://www.nover.studio/) \]


## Community Highlights#


- supabase-py has crossed 1 million downloads per day on PyPI. \[[Twitter](https://x.com/kiwicopple/status/2057374599793311821?s=20) \]
- A developer replaced two years of custom Okta token exchange logic with Supabase's custom OAuth/OIDC provider in 40 minutes. \[[Reddit](https://www.reddit.com/r/Supabase/comments/1tdulav/custom_oauthoidc_is_finally_ga_replaced_our/) \]
- The Multigres team used TLA+ to find a silent data-loss bug in pg_rewind that can leave a standby carrying phantom writes after two rapid failovers. The proposed fix embeds a UUIDv7 in each timeline history entry to distinguish independent promotions. \[[Blog](https://multigres.com/blog/2026/05/04/tla-pg-rewind) \]
