---
schema_version: "1.0.0"
document_id: "004141309212fc044bb2e9b3bacb9f70a1516eceff402b7921472146cd8f3e28"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/the-cloud-marketplace-operations-runbook/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T00:55:49.760493+00:00"
fetched_at: "2026-08-20T00:55:51.412607+00:00"
content_hash: "sha256:41cf5e15c67fc0403519228dc21883ecc5821907922ee0c9c7c21b338d01100c"
---

# The Cloud Marketplace Operations Runbook

*A cloud marketplace operations runbook is a list of the recurring work a marketplace business has to do, sorted by how often each task comes due rather than by which team owns it.*


---


Most marketplace operations knowledge lives in one person’s head and one Slack thread. That works until the person is on holiday, or the business is on three marketplaces instead of one, and then the questions arrive with no owner: did we submit final usage for the account that churned? Is anything renewing next month that nobody has quoted? Why does the payout report disagree with the dashboard again?


None of these tasks is hard. The failure is that they are invisible between the moments they matter, so they get skipped until a number is wrong or a customer is annoyed. A runbook makes the work visible by writing it down against a clock.


This post is that clock. It is organised by cadence — daily, weekly, monthly, quarterly — and each item links to the deep dive that explains how to do it properly. Treat it as the index to a marketplace operation, not a replacement for the pages it points at.


---


## What belongs in a marketplace operations runbook?


A marketplace operations runbook contains every task that recurs on a schedule and has a consequence if skipped. That is a narrower set than “everything the team does” — it deliberately excludes one-off decisions and project work, because a runbook is for the load that never stops, not for the thing you do once.


The tasks sort cleanly into four buckets by frequency, and the frequency is not arbitrary: it follows where a delay does damage. Anything a customer is waiting on is daily. Anything with a monthly deadline is monthly. The buckets below are the whole runbook; the rest of this page is each bucket in detail.


Cadence What comes due Why this frequency


**Daily** Provisioning, entitlement changes, event failures, advisories A buyer or a broken subscription is waiting now


**Weekly** Offer pipeline, expiring quotes, co-sell status, usage sanity check Slow enough to batch, fast enough to catch drift


**Monthly** Reconciliation, close, payout verification, metering completeness The billing period is the unit of truth


**Quarterly** Renewals, co-terming, listing hygiene, access review Contract and catalog cycles turn on this clock


---


## Daily: the work with someone waiting


Do daily the tasks where the cost of a one-day delay lands on a customer. There are four, and all four share a property — the failure is invisible to you and obvious to the buyer.


