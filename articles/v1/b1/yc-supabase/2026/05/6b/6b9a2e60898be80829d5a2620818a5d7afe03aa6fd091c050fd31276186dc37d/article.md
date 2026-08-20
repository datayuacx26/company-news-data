---
schema_version: "1.0.0"
document_id: "6b9a2e60898be80829d5a2620818a5d7afe03aa6fd091c050fd31276186dc37d"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-news-import-538a148a7a76"
canonical_url: "https://supabase.com/changelog/45702-developer-update-may-2026"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-24T02:46:08.732472+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:516959654dd228ea91557810210bfffe70cf68be41fd5fac5e9371ca4a777618"
---

# Developer Update - May 2026

Here's everything that happened with Supabase in the last month:


## Custom OAuth/OIDC providers for Supabase Auth#


Connect any OAuth2 or OpenID Connect identity provider to your Supabase project, including GitHub Enterprise, regional IdPs, and any standards-compliant provider, with PKCE enabled by default.


\[[Blog](https://supabase.com/blog/custom-oauth-oidc-providers) \]


## New tables in the public schema are no longer auto-exposed to the Data API#


Starting April 28, new Supabase projects can opt out of automatic Data API exposure for public schema tables. Explicit Postgres grants are now required to make a table reachable via PostgREST or GraphQL. This becomes the default for all new projects on May 30.


\[[GitHub Discussion](https://github.com/orgs/supabase/discussions/45329) \]


## Supabase is now ISO 27001 certified#


Supabase is certified to ISO/IEC 27001:2022, covering the information security management system across the entire platform.


\[[Blog](https://supabase.com/blog/supabase-is-now-iso-27001-certified) \]


## Stripe Sync Engine moves to Stripe#


The Stripe Sync Engine, originally built by Supabase, is now part of the Stripe GitHub org. It is open source and maintained by Stripe going forward.


\[[Blog](https://supabase.com/blog/stripe-sync-engine-transfer) \]


## Supabase brand survey#


Help shape the direction of Supabase. The brand survey takes a few minutes and closes soon.


\[[Take the survey](https://supabase-brand-survey-2026.lovable.app/) \]


## @supabase/server#


A new SDK that handles auth, client creation, CORS, and context injection across runtimes. Works on Edge Functions, Vercel Functions, Deno, Bun, and Cloudflare Workers.


\[[Blog](https://supabase.com/blog/introducing-supabase-server) \] \[[Docs](https://supabase.github.io/server/) \]


## Quick Product Announcements#


- The Supabase app in the Stripe Marketplace is now generally available. \[[Stripe Marketplace](https://marketplace.stripe.com/apps/supabase) \]
- Branching without Git is now the default. Create branches directly from the dashboard without a GitHub integration. \[[Blog](https://supabase.com/blog/branching-without-git-is-now-the-default) \]
- Data API settings revamped: new per-table and per-function toggles let you control which tables are exposed to PostgREST and GraphQL, with a default-privileges switch at project creation. \[[Docs](https://supabase.com/docs/guides/database/data-api) \]
- The Supabase changelog now has RSS feeds, tag filtering, and a` .md` feed, plus links to copy any entry as Markdown or ask Claude/ChatGPT. \[[Changelog](https://supabase.com/changelog) \]
- Wrappers v0.6.0 ships with a new OpenAPI FDW, Snowflake timeout support, Clerk CRUD, and several bug fixes. \[[GitHub](https://github.com/supabase/wrappers/releases/tag/v0.6.0) \] \[[Docs](https://fdw.dev/) \]
- Supabase Agent Skills: an open-source set of instructions that teach AI coding agents how to build on Supabase correctly. \[[Blog](https://supabase.com/blog/supabase-agent-skills) \]
- Terraform Provider v1.9.0 adds Edge Functions resource, Edge Function secrets resource, and a network bans data source. \[[Docs](https://registry.terraform.io/providers/supabase/supabase/latest/docs) \]


## Made with Supabase#


- Replist: Track your repertoire, sharpen your practice, and connect with the musicians you play with. \[[Website](https://www.replistapp.com/) \]
- Grepture: Trace, evaluate, and protect every LLM call. Drop-in SDK. 80+ detection rules. \[[Website](https://grepture.com/) \]
- Causo: Agents that connect you with best fit partners at VCs. \[[Website](https://causo.ai/) \]
- Screenfully: An app demo creator on your phone. No need to connect to a Mac. \[[Website](https://screenfully.app/) \]
- Anamap: Cartos by Anamap is an AI agent that investigates your dashboards, site behavior, and code releases to find the root cause of metric drops in plain English. \[[Website](https://anamaps.com/) \]
- Crewform: Build your AI team with Crewform. Orchestrate specialized, autonomous agents to collaborate on complex tasks and connect outputs to your stack. \[[GitHub](https://github.com/CrewForm/crewform) \]


## Community Highlights#


- The Supabase GitHub repo hit 100K stars and 8 million developers. \[[Blog](https://supabase.com/blog/100000-github-stars) \]
- Introducing the OSSCAR: An index of the fastest-growing open-source orgs. \[[Blog](https://supabase.com/blog/introducing-osscar-index) \]
- The State of Startups 2026 survey is still open. \[[Survey](http://www.supabase.com/state-of-startups) \]
- How I Scaled My NextJS + Supabase App to 25,000 Users \[[YouTube](https://www.youtube.com/watch?v=k-ETON-f7Sw) \]
- Stop Building Custom Auth — Auth0 vs Supabase: One Saved Us 3 Months of Engineering Work \[[Blog](https://medium.com/@ArkProtocol1/stop-building-custom-auth-auth0-vs-supabase-one-saved-us-3-months-of-engineering-work-756c4ecf3722) \]


---


*This discussion was created from the release[Developer Update - May 2026](https://github.com/supabase/supabase/releases/tag/v1.26.05) .*
