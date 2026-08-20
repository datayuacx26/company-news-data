---
schema_version: "1.0.0"
document_id: "c81b04244878128c69b42df551c64cd489db7845355327e306905c4e2d8189bd"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/the-second-half-of-pricing"
published_at: "2026-08-07T12:03:40+00:00"
first_seen_at: "2026-08-07T13:03:26.788735+00:00"
fetched_at: "2026-08-07T13:03:28.322897+00:00"
content_hash: "sha256:5dcf44c71e570386afeb699843613ab10ca2f35302a4149301a4dfd93355deb3"
---

# The AI pricing shift nobody priced correctly

A monthly invoice can explain what happened. It cannot decide whether the next agent run should start.


That was fine for subscription software. A plan controlled access. The price was fixed. Finance reviewed the result monthly or annually, long after the product had been used.


AI does not move on that cadence. An agent can burn through thousands of model calls before finance opens a dashboard. A routing change can move the same workflow from a cheap model to an expensive one. Longer context, another tool call, or a higher retry rate can change the cost of the same customer behavior overnight.


By the time the invoice exists, the product has already spent the money.


The shift to usage-based, outcome-based, and hybrid pricing fixed the visible half of pricing: what the product sells and how the charge is calculated.


The second half happens while the product is being used.


## The second half of pricing


Pricing now has two jobs.


The first is commercial. Decide what the customer buys: access, usage, an outcome, or some combination of the three.


The second is operational. Decide what the customer can do next based on the contract, wallet, entitlement, accumulated charge, and willingness to pay.


For an AI product, that decision belongs in the usage path. Before a run, the system should estimate cost and check the customer’s allowance. While it is happening, it should meter consumption, update the forecast, and compare it with thresholds. When a limit is close, the product needs a policy: ask for approval, use a cheaper model, draw from an overage pool, or stop.


The invoice comes after. It reconciles those decisions with the raw events.


This is already becoming a massive market. In[Why Stripe may pay $10B for a router everyone is building](https://getlago.com/blog/why-stripe-may-pay-10b-for-a-router-everyone-is-building) , the scarce asset is not the router. It is the position in the live request path.


Stripe approaches it from payments and billing. Cursor and Runway already see the user’s intent. Snowflake sees the workload beside the customer’s data and consumption. Ramp starts from the budget. Kong is combining the gateway with metering and entitlements.


They are moving toward the same point: the moment an AI request can still change the model, the cost, what the customer is allowed to do, and what they will pay.


That is also why[Anthropic’s split between interactive and agentic usage](https://getlago.com/blog/anthropic-finger-based-pricing) was more than a pricing change. It changed what different kinds of usage were allowed to consume. The meter became part of the product contract.


## The risk moved


Software pricing spent the last decade trying to get closer to value. Seats gave way to usage. Usage blended with subscriptions. AI made[outcome-based pricing](https://getlago.com/blog/outcome-based-pricing) credible because a product could count completed work, not just the people who logged in.


The direction was right. But better alignment was often presented as simpler pricing.


It is not simpler. It moves risk.


With a fixed subscription, the vendor absorbs consumption risk. A customer can use far more than expected and still pay the same price.


With usage-based pricing, more of that risk moves to the buyer. The vendor protects its margin. The customer now has to forecast a bill from API calls, tokens, model mix, agent steps, storage, or anything else that moves independently of headcount.


Outcome-based pricing moves a different risk back to the vendor. The buyer pays after value is proven. The vendor now has to define and defend the result. Was the ticket really resolved? Did the AI create the outcome, or merely participate in it?


Hybrid pricing divides these risks through the contract. It does not remove them.


The commercial upside is real.[Maxio’s 2025 pricing benchmark](https://www.maxio.com/resources/2025-saas-pricing-trends-report) found that companies using hybrid pricing had the highest median growth rate in its sample, at 21%. A base commitment protects revenue. A variable component lets the contract expand with customer value or cost to serve.


AI makes that connection necessary. Two customers on the same plan can have completely different costs. One sends short prompts to a small model. Another runs agents for hours, retries steps, calls tools, and routes hard tasks to a reasoning model.[The AI margin cliff](https://getlago.com/blog/ai-margin-cliff) starts when that variable cost grows faster than the contract attached to it.


Usage pricing helps the vendor see and recover the difference. It can create a new problem for the buyer if the only moment they see the difference is on the invoice.


[Zylo’s 2026 SaaS Management Index](https://zylo.com/blog/ai-cost/) found that organizations spent an average of $1.2 million on AI-native applications, up 108% year over year. In the same research, 78% of IT leaders reported unexpected charges tied to consumption-based or AI pricing.


That is not proof that usage pricing failed. It is proof that alignment and predictability are separate product problems.


## Predictability is not a flat price


Buyers say they want predictable pricing. Vendors hear “fixed price.” Those are not the same thing.


A prepaid wallet with a visible depletion rate can be predictable. So can a commitment with live usage against it, an allowance with approval before overages, or a forecast that updates when the model mix changes.


The test is what happens before the next action. Can the customer understand what it may cost, where they stand, and what the product will do at the limit?


That is a product experience, not a sentence in an order form.


Showing that a customer consumed 14 million tokens is technically transparent. It may still be useless. Tokens do not tell finance which workflow caused the spike, why a different model was used, or whether the same outcome could have cost less.


The billable unit has to trace back to product behavior. The forecast has to use the same pricing logic as the invoice. The threshold in the product has to match the contract finance signed.


Otherwise, transparency is just a detailed surprise.


The industry did not price AI incorrectly because it chose[usage-based pricing](https://getlago.com/blog/usage-based-pricing) , outcomes, or hybrid contracts. Those models solve real problems.


It priced the charge and left the decision for later.
