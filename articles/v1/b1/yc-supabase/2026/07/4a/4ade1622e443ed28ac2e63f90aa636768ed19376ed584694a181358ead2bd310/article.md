---
schema_version: "1.0.0"
document_id: "4ade1622e443ed28ac2e63f90aa636768ed19376ed584694a181358ead2bd310"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/47796-developer-update-july-2026"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:664bf4c2d4bd847972ce0af49634763d37f42e4befa94d09cc0d0c2dd263305d"
---

# Developer Update - July 2026

Here's everything that happened with Supabase in the last month:


## OpenCode integrates with Supabase#


OpenCode connects your agent to your Supabase database, Edge Functions, and logs. It configures the MCP setup for you.


[Read the blog →](https://supabase.com/blog/agentic-coding-on-supabase-with-opencode)


## TanStack DB syncs with Supabase#


` @supabase-labs/tanstack-db` syncs TanStack DB collections with your Supabase tables over PostgREST and Realtime. It's available in alpha.


[Watch the demo →](https://x.com/supabase/status/2069429278253498562)


## Wrappers adds a MongoDB foreign data wrapper#


Wrappers v0.6.2 lets you query and join MongoDB collections directly from Postgres. It also fixes OpenAPI FDW pagination.


[Read the docs →](https://supabase.com/docs/guides/database/extensions/wrappers/mongodb)


## Multigres supports LISTEN/NOTIFY across pooled connections#


Multigres keeps Postgres LISTEN/NOTIFY working even when connections are pooled away from clients.


[Read the blog →](https://multigres.com/blog/2026-05-24-listen-notify)


## Realtime Broadcast supports binary payloads#


Realtime Broadcast now sends and receives binary payloads in addition to JSON. Binary payloads cut encoding overhead for cases like sensor telemetry and live screenshot streaming.


The Dart, Kotlin, and Python clients don't support binary payloads yet, and older SDK versions silently drop them. Update your client before you rely on it.


[Read the docs →](https://supabase.com/docs/guides/realtime/broadcast)


## Quick Product Announcements#


- Postgres` log_connections` now defaults to off for new projects on all tiers as of July 9, and existing Free and Pro projects are being migrated to the new default. \[[GitHub Discussion](https://github.com/orgs/supabase/discussions/47197) \]
- ` pg_graphql` v1.6.2 ships with GraphQL schema introspection off by default, so enable it per schema if you use GraphiQL or codegen tools. \[[GitHub Discussion](https://github.com/orgs/supabase/discussions/46320) \]
- Audit Log Drains are available, so you can stream your project's audit logs to an external destination. \[[Docs](https://supabase.com/docs/guides/security/platform-audit-logs#accessing-audit-log-drains) \]
- Connect copies every environment variable` @supabase/server` needs in a single click. \[[Demo](https://x.com/softwarecuddler/status/2067649901811609655) \]
- Self-hosted Docker defaults changed:` API_EXTERNAL_URL` now includes the` /auth/v1` prefix, and the default image moves to Postgres 17. \[[GitHub Discussion](https://github.com/orgs/supabase/discussions/47093) \]


## Meet the Supabase team#


- **Supabase Live:** Building high quality Supabase apps using TRAE. July 22 at 7 pm PT. \[[Register](https://supabase.com/events/supabase-trae-high-quality-apps) \]
- Supabase x Claude Community Meetup in Dublin. \[[Register](https://luma.com/dublin-8-2026-07-meetup) \]
- Hangout with the Supabase team during Casual Wednesdays on Discord at 10:00 am PT. \[[Join](https://discord.supabase.com/) \]


## Made with Supabase#


- Shapeships: A multiplayer browser game using simultaneous-turn mechanics, built on an authoritative Supabase backend. \[[Website](https://shapeships.juddmadden.com/) \]
- Blind OS: An autonomous outreach and client-management system that finds leads, qualifies them, and follows up on payments. \[[Website](https://blindos.co/) \]
- rlsautotest: Generates pgTAP tests and seed data from your Supabase RLS policies to prove, per table and identity, who can read or write which rows. \[[GitHub](https://github.com/unitautogen/rlsautotest) \] \[[PyPI](https://pypi.org/project/rlsautotest/) \]
- Heym: An open-source visual AI workflow automation platform with a native Supabase node for querying and mutating tables through PostgREST. \[[GitHub](https://github.com/heymrun/heym) \]


## Community Highlights#


- A practical security checklist for non-technical builders, with Row Level Security flagged as the single most important fix. \[[Read](https://javieroch.substack.com/p/the-vibe-coders-survival-guide-what) \]
- A transparent cost breakdown for a personal life-management system built with Claude Code and Supabase, plus an honest look at the security tradeoffs of self-hosting your own data. \[[Read](https://jeradhill.substack.com/p/what-it-actually-costs-to-run-my) \]
- A widely-shared guide to vibecoding with Claude Code positions Supabase as the default data layer and login system in its standard setup flow. \[[Read](https://ruben.substack.com/p/the-claude-code-bible) \]


---


*This discussion was created from the release[Developer Update - July 2026](https://github.com/supabase/supabase/releases/tag/v1.26.07) .*
