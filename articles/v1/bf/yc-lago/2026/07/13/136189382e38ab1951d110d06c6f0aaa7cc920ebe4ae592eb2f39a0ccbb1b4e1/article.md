---
schema_version: "1.0.0"
document_id: "136189382e38ab1951d110d06c6f0aaa7cc920ebe4ae592eb2f39a0ccbb1b4e1"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/lago-vs-orb"
published_at: "2026-07-21T12:39:09+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:c26b72733899fcb04e72dfcf4e28ef320cef9260d5ad665b539de112b7ca1e88"
---

# Lago vs Orb: Open-Source Billing vs a Billing Specialist Now Inside Adyen

---


## Lago vs Orb


Lago is open-source usage-based billing infrastructure you can self-host or run on Lago Cloud. Orb is a closed-source, cloud-only billing specialist, acquired by Adyen in a deal that closed July 1, 2026. The core difference isn't feature depth. Both meter usage well. It's where your billing logic lives: in code you own, or inside a vendor's infrastructure, now a payment processor's infrastructure.


If you're evaluating both, the honest way to frame it: Orb is the more mature product for SQL-driven pricing experimentation and enterprise RevRec. Lago is the only one of the two where you can read the rating engine, run it on your own infrastructure, and keep billing logic independent of any payment processor's roadmap.


## What each platform actually is


