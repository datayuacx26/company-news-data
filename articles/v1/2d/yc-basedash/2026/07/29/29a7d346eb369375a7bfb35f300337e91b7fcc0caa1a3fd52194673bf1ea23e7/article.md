---
schema_version: "1.0.0"
document_id: "29a7d346eb369375a7bfb35f300337e91b7fcc0caa1a3fd52194673bf1ea23e7"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-the-basedash-developer-platform/"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-23T03:57:00.283818+00:00"
fetched_at: "2026-07-28T20:33:07.280239+00:00"
content_hash: "sha256:72a15d797b5b748b48f52a56ed2dddb6cc5a781860ac139f26508739ff4f5094"
---

# Introducing the Basedash developer platform

Today we’re launching the **Basedash developer platform** — everything Basedash does, now available through the API.


Over the past year we’ve shipped an[AI data analyst you can chat with](https://www.basedash.com/features/ai-data-analyst) ,[automatic daily insights](https://www.basedash.com/features/insights) ,[automations](https://www.basedash.com/features/automations) that deliver reports on a schedule, and[dashboards](https://www.basedash.com/features/dashboards) that build themselves. All of it was built for internal teams. As of today, all of it is programmable: create chats, stream the analyst’s answers, render the charts it builds, manage dashboards and automations — from your own code, in your own product, behind your own UI.


## Why we built it


Two requests kept coming up, from opposite directions.


Product teams told us: “Our customers keep asking for analytics. We trust the Basedash analyst internally — can we put it in front of our customers?” Building customer-facing analytics from scratch is a brutal project: a query layer, charting, permissions, multi-tenant isolation, and now — because every customer expects it — an AI analyst that actually gets answers right. That’s quarters of work before you ship anything.


Data teams told us the opposite: “We love the product, but we want to drive it from our own systems.” Trigger an analysis from a support ticket. Render a chart into an internal tool. Wire insights into a workflow no BI vendor anticipated.


Both requests have the same answer: stop treating the platform as an app, and expose it as infrastructure. So we did.


## Ask. Stream. Render.


The core loop is three steps.


**Ask.**` POST /chats` creates a conversation with the analyst against your connected data sources. Send a message — “Why did revenue spike last week?” — the same way you’d type it into Basedash.


**Stream.** The analyst’s work streams back as server-sent events: status updates as it explores your schema, the SQL it writes and verifies, the charts it builds, and the final answer. You can render its progress live in your own interface, or just wait for the completed message — both are supported, with idempotency keys for safe retries.


**Render.** The response includes structured chart objects, and every chart has an image endpoint, so you can render results natively in your own components or drop in a generated image anywhere — a web app, an email, a PDF report.


Beyond chat, the API covers the whole platform: charts, dashboards and their tabs, insights, automations and their runs, data sources, metric definitions, skills, members, groups, audit logs, and AI usage. If Basedash can do it, your code can too.


## Build your product’s analytics on Basedash


The headline use case: your product’s customer-facing analytics, powered by Basedash, wearing your UI.


We already supported[embedding](https://www.basedash.com/blog/introducing-basedash-embedding) — drop the Basedash app or a dashboard into your product with an iframe, scoped per customer. That’s still the fastest path, and it’s still there. But plenty of teams want analytics that look and feel like *their* product: their design system, their components, their interaction patterns. The developer platform makes that possible — build a fully custom UI and let Basedash do everything behind it. Your customers ask questions in your interface; our analyst explores their data, writes and verifies the queries, and hands your frontend the answer and the charts.


The multi-tenancy story carries over from embedding:[row-level security](https://www.basedash.com/docs/features/row-level-security) scopes every query, so customer A can ask anything and never touch customer B’s rows. Scoping is enforced server-side — not something your frontend has to get right.


## Or extend your internal BI


The same API works inward. Teams are wiring Basedash into the systems they already run:


- Kick off an analysis when something happens elsewhere — a deal closes, a ticket escalates, a deploy ships — and post the answer where the team lives
- Render live Basedash charts into internal tools, wikis, and status pages using the chart image endpoint
- Manage dashboards, automations, and data sources as code, the way you already manage infrastructure
- Pull audit logs and AI usage into your own compliance and cost tooling


And when the thing calling Basedash is an agent rather than your code, the[Basedash MCP server](https://www.basedash.com/blog/introducing-the-basedash-mcp-server) exposes the same analyst as a tool: connect Claude Code, Cursor, ChatGPT, or an agent you’re building yourself, and it can query your data and get verified answers the same way the API does. The two compose well — the MCP server for conversations, the API for everything you ship.


## The #1 ranked AI data analyst


An analytics API is only as good as the analyst behind it. On[BI Bench](https://www.basedash.com/bi-bench) , our public benchmark that runs AI data analyst agents against a real database with a genuinely messy schema, Basedash ranks #1 on accuracy — ahead of coding agents like Claude Code and BI platforms like Sigma and Metabase.


Fitting for a launch on OpenAI day: the analyst runs on GPT-5.6. Every answer is grounded in your actual schema, validated before execution, and traceable to the queries that produced it — which is what makes it something you can responsibly put in front of your customers.


## Getting started


The API is available today for all Basedash workspaces.


1. [Sign up for Basedash](https://charts.basedash.com/signup) (or log in) and connect a data source
2. Create an API key in **Settings → API keys**
3. Make your first request —` POST /chats` with a question about your data
4. Stream the answer into whatever you’re building


Authentication, endpoints, and response formats are covered in the[API reference](https://www.basedash.com/docs/api-reference/overview) .


## What’s next


Every feature we ship from here lands in the product and the API.[Chat](https://www.basedash.com/features/ai-data-analyst) ,[insights](https://www.basedash.com/features/insights) ,[automations](https://www.basedash.com/features/automations) , and[dashboards](https://www.basedash.com/features/dashboards) were built to answer your team’s questions; now they can answer your customers’ too.


**Your AI data analyst, now yours to build on — live today for every Basedash workspace.**
