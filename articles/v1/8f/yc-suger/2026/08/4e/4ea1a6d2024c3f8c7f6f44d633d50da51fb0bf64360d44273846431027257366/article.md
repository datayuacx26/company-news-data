---
schema_version: "1.0.0"
document_id: "4ea1a6d2024c3f8c7f6f44d633d50da51fb0bf64360d44273846431027257366"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/free-trials-on-cloud-marketplaces/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:62c5fe9c73e9f99105e8fac0bdd5c49563d3a64b5a1b3123e0242441f415eec9"
---

# Free Trials on Cloud Marketplaces: An ISV Guide

*A cloud marketplace free trial is time-limited or usage-limited free access to a product, granted through the marketplace listing, that converts into a paid subscription without the buyer starting a new purchase. The entitlement, the clock, and the conversion are all managed by the marketplace — not by your billing system.*


---


Adding a free trial to a marketplace listing looks like a checkbox, and teams treat it like one. Then the first trial expires, nobody’s system notices, and a prospect who was ready to buy spends a week in support instead.


The gap is structural. Your website trial is something your product owns end to end: you decide who gets it, how long it lasts, what happens at the end, and when to email them. A marketplace trial hands most of that to the cloud. What you keep is provisioning, the in-product experience, and the conversion conversation — and if you haven’t wired those to the marketplace’s signals, the trial runs without you.


Here’s how the trial constructs work, and what to decide before turning one on.


---


## **What is a free trial on a cloud marketplace?**


A cloud marketplace free trial is free access to your product granted from the listing, with an entitlement the marketplace tracks and an end point after which the buyer either converts to a paid subscription or loses access.


The important word is *entitlement* . A marketplace trial creates a real entitlement record in the cloud’s system, and your product is expected to honour it, provision against it, and stop when it ends. That is why a trial is a systems project rather than a marketing setting.


The upside is the conversion path. A buyer who trials through the marketplace and then subscribes never leaves the procurement route they already have — the purchase lands on their cloud bill and can count toward committed spend, which is the same advantage every other marketplace transaction carries.


---


## **The shapes a trial can take**


**Time-limited** — free access for a fixed period, after which the subscription converts or ends. The default, and the right one when your value is obvious within days.


**Usage-limited** — a free usage allowance that gets consumed, common on usage-based products where value tracks consumption rather than calendar time. Better than a clock when your product is used in bursts.


**Feature-limited** — a permanently free tier with capabilities held back. This is a *product* decision rather than a marketplace one, and worth separating from the trial conversation entirely.


**Test drive** — Microsoft’s distinct construct: a pre-configured demonstration environment a prospect can explore without deploying anything into their own tenant. It is a demo mechanism, not an entitlement, and it converts differently.


Most teams should start with the shape their product already uses direct. A marketplace listing is a bad place to run your first experiment in trial design.


---


## **How the clouds handle it**


AWS Marketplace Microsoft Google Cloud Marketplace


**Trial on SaaS listings** Yes — on SaaS contracts and on SaaS usage-based pricing products Yes, configured with the offer in Partner Center Yes, configured with the listing in Producer Portal


**Trial on image / VM listings** Yes Varies by offer type Varies by listing type


**Demo-environment option** — **Test drive** —


**Who tracks the entitlement** The marketplace The marketplace The marketplace


**Who provisions the customer** You You You


**Conversion path** Trial entitlement converts to a paid subscription Trial converts to the paid plan Trial converts to the paid plan


The row that matters is the second from the bottom. Every cloud tracks the trial; every cloud expects **you** to provision, honour, and end it. A trial that your product cannot distinguish from a paid subscription is a trial that never ends.


Confirm the current trial options for your specific offer type in each cloud’s own console before you commit to a launch date — the supported combinations move, and they vary by listing type more than by cloud.


---


## **The AWS eligibility rules that catch people out**


AWS publishes constraints that shape both your funnel and your support load:


- **One trial per AWS account, per product.** A buyer’s AWS account can use a free trial for a given SaaS product once. A prospect who trialled last year and comes back cannot simply trial again.
- **Free usage isn’t shared across linked accounts.** In an AWS organization, the free usage amount granted during a trial is not pooled across linked accounts.
- **But linked accounts can each create their own trial.** Different linked accounts under a single payer account can create their own individual trials.


