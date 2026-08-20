---
schema_version: "1.0.0"
document_id: "d7761e8babcbfaf07ae7fd72f35feffaa14bcd7255ff40a45d06e1c3aa91fa2b"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/orb-adyen-acquisition-lago-alternative"
published_at: "2026-06-29T10:07:00+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:4bca3ef230c692dfa3a38167a1220b8762ae96be9c5d80c3c6793e2ba7e116cf"
---

# Orb Is Being Acquired by Adyen. Here's What That Means If You're Evaluating Usage-Based Billing Platforms.

On June 11, 2026, Adyen announced it was acquiring Orb. Close was guided for around July 1, 2026.


If you're in the middle of evaluating usage-based billing platforms — comparing Lago and Orb, or Lago and anything else — that changes the comparison. Not in ways that are simple to summarize, but in ways that matter for long-term vendor independence, PSP neutrality, and where your billing logic ends up living.


This article explains what the acquisition means, how the two platforms actually compare, and when each is the right choice.


## What the Adyen acquisition means


Orb was the most technically sophisticated pure-play usage-based billing specialist on the market before the acquisition. OpenAI, Vercel, and Dune ran on it. Its pricing primitives, credit model, and event ingestion were genuinely strong — we said so internally before writing this.


Adyen is one of the world's largest payment processors, processing hundreds of billions in annual volume for companies like Spotify, Microsoft, and eBay.


The combination creates a unified monetization infrastructure play: Orb's billing and metering engine embedded in Adyen's payment rails. For buyers already using Adyen for payments, that could be a compelling integration. For buyers who want their billing logic independent of any specific payment processor, it's a different calculation.


Adyen's announcement says multi-PSP support "continues initially." That qualifier matters. The same pattern played out when Stripe acquired Metronome earlier in 2026 — the billing specialist and the payment processor's roadmaps converge over time. That's not a criticism of either acquisition; it's the strategic logic behind them.


Lago's position is different. We integrate with Stripe, Adyen, GoCardless, and others — but we don't route through any of them by default. If your billing strategy requires the right to change processors, route different customer segments through different PSPs, or operate across geographies with different payment method requirements, that independence is now a more concrete differentiator than it was six months ago.


## What each platform actually is


**Lago** is open-source billing infrastructure: usage metering, rating, invoicing, prepaid credits and wallets, entitlements, and subscription management. Self-hosted on your own infrastructure, or on Lago Cloud. The code is on[GitHub](https://github.com/getlago/lago) under the AGPL license.


**Orb** is a closed-source, cloud-only usage-based billing specialist with deep pricing primitives, a polished product surface, and strong AI/API billing capabilities. Now being acquired by Adyen.


The core architectural difference: Lago is a control layer — billing logic lives close to your product, your data, and your infrastructure. Orb is a managed specialist — billing logic lives in Orb's (and going forward, Adyen's) infrastructure.


## Deployment and data control


This is a binary. Lago supports self-hosted deployment. Orb does not.


For many teams, that distinction is theoretical — cloud-hosted billing is fine, and the operational overhead of self-hosting isn't worth it. For regulated industries, it's not theoretical at all. Healthcare companies under HIPAA, financial services firms with data residency mandates, government contractors with security review requirements — for these buyers, the billing system cannot be a cloud-only SaaS product, full stop.


With Lago self-hosted, you control the billing database, the event ingestion pipeline, the webhook infrastructure, and the codebase itself. The AGPL license means the code is readable and auditable — relevant for financial software where a calculation error has direct downstream consequences.


With Orb post-acquisition, all billing event data and payment transaction data will increasingly share infrastructure with Adyen. That's worth examining explicitly if your compliance requirements have opinions about co-mingled financial data infrastructure.


## Usage metering: where both platforms are strong


Both platforms handle high-volume usage metering well. This is not a "Orb can't meter events" argument.


Lago's ingestion paths: REST API, batch, SDK, Kafka, Redpanda, Kinesis, and S3. The ingestion pipeline is open-source, inspectable, and modifiable. If you need custom deduplication logic, specific event transformation rules, or integration with a streaming architecture, you implement it directly in the codebase.


Orb's ingestion is specialist-grade and was strong before the acquisition. The difference is control: with Orb, custom logic requires working with their engineering team. With Lago, you own the pipeline.


Where this matters in practice: AI companies with model-specific token pricing, infrastructure companies with complex multi-dimensional usage, and any team that expects their metering logic to change as their product evolves. The more your billing logic is a strategic variable rather than a fixed configuration, the more ownership matters.


## Prepaid credits and wallets


Both platforms handle prepaid credits. This is one area where the original comparison frequently undersells Orb — their credit model is genuinely strong and was one of the features that made Orb compelling for AI companies like OpenAI.


Lago's wallet is native billing infrastructure: credits that expire, roll over on configurable schedules, trigger consumption alerts, and connect directly to entitlements. When a customer's balance reaches zero, access changes immediately — not at the next invoice cycle. The enforcement is real-time because the wallet and the entitlement layer are the same system.


Orb's credit model operates through a managed platform. The depth is there; the question is whether you want that logic to be open and inspectable, or managed by a vendor. For AI companies where credit mechanics are product-facing — customers see their balance, get alerts at 80%, have hard limits at zero — the implementation architecture affects the product experience, not just the billing configuration.


One practical difference: Lago's wallet integrates with entitlements natively, so a credit balance change can automatically gate product access. With Orb, that integration requires building the connection between Orb's credit state and your application's access control layer.


