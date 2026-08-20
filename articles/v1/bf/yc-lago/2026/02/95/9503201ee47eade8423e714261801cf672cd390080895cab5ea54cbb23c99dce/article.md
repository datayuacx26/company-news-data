---
schema_version: "1.0.0"
document_id: "9503201ee47eade8423e714261801cf672cd390080895cab5ea54cbb23c99dce"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/paypal-x-lago"
published_at: "2026-02-13T12:02:46+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:57:41.767482+00:00"
content_hash: "sha256:4df932f9e93a36fdf2af6d5f4700d94b39cdded108c5acdd3e5aa379a169f407"
---

# Why PayPal uses Lago for complex billing at enterprise scale

When PayPal wanted to offer billing to merchants on its platform, they didn’t want to build complex billing from scratch and maintain it forever. That’s why they picked Lago.


In PayPal’s integrated finance ecosystem, billing was the missing piece.


## Why It Matters


If you're evaluating billing infrastructure in a regulated industry like financial services, PayPal's decision reveals something important: the market has changed.


First, complex billing is becoming more important: AI is increasing marginal costs and compressing profit margins. Pricing is getting more complex and changes more often.


Second, open-source isn't just for scrappy startups anymore. You can have complete code transparency, self-hosted deployment and strategic control without sacrificing reliability or scale.


Third, as more companies need billing systems, more and more vendors want to offer billing to their customers. But billing is notoriously hard to build. This is why embedded billing like Lago is becoming more popular.


Now, instead of spending 2-3 years reinventing the wheel, you can launch in months and still customize everything.


## The Technical Challenge


PayPal's requirements weren't trivial:


- Metering events at extremely high scale
- Real-time usage aggregation of events
- Multi-currency, multi-entity billing across 200+ markets
- Sub-second latency for usage tracking


Existing solutions couldn't cut it: Proprietary SaaS platforms offered black-box billing logic without self-hosting options. Building in-house meant a years-long timeline and an ongoing maintenance burden.


Lago offered something different:


- Compliance and security via self-hosting (enabled by open source), combined with enterprise reliability and architectural flexibility.
- Support for the pricing models PayPal’s merchants are most likely to succeed.
- Open-source transparency that shows how the billing calculations worked, which turns out to be non-negotiable when those calculations affect millions in revenue.


## What PayPal actually gets


Here's the thing about embedded licensing that most case studies miss: it's not just about saving development time. It also ensures security, preserves optionality and creates an ongoing partnership.


PayPal gets Lago's proven foundation and continuous product innovation. But unlike other embedded billing customers who are constrained by vendor APIs, PayPal can customize at the code level. They can modify the source, build differentiated features, and move faster than competitors stuck with closed platforms.


The infrastructure itself is production-grade—processing over 1 million events per second with 99.9% uptime. All the code is inspectable, which matters for compliance. The deployment is self-hosted, so sensitive financial data never leaves PayPal's security perimeter. And because it's open-source, there's no vendor lock-in.


Strategic flexibility, in other words. The kind that gives you exit options and negotiating leverage.


## The Bottom Line


PayPal's choice signals where the billing market is headed. Companies don't want to choose between speed and control anymore—they want both. They don't want to build billing infrastructure from scratch, but they also won't accept black-box solutions that constrain their product roadmap.


Embedded open-source infrastructure solves that tension. You get the speed of a ready-made solution with the flexibility of building in-house. For companies processing millions of transactions across global markets, that combination isn't just nice to have. It's how you stay competitive.