Read together, these decide what happens when a large enterprise “tries” your product. Different teams inside the same customer can each start a trial from their own linked account — which is either healthy bottom-up adoption or an accidental way to run a long free pilot, depending on whether anyone on your side is watching. Track trials by customer, not by account, or you will not see the difference.


---


## **What should happen when a trial ends**


Four decisions, all of which have to be made before launch because the marketplace will not make them for you.


**1. Does access stop, degrade, or continue?** A hard stop is honest and creates urgency. A silent continuation trains buyers that the paid plan is optional. Pick one deliberately.


**2. Who is told, and when?** The marketplace signals the entitlement change. Whether a human at your company knows is entirely up to how you consume those events. A trial expiring with nobody notified is the most common failure of the whole motion.


**3. Does the trial convert automatically, or does someone sell?** Self-serve conversion suits smaller deals. Above a certain value, the right move at day 10 of a 14-day trial is a private offer, not an email. That transition — from trial to negotiated offer — is where marketplace trials actually earn their place in an enterprise motion, and[what buyers see when you send a private offer](https://www.suger.io/resources/blog/what-buyers-see-when-you-send-a-private-offer/) covers the receiving end.


**4. What happens to their data?** Retention after an unconverted trial is a policy question your privacy documentation probably already answers. Make sure the marketplace path is covered by the same answer.


---


## **Where marketplace trials break**


**Provisioning lag.** The buyer starts a trial and waits for access. Every hour there is spent on your onboarding rather than your product.


**No expiry handling.** Your product doesn’t act on the entitlement ending, so trials become indefinite free usage.


**Metering that ignores the trial.** Usage-based products that meter during a free trial can produce charges nobody expected. Trial usage and billable usage must be distinguishable in your metering logic.


**No handoff.** A high-value trial goes unnoticed by sales because trial entitlements land in a marketplace console nobody watches daily.


All four are the same root cause: the marketplace holds the state, and your systems aren’t subscribed to it. Suger keeps marketplace entitlements — trial and paid — in one place, with the events that change them available to your workflows and CRM, so a trial starting, expiring, or converting is something your team acts on rather than discovers.[Billing and metering](https://www.suger.io/platform/billing-metering/) covers entitlements and usage;[workflow automation](https://www.suger.io/platform/workflows/) covers acting on the events.


For the wider growth motion around trials,[how to achieve PLG growth on AWS Marketplace](https://www.suger.io/resources/blog/how-to-achieve-plg-growth-on-aws-marketplace/) covers the funnel that a trial sits inside.


---


## **Frequently asked questions**


**What is a cloud marketplace free trial?** Free access to your product granted through a marketplace listing, with an entitlement the marketplace tracks and an end point after which the buyer converts to a paid subscription or loses access.


**Does AWS Marketplace support free trials?** Yes, on SaaS contracts and on SaaS usage-based pricing products, as well as image-based listings. Each AWS account can use a free trial for a given SaaS product once.


**Can different teams at the same customer each start a trial?** Yes. Different linked accounts within a single payer account can create their own individual trials, and free usage granted in a trial isn’t shared across linked accounts.


**What happens when a marketplace free trial ends?** That’s your decision, not the marketplace’s. The entitlement changes state; whether access stops, degrades, or continues depends on how your product responds to that signal.


**Should a trial convert automatically or trigger a sales conversation?** Below a certain deal size, self-serve conversion is right. Above it, the better move mid-trial is a private offer, which is also how the purchase draws down the customer’s committed cloud spend.


**Do free trials interfere with usage metering?** They can. Usage-based products need to distinguish trial usage from billable usage in metering logic, or a trial can generate charges the buyer didn’t expect.


---


## **Takeaways**


- The marketplace owns the trial entitlement; you own provisioning, expiry behaviour, and conversion. Wire your systems to its events before launch.
- Choose the trial shape your product already proves value with. A listing is a poor place to experiment.
- Know the AWS rules: one trial per account per product, free usage not pooled across linked accounts, but each linked account can start its own.
- Decide what happens at expiry — stop, degrade, or continue — deliberately, and make sure a human is notified.
- Above a certain deal size, the right mid-trial action is a private offer, not an automated email.


---


A trial is an entitlement before it is a marketing tactic. See how[billing and metering in Suger](https://www.suger.io/platform/billing-metering/) tracks trial and paid entitlements across every marketplace you sell on.
