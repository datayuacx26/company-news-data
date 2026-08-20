---
schema_version: "1.0.0"
document_id: "573d1f29df6213d1aeeb6519e3afcb1f0ce42d2d5bcc5e734ff377da6b78cce8"
company_key: "yc-wasp"
company: "Wasp"
source_id: "yc-wasp-rss-5b1984e54864"
canonical_url: "https://wasp.sh/blog/2026/07/20/made-with-wasp-searchcraft"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-24T15:05:56.365070+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:c99332de549112b56e400423b794fefece79b8d71371b5b0a105d626d3976bcd"
---

# Made with Wasp: how Searchcraft built a full-stack SaaS around their Rust search engine

## TL;DR​


- **[Searchcraft](https://searchcraft.io/)** is a search engine written in Rust: an Algolia / Elasticsearch alternative that's light enough to run full-text search over all of English Wikipedia[on a single Raspberry Pi](https://www.youtube.com/watch?v=SjuPn6_yl2s) .
- Their full-stack customer dashboard (auth, billing, analytics, index management) is built with **Wasp** and the[Open SaaS](https://opensaas.sh/) template.
- They could have built the SaaS layer themselves, but chose not to, so their engineering time stays in the Rust engine where they actually compete.
- Wasp compiles to standard React + Node.js + Prisma, deployed on their own AWS and CI/CD. No lock-in, no restructuring.


## What is Searchcraft?​


[Searchcraft](https://searchcraft.io/) is a search engine written in Rust, built for product search, site search, and content discovery, competing with Elasticsearch, Algolia, OpenSearch, and Solr on a simple bet: **search should be fast to integrate, light on resources, and easy to run.**


How light on resources? Their CTO, Don, runs full-text search over the *entire English Wikipedia* on a single Raspberry Pi. That's the engine's whole personality in one image: it doesn't need a cluster to be useful.


The team is small and bootstrapped: a handful of engineers who spent years running a dev agency before going all-in on their own product. That history matters here, because they shipped client work in Next.js, Nuxt, and Angular, whatever stack the client brought.


And they're in production. Their first customer is a publisher running 22 local news sites with around 4 million active readers, rolling out site by site, with e-commerce and B2B SaaS next in line. Searchcraft ships as a managed cloud service or self-hosted, with SDKs for JavaScript, TypeScript, and PHP.


## Every engine needs a chassis - so they chose Wasp​


The onboarding screen - Searchcraft's design is super cool!


Searchcraft's plan is developer-led growth: a developer who finds it at 2am has to get from curiosity to a working search box on their own. That means sign-up, orgs, roles, indexes, API keys, analytics, billing. **A full, production-ready SaaS application (auth, database, background jobs, emails, payments, admin) wrapped around the thing you actually sell.**


They could have built it, but chose not to. The SaaS layer is a hundred solved problems that still take months to solve again and then ask for maintenance forever, and every hour on auth flows and CRUD plumbing is an hour not spent on relevance ranking, ingestion speed, or memory footprint, the things Searchcraft actually competes on.


The answer came from inside the house: Don, the CTO, had been following Wasp.[Open SaaS](https://opensaas.sh/) , Wasp's open-source template, meant auth, payments, and an admin panel existed on day one, as code they own, not services they rent. And they evaluated it like engineers: everything Wasp generates is standard React, Node.js, and Prisma they could read, trust, and run themselves, no magic to open up. **It's their code** , and it compiles to one integrated stack built to keep holding together as the app grows.


## What they built: the full-stack customer dashboard​


The dashboard is Searchcraft's control room, "a product within a product," where customers run everything from document ingestion to search tuning, team management, and analytics.


Three moments show what's actually running under the UI, because the dashboard isn't a static admin panel, it's a full-stack app where Wasp is doing real work end to end:


What's behind this screen: server-side queries turning raw search events into live stats and multi-tenant access control, all in one Wasp codebase.


What's behind this screen: a multi-tenant index registry in Prisma, kept in sync with the Rust engine over one API, with search events rolled up into the time windows above. The chart is React; the plumbing is Wasp's type-safe operations.


What's behind this screen: subscription plans and upgrades, the billing machinery every SaaS needs and nobody wants to hand-roll, running in the same Wasp app and wired into the quotas from the first screen.


Around those three screens sits the rest of a real SaaS: an ingestion pipeline that infers the schema as it pulls documents in, crew management with roles, access keys as managed secrets, and a code-snippets page that takes a frontend dev from API key to working search box in minutes. Multi-tenant, role-based, revenue-carrying, and it looks the part. For a search company, the user portal *is* the brand.


## How it runs: Wasp next to Rust​


The shape is the interesting part. On one side, the Searchcraft engine and its supporting services (provisioning, ingestion), all Rust, all theirs. On the other, the customer dashboard: a Wasp app (React + Node.js + Prisma) that talks to the engine over an API. Customer apps consume search through the SDKs and never know the portal exists.


Here's the architecture, drawn by the Searchcraft team:


The dashboard, a Wasp app, handles users, configuration, and analytics; the Searchcraft engine and its Rust services do the search. One API connects them, and customer apps reach search through the SDKs.


**Two systems, one product, one API between them.**


Wasp didn't ask them to restructure anything; it slotted in next to the architecture they already had. If your real product lives in Rust, Go, Python, or Java, this is the part that applies to you.


**Deployment, their way.** Wasp comes with one-line deployment commands for popular hosts, but teams keep full freedom over where the app runs. Searchcraft already had infrastructure on AWS, so that's where it went: their own CI/CD on Bitbucket Pipelines, client and server deploying separately onto the setup they already trust. Because a Wasp app compiles to portable artifacts, both doors stay open, and having that choice was part of the point.


## What's next​


Searchcraft is expanding the platform: Searchcraft Cloud, a self-hosted Core for teams running their own infrastructure, more SDKs, and AI-powered Search Result Summaries, with more coming for LLM-native search:


> **"We are building in MCP support so that you can connect it to an LLM if you want."**


Search that agents can talk to, from an engine small enough to run anywhere: it's a good bet, and they get to make it because their engineering time goes into the engine, not the app around it. That was the whole trade: the hard problems stay in Rust, where their edge is, and the SaaS layer lives in a Wasp app they fully own.


If you're in the same spot (a real product in a custom backend, and you need a full-stack dashboard around it), that's the conversation[Wasp](https://wasp.sh/) wants to have with you.


---


*Thanks to the Searchcraft team for the walkthroughs, the diagram, and the stories. Check out[Searchcraft](https://searchcraft.io/) !*
