---
schema_version: "1.0.0"
document_id: "1e7eec629489b2e94470c304418ae8647b0aa710bd817242ecb92a4afb3057df"
company_key: "yc-nango"
company: "Nango"
source_id: "yc-nango-news-import-e2ee807d6673"
canonical_url: "https://nango.dev/blog/what-engineering-teams-look-for-in-api-integration-platforms/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T06:37:14.661998+00:00"
fetched_at: "2026-08-15T06:37:17.055777+00:00"
content_hash: "sha256:619212c10d279270808a0409b7c726f0b6335cc8c0ac5f0a77c4b670aa0d6675"
---

# What engineering teams look for in API integration platforms

Between January and August 2026, we talked to more than 300 engineering teams evaluating integration platforms for agentic and SaaS use cases. These were real buyers comparing Nango and other vendors for their customer-facing integrations.


Most had already operated API integrations in production, and came with evaluation criteria centered on control, reliability, and security, learned from their own use cases.


This post covers what teams look for in an API integration platform. The criteria apply to any platform in the category (agentic integrations, embedded iPaaS, unified APIs, SaaS integration platforms) and to infrastructure you build in-house.


## The criteria at a glance


Criterion The question teams asked


Control over integration logic Can we change an integration without waiting on your roadmap?


Catalog depth What does "supported" cover for the APIs on our roadmap?


Complete auth handling How do you detect revoked credentials, and what does re-auth look like?


Reliability at scale What happens when a webhook is dropped or a backfill sync hits a limit?


Debuggable observability Can we see every request for one customer's connection?


Deployment and data residency Can this run in the EU, or inside our own cloud account?


Security posture What does an attacker get if you are breached?


Projectable pricing What does our usage shape cost at 10x?


AI agent readiness Can agents call the integrations, and can coding agents build them?


Exit path Could we leave without rewriting everything?


## The ten criteria engineering teams care about


### 1. Control over the integration logic


The most common question across customer calls: what happens when the pre-built integration does not do what we need?


