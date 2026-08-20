---
schema_version: "1.0.0"
document_id: "1d21b4cbb86463d84386eb62071ed9b0599f35569aafd34cb592afc3b8439f92"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/embedded-software"
published_at: "2026-03-26T16:59:46+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:06447f02317d3d50aef03b0ab2dd71e47c259e6321dbf47622fdd2e647beec93"
---

# Embedded software is the biggest growth opportunity for open source

When you link your bank account to Venmo, you're not using Venmo's technology, you're using Plaid. An Uber sends you a ride notification, that originates with Twilio. Maps in Strava, Doordash or Instacart? Powered by Mapbox.


These are some of the most successful software companies ever built, and most of their end users have never heard of them. They don't compete for the spotlight — they disappear inside other people's products.


In hardware, this model already has a famous name: TSMC. The Taiwanese fab manufactures the chips inside nearly every device you own, from iPhones to Teslas, yet most people couldn't name them. It's physically impossible for most companies to make advanced microchips, so they buy from the company that can.


The same thing is happening in software — and it's accelerating. It's less visible since there are no obvious supply chains, no suspiciously similar-looking products offered by multiple brands. But it's a great business model, especially for open-source companies. It's called OEM or embedded software.


**Every software company wants the spotlight. The money is in being the invisible layer underneath.**


## The invisible software supply chain


You've almost certainly used embedded software without knowing it. The pattern is everywhere: a product you interact with daily is quietly powered by an open-source project you've never heard of.


**The most valuable software is the software nobody knows they're using.**


Here are real examples:


This pattern is everywhere in software. Siemens ships "Siemens Dashboard Designer" to industrial manufacturers — it's Grafana underneath. Thousands of SaaS products rely on Redis for caching and real-time data without their users ever knowing. Databricks built a multi-billion dollar platform on top of Apache Spark. And when[PayPal](https://getlago.com/blog/paypal-x-lago/) needed a billing backbone, they came to us.


The pattern extends further: Plaid powers the bank-linking experience inside thousands of fintech apps, Twilio handles communications for platforms like Uber and Airbnb, Persona offers identity verification. The list keeps growing because embedded software solves a fundamental problem: building category-specific infrastructure from scratch is brutally expensive, and most companies shouldn't try.


## Why billing can't be rented


But here's the thing that makes embedded billing different from embedded analytics or embedded dashboards: **you can't rent the billing layer.**


**An analytics outage is an annoyance. A billing outage is a catastrophe.** When Synapse, the embedded banking platform,[collapsed in 2024](https://www.yalejournal.org/publications/the-synapse-collapse) , $265 million in customer funds were frozen. If you were offering banking through Synapse, your customers didn't just lose a feature — they lost access to their money. The trust damage was existential.


This is the core argument for why embedded billing has to work differently. If you're going to offer your customers a financial backbone — billing, invoicing, etc. — you need to control *everything* . The hosting. The uptime. The data. The code. You can't afford a dependency you don't own.


Closed-source embedded products like Sisense can work for analytics: you build on their APIs, accept their hosting, and if there's an outage, it's inconvenient. But **billing is the financial nervous system of your product.** A dependency there isn't a trade-off — **it's a liability.** This is why embedded billing customers are among the most demanding: they need to hand their engineers the full source code, run it on their own infrastructure, and modify it to fit their stack. That requires something (almost) only open-source software can deliver: handing over your entire codebase, being hosting-agnostic, and having zero API dependencies.


## Why we launched Lago Embedded


Today, we view Lago Embedded as one of the most promising growth vectors for Lago. But we said no the first time someone wanted to use Lago to offer billing as part of their own product.


At the time, it didn't make sense. The prospect was tiny, Lago not a mature product and billing wasn't a well-known category. We didn't give it much thought at a time. It was just an odd request that didn't work out. We lost track of the concept in the busyness of startup life.


Lago grew a lot over the next few years. We signed enterprise customers (and built enterprise features like multi-tenancy), leaned into our open-source DNA with self-hosted offerings and cemented our foothold in the billing space.


The billing space itself also transformed: As AI introduced marginal cost to software products, pricing became hard again. Companies everywhere scrambled to introduce credits, usage-based, overages and other pricing strategies that required more complex billing than flat subscriptions.


**Like a nerdy kid in school whose time to shine comes when the athletes forgot their homework, complex billing was in demand.**


Both Lago and our category had changed when PayPal approached us. When PayPal came to us, the ask forced us to solve problems we'd never had to solve before — millions of events per second, on-the-fly org provisioning, global scale.[So we said yes](https://getlago.com/blog/paypal-x-lago) .


With embedded (also called OEM), Lago becomes part of our customers' product offerings. Our customers offer a billing product powered by Lago to *their* customers. This means our customers get the entire source code, which lives in their own repo, on their own infrastructure. They can extend, modify or integrate our product as they want to. As an example, a vibe-coding platform like Lovable could offer its users Lago-powered billing so they can monetize their creations on the platform with credits, usage-based pricing or other complex pricing strategies.


## The engineering reality of embedded products


Open-source companies already have the muscle for this: they're accustomed to handing over codebases, supporting self-hosted deployments, and operating without API lock-in. But that doesn't mean you can take your Github repo and offer it as an embedded product.


### Partners will customize


While most SaaS customers buy your product and use it as-is, embedded customers are more likely to use the product in their own repo and customize it. They usually do this for one of two reasons:


The first is that they want to offer a simplified feature set. A vibe-coding company might want to offer complex billing and a variety of pricing strategies, but not expose some of Lago's more complex features (multi-entity billing, custom SQL billable metrics etc.) to users who could get overwhelmed by it.


The second is deeper integration with the rest of their product, internal tools or similar things. A payment provider might want a deeper integration with their own payment services, but not offer Lago's other PSP integrations.


All of this is enabled by licensing. The AGPL license ([which Lago is under](https://getlago.com/blog/open-source-licensing-and-why-lago-chose-agplv3) ) means people can use Lago commercially to power their own billing, but cannot commercialize it themselves — they cannot offer a Lago-powered product without a commercial agreement. This is what makes embedded one of the best growth levers for open-source software: **the license protects the business model while the open codebase gives customers the control they need.**


## Embedded software unlocks new markets


An OEM product expands the target market for software companies. Customers that would never buy a given product (or already have a satisfying solution in place) might now do so. This also typically means bigger, longer-lasting deals: Companies who launch new products to round out their feature set tend to be large, and they're unlikely to ship a product and sunset it immediately afterwards.


**The best software companies of the next decade won't have users. They'll have users' users.**


The companies that quietly power the next decade of software infrastructure won't be household names. **They'll be the Plaids, the Twilios, the TSMCs of their category — invisible, essential, and compounding.**