## Pricing primitives: SQL vs. JSON/API


Orb uses SQL-like syntax for defining pricing rules. Finance and data teams who are comfortable writing SQL queries can configure complex pricing logic without engineering involvement. For organizations where pricing is owned by a data or finance team, this is a genuine advantage.


Lago uses JSON-based configuration with a web UI and API. Pricing configuration is closer to your product's data model and integrates naturally with programmatic infrastructure. For engineering-led teams who want billing logic in version control alongside product code, this fits better.


Neither is objectively better. The right answer depends on who owns pricing in your organization and what tooling they're comfortable with.


## PSP neutrality: the post-acquisition question


Before June 11, 2026, both Lago and Orb were described as payment-processor neutral billing layers that integrated with multiple PSPs.


After June 11, that description needs a caveat for Orb. Adyen's announcement describes a "combined monetization infrastructure" — billing and payment processing in one stack. Multi-PSP support "continues initially." The long-term trajectory is toward tighter integration with Adyen's payment rails.


Our internal feature analysis flags Orb's PSP neutrality as "Watchlist — Adyen says multi-PSP support continues initially, but strategic convergence raises long-term neutrality questions." That's the honest read.


Lago integrates with Stripe, Adyen, GoCardless, and others. Your billing logic — metering, rating, credit drawdown, invoice generation — stays in Lago regardless of which processor handles collection. If you're routing enterprise customers through Adyen and self-serve through Stripe, that stays configurable. If you want to add a processor in a new geography, same.


If you're building billing infrastructure with a 5+ year horizon, the question "which payment processor will my billing logic be tied to in 2029?" is worth asking explicitly.


## Feature comparison


## When Lago is the right choice


**You need self-hosted deployment.** Regulated industries where billing data cannot leave your own infrastructure. No workaround on Orb's side — it's cloud-only, and post-acquisition, that means Adyen's infrastructure.


**PSP neutrality matters long-term.** If you want billing logic that doesn't converge with any specific payment processor's roadmap over time, Lago is now the clearest option in this category. Metronome is inside Stripe. Orb is going inside Adyen. Lago isn't going anywhere.


**Billing logic is open architecture.** Open-source inspectability for security audits. The ability to modify the rating engine directly. Billing logic in version control alongside product code. For engineering-led teams, these aren't cosmetic differences.


**Pricing changes frequently.** AI companies repricing as compute costs drop. API companies adding new metrics. Infrastructure tools launching enterprise tiers with custom rate cards. If pricing iteration velocity matters and you want that in your team's control, the open-source model wins.


**Cost predictability at scale.** Orb's pricing is custom and not publicly listed. Lago Cloud is a flat monthly fee. The open-source version is free to run on your own infrastructure. At high event volumes, the economics of a flat-fee platform versus consumption-based vendor pricing are significant.


## When Orb is (or was) the right choice


**You're already on Adyen.** If Adyen handles your payments and you want billing and payment collection in a tighter integration, the acquisition may ultimately create a compelling combined stack. That depends on how Adyen builds out the combined product.


**You want managed depth without operational overhead.** Orb's product was genuinely polished. If you want strong usage billing primitives without running your own infrastructure, and the PSP convergence isn't a constraint for your strategy, Orb has delivered well for the companies that chose it.


**RevRec is a primary requirement.** Orb has stronger enterprise revenue recognition capabilities than Lago. If finance owns the billing project and ASC 606/IFRS 15 compliance is central, Orb's RevRec depth matters.


**Your team doesn't want to own billing infrastructure.** Self-hosted Lago requires DevOps capability. If that's not in your team and you don't want to build it, a managed platform removes that operational surface.


Honest note: Orb has a technically strong product. The reason we're recommending Lago over it isn't capability gaps — it's the strategic question of where you want your billing logic to live as the Adyen acquisition develops.


## Vendor risk: the honest version


"Open source eliminates vendor risk" is too clean. Running self-hosted infrastructure has its own operational risks — infrastructure failures, upgrade complexity, the engineering time required to maintain it. Those are real costs.


What open source actually provides: optionality. If Lago the company stopped operating tomorrow, the codebase remains fully accessible and forkable. Your billing infrastructure wouldn't disappear. That's a different risk profile than a closed-source SaaS platform, not a risk-free one.


For Orb post-acquisition: Adyen is a stable, public company. That actually resolves some of the startup-stage vendor risk Orb carried as an independent. The new risks are strategic — roadmap alignment with Adyen's priorities, long-term PSP neutrality, and what "billing specialist" means when the parent company's primary business is payment processing.


Neither option is risk-free. The question is which risk profile fits your organization's capabilities and priorities.


## The honest summary


Six months ago, comparing Lago and Orb was a clean trade-off: open-source / self-hosted / PSP-neutral versus a polished closed-source specialist. Both strong, different philosophies.


Today, one of those options is being absorbed into a global payment processor. That changes the long-term calculus for buyers who care about billing independence, open architecture, and the right to change payment processors.


[Mistral](https://getlago.com/customers/mistral) and[Blacksmith](https://getlago.com/blog/blacksmith-billing) run usage-based billing on Lago.[Deploy the open-source version](https://github.com/getlago/lago) , read the[documentation](https://getlago.com/docs/welcome) , or[book a demo](https://getlago.com/book-a-demo) to talk through your specific pricing model.
