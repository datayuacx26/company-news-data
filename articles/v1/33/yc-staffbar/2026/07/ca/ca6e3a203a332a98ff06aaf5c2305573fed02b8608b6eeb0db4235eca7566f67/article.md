---
schema_version: "1.0.0"
document_id: "ca6e3a203a332a98ff06aaf5c2305573fed02b8608b6eeb0db4235eca7566f67"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/external-checkout-a-b-testing-and-trial-toggles-confirmed-apples-rules-for-ios"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:5a0af69d415b7e41df9e52616683f749c9dfc4dbe11ff8e263942f89eea2e461"
---

# External checkout, A/B testing, and trial toggles: confirmed Apple's rules for iOS

Superwall recently met with someone senior at Apple to ask questions developers have been asking for years — what's actually allowed on the App Store when it comes to paywalls, pricing experiments, and external checkout links.


The App Store Review Guidelines are intentionally subjective. Apple doesn't take an official stance on a guideline unless they update it. Some topics may never enter the official guidelines — that doesn't mean they're allowed. Behind the published text, there's a layer of internal policy that isn't written down anywhere and is subject to change. The guidelines are the tip of the iceberg.


This article is based on[Jake Mor's thread on X](https://x.com/jakemor/thread/2062199338688795117) summarizing that conversation. These aren't official guidelines — they're direct answers from a senior Apple contact on four specific topics. Here's what we learned.


## App2Web: web checkout links are allowed in the US


> A web checkout link can be used in addition to — but not as a replacement for — in-app purchase in the United States.


The rules:


1. Linking out is 100% allowed for any reason in the US.
2. The link must open Safari, not an SFSafariViewController or in-app browser. Apple has made exceptions, but Safari is the expected behavior.
3. This is not a loophole — it's covered by[Apple's Guideline 3.1.1(a)](https://developer.apple.com/app-store/review/guidelines/#business) , which allows US storefront apps to include buttons, external links, or calls to action for purchase methods other than in-app purchase.


In practice, this means a paywall can show both IAP and Stripe products. A user taps the Stripe product, Safari opens, they check out through Stripe, and get redirected back to the app.[Superwall's App2Web flow](https://superwall.com/docs/web-checkout/web-checkout-direct-stripe-checkout) handles this end to end.


## Transaction abandon: one offer is fine, nagging is not


> Transaction abandon is when a user starts a purchase, cancels it, and is then shown another offer.


Apple's position:


- Showing any offer after a cancelled transaction is allowed — any price, any product, any type of deal.
- What's not allowed is looping the user — nagging them repeatedly, resurfacing the offer in every session, or creating a flow where they can't escape the offer without buying.


One tasteful offer = allowed. Relentless pestering = manipulative flow = rejected. It comes down to execution.


This is subjective to the reviewer. If it feels like a genuine alternative — a lower-priced plan, a different billing cycle — it's fine. If it feels like pressure, it's not. Superwall has[a guide on building abandoned transaction paywalls](https://superwall.com/docs/dashboard/guides/tips-abandoned-transaction-paywall) that stays on the right side of this line.


## Free trial toggles: banned entirely


> Free trial toggles on paywalls are not allowed. Full stop.


The reason: too many developers used them as a dark pattern. The typical abuse looked like this:


- Product A: monthly, no trial.
- Product B: yearly, with trial.
- User selects Monthly, thinks they're paying monthly.
- User toggles "Free Trial" — thinks the trial is now enabled on monthly.
- What actually happened: the product selector silently changed to yearly. The user checks out thinking they got a trial on their monthly plan.


This was too hard for reviewers to distinguish from a legitimate, honest free trial toggle. So Apple banned the UI pattern entirely — not because every implementation was deceptive, but because too many were.


If an app needs to offer a trial, the trial should be attached directly to the product and clearly described. No toggle that swaps the underlying product.


## Remote paywall updates and A/B testing: allowed and encouraged


> Remote paywall updates and A/B testing are 100% allowed and encouraged. Apple wants developers to win.


But — shipping a non-compliant paywall is not allowed, regardless of how it got there. A paywall pushed via a remote update is held to the same standard as one submitted in a binary.


- If a remote update breaks the rules, the app gets flagged.
- If it happens repeatedly, the app gets banned.


The risk is on the developer. The tool doesn't matter — the paywall does.


## The bigger picture: push boldly, stay honest


The largest takeaway from the conversation: Apple's guidelines are a living, breathing document. Reviewers are human. A lot is up for interpretation — intentionally.


Apple told us they want developers to push against the guidelines. That's how new patterns emerge. Crypto apps, vibe coding tools, new monetization flows — many started in a gray area before the guidelines caught up.


When a developer pushes too far, Apple warns first. When enough developers hit the same wall, Apple updates the rules. The cycle is deliberate.


Build boldly. Stay clear and honest with your customers. And when in doubt — reach out. Superwall has a direct line to Apple and can help get a real answer.


---


The original thread and the full conversation is in[Jake Mor's post on X](https://x.com/jakemor/thread/2062199338688795117) . These aren't official guideline updates — they're answers from a direct conversation with Apple, and they're the clearest signal available on how these rules are interpreted today.


If you're on Superwall — these answers are already built into how the product works.[App2Web](https://superwall.com/docs/web-checkout/web-checkout-direct-stripe-checkout) ,[transaction abandon paywalls](https://superwall.com/docs/dashboard/guides/tips-abandoned-transaction-paywall) , and[A/B testing](https://superwall.com/docs/dashboard/dashboard-campaigns/campaigns-starting-an-experiment) all follow the patterns Apple described.


If Superwall is new to you —[create a free account](https://superwall.com/sign-up) and see how it works. Questions? Reach out — we'll help.


## FAQ


Can I add a web checkout link to my iOS paywall in the US? Yes. Apple's Guideline 3.1.1(a) explicitly permits US storefront apps to include buttons, external links, or calls to action for purchase methods other than in-app purchase. The checkout link must open Safari, and you must keep IAP available alongside it. Removing IAP entirely and replacing it with web checkout is not permitted.


What counts as "nagging" after a cancelled transaction? Showing one offer after a cancelled transaction is fine. Repeatedly resurfacing that offer across sessions, or building a flow where the user cannot exit without buying, is the behavior Apple flags as manipulative. A single, clearly escapable offer is the safe pattern.


Why did Apple ban free trial toggle UI patterns? The toggle was widely abused to silently switch the selected product from a monthly plan to an annual one when the user activated the "trial." Because the deceptive version looked identical to an honest one, Apple banned the toggle pattern altogether. Trials should be attached directly to a product and described clearly in the product label.


Can we push paywall updates remotely without resubmitting to the App Store? Yes, and Apple encourages it. Remote updates and A/B testing are fully permitted. The compliance standard is the same as for a binary submission: if the paywall breaks the rules after a remote update, the app is treated as if a non-compliant binary was submitted.


Does running A/B tests on paywalls require Apple's approval? No. A/B testing paywall content is allowed without pre-approval. The developer is responsible for ensuring every variant in the test meets App Store guidelines. A variant that would be rejected in a binary submission can still get an app flagged when delivered remotely.


What should I do if I'm unsure whether a paywall pattern is compliant? Apple's preference is that developers push and ask questions. If a flow is flagged, Apple typically warns before banning. Superwall has a direct line to Apple for compliance questions — reach out if you need a concrete answer on a specific pattern.
