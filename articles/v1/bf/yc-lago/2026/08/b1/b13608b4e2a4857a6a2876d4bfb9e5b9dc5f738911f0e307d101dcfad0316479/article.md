---
schema_version: "1.0.0"
document_id: "b13608b4e2a4857a6a2876d4bfb9e5b9dc5f738911f0e307d101dcfad0316479"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/events-based-billing"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T15:44:22.920553+00:00"
fetched_at: "2026-08-10T15:44:23.925845+00:00"
content_hash: "sha256:eeb6d5ef34a63a2b23e50fe4ffabc98af856521d94bcefe9b67369ded0a12aff"
---

# Event-Based Billing Explained: Examples & How to Build It

Ten years ago HubSpot switched part of its pricing to metered billing and its net revenue retention went from 70% to something close to 100%, without scaring off new customers.¹ That's the kind of number that gets a pricing team a seat at the strategy table. It's also, if we're honest, a secondary-source stat I couldn't trace back to HubSpot itself, so treat it as directional rather than gospel.


## **What event-based billing actually is**


Event-based billing, usage-based billing, metered billing for SaaS: three names for the same idea. You charge for what someone actually consumes, not for the right to use your product regardless of whether they touch it. Think electricity. Nobody pays a flat fee for power and hopes it roughly matches what they used. You pay per kilowatt-hour. SaaS companies doing this well charge per API call, per GB processed, per feature invoked, per transaction completed. If you want the fuller version of this,[Lago's guide to building a usage-based billing system](https://getlago.com/blog/usage-based-billing-system) goes deeper than I will here.


### **How it works, mechanically**


Four moving pieces. You capture the event when it happens. You aggregate raw events into something billable, a count, a sum, a rate. You run that through a pricing model to turn consumption into a dollar amount. And you generate an invoice from the total.[Lago's docs on event ingestion](https://getlago.com/docs/guide/events/ingesting-usage) walk through the capture side if you're building this yourself.


I'll skip a specific throughput number here. I went looking for the "processes a million events per second" claim that shows up in a lot of billing marketing copy, including an earlier draft of this piece, and couldn't source it to anything concrete. The honest version: modern billing platforms are built for high-throughput event capture, and the goal is invoicing that feels close to real time even when the pricing logic underneath is complicated. If you need the actual number for your evaluation, ask the vendor directly and get it in writing.


## **Three examples, because abstractions don't stick**


Stripe Radar screens every transaction for fraud, and each screening event is billable. Here's where an earlier version of this article went wrong: it described Radar as charging a flat $0.05 per transaction. That's not how Stripe actually prices it. Radar is sold as tiered subscriptions, Standard, Plus, Pro, with a pay-as-you-go option for teams that don't want to commit to a plan.⁵ The mechanic that matters for this article still holds: fraud screening is priced against volume of activity, not folded invisibly into a flat platform fee.


OpenAI charges by the token, input and output priced separately, more expensive models costing more per token. I'm not going to hardcode a specific GPT-4 rate here, because GPT-4 isn't even on OpenAI's current pricing page anymore \[VERIFY current model and rate before publishing\]. That's actually the point worth making: token pricing changes fast enough that any number in this article will be stale within a quarter. Check the live page before you quote one, and expect the same to apply to whatever example replaces this one next year.


Cloud infrastructure providers do the same dance at a bigger scale. CPU hours, gigabytes stored, network requests, each metered, each priced against a rate card that shifts by volume and tier.[Lago's writeup on metered billing for cloud infrastructure](https://getlago.com/blog/metered-billing-for-cloud-infrastructure) covers this pattern in more detail.


## **Building it, in four steps**


*Step 1, figure out what's actually billable.* Not every feature deserves its own meter. Survey customers on what they value, look at usage patterns for what's actually resource-intensive on your end, and map both against the infrastructure cost of running them. The features that are expensive for you *and* valuable to them are your billable events. Everything else is probably part of a base plan.


*Step 2, build the metering.* This is the part people underestimate. You need real-time capture that doesn't slow down your app, deduplication so a retried request doesn't get billed twice, and aggregation that scales as your event volume grows, because it will grow faster than you plan for. You can build this in-house, buy a dedicated platform like[Lago](https://getlago.com/platform) , or do some hybrid of the two. Most teams that go fully in-house find out the hard way that billing accuracy is a much bigger surface area than it looks from the outside.


*Step 3, price it.* Volume-based is the simplest: a flat rate per unit, easy to explain, easy to build.[Lago's docs on volume pricing](https://getlago.com/docs/guide/plans/charges/charge-models/volume) cover the mechanics. Tiered pricing, sometimes called graduated pricing, charges different rates as usage crosses thresholds, which rewards your biggest customers with better unit economics as they grow.[The graduated model docs](https://getlago.com/docs/guide/plans/charges/charge-models/graduated) are the reference if you're implementing this. Hybrid models bolt a usage component onto a base subscription, giving you revenue predictability from the base and upside from the usage, which is honestly where most companies actually land once they've tried pure usage pricing and found it too volatile for finance to plan around.


And that's worth sitting with for a second, because usage-based pricing isn't a free lunch. Customers can get hit with bill shock when usage spikes. Your own revenue gets harder to forecast. Sales reps who are used to quoting a flat annual number now have to explain a range. None of that means don't do it. It means you pair it with usage alerts, spending caps, and invoices that break down exactly what was charged and why, which is the next problem to solve, not a reason to stop here.


*Step 4, automate the billing.* Connect the usage data to invoicing, wire up payment collection, handle tax and currency if you're global, and give customers a way to see their own usage before the invoice lands. Nobody likes a surprise bill. Dashboards, usage alerts before someone hits a threshold, and downloadable usage reports all do the same job: they turn "why is my bill higher this month" into a question the customer can answer themselves before they email support.


### **Why bother**


Because it changes the shape of the relationship. Customers start small, scale usage as they get value, and the price grows with them instead of forcing an annual renegotiation. Companies see improvement in retention metrics when they make this switch.¹ And the market's moving this direction anyway: the 2025 Monetization Monitor found 59% of software companies expect usage-based approaches to take a bigger share of revenue, and buyers now prefer usage-based pricing over traditional subscriptions, 42% to 38%.⁸[Lago's playbook on architecting billing systems](https://getlago.com/blog/architect-billing-systems) is worth a read if you're the one who has to actually build the plumbing behind that shift.


## **Quick FAQ**


*Is event-based billing the same as usage-based billing?* Yes, same model, different name depending on who's selling it to you.


*Can I combine it with a subscription?* Yes, and most companies do. Hybrid pricing, a base fee plus usage on top, is more common than pure usage pricing in practice.


*What breaks first when you build this yourself?* Data accuracy. Deduplication, late events, reconciling usage against what actually got invoiced. It's unglamorous work and it's where in-house metering systems accumulate the most bugs, which is exactly why dedicated billing platforms exist.
