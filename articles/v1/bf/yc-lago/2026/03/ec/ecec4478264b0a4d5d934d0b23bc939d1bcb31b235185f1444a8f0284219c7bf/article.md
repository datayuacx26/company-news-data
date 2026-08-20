---
schema_version: "1.0.0"
document_id: "ecec4478264b0a4d5d934d0b23bc939d1bcb31b235185f1444a8f0284219c7bf"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/vibe-coding-platforms-monetize-their-users-cant"
published_at: "2026-03-11T10:55:51+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:57:37.318038+00:00"
content_hash: "sha256:0bc2d572a7e70c779b3d0fbdc9d40a6415c4175bbd7af27e1ef238662db3eb6e"
---

# Vibe-coding platforms monetize. Their users can't.

Lovable crossed[$200M ARR in under a year](https://techcrunch.com/2025/11/19/as-lovable-hits-200m-arr-its-ceo-credits-staying-in-europe-for-its-success/) . Replit charges up to $200/month per seat.[Base44 got acquired by Wix for $80M](https://www.wix.com/press-room/home/post/wix-acquires-base44) .


These platforms know how to monetize. Their users don't.


## The Shopify lesson


The obvious pushback: who cares? Most vibe-coded apps are toys. Mostly true.


But in 2025,[merchant solutions made up over 75% of Shopify's total revenue](https://www.macrotrends.net/stocks/charts/SHOP/shopify/revenue) . Merchant solutions = payment processing, capital lending, shipping. Everything Shopify earns when merchants transact through the platform. Not subscriptions to the website builder. The cut on money flowing through it.[Shopify Payments processes roughly 62-64% of all GMV on the platform](https://www.marketplacepulse.com/stats/shopify-gross-payments-volume) . That take rate is what turned a website builder into a $100B+ company.


[Wix's transaction revenue hit $255M in 2025](https://www.globenewswire.com/news-release/2026/03/04/3249040/0/en/Wix-Reports-Fourth-Quarter-and-Full-Year-2025-Results.html) , growing 19% YoY. The fastest-growing line in the company.


The pattern: platforms that capture the transaction layer build stickier businesses than platforms that only charge for tooling. Users who make money on your platform don't leave. Users who don't, eventually do. And the platform itself generates more revenue per user by taking a cut on transactions than by only charging a flat subscription.


Wix seems to get this. Buying Base44 expands the surface where users will need commerce infrastructure. The question is whether vibe-coding-native platforms see it too.


## The Stripe wall


Jason Liu named it well in his essay on[the vibe coder economy](https://jxnl.co/writing/2025/06/12/lovable-monetization-and-the-vibe-coder-economy/) : creators hit "the Stripe wall."


You can vibe-code an app in a weekend. The moment you try to charge for it: Stripe test mode, webhook config, refund flows, subscription lifecycle. Building felt like magic. Monetization feels like accounting.


His fix is telling: instead of charging creators credits, Lovable should take a cut of what creators earn. Because pricing setup is too hard for creators to do themselves, and this would align interests: platforms earn when creators earn.


But the wall isn't Stripe checkout. Wiring up a flat $9/month subscription is doable today. Ugly, but doable.


The wall is pricing flexibility. An image generator app needs per-generation pricing. An API wrapper needs metered billing with overages. These are standard pricing models for modern software. But on a vibe-coding platform, anything beyond "one price, one button" means custom webhook handlers, proration edge cases, state sync debugging. The no-code promise breaks at the exact moment the creator needs it most.


The evidence is directional, not definitive, but consistent. On Base44's feedback board, a[baked-in payment solution](https://feedback.base44.com/p/baked-in-payment-solution-no-more-stripe-payment-setup-pain) is one of the top requests. On Replit's forum,[creators report payments going through but the app not registering it](https://replit.discourse.group/t/stripe-monthly-subscription-functionality/6821) . Webhooks drop, sessions time out. On HN,["Is Lovable getting monetization wrong?"](https://news.ycombinator.com/item?id=44377495) drew real debate.


## Why Stripe Billing doesn't close the gap


"Just use Stripe Billing." Fair response. Great product for a SaaS company managing its own subscriptions.


Different problem here. You're not billing your own customers. You're enabling thousands of creators to bill theirs. The billing system needs to be configurable through a UI by non-technical users. It needs to handle whatever pricing model a creator invents (usage-based, credits, hybrid, tiered) without re-wiring.


There's also an architecture gap. Billing runs in the background: renewals at 2am, retry logic on failed charges, continuous usage metering. That means your app needs something always listening. A persistent server or at minimum a serverless function with a stable endpoint. Replit offers always-on deployments. Lovable deploys to Vercel with serverless functions. Technically solvable. But the creator still needs to sync Stripe's subscription state with their app's access logic. Who gets what feature, when. What happens when a payment fails. Stripe Billing handles the payment retry. It doesn't tell your app to revoke access.


Stripe Billing only works with Stripe payments. If a platform wants to offer creators a choice of PSPs, or switch providers down the line, that's a hard lock-in on both the platform and every creator on it. And Stripe's own answer to usage-based billing, acquiring Metronome, tells you something. Metronome is built for finance teams with dedicated engineering resources managing their own billing. Different thing from embedding billing inside a platform where thousands of non-technical creators each need to configure their own pricing through a UI.


## What should happen (biased take)


I'll be direct: Lago would benefit from platforms solving this the way I'm about to describe.


Three paths.


**Build a billing product in-house.** Building billing as an internal tool is already hard: proration, tax, dunning, compliance, usage metering. Stripe Billing, Zuora, and others exist because billing is a product in itself. Now multiply that by thousands of creators who each need to configure their own pricing. That's not building billing. That's building a billing platform. Multi-year commitment.


**Extend what payment processors already offer.** None of these processors are known for billing flexibility. There's a reason[Stripe paid $1B for Metronome instead of fixing billing themselves](https://getlago.com/blog/why-stripe-paid-1b-for-metronome-instead-of-fixing-billing) . And you inherit their lock-in. Worse, you force every creator on your platform into that same lock-in.


**Embed a billing engine.** The usual objections to embedding: black box, dependency, lack of customizability. This is where open-source changes the equation. Something like Lago: full code transparency (inspect every line), runs on-prem if you want, extensible because you can customize the code and build your own version on top, payment-processor agnostic. Designed to be white-labeled inside a platform so creators configure pricing through a UI, not code. That's[the infrastructure underneath AI billing](https://getlago.com/blog/ai-billing-infrastructure) platforms need if they want creators to succeed.


Whether it's with Lago or not, the platform that makes "set up usage-based pricing" as easy as "deploy your app" will have cracked something the last generation took a decade to learn.
