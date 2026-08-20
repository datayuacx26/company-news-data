---
schema_version: "1.0.0"
document_id: "20c144b72fe28430580d6ca6605d7a169879298a696d054824d1b84871856ed2"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/what-is-metered-billing"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T17:04:32.115513+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:1804e5a68aa32895927662e2813879689296b9e3af717f27bad92c1a419b90ee"
---

# What Is Metered Billing? How It Works

Metered billing charges customers based on what they actually use: API calls, compute time, storage, tokens, measured and billed after the fact, rather than a fixed price agreed upfront. It's the mechanism underneath usage-based pricing. Usage-based pricing decides *what* to charge for and *how much* ; metered billing is what measures usage accurately enough to bill on it. GitHub uses it for Copilot's AI Credits. Stripe ships it as a built-in billing feature. Lago meters usage as event data in real time, at up to one million events per second, so pricing logic runs on usage as it happens instead of in a nightly batch.


## What is metered billing?


Metered billing is a pricing mechanism where a customer's invoice is calculated from measured consumption rather than a pre-agreed flat fee. The bill isn't known in advance. It's the output of tracking usage over a billing period and applying a rate to it, the same way a utility company reads a water or electricity meter before sending an invoice.


That's the whole idea. Everything else, usage-based pricing, hybrid plans, overage tiers, is a decision layered on top of the fact that usage was metered in the first place. For how this term sits alongside other billing and pricing vocabulary, see[Lago's pricing and billing glossary](https://getlago.com/blog/pricing-billing-glossary) .


## How does metered billing work?


Metered billing runs on four steps: usage is captured as events, those events are aggregated into a billable metric, a pricing rule is applied to that metric, and the result becomes a line on an invoice.


**Capture.** Every unit of consumption (an API call, a gigabyte stored, a token generated) gets sent as a usage event, typically over an events API as it happens rather than pulled from logs after the fact. At scale, this is a real throughput problem before it's a pricing problem. Lago's ingestion layer is built to handle up to 1,000,000 events per second, because a metering system that can't keep up with usage can't be trusted to bill on it.


**Deduplicate.** Events get retried, duplicated, and sent out of order. A metering system has to treat each event as idempotent, so the same usage doesn't get counted twice and a customer doesn't get billed for something that never happened.


**Aggregate.** Raw events become a billable metric using an aggregation rule. The four common ones: COUNT (how many API calls this month), UNIQUE COUNT (how many distinct active users, not requests), SUM (total GB transferred), and MAX (peak concurrent seats in a billing period, even if usage dropped afterward). Which one you pick changes what the customer is actually paying for: COUNT bills for activity, SUM bills for volume, MAX bills for peak commitment.


**Rate and invoice.** The aggregated metric gets a price applied (a flat per-unit rate, a graduated tier, or a volume discount) and rolls into the invoice at the end of the billing cycle. This whole pipeline is usually driven by a metering API: a product sends events in, the billing platform meters, prices, and invoices them out, without a human touching a spreadsheet in between. This is exactly the shape of the problem for[API products billing on usage](https://getlago.com/blog/api-billing-monetization) , where requests, tokens, or bytes are the unit and the metering has to keep pace with production traffic.


For the deeper architecture behind this (idempotency keys, aggregation at scale, rating engine design), see[the full playbook on architecting usage-based billing systems](https://getlago.com/blog/architect-billing-systems) .


## Metered billing vs. usage-based billing: what's the difference?


Usage-based billing is the pricing strategy: the decision to charge by consumption instead of a flat seat fee. Metered billing is the infrastructure underneath it: the system that actually measures usage accurately enough to bill on. You can't run usage-based pricing without some form of metering, but metering itself is a technical concern, not a pricing decision.


Take the same API product priced two ways. Priced per-seat, the invoice is $99/month regardless of how many requests a customer sends. Priced on usage, the invoice is $0.002 per request, and metered billing is the part of the system that counts those requests correctly, in real time, at whatever volume the customer sends them.


For the pricing-model side of this (which usage metric to pick, how to structure tiers, how usage-based pricing affects retention), see[the complete guide to usage-based billing](https://getlago.com/blog/usage-based-billing-system) .


## Metered billing vs. subscription billing


Subscription billing charges a fixed, recurring fee regardless of usage. The invoice is the same every month whether a customer barely touches the product or runs it constantly. Metered billing charges based on consumption, so the invoice total moves with usage.


Neither is strictly better. They solve for different things. Subscription billing gives customers predictable costs and gives the vendor predictable revenue, which is why it's still the default for products with steady, bounded usage. Metered billing fits products where cost genuinely scales with consumption: infrastructure, AI inference, API platforms, where a flat fee either overcharges light users or undercharges heavy ones.


Most companies don't pick one or the other outright. The common middle ground is hybrid: a flat base subscription that covers a usage allotment, with metered overage billing once a customer exceeds it. It's the structure behind most modern API and infrastructure pricing, and it's why "metered billing" and "subscription billing" are better understood as two mechanisms a pricing model can combine, not two competing categories.


## Do you need dedicated metering software?


If you're tracking a handful of usage dimensions at low volume, you can probably get away with logging usage in your own database and running a monthly script against it. That approach breaks down once you're handling high-volume events, need aggregation in real time rather than at month-end, or have to guarantee billing accuracy at a scale where a rounding error or a dropped event turns into a real revenue or trust problem.


At that point, the choice is build a metering layer in-house or adopt one as part of a billing platform. Building it yourself means owning idempotency, aggregation logic, and rating rules as a permanent piece of engineering surface area. Solvable, but it's plumbing your team maintains instead of product your team ships. Buying it means picking a platform that fits how your team actually works: some are closed, hosted-only SaaS, and that tradeoff gets more concrete once a vendor changes ownership. Take Orb,[now being acquired by Adyen](https://getlago.com/blog/orb-adyen-acquisition-lago-alternative) : teams evaluating usage-based billing platforms suddenly have to weigh a payments processor's roadmap alongside the metering feature set. Lago is open-source and self-hostable, which matters specifically for teams that want usage data to stay on their own infrastructure or need to customize pricing logic beyond what a vendor's UI exposes.


For the cloud-infrastructure-specific version of this build-vs-buy question (instance metering, compute billing, multi-tenant usage tracking), see[metered billing for cloud infrastructure](https://getlago.com/blog/metered-billing-for-cloud-infrastructure) .


## Metered billing in practice: real examples


**GitHub Copilot.** In April 2026, GitHub announced it was moving Copilot off a fixed monthly allotment of "premium request units" and onto GitHub AI Credits, a token-based consumption model where usage is calculated from actual input, output, and cached token consumption at each model's listed API rate, effective June 1, 2026. The change was a direct response to agentic coding sessions consuming far more compute than a flat per-request allotment could account for: a case study in what happens when usage stops being roughly uniform and a flat-rate model starts systematically over- or under-charging.


**Stripe.** Stripe ships metered billing as a native feature of Stripe Billing. Customers report usage via API, Stripe aggregates it against a subscription item, and the invoice reflects metered usage on top of (or instead of) a flat fee. It's a reasonable default if a team is already fully inside Stripe's payments stack and doesn't need usage data or pricing logic to live anywhere else, though[attempting to replicate a genuinely complex usage-based pricing model on Stripe Billing](https://getlago.com/blog/is-stripe-using-stripe) surfaces real limitations once percentage-based fees and multi-dimensional usage enter the picture.


**Mistral.**[Mistral bills for its frontier AI models](https://getlago.com/blog/mistral-billing) by converting API events into billable metrics with Lago, aggregating usage across dimensions like input versus output tokens and model type rather than a single flat per-token rate. It's metering with enough granularity to price each dimension of an AI product separately, not just count total tokens.


**Blacksmith.** Blacksmith runs compute-intensive CI/CD infrastructure, a workload where cost is inherently tied to usage, not seats. Using Lago to meter and bill that usage, Blacksmith grew to 700+ paying customers and eight figures in ARR in its first year live. It's a case where the billing layer wasn't a back-office afterthought. Accurate metering was a precondition for a pricing model that could scale with a compute-heavy product at all.


## Key takeaways


- Metered billing measures usage and bills after the fact. It's a mechanism, not a pricing decision on its own.
- Usage-based billing is the pricing strategy; metered billing is the infrastructure that makes it possible.
- Subscription and metered billing aren't opposites. Most real pricing models combine a flat base with metered overage.
- Dedicated metering software earns its place once usage volume, real-time aggregation needs, or accuracy requirements outgrow a homegrown script. Build vs. buy is a real tradeoff, not a foregone conclusion.
- GitHub's 2026 move to token-based Copilot billing and Blacksmith's compute-metered CI/CD platform are both concrete evidence that metered billing isn't theoretical. It's what usage-driven products end up needing once flat pricing stops matching actual cost.


[Lago meters usage as event data in real time](https://getlago.com/platform/usage-metering) , up to 1,000,000 events per second, and is open-source and self-hostable for teams that want that infrastructure on their own terms.
