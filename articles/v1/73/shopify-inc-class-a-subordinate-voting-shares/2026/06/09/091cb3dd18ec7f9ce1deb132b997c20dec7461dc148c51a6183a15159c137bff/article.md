---
schema_version: "1.0.0"
document_id: "091cb3dd18ec7f9ce1deb132b997c20dec7461dc148c51a6183a15159c137bff"
company_key: "shopify-inc-class-a-subordinate-voting-shares"
company: "Shopify Inc."
source_id: "shopify-inc-class-a-subordinate-voting-shares-news-import-7af15cdf0c76"
canonical_url: "https://www.shopify.com/news/spring-26-edition-dev"
published_at: "2026-06-17T08:00:00+00:00"
first_seen_at: "2026-07-22T13:22:18.403246+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:ef16fd1b6021fe3cd7716f5f331fbfaa7f80716daa326defde6af57b85e2f201"
---

# Agentic commerce for every developer: The Spring '26 Edition

AI is changing how buyers find products, and Shopify makes sure merchants show up wherever that's happening.


The Universal Commerce Protocol (UCP) is what makes that possible. It’s the open standard for how AI agents transact with merchants. Meanwhile Catalog API is UCP's discovery layer, turning billions of products from millions of merchants into structured, queryable data across AI surfaces.


In Winter '26, Shopify went all-in on agentic commerce and rebuilt the dev platform for AI.