Lago is open-source: metering, rating, invoicing, prepaid credits and wallets, entitlements, subscription management. AGPLv3 license, code on[GitHub](https://github.com/getlago/lago) . Deploys self-hosted or on Lago Cloud.


Orb is a closed-source usage-based billing specialist. Strong pricing primitives, a polished product surface, deep AI/API billing capabilities. OpenAI, Vercel, and Dune ran their billing on it. Cloud-only, no self-hosted option. As of July 1, 2026, it's a subsidiary of Adyen.


That acquisition changes more than a logo. We wrote the full breakdown in[Orb Is Being Acquired by Adyen](https://getlago.com/blog/orb-adyen-acquisition-lago-alternative) : the short version is that Orb's stated "multi-PSP support continues initially" is the same qualifier that preceded Metronome's roadmap converging with Stripe's after its January 2026 acquisition. If PSP neutrality matters to your billing strategy, that's a live variable with Orb now, not a settled one.


## Deployment: self-hosted vs. cloud-only


This is the one binary that doesn't have a "well, it depends" answer. Lago runs on your own infrastructure or on Lago Cloud. Orb runs on Orb's infrastructure, full stop, and after the acquisition, that infrastructure sits closer to Adyen's.


For most SaaS companies this is a preference, not a requirement. For healthcare companies under HIPAA, financial services firms with data residency mandates, and government contractors under security review, it's a hard requirement a cloud-only vendor cannot satisfy regardless of how good the product is.


To be precise about what "cloud-only" means here: Orb's security documentation describes AWS cloud infrastructure and production controls. There's no public self-hosted deployment path in Orb's documentation. That's a description of what's publicly documented, not a claim about what Orb could theoretically build.


Self-hosted Lago means you control the billing database, the event pipeline, the webhook infrastructure, and the rating engine's source code. AGPLv3 means the code is auditable: for financial software, where a calculation bug has direct revenue and compliance consequences, that's not a nice-to-have.


## Usage metering and event ingestion


Both platforms meter high-volume usage well. This isn't a case where one can and one can't.


Lago ingests through REST, batch, SDK, Kafka, Redpanda, Kinesis, and S3. The pipeline is open-source: custom deduplication logic or event transformation rules get built directly into the codebase your team already owns. Orb's ingestion is specialist-grade, built for exactly this problem before the acquisition. The difference is who implements custom logic: with Orb, that's a conversation with their engineering team; with Lago, it's a pull request.


Where this shows up in practice: AI companies with per-model token pricing, infrastructure companies with multi-dimensional usage, any team that expects metering logic to change as the product does.


## Prepaid credits and wallets


This is a category where Orb is genuinely strong and shouldn't be undersold. Orb calls its credit primitive license allocations: credits tied to fair-use enforcement and reporting, one of the features that made Orb compelling for AI companies like OpenAI. Credits that expire, drawdown tracking, balance visibility: the depth is real.


Lago's wallet is native billing infrastructure, not a bolt-on: credits that expire and roll over on configurable schedules, real-time consumption alerts, and a direct connection to entitlements. When a wallet hits zero, product access changes immediately, because the wallet and the entitlement layer are the same system, not two systems kept in sync via webhook. With Orb, wiring a credit balance to your application's access control is an integration you build on top.


For AI companies where credit mechanics are customer-facing, balance shown in the UI, alert at 80%, hard cutoff at zero, that architecture difference shows up in the product experience, not just the billing config.


## Pricing primitives: SQL vs. JSON and API


Orb defines pricing rules in SQL-like syntax, including dimensional pricing over usage event properties. A finance or data team comfortable writing SQL can configure complex pricing logic without pulling in engineering. That's a real advantage for organizations where pricing is owned outside the engineering org.


Lago uses JSON-based configuration through a web UI and API. Pricing logic sits closer to your product's data model and lives in version control alongside product code, which is where engineering-led teams want it.


Neither wins outright. It comes down to who owns pricing at your company and what tooling they already trust.


## Pricing simulation: the honest gap


Orb ships packaged Simulations: comparing pricing scenarios, modeling customer and revenue impact, before a change ships. That's a real product surface, not marketing.


Lago has projection and instant-estimate primitives live in the open-source product: forecasted usage against a proposed rate card, fee estimates ahead of invoicing. What Lago doesn't have is a packaged simulation UI that mirrors what Orb ships. If pricing scenario modeling in a polished interface is a hard requirement, that's a genuine gap, not something to paper over with a roadmap promise.


## PSP neutrality, post-acquisition


Before June 11, 2026, both platforms described themselves as payment-processor neutral. Lago still is: it integrates with Stripe, Adyen, GoCardless, and others, and doesn't route through any of them by default. Route enterprise customers through Adyen and self-serve through Stripe, no change to how billing logic works.


Orb's neutrality now has an asterisk.[Adyen's announcement](https://www.adyen.com/press-and-media/jtrg4qd7j3p4rj) says it will keep supporting multi-PSP environments in the first phase, with longer-term convergence toward a single combined billing and payments infrastructure. Metronome went through the same transition inside Stripe. The pattern isn't a criticism of either acquisition, it's the strategic logic behind them: the billing specialist's roadmap converges with the parent processor's over time.


If you're building on a five-year horizon, "which payment processor will my billing logic answer to in 2029" is worth asking before you sign.


## Pricing: what you actually pay


Lago's open-source core is free to self-host; you pay for your own infrastructure and the engineering time to run it. Lago Cloud is a flat monthly fee that doesn't scale with revenue.


Orb doesn't publish pricing. It's based on billings and events processed, with a platform fee layered on Advanced and Enterprise tiers, historically estimated around $720/month at entry level before Orb pulled its public pricing page. Current numbers require a sales conversation, and Adyen's ownership may shift them further.


## Lago vs Orb: feature comparison


## When Lago is the right choice


Self-hosted deployment is a requirement, not a preference: regulated industries, data residency mandates, air-gapped environments. PSP neutrality matters on a multi-year horizon, and you don't want billing logic implicitly tied to a payment processor's acquisition strategy. Billing logic is a strategic asset your team wants to read, audit, and modify directly, not a configuration you request changes to. Pricing changes often, AI token rates, new credit packs, new enterprise tiers, and you want that iteration speed in your own hands.


## When Orb is (or was) the right choice


You're already on Adyen for payments and a tighter combined stack is genuinely appealing, contingent on how that product develops. Enterprise RevRec is a primary requirement and finance owns the billing project. Packaged pricing simulation, comparing scenarios and modeling revenue impact in a polished UI rather than through export or API workflows, is a hard requirement. You want a managed specialist's depth without operational ownership, and PSP convergence isn't a constraint your organization cares about.


## The honest read


Six months ago this was a clean trade-off: open, self-hostable, PSP-neutral versus a polished closed-source specialist. Both strong, different philosophies, no acquisition in the picture.


Today one side of that comparison answers to a global payment processor's roadmap. That doesn't make Orb's product worse. It changes what you're actually buying when you buy it.


[Mistral](https://getlago.com/customers/mistral) and[Blacksmith](https://getlago.com/blog/blacksmith-billing) run usage-based billing on Lago. Read the[documentation](https://getlago.com/docs/welcome) , deploy the[open-source version](https://github.com/getlago/lago) , or[book a demo](https://getlago.com/book-a-demo) to walk through your pricing model.
