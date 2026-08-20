---
schema_version: "1.0.0"
document_id: "0b53035ed923f18503eabe6f2741a23a2f77e46a02f64ed065345f86a60c4d61"
company_key: "yc-nango"
company: "Nango"
source_id: "yc-nango-news-import-e2ee807d6673"
canonical_url: "https://nango.dev/blog/merge-dev-vs-nango/"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T05:12:58.696716+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:ee62e83395acd23a92020c2822ee6ae270020d6f6193bca6196ac178ab0cb462"
---

# Merge.dev vs Nango: Which API integration platform should you choose in 2026?

## TL;DR


Merge.dev ships two products: a unified API across 9 categories (HRIS, ATS, CRM, accounting, and more) and Merge Agent Handler, a pre-built MCP tool catalog for AI agents. Both stay inside Merge’s fixed schema and pre-built catalog. Custom fields and custom objects are limited. Custom AI tool calls cannot be deployed to the runtime. Neither product supports AI coding agents to build new integrations.


Nango is an[agentic API integrations platform](https://nango.dev/blog/best-agentic-api-integrations-platform) where engineers, or coding agents like Claude Code, Cursor, and Codex, write integrations as code. Nango’s cloud runtime runs them securely and at scale. Integrations cover API auth, tool calls, data syncs, webhooks, and unified APIs on a single runtime.


**Quick verdict:**


- **Pick Nango** if integrations are core to your product, or if your AI agents need tool calls you cannot find in a pre-built catalog. Coding agents build and iterate on the integrations on a runtime that covers auth, syncs, webhooks, and MCP.
- **Pick Merge.dev** if your entire integration roadmap fits inside its 9 categories with no custom fields or custom objects, and a small, predictable number of linked accounts.


## Merge.dev: a unified API and a separate MCP tool layer


Merge popularized the unified API pattern for B2B SaaS integrations. Their normalized common data models for HRIS, ATS, CRM, and accounting let many products ship integrations in days instead of weeks. The trade-off, which the rest of this post covers, is the one that comes with any pre-built abstraction: you build inside what the vendor ships.


Merge currently ships two products:


**Merge unified API.** A pre-built unified API across 9 categories: HRIS, ATS, CRM, accounting, ticketing, file storage, knowledge base, marketing automation, and chat. Around 220 integrations total. You integrate once with Merge’s` Contact` ,` Employee` ,` Invoice` , or` Ticket` models, and Merge syncs each provider’s native schema into that shape.


**Merge Agent Handler.** Launched October 2, 2025. A tool-calling layer for AI agents that exposes Merge’s connectors as MCP tools. It ships a pre-built tool catalog, supports public MCP servers, and lets you compose Tool Packs with narrow permissions. It is sold and priced separately from the unified API.


Both products share the same constraint: you build with what Merge ships. Customization, code-level access, and a coding-agent build loop are not part of the platform.


## Nango: build integrations with AI


[Nango](https://nango.dev/) helps you build integrations with AI, in code, for your product. Engineers or coding agents like Claude Code, Cursor, and Codex write integrations as code in your repo, and Nango’s cloud runs them securely and at scale.


Integrations are a three-part system on Nango:


1. **Integrations as code, written by your coding agent.** A universal AI builder[skill](https://nango.dev/docs/guides/functions/functions-guide#function-ai-builder) that works with any coding agent (Claude Code, Cursor, Codex, etc.). The coding agent writes the integration, tests it against a real connection, and iterates on real errors.
2. **A secure and scalable runtime.** Syncs and tool calls run on the Nango cloud with tenant isolation, so a noisy customer’s executions cannot starve another’s.
3. **A single interface for every integration pattern.**[API auth](https://nango.dev/docs/guides/auth/auth-guide) , tool calls,[data syncs](https://nango.dev/docs/guides/functions/syncs/sync-functions) ,[webhooks](https://nango.dev/docs/getting-started/use-cases/webhooks-from-external-apis) , polling triggers, and[custom unified APIs](https://nango.dev/blog/build-unified-api-for-product-integrations) .


Nango is[open source](https://github.com/NangoHQ/nango) , supports[900+ APIs](https://nango.dev/api-integrations) across 30 categories, and is used in production by[hundreds of fast-growing SaaS and AI companies](https://nango.dev/customers) .


## Six limits you hit with Merge.dev


Merge fits a narrow scope: a customer connects an HRIS or CRM, you read normalized fields, you move on. As soon as the integration surface widens, six specific limits start to show up.


### End users authorize Merge, not your app


White-label authentication is gated to the Enterprise plan. On lower tiers, your customers see Merge’s branding during the connect flow, and the OAuth grant is to Merge, not to your application. For enterprises, that introduces a third-party data processor into the security review.


### The universal schema is too narrow


Merge maps every CRM, HRIS, or accounting system to a common data model. The model only contains fields most providers expose. On the Contact entity, Salesforce and HubSpot share roughly five common fields. Anything beyond that, including custom objects, custom fields, and provider-specific endpoints, lives outside the unified API.


Custom fields are gated to the Professional plan. Custom objects only work for Salesforce, HubSpot, and Zendesk Sell. Passthrough Requests send you back to writing direct provider integrations, where you may hit provider rate limits.


### You cannot extend integrations


You cannot add a missing field, add a new API yourself, or change how an integration runs. In practice, this means:


- You cannot add support for APIs Merge does not already cover.
- You cannot access data that Merge’s universal schema does not expose.
- You cannot customize how an integration behaves for a specific customer.
- You cannot pull deeply custom data or custom schemas from the source system.


### Merge Agent Handler does not cover the full AI integration stack


Agent Handler is a separate product. It ships pre-built tools that AI agents can call over MCP. What it does not do:


- **No custom tool calls deployed to the platform:** You cannot author an intent-shaped tool in code and deploy it to Merge’s runtime. An example would be a single` onboard-customer` tool that creates a Slack channel, invites users, and posts a welcome message. You chain pre-built tools instead. That inflates context size and hurts agent reliability. See[how to build reliable tool calls for AI agents](https://nango.dev/blog/build-reliable-tool-calls-for-ai-agents-integrating-with-external-apis) .
- **No data syncs or webhooks:** Agent Handler is tool-call only. If your AI product also needs continuous syncs, webhook ingestion, or polling triggers, you need a second platform.
- **No coding-agent build loop:** Agent Handler does not ship a skill for Claude Code, Cursor, or Codex that lets a coding agent author and deploy a new tool to the runtime.
- **Sold and priced separately:** Using both Merge unified API and Merge Agent Handler means two contracts for similar use cases.


### Sync delays cap real-time use cases


On Merge’s Launch plan, syncs run daily by default. Customizable sync frequencies are a Professional/Enterprise feature, and the actual cadence varies by integration.


Real-time use cases, such as an AI agent reading the freshest CRM record or an alert pipeline that fires on a closed deal, cannot be fully supported with Merge.


### Key production features sit behind the Enterprise plan


Several capabilities most teams need in production are Enterprise-only:


- **Scopes management:** Control which external API permissions your integrations request.
- **Enhanced deletion detection:** Track when records are deleted in external systems.
- **Observability APIs:** Show customers when data was last synced.
- **Longer log retention:** 90+ days on Enterprise versus 30 days on Professional.


For small teams, including founders building their first integrations, this can mean an Enterprise contract is needed just to ship the integration the product requires.


## What engineers like about Nango


Nango covers both the Merge unified API and the Merge Agent Handler use cases on a single stack. Code-level access and an AI coding agent build loop come built in.


### Coding agents build the integrations


Install the Nango[Function Builder skill](https://nango.dev/docs/guides/functions/functions-guide#function-ai-builder) once. Your coding agent then reads API docs, writes the integration, and tests it against a real connection. It iterates on real errors and deploys.


The skill works the same way for any of the 900+ APIs Nango supports. See[using AI coding agents for building API integrations](https://nango.dev/blog/using-ai-coding-agents-for-building-api-integrations) for the full workflow, and our writeup on how we[built 200+ integrations in 15 minutes](https://nango.dev/blog/learned-building-200-api-integrations-with-opencode) using the same loop.


### Build custom unified APIs in your data model, not Merge’s


If you still want the single-interface benefit of a unified API, you can build it with Nango and a coding agent. Define your own universal data model with the fields and entities your product actually needs. For example, an` Employee` model with` id` ,` firstName` ,` email` ,` jobTitle` ,` managerEmail` , and` customFields` across BambooHR, HiBob, and Personio.


Your coding agent writes one sync per provider that maps each provider’s native fields into your schema. You keep full control over transformations, custom fields, and provider-specific endpoints. The full guide is in[how to build a custom unified API for your product integrations](https://nango.dev/blog/build-unified-api-for-product-integrations) .


### Every integration use case, on one platform


Tool calls, durable[data syncs](https://nango.dev/docs/guides/functions/syncs/sync-functions) ,[webhook processing](https://nango.dev/docs/getting-started/use-cases/webhooks-from-external-apis) , polling triggers, and per-customer configuration all run on Nango. Merge needs two products to cover the same surface. Even then, Merge Agent Handler ships no webhook ingestion at all, and its tool calls are limited to the pre-built catalog Merge maintains.


Nango also ships a first-class[MCP server](https://nango.dev/docs/guides/functions/tool-calling) that exposes your custom tools to AI agents inside your product.


### White-label auth for 900+ APIs


A drop-in[Connect UI](https://nango.dev/docs/guides/auth/auth-guide) handles OAuth, API keys, basic auth, JWT, and the[MCP Auth](https://nango.dev/docs/updates/changelog#support-for-mcp-auth) standard. Your end users authorize against your brand, not Nango’s. White-label is the default, not an Enterprise add-on.


You can also run[headless auth](https://nango.dev/docs/guides/auth/customize-connect-ui#build-your-own-auth-ui) if you want to ship your own UI. Either path keeps the OAuth grant with your application, which is the model enterprise customers expect.


### Per-customer configuration


Custom field mappings, filters, and per-customer settings live in code and ship with the integration. See the[customer configuration guide](https://nango.dev/docs/getting-started/use-cases/customer-configuration) .


### Deep, real-time observability in every plan


Every operation generates[detailed logs](https://nango.dev/docs/guides/platform/observability) , exports via OpenTelemetry, and supports custom log messages. Not Enterprise-gated.


### Enterprise compliance


[SOC 2 Type II, GDPR, and HIPAA compliant](https://trust.nango.dev/) . Self-hosting is available on the Enterprise plan.


## Merge.dev vs Nango: feature comparison


Feature Nango Merge.dev unified API Merge Agent Handler


Supported APIs / connectors 900+ across 30 categories ~220 across 9 categories Pre-built tool catalog across Merge categories


Pre-built common data models out of the box No (build them with AI builder skill) Yes (fixed schema per category) N/A


AI coding agent support (Claude Code, Cursor, Codex) Yes (universal builder skill) No No


Custom tool calls for AI agents Yes No No


Data syncs and webhook processing Yes Syncs only, within unified-API categories No


Customization (custom fields, custom objects, provider endpoints) Full, in code Custom fields on Professional+, custom objects for 3 CRMs only N/A


Custom unified APIs in your schema Yes No (fixed Merge schema) N/A


White-label auth Yes (default), with headless option Enterprise-only N/A


Observability with full request/response logs Yes (all plans, OpenTelemetry export) Enterprise-only APIs Yes


Open source and self-hosting Yes No No


## When to choose Merge


If your integration use case is simple, well covered by one of Merge’s unified APIs, and you do not see your integrations growing beyond that, Merge can be a reasonable choice.


The shorthand is, you are okay with Merge’s limitations:


- Staying inside Merge’s 9 categories for the lifetime of your product.
- A fixed universal schema with no custom fields, custom objects, or provider-specific endpoints.
- End users authorizing Merge during the OAuth flow (unless you are on Enterprise).
- Daily syncs as the default cadence, with no real-time guarantees.
- Waiting on Merge’s roadmap for new APIs or new fields.
- An annual contract with linked-account commitments.


If any of those is a deal-breaker, Merge will be the limiting factor before you scale your customer base much further.


## When to choose Nango


Nango is the better fit if any of the following is true:


- **Integrations are a core product feature** , not a checkbox.
- **You want coding agents to build and iterate on integrations** in your repo, with dry runs against real APIs and real credentials.
- **You need custom unified APIs** with fields and entities your product actually uses.
- **Your AI agents need custom tool calls** that ship in your product, with strict schemas and per-user permissions.
- **You need integrations beyond Merge’s nine categories** , especially the long tail of vertical SaaS and developer APIs.
- **You want one platform for tool calls, syncs, webhooks, and unified APIs** , not two contracts on the same connector layer.


## How to migrate from Merge.dev to Nango


Most teams migrate when they hit a customization wall (custom fields, custom objects, provider-specific endpoints) or when an integration use case grows beyond what Merge supports.


The pattern that works in practice:


1. **Build any new integrations on Nango first.** Do not start new work on Merge. This caps the scope of what eventually needs to be migrated.
2. **Install the[Nango function builder skill](https://nango.dev/docs/guides/functions/functions-guide)** in your coding agent.
3. **Have your coding agent migrate existing integrations one by one.** The skill reads the API docs, drafts the code, tests it against a Nango test connection, and iterates on real responses. Auth, retries, pagination, and observability are handled by the runtime.
4. **Use Nango alongside Merge during the cutover.** Sync the same data through both, and switch your product to read from Nango after testing.
5. **Migrate customer connections.** Use Nango’s white-label[Connect UI](https://nango.dev/docs/guides/auth/auth-guide) to re-authorize customers under your brand.
6. **Decommission Merge.** Cancel the contract at the renewal boundary.


## FAQ


**What is the difference between Merge.dev’s unified API and Merge Agent Handler?**


Merge’s unified API is a pre-built unified API across HRIS, ATS, CRM, accounting, and other categories. Merge Agent Handler, launched October 2025, is a separate product that exposes pre-built tools to AI agents over MCP. Agent Handler does not include data syncs or webhook processing.


**Does Merge.dev support custom fields and custom objects?**


Custom fields are available on the Professional plan and above. Custom objects are supported only for Salesforce, HubSpot, and Zendesk Sell.


**Can a coding agent build new integrations on Merge.dev?**


No. Merge does not ship a skill for coding agents that authors and deploys a new integration. Nango is the only platform here that does.


**Can I self-host Merge.dev?**


No. Merge is closed source and SaaS-only. Nango is[open source](https://github.com/NangoHQ/nango) with self-hosting available, which matters for teams with data residency or compliance requirements that rule out a third-party hosted platform.


**Which platform is better suited for AI agent integrations?**


Nango. For an AI agent integration to work in production, you need tool calls, auth, webhooks, real-time triggers, customization, deep observability, and ideally a coding agent that builds new tools as you need them. Nango covers all of these on one runtime.


## Conclusion


Merge.dev works for teams whose integrations are simple, fit a fixed schema, and never need to be customized. Nango is the most flexible platform across the surface area. AI coding agents build new integrations across 900+ APIs. Durable syncs and webhook processing run on the same runtime. A built-in MCP server exposes custom tool calls. White-label auth ships by default. Nango is the only platform in this comparison that covers both the Merge unified API use case and the Merge Agent Handler use case on a single stack.


## Related reading


- [Best Merge.dev alternatives in 2026](https://nango.dev/blog/4-most-popular-merge-dev-alternatives)
- [Merge pricing: what you should know before you commit](https://nango.dev/blog/merge-pricing)
- [Best unified API platforms to consider in 2026](https://nango.dev/blog/best-unified-api)
- [Best agentic API integrations platform in 2026](https://nango.dev/blog/best-agentic-api-integrations-platform)
- [Best API integration platforms to use with Claude Code, Cursor, and Codex](https://nango.dev/blog/best-api-integration-platforms-claude-code-cursor-codex)
- [Why B2B SaaS teams outgrow pre-built unified APIs](https://nango.dev/blog/why-b2b-saas-outgrow-pre-built-unified-apis)
- [How to build a custom unified API for your product integrations](https://nango.dev/blog/build-unified-api-for-product-integrations)
- [How Nango is different from embedded iPaaS or unified APIs](https://nango.dev/blog/how-is-nango-different-from-embedded-ipaas-or-unified-api)
- [Just-in-time integrations](https://nango.dev/blog/just-in-time-integrations)
