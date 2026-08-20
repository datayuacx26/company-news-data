---
schema_version: "1.0.0"
document_id: "a04f3ecc2f1bc956e07734230043a35995b01d1bee1991af4dbb0c1234959ace"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-embedding/"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:9eecea2913e29e93f8146a8887996bbd8b84ba7ebc69315fff458c189ac084d1"
---

# Introducing Basedash Embedding

Today we’re launching **Embedding** — a way to put the full power of Basedash inside your own product, so the customers using your app get dashboards and the Basedash AI agent on their own data, without ever leaving you.


Embed a single chart or dashboard with one iframe, or embed the entire Basedash app so your customers can chat with the agent, build their own dashboards, and get automatic insights. Row-level security keeps every customer scoped to their own data, and customization controls let you decide exactly which features they see.


Analytics, inside your product. Scoped to every customer.


## Why we built Embedding


Every B2B product eventually gets the same request: “Can we see our own data?” Your customers want analytics on the activity they generate inside your app — campaign performance, usage, spend, outcomes — and they want it where they already work, not in a separate tool with a separate login.


Building that yourself is a project, not a feature. Charting libraries, a query layer, permissions, multi-tenant data isolation, a place for customers to ask their own questions — it’s months of work that isn’t your core product, and it’s never quite done.


Embedding turns that project into a feature you ship in days. You already trust Basedash internally to read your data and answer questions about it. Now you can hand that same experience to your customers, scoped to exactly the data each one is allowed to see.


## Two ways to embed


There are two levels of embedding, and you can use either or both.


**Static embedding** is the simplest. Build a chart or dashboard in Basedash, turn on a public embed link, and drop the iframe into your site or app. Viewers see a live, interactive dashboard with no Basedash account required. It’s perfect for a public metrics page, a status board, or a single report you want to surface inside an existing screen.


**Full app embedding** is the powerful one. Embed the entire Basedash app behind JWT-based single sign-on, and your customers get the whole experience inside your product: chat with the AI agent, build their own dashboards, set up automations, and get proactive insights — all on their own data, all without leaving you.


## How it works


For static embedding, it’s a share link:


1. Open a chart or dashboard and click **Share**
2. Enable the **Embedding** toggle
3. Copy the iframe snippet into your app


For full app embedding, it’s an iframe plus a JWT your backend signs:


1. Enable **full app embedding** in **Settings → Embedding** and grab your JWT secret
2. Generate a short-lived JWT server-side with the user’s email and your organization ID
3. Load the iframe pointing at the SSO endpoint, and Basedash creates a scoped session for that user


That’s the whole integration — one iframe and a signed token. No SDK to install, no data pipeline to build.


## Every customer sees only their own data


The reason embedded analytics is hard to ship safely is multi-tenancy: customer A must never see customer B’s data. Basedash handles this with **row-level security** .


When you generate the JWT for a user, you include the parameters that scope them — a` company_id` , a` tenant_id` , whatever your model uses. Basedash locks those values server-side and applies them to every query that user’s session runs. Customers can ask the agent anything and build any dashboard they like; they only ever touch their own rows. The locked values can’t be tampered with from the browser, because they’re signed into the token your server controls.


The same mechanism powers secure filtering on static dashboard embeds: sign the locked filter values into the URL, and each viewer sees a dashboard filtered to exactly their slice of the data.


## You control exactly what they get


Embedded Basedash is yours to shape:


- **Theme** — match light, dark, or your customer’s system preference
- **Visible features** — show or hide Insights, suggested prompts, and the organization name
- **Allowed origins** — restrict which domains are allowed to embed your organization, down to specific subdomains
- **Branding** — the embed wears a quiet “Powered by Basedash” rather than taking over your product


Turn features on as your customers are ready for them. Start with read-only dashboards, then open up the agent and dashboard building when you want a more self-serve experience.


## What teams build with Embedding


The shape we see most often: a B2B product that already has a customer-facing dashboard, bolting on real analytics without building a BI tool from scratch.


- A marketing agency embeds Basedash into its client portal, and each client can ask “which campaigns had the best ROAS last month?” and get an answer and a chart on the spot — seeing only their own account.
- A SaaS platform gives every customer an in-product usage and revenue dashboard scoped to their workspace.
- A customer success team ships account-health views that customers can explore themselves, instead of exporting spreadsheets.


## How we think about Embedding


Internally, we’ve used the same embedding primitives to surface scoped Basedash views in our own tools — the proof that the full app embed and row-level security hold up under real multi-tenant use. The thing that makes it click: your customers don’t get a static export, they get an analyst. They can follow up, slice differently, and build the view they actually wanted, all without a single support ticket landing on your team.


## Getting started


Embedding is available today for all Basedash workspaces.


1. [Sign up for Basedash](https://charts.basedash.com/signup) (or log in)
2. For a single dashboard: open it, click **Share** , and enable **Embedding**
3. For the full app: go to **Settings → Embedding** , enable full app embedding, and grab your JWT secret
4. Generate a JWT server-side and load the iframe — your customers are in


For setup details, JWT claims, customization options, and row-level security, see the[Embedding feature page](https://www.basedash.com/features/embedding) or the[embedding docs](https://www.basedash.com/docs/features/embedding) .


## What’s next


Embedding extends everything Basedash already does —[AI chat](https://www.basedash.com/features/ai-data-analyst) , the[Dashboard Agent](https://www.basedash.com/features/dashboards) ,[Insights](https://www.basedash.com/features/insights) , and[Automations](https://www.basedash.com/features/automations) — out to the customers who use your product. The same governed analytics you run internally, now a feature you can ship.


**Embed Basedash today and give your customers analytics inside your own product.**