[Spring '26 throws the doors wide open.](https://shopify.com/editions/spring2026?utm_source=newsroom&utm_medium=newsroom&utm_campaign=spring26edition-launch_Q226SETPNZ&utm_content=dev-newsroom-v1) The agentic commerce infrastructure is now self-serve for every developer. Underneath, the foundation gets rebuilt for production scale, and the friction developers have been working around smooths out.


Here's what's new.


## 1. Agentic commerce is open to the entire developer ecosystem.


Building on Shopify's agentic commerce layer used to require approval. That requirement is gone. With UCP and Catalog API, we give developers the tools to build end-to-end agentic experiences all on their own. Developers don't apply for UCP access, they register their agent profile in the Developer Dashboard and call the public MCP endpoint. From there, they can build across the full flow, from product search to checkout. (If you want to see what this looks like, check out[these shopping app demos](https://www.shopify.com/news/spring-26-edition-design) we made.)


### UCP


The[Universal Commerce Protocol](https://www.shopify.com/editions/spring2026#universal-commerce-protocol) is the standard Shopify co-developed with Google. It's the infrastructure for how AI agents transact with merchants, covering the full commerce journey from discovery to carts to checkout. UCP has broad industry support from Amazon, Meta, Microsoft, Salesforce, Stripe, Etsy, Target, and Wayfair.


What's new in Spring '26 is that any developer can access millions of Shopify merchants using Shopify's tools.


The UCP Skill, part of Shopify's open-source AI Toolkit, packages 20 years of commerce knowledge into something an agent can use out of the box. It helps agents navigate UCP on their own: introspect what's available, pull schemas, learn the shape of any operation without a human wiring it up. UCP CLI provides structured commands alongside. Overall, Universal Commerce Protocol gives builders the tools they need to create new shopping experiences. Full documentation lives[here](https://shopify.dev/docs/agents) .


### Catalog API


Catalog API turns Shopify's global product catalog into structured, queryable infrastructure for AI agents. Products from millions of merchants on Shopify are optimized with standardized data and real-time accuracy, so agents can discover, understand, and present them correctly across AI surfaces.


There are three things to know about Catalog API this Edition.


**It opens up.** Catalog access takes just an API key, no approval needed. ChatGPT, Copilot, and the Shop App are the most visible surfaces today, but the same product data can power the next wave of apps, agents, and marketplaces forming around them.


**It gets more useful.** Catalog was already queryable. This Edition makes it smarter at finding the right product.[Image search](https://www.shopify.com/editions/spring2026#catalog-api-image-search) returns visually similar products across Catalog. Multi-modal search combines text and image. The lookup endpoint resolves a list of product URLs into real Catalog matches. Richer metadata like size, color, and delivery estimates gives agents more context to present products accurately. AI searches powered by Shopify Catalog convert at 2x the rate of those using scraped data.


And Catalog API[supports Shop sign-in](https://www.shopify.com/editions/spring2026#catalog-api-supports-shop-sign-in) , so developers can add it to experiences they build on top, allowing shoppers to connect their Shop account and bring their saved details with them. Developers get a trusted, ready-made buyer identity instead of building their own login layer.


**It will pay builders.** Promoted placements will give developers and partners a future path to earn revenue when their Catalog-powered experiences drive sales. That gives developers a direct way to participate in the value they help create.


## 2. Shopify becomes native to how developers build and how merchants work.


Context switching breaks momentum for developers and merchants. Developers lose flow when they leave the editor to check docs or test an API. Merchants lose time when they leave Sidekick to switch between apps.


### Sidekick App Extensions: Simplifying merchant workflows


With the number of weekly active shops using Sidekick up 4x year over year in Q1, Sidekick is becoming where merchants run their business. Apps need to be there too. Now, any developer in Shopify’s ecosystem can build a Sidekick App Extension. App developers can surface their app's data and actions directly inside Sidekick conversations.


Data extensions let Sidekick surface a partner app's data when a merchant asks a relevant question whether it’s about campaign performance or loyalty stats. Sidekick pairs that data with Shopify store context and returns one response.


Action extensions go further. Sidekick routes merchants to the right spot inside an app and stages changes for confirmation. Merchants act without leaving the conversation.


App Extensions launches with 15+ partners, including Klaviyo, Loop, Smile, Judge.me, Matrixify, Avia, Seguno, Checkout Links, and Yotpo. Any developer in Shopify's ecosystem can now build in Sidekick.


### AI Toolkit


The[Shopify AI Toolkit](https://www.shopify.com/editions/spring2026#shopify-ai-toolkit-for-developers) is now GA, and it dramatically reduces the time needed to build apps and integrations. One plugin gives your editor (Cursor, Claude Code, Codex, VS Code, and more) direct access to Shopify's reference guides, live store data, and admin tools. Your assistant auto-fills the right data structures and catches errors as you write, all without leaving the editor. The AI toolkit bundles a set of skills and the Shopify CLI together. The skills help agents understand *how* to build and the CLI then executes the appropriate commands. This combination is powerful and[highly token efficient,](https://www.shopify.com/editions/spring2026#shopify-dev-mcp-optimized-token-usage) reducing spend.


###


### Static Apps


Developers can build and deploy a fully functional Shopify app without running a server. Shopify handles the hosting, App Bridge and Polaris are auto-injected, and authentication is handled by direct API access.[Static apps](https://shopify.dev/docs/api/app-home-ui-extension/latest) can also leverage Shopify metaobjects and metafields to store relevant merchant data. These updates are great for agencies building apps for merchants, because they don’t need to worry about hosting costs or their data security.


## 3. The infrastructure developers depend on gets rebuilt.


Great developer tools are only as strong as the systems underneath them. This Edition strengthens the foundation. Webhooks you can trust to deliver, billing handled end-to-end by Shopify, and a single dashboard for everything you manage.


### Next Gen Events


Apps need to know precisely when a commerce event happens, like a price change. In order to enable this, Shopify has[rebuilt its webhook system](https://www.shopify.com/editions/spring2026#redesigned-webhook-architecture) from the ground up. Field-level change filtering lets developers subscribe to specific changes like customer address information or inventory updates, rather than every update on a resource. Customizable GraphQL payloads let developers define exactly the data they need.


### App Events


Understanding how merchants leverage your app used to require separate analytics tools. Apps could receive data from Shopify (webhook deliveries, Function executions, API calls) but if a sync failed, a feature went unused, or a merchant hit an error, you had no way to log that back to Shopify. Debugging meant switching between your own monitoring tools, separate databases, and dashboards outside of Shopify.


App Events changes that.The[App Events API](https://shopify.dev/docs/api/app-events/latest) accepts any event from your app that you want to track. Define the \`event_handle\` and attributes (e.g. \`bulk_edit_completed\`, \`report_generated\`, \`onboarding_completed\`, \`email_sent\` and more), and it flows into your Dev Dashboard Logs automatically, alongside your existing data, giving you a complete view of your app’s performance in one dashboard.


### App Pricing


[Shopify App Pricing](https://www.shopify.com/editions/spring2026#simple-billing-with-shopify-app-pricing) is the new home for app billing. If you've ever shipped a Shopify app with anything more complex than a flat monthly fee, you've probably spent weeks on billing infrastructure you didn't want to write: metering, invoicing, payment collection, edge cases for refunds and plan changes. All of it has to work and none of it is the commerce product you set out to build.


###


Shopify App Pricing is powered by the App Events API for usage charges. Developers send billable events (and custom app events) to one endpoint and Shopify meters them automatically, calculates charges based on the pricing model configured, and includes them on the merchant's Shopify invoice with clear line items.


Shopify App Pricing replaces both Managed Pricing and the legacy Billing API.


### Flexible Fulfillment


Shopify is evolving how our fulfillment works. Buyers shop across channels, mix online and in-person purchases, and expect their delivery options to match. The first piece of that rebuild lands this Edition with[ship and pickup in one order](https://www.shopify.com/editions/spring2026#ship-and-pick-up-in-one-order) , which is moving from developer preview to GA.


Buyers can now select different delivery methods for different items within the same checkout—some shipped, some picked up in-store. The depth of the work shows up in small details: checkout makes delivery options easy to scan, and buyers can switch between ship and pickup for individual items.


For developers, this means apps, functions, integrations, and workflows that touch delivery, fulfillment, or order logic should be tested mixed-fulfillment orders. Mixed-fulfillment orders introduce patterns most apps weren't built for, with most assuming a single delivery method per order. Now's the time to make critical updates to be compatible with this release.


Like Events and App Pricing, fulfillment is the kind of foundational layer most people don't think about until it breaks. Shopify is rebuilding it now so that doesn’t happen.


### Developer Dashboard


The[Developer Dashboard](https://www.shopify.com/editions/spring2026#localized-dev-dashboard) is now the home for all building. Dev stores, client transfer stores, and collaborator access all live in the same surface. Logs and monitoring get a full overhaul with time-series visualization, group-by filtering, and app health gauges. Navigation is simplified, with a cleaner structure replacing three-level nesting.


Shopify treats developer surfaces with the same care as the rest of the platform. Developers spend most of their time in a dashboard, so investing in this one matters.


## Your move


AI is expanding who gets to build, and the ecosystem is wider than it's ever been. Shopify builds, partners build, and merchants grow.


Spring '26 Edition puts the commerce layer and the infrastructure in developers' hands.[See everything that shipped.](http://shopify.com/editions/spring2026)