Teams that had used pre-built unified APIs (Merge, Apideck, Finch) hit this wall on custom fields. Normalized data models cover less than you would expect.[Salesforce and HubSpot contacts share only about five fields out of the box](https://nango.dev/blog/four-ways-to-build-in-app-integrations) ; the data your product actually needs (deal conventions, custom objects, regional fields) lives outside the shared schema.


What teams checked:


- **Code access:** Can you read and edit the integration logic itself, or only configure it?
- **Roadmap independence:** If a field or endpoint is missing, can your team add it the same week, or do you file a feature request and wait?
- **The cost of workarounds:** Raw API access matters, but if it is just a bare passthrough request, you own auth, retries, and rate limits again for that call.[Why B2B SaaS teams outgrow pre-built unified APIs](https://nango.dev/blog/why-b2b-saas-outgrow-pre-built-unified-apis) covers this failure mode in detail.


Teams that overlooked this tended to come back 12 to 18 months later asking about migration paths. Migrations were the expensive failure mode in almost every story we heard.


### 2. Catalog depth


Few teams asked how many APIs a platform supports in total. Most asked what “supported” means for each API on their roadmap.


“Supported” can mean a full sync with pagination, rate-limit handling, and tests. It can also mean a logo and an auth flow. The gap between the two usually becomes visible only after you have committed.


What teams checked:


- **Per-provider depth:** Which endpoints, objects, and auth modes does the integration actually cover?
- **Long-tail turnaround:** For an API the platform does not support yet, is the answer days, weeks, or “on the roadmap”?
- **Self-serve extension:** Can your team add a new provider without the vendor’s involvement?


### 3. Auth that is handled completely


OAuth is nominally a standard.[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749#section-1.8) itself warns that it is “likely to produce a wide range of non-interoperable implementations”, and that warning holds in practice: provider-specific parameters, token rotation policies, and unannounced breaking changes. Operating auth for 900+ APIs, we found that[16% of commits to our provider configurations are fixes](https://nango.dev/blog/lessons-from-operating-api-auth-for-900-apis) for changes providers never announced.


What teams checked:


- **Token lifecycle:** How are refresh race conditions and token rotation handled? With a rotation-enforcing provider, a small bug destroys connections permanently.
- **Revocation detection:** How does the platform know credentials stopped working before your customer tells you?
- **Re-auth experience:** When a user has to reconnect, what do they see, and whose brand is on it?
- **Beyond OAuth:** HR, finance, and ERP APIs often use API keys, client credentials, or custom schemes. Check that the platform treats these as first-class.
- **Provider gatekeeping:** Google’s restricted scopes require a[security assessment with annual reverification](https://developers.google.com/identity/protocols/oauth2/production-readiness/restricted-scope-verification) . Ask who owns that process, you or the platform, and what happens while verification is pending.


### 4. Reliability at scale


An important part of reliability is not missing updates when data changes in third-party systems, which in practice means webhooks. Many providers tell you not to rely on webhooks alone:[Stripe does not guarantee event ordering](https://docs.stripe.com/webhooks) , and it retries failed deliveries for up to three days, and[Shopify’s docs state that webhook delivery “isn’t always guaranteed”](https://shopify.dev/docs/apps/build/webhooks) and recommend reconciliation jobs. Teams that had built replication on webhooks alone described silent data gaps that surfaced as customer incidents.


What teams checked:


- **Gap recovery:** How does the platform detect and reconcile missed or out-of-order webhooks?
- **Backfill limits:** What are the execution time and memory limits, and what happens to a sync that exceeds them?
- **Rate limits:** Are provider limits handled per connection, and what happens when one customer’s burst consumes the quota?
- **Tenant fairness:** Does one large customer’s sync delay every other customer’s?


### 5. Observability you can debug with


When an integration breaks, it usually breaks for one customer, on one connection, because of one payload. Teams wanted request-level logs per connection: what exactly was sent, what came back, and when.


Teams that had used platforms without that visibility described the same loop: file a support ticket to debug your own product, wait, apologize to your customer, repeat. Several ranked the quality of error messages above the size of the catalog.


What teams checked:


- **Request-level logs:** Every API call, per connection, with searchable payloads and sensitive fields redacted by default. A[Management MCP server](https://nango.dev/docs/reference/backend/management-mcp) helps here, so coding agents (Claude Code, Codex, Cursor) can debug failures directly from the logs.
- **Error quality:** Does a failure tell you which record failed and why, or does it say “sync failed”?
- **Export:** Can logs and traces flow into your existing monitoring stack (OpenTelemetry or similar), with your alerting rules?
- **Audit trail:** For enterprise buyers, a record of who changed what, and when.
- **Log controls:** Can you configure retention, access controls, data residency, and redaction so credentials and sensitive customer data do not leak into logs?


### 6. Deployment options and data residency


For a large share of teams, deployment was a pass/fail filter applied before any feature discussion. EU customer data cannot flow through US-only infrastructure without a[legal transfer pathway under GDPR Chapter V](https://www.edpb.europa.eu/sme-data-protection-guide/international-data-transfers_en) . Healthcare teams needed a[business associate agreement](https://www.law.cornell.edu/cfr/text/45/164.504) signed before discussing features. Teams serving Singapore, the Gulf states, and regulated European industries brought residency requirements of their own.


What teams checked:


- **Regions:** Where does the managed cloud run, and is there a region that satisfies your customers’ rules?
- **Self-hosting:** Is the self-hosted version at feature parity with the cloud, or a stripped-down build?
- **Your own cloud:** Can the platform run inside your AWS, GCP, or Azure account (BYOC), and who operates it there?
- **Consistency:** The same APIs and tooling across deployment modes, so you are not building twice.


### 7. Security posture


An integration platform stores your customers’ credentials and executes API calls on their behalf. It becomes part of every security review and procurement process you go through. SOC 2 Type II reports and penetration tests came up on most calls.


What teams checked:


- **Who the end user authorizes:** Does the OAuth consent screen show your app’s name or the platform’s? A third-party name on the consent screen raises questions in your customers’ security reviews.
- **What an attacker gets:** In May 2026, an attacker exfiltrated[5,001 GitHub OAuth tokens and 5,241 API keys from Composio](https://material.security/resources/the-composio-breach-one-token-10242-doors) . Teams asked how credentials are encrypted, how they are scoped, and what a compromise of the vendor’s own systems would expose.
- **Shared OAuth apps:** Shared developer apps are convenient. In production, a policy change or rate limit on a shared app affects every customer on it at once. Own your OAuth apps for anything that matters.


### 8. A pricing model you can project


Integration platform pricing comes in many shapes: per connection, per task or record, by compute or concurrency, or a flat license. Per-connection pricing is expensive for products with many mostly idle connections. Per-task pricing is expensive for high-frequency syncs. Concurrency pricing is expensive for bursty workloads.


What teams checked:


- **Your shape at 10x:** Model your own usage 12 to 18 months out, with your growth assumptions, and get the vendor to confirm the math.
- **Inactive connections:** Do you pay for connections nobody is using?
- **The knobs:** Can you tune sync frequency, switch polling to webhooks, or batch requests to control the bill yourself?
- **Pass-through economics:** If integrations are part of what you sell, does the model let you price them sanely for your own customers?


### 9. AI agent readiness


Two separate questions hid under “does it work with AI agents”:


**Runtime, agents calling integrations:** Products embedding AI agents need integrations exposed as tools: typed definitions the model can call, scoped per connection so an agent acting for one tenant cannot reach another tenant’s data. Teams building agent products asked for guardrails at the parameter level.


**Build time, agents building integrations:** By mid-2026, “can our coding agent build the connector” had become a standard evaluation question. Teams wanted platforms that work well with Claude Code, Codex, and Cursor.


What teams checked:


- **Tenant isolation:** Is every tool call bound to the correct customer connection on the server, rather than relying on the model to choose safely?
- **Least privilege and approvals:** Can you limit tools, scopes, and parameters per agent?
- **Validated tool contracts:** Are inputs and outputs typed and validated before an external API call runs?
- **Production behavior:** How does the platform handle retries, idempotency, timeouts, and audit logs when an agent repeats or partially completes an action?
- **Build and release controls:** Can coding agents test against real connections, submit generated source for review, and use the same versioning, CI, and rollback process as human-written integration code?


### 10. Exit path


Vendors get acquired and roadmaps change: Workday[announced its acquisition of Pipedream on November 19, 2025](https://newsroom.workday.com/2025-11-19-Workday-Signs-Definitive-Agreement-to-Acquire-Pipedream) . Teams that had been through a platform migration asked about the exit before they asked about the product.


What teams checked:


- **Credential portability:** Can you export your customers’ tokens and API keys if you leave, or does leaving mean asking every customer to re-authorize?
- **Data portability:** Can you export synced data and configuration in a usable format?
- **Source availability:** Is the platform’s code public and auditable, and could you keep running it if the vendor disappeared?


## What teams did not ask about


Vendor checklists emphasize three things that rarely came up on real evaluation calls:


- **Unified data schemas:** Teams building AI features told us normalized schemas matter less than they used to, because models handle raw API responses well. What they wanted was complete data, since fixed common schemas drop the custom fields they need.
- **Visual workflow builders:** Engineering teams treated low-code builders as a cost. They wanted integration logic in version control, code review, and CI like the rest of their product.
- **Internal automation features:** Teams distinguished sharply between product integrations (embedded in their app, used by their customers) and internal automation (connecting their own tools). iPaaS solutions built for internal workflows kept surfacing as mismatches for product use cases.


## How teams ran the evaluation


The teams that made confident decisions ran the same shape of proof of concept, usually in one to two weeks:


1. **Pick your hardest integration, not your easiest:** Any platform demos well on Slack. Evaluate it on NetSuite, SAP, or whatever the most complex item on your roadmap is.
2. **Test edge cases early:** Add a custom field, call an endpoint the platform does not cover, and measure what that actually takes.
3. **Break things on purpose:** Revoke a token mid-sync. Replay a webhook. Then read the logs and judge whether you could have debugged the failure without filing a support ticket.
4. **Start procurement in week one:** Along with the technical evaluation, immediately send the compliance documents (SOC 2 report, DPA, BAA) to whoever runs security reviews.
5. **Model the bill at 10x:** Get your projected usage priced before the trial ends.


## Where Nango fits


Nango is an API integration platform with a catalog of 900+ APIs and enterprise-ready infrastructure built for scale and customization. Its code- and developer-first approach keeps integration logic under your control. Here is how Nango maps to the evaluation criteria above:


Criterion Relevant Nango capabilities


Control over integration logic TypeScript functions, editable source, CLI, Git and CI/CD, coding-agent workflows


Catalog depth 900+ supported APIs, 6,000+ pre-built tools and syncs, flexibility to add your own provider or custom tools


Complete auth handling Managed OAuth and API-key auth, token refresh, encrypted credential storage, branded Connect UI, pre-provisioned and customer-owned OAuth apps


Reliability at scale Incremental syncs, webhooks, retries, pagination, rate-limit handling, connection-scoped execution


Debuggable observability Per-connection logs, searchable requests, metrics and alerts, OpenTelemetry, Management MCP server (beta)


Deployment and data residency Nango Cloud, enterprise self-hosting at feature parity with the cloud, BYOC on AWS, GCP, or Azure


Security posture SOC 2 Type II, GDPR, HIPAA with a BAA, AES-256-GCM encryption, RBAC, SSO, scoped API keys, audit logs


Projectable pricing Published unit pricing and included allowances for connections, proxy requests, compute, function runs, logs, sync storage, and webhooks. Volume discounts.


AI agent readiness MCP and OpenAI-compatible tool definitions, connection-scoped actions, Claude Code, Cursor, and Codex support


Exit path Open-source codebase, portable TypeScript source, self-hosting, credential export when using customer-owned OAuth apps


### Sources


- [Coding-agent setup](https://nango.dev/docs/getting-started/coding-agent-setup)
- [API integrations catalog](https://nango.dev/api-integrations)
- [Add or request an API provider](https://nango.dev/docs/integrations/contribute-or-request-api)
- [Auth guide](https://nango.dev/docs/guides/auth/auth-guide)
- [Observability](https://nango.dev/docs/guides/platform/observability)
- [Self-hosting](https://nango.dev/docs/guides/platform/self-hosting)
- [Security](https://nango.dev/docs/guides/platform/security)
- [Pricing](https://nango.dev/pricing)
- [Tool calling and MCP](https://nango.dev/docs/guides/functions/tool-calling)
- [Nango GitHub repository](https://github.com/NangoHQ/nango)


If you are running an evaluation, use the[quickstart](https://nango.dev/docs/getting-started/quickstart) to test these capabilities on one of your harder integrations.


## FAQ


**How do you choose an API integration platform?**


Choose based on how much control you keep over integration logic, whether the platform passes your customers’ security reviews, and whether the pricing model fits your usage shape at 10x scale. Then verify catalog depth against your specific roadmap with a one-to-two-week proof of concept on your hardest integration, and judge observability by debugging a failure you cause on purpose.


**What is the difference between an embedded iPaaS, a unified API, and an integration platform?**


An embedded iPaaS gives you low-code workflow tooling to build integrations one by one inside your product, a unified API gives you one normalized schema across a category of APIs, and broader integration platforms give you the infrastructure (auth, syncs, webhooks, observability) while integration logic stays under your control.[Five ways to build product integrations in 2026](https://nango.dev/blog/four-ways-to-build-in-app-integrations) compares the approaches in depth.


**How do you manage multiple API integrations efficiently?**


Centralize what repeats across every integration (auth and token lifecycle, retries, rate limits, logging) in one layer, and keep per-API logic small, isolated, and in version control. Monitor per connection rather than per integration, because failures happen to individual customers. This is the main argument for using an API integration platform like Nango instead of accumulating one-off connectors.


**When does it make sense to build API integrations in-house?**


Building in-house makes sense when you have a small number of integrations, requirements too specific for any platform, and the capacity to own maintenance permanently, including provider breaking changes and token lifecycle infrastructure. Most of the cost shows up after the first version ships, in maintenance.[Product integrations: build or buy?](https://nango.dev/blog/product-integrations-build-or-buy) walks through the decision.


**What do AI agents change about evaluating integration platforms?**


Agents add two criteria: a runtime where agents call integrations as scoped, observable tools (MCP or OpenAI-compatible definitions), and build-time support for coding agents to create new integrations. The older criteria do not go away, because agents fail on the same auth, rate-limit, and reliability problems humans do.


## Related reading


- [What we learned operating auth for 900+ APIs](https://nango.dev/blog/lessons-from-operating-api-auth-for-900-apis)
- [Why B2B SaaS teams outgrow pre-built unified APIs](https://nango.dev/blog/why-b2b-saas-outgrow-pre-built-unified-apis)
- [Five ways to build product integrations in 2026](https://nango.dev/blog/four-ways-to-build-in-app-integrations)
- [Product integrations: build or buy?](https://nango.dev/blog/product-integrations-build-or-buy)
- [How to find the best integrations partner](https://nango.dev/blog/how-to-find-the-best-integrations-partner)