**Confirm provisioning kept up.** Every purchase and every entitlement change should have granted access without anyone watching. The task is not to grant it by hand; it is to confirm nothing fell through, because a buyer who paid and cannot log in is the worst first impression a marketplace deal can make. The shape of that failure is in[why marketplace provisioning fails silently](https://www.suger.io/resources/blog/marketplace-provisioning-failures/) .


**Clear event and webhook failures.** Marketplace state arrives as events, and events fail — a receiver was down, a payload changed, a retry ran out. A failed entitlement event is a provisioning failure that has not surfaced yet. What can break and how to catch it is covered in[marketplace events and webhooks](https://www.suger.io/resources/blog/marketplace-events-and-webhooks/) .


**Read the advisories.** Cloud marketplaces issue advisory notices about account closure, compromise, abuse or fraud on an agreement. These are not automatable — each one is a judgement about a specific customer — but they are time-sensitive, so they belong on the daily list even though the response does not.


**Glance at usage ingestion.** Not a full audit — just confirm meters are still reporting. A dimension that silently stopped sending is a revenue leak that compounds every day it goes unnoticed;[when usage records fail](https://www.suger.io/resources/blog/when-usage-records-fail/) walks through the specific ways this happens.


The daily list should take minutes, and if it takes longer than that most days, the honest read is that too much of it is still manual — which is the argument for[running the operation API-first](https://www.suger.io/resources/blog/running-marketplace-operations-api-first/) .


---


## Weekly: catching drift before it sets


Do weekly the work that is too slow to need daily attention but drifts if left a month. This is the cadence for pipeline and consistency checks.


- **Offer pipeline.** Which private offers are drafted, sent, accepted, expired? An offer that expired unnoticed is a deal that quietly went cold. Creating and tracking these at volume is easier off the console — see[creating private offers without the console](https://www.suger.io/resources/blog/creating-private-offers-without-the-console/) .
- **Expiring quotes and trials.** Anything with a clock — a private offer nearing its expiry, a trial about to convert or lapse — needs a human before the date, not after.
- **Co-sell status.** Referrals submitted to a cloud provider come back with a status on the provider’s schedule, not yours. A weekly pass keeps them from stalling in a state nobody is watching.
- **Cross-marketplace sanity check.** If you sell on more than one marketplace, the weekly question is whether the same customer looks the same everywhere. This is where a single object model earns its keep, and where its absence starts to show; the vocabulary for it is in the[Cloud GTM glossary](https://www.suger.io/resources/blog/cloud-gtm-glossary/) .


The weekly review is also where you notice the difference between an[offer and an entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) starting to blur in your own records — the offer says what was agreed, the entitlement says what the customer can actually use, and when a rep conflates them the reconciliation at month end inherits the confusion.


---


## Monthly: closing the billing period


Do monthly everything anchored to the billing cycle, because the month is the unit that finance, the marketplace and your own ledger all agree to measure in. Three tasks matter, and they run in order.


First, **confirm metering is complete for the period** — every dimension that should have reported did, including the final partial usage for any customer whose agreement ended mid-month. Second, **reconcile** : the marketplace’s payout report, your metered usage and your invoicing should describe the same money, and when they do not you need to know why before the number leaves the building. Third, **verify the payout** against what you recognised.


These three are one workflow, and running it the same way every month is what turns a scramble into a checklist — that checklist is[the monthly marketplace close](https://www.suger.io/resources/blog/the-monthly-marketplace-close-checklist/) . The reason the numbers so rarely match on the first pass, and how to reconcile them, is[why marketplace numbers never match](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) .


If reconciliation keeps surfacing the same category of surprise, that surprise is a candidate for an alert rather than a monthly discovery — deciding[what to alert on in marketplace operations](https://www.suger.io/resources/blog/what-to-alert-on-in-marketplace-operations/) is how a recurring month-end shock becomes a same-day notification instead.


---


## Quarterly: the contract and catalog clock


Do quarterly the work that turns on contract and catalog cycles rather than the billing period. These tasks are easy to defer because nothing breaks the day you skip them — the cost shows up a quarter or two later as a lapsed renewal or a stale listing.


- **Renewal and expansion pipeline.** Look a quarter ahead at what is renewing and start those conversations early. Multi-marketplace and multi-agreement customers add a wrinkle: agreements that should renew together drift apart unless you[co-term and expand them](https://www.suger.io/resources/blog/co-terming-and-expanding-marketplace-agreements/) deliberately.
- **Listing hygiene.** Descriptions, pricing dimensions and screenshots go stale. A quarterly pass keeps every listing current across the six marketplaces Suger supports.
- **Access review.** Who can create an offer, approve a discount or export revenue data? Permissions accrete; a quarterly review removes the ones that should have lapsed.
- **Metering-model review.** As the product changes, the[metering dimensions](https://www.suger.io/resources/blog/designing-metering-dimensions/) you bill on can fall out of step with what you actually sell. Quarterly is often enough to catch it, and early enough to change it before it is baked into a year of contracts.


---


## How the runbook changes as you scale


The runbook does not change; who runs it does. On one marketplace with a handful of customers, one person runs every cadence from the console and it is fine. The tasks are the same at every size — what changes is that the console stops being able to keep up, and the daily list quietly becomes a full-time job.


The tell is the daily bucket. When confirming provisioning and clearing event failures takes a person all morning, the operation has outgrown the console and the fix is to move the recurring, rule-based tasks onto an API so the human time goes to the judgement calls — advisories, discount approvals, first-of-a-kind offers — that should never have been automated anyway. A structured way to think about that first year of growth is in[the first 90 days on a cloud marketplace](https://www.suger.io/resources/blog/first-90-days-cloud-marketplace/) .


---


## Frequently asked questions


**What is a cloud marketplace operations runbook?** It is a list of the recurring work a marketplace business must do — provisioning, offers, entitlements, metering, reconciliation, renewals — sorted by how often each task comes due rather than by which team owns it.


**How often should marketplace operations tasks run?** By consequence. Anything a buyer is waiting on is daily. Pipeline and consistency checks are weekly. Reconciliation and close are monthly. Renewals, listing hygiene and access reviews are quarterly.


**Which marketplace tasks are the most time-sensitive?** The daily ones, because their failures land on a customer: confirming provisioning kept up, clearing failed entitlement events, reading advisories, and checking that usage meters are still reporting.


**What belongs in the monthly marketplace close?** Confirming metering is complete for the period, reconciling the marketplace payout report against your usage and invoicing, and verifying the payout against what you recognised — run in that order, the same way every month.


**Does the runbook change when selling on several marketplaces?** The tasks stay the same, but a weekly cross-marketplace consistency check becomes essential, and the console stops scaling sooner. Multi-marketplace operations need one shared object model to stay answerable.


---


## Takeaways


- A runbook makes invisible recurring work visible by writing it against a clock; sort tasks by how often they come due, not by who owns them.
- Daily is for anything a buyer is waiting on: provisioning, failed events, advisories, meter health.
- Weekly catches drift — offer pipeline, expiring quotes, co-sell status, cross-marketplace consistency.
- Monthly is the billing period: complete metering, reconcile, verify the payout, in that order.
- Quarterly turns on contract and catalog cycles: renewals, listing hygiene, access review, metering-model fit.
- The tasks do not change as you grow — the console’s ability to keep up does. That is the signal to move rule-based work onto an API.


Suger runs this cadence across the six marketplaces it supports — listings, private offers, entitlements, metering, reconciliation and co-sell in one place — so the runbook is one operation instead of one per console.[See the Suger platform](https://www.suger.io/platform/) , browse the[product documentation](https://doc.suger.io/) , or[talk to our team](https://www.suger.io/contact-us/) .
