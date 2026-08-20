---
schema_version: "1.0.0"
document_id: "e082ed633a08f704bbe3e12e90b7d8ee639ac6d9e5bb1e76b1941c8fcce2d3bb"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/what-happens-when-a-marketplace-trial-ends/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T09:12:25.375763+00:00"
fetched_at: "2026-08-15T09:12:26.807099+00:00"
content_hash: "sha256:3836368777f68868b3c038a38c2394a6b05a55030ada200c53aa58cecad38f55"
---

# What Should Happen When a Marketplace Trial Ends

*A cloud marketplace free trial is a time-boxed entitlement that expires on a date the platform records at signup. What happens at that date is entirely up to the seller — the marketplace ends the entitlement, it does not run your conversion.*


---


Most trial programmes are designed forwards and abandoned backwards. Somebody specifies the signup flow, the welcome email and the in-product tour, and then the trial ends and a support agent finds out because a customer emails asking why they are locked out.


The end of a trial is the part with revenue attached, and it is the part nobody owns. It is also the most knowable moment in the whole funnel: the expiry date exists from the first day, in a system you can query.


Here is what should happen in the days around it.


---


## **How does a marketplace free trial actually end?**


A marketplace free trial ends when its entitlement expires, and the marketplace tells you it has. It does not convert to a paid subscription by itself, and it does not ask the buyer whether they meant to continue.


On AWS Marketplace, a SaaS free trial offer carries a length the seller sets[between 7 and 90 days](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-free-trials.html) . Each AWS account can use a free trial for a given product once, and that allowance is not shared across linked accounts under one payer — so a large customer can legitimately trial the same product from several accounts.


Two details from the same documentation matter more than they look:


- A seller can create **one public free trial offer per public SaaS product** . Your trial is a property of the listing, not a per-deal setting.
- If you cancel a trial offer, **agreements already active continue until they expire** . Cancelling the offer stops new trials; it does not end the ones you have.


The signal that a subscription is a trial arrives with the subscription itself. AWS Marketplace’s notifications carry an` isFreeTrialTermPresent` flag, and buyers arriving from a trial reach your registration URL with an` x-amzn-marketplace-offer-type=free-trial` token attached. If you are not reading either, every trial in your system looks like a customer.


---


## **The five things that have to happen, and when**


The table is the sequence. The dates are relative to the expiry the platform already holds.


When What has to happen Who owns it


At signup Record that this is a trial, and record its end date. Both arrive with the subscription. Engineering


Mid-trial Establish whether the account has done the thing that predicts renewal. Not “did they log in” — the specific action your product exists to perform. Product / growth


~2 weeks out A human decision: is this a deal worth working, or a self-serve conversion? Enterprise trials that end without a conversation end. Sales


Expiry day Access changes state. The buyer should already know it is going to. Engineering


After expiry Final usage is reported, and the account moves to a known state — converted, lapsed, or explicitly re-engaged. Engineering / finance


The failure mode is not that teams get these wrong. It is that only the first and fourth are automatic, so the middle three depend on somebody remembering.


---


## **The expiry-day mechanics people get wrong**


Two things happen at expiry that are easy to discover the expensive way.


**Usage reporting has a deadline.** For usage-based products, AWS Marketplace sends a` License Deprovisioned - Manufacturer` event and gives sellers[one hour to submit final usage records](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-eventbridge-integration.html) with` BatchMeterUsage` . After that window closes, entitlements are fully revoked and usage reporting is no longer accepted. If your metering runs on a nightly batch, a trial that ends at 09:00 has its last hours of usage dropped — and on a trial with an overage component, that is revenue you cannot bill.


The older SNS integration expresses the same idea differently: an` unsubscribe-pending` message arrives first, and the seller has[about one hour](https://docs.aws.amazon.com/marketplace/latest/userguide/saas-notification.html) to send final metering records before` unsubscribe-success` closes the door.


**Revoking access is your job, not the platform’s.** The marketplace ends the entitlement. Whether your product notices is a question about your own entitlement checks. A trial that silently keeps working is not generosity — it is an unpriced customer, and it teaches the buyer that the trial never really ended.


---


## **What “trial ended” should mean in your own systems**


The end state that matters is not a flag on a subscription. It is an answer to a question a finance lead will ask: how many trials ended last quarter, and what happened to each one?


That needs three fields your systems probably do not agree on today:


- **Was this a trial?** From the marketplace’s own signal, not from a guess about price being zero.
- **When did it end, and did it end on schedule?** A trial that ended early because the buyer unsubscribed is a different event from one that ran its term.
- **What state did the account land in?** Converted to a paid agreement, lapsed, or re-trialled from a different account.


Reconciling those against the marketplace’s own record is the same problem as reconciling any other agreement — the platform holds the truth, and your CRM holds an opinion. We wrote about the general case in[Why Marketplace Numbers Never Match](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) , and the trial cohort is where the mismatch shows up first, because trials churn faster than paid agreements.


---


## **The conversation, not the countdown**


The instinct at the end of a trial is to add automation: a reminder sequence, an in-product banner, a discount that appears on day 12.


For self-serve products that is correct. For anything with a deal desk attached it is a category error. An enterprise trial is an evaluation running inside a procurement process, and the thing that converts it is a private offer with terms the buyer’s finance team can accept — not an email.


The practical consequence is that the mid-trial checkpoint should produce a routing decision, not a nurture sequence. Trials above a certain account size go to a human, who has enough time before expiry to negotiate and issue an offer. Trials below it go to the automated path. Getting that split wrong in either direction is expensive: automating enterprise trials loses deals, and hand-working small ones costs more than they return.


If the outcome is a private offer, the buyer-side experience of accepting one is worth understanding before you send it — we covered that in[What Buyers See on a Private Offer](https://www.suger.io/resources/blog/what-buyers-see-when-you-send-a-private-offer/) .


---


## **Frequently asked questions**


**Does a marketplace free trial convert to paid automatically?** No. The marketplace ends the trial entitlement on its expiry date. Converting the account to a paid agreement is a separate purchase the buyer has to make, either self-serve or through a private offer.


**How long can an AWS Marketplace SaaS free trial run?** AWS Marketplace allows a SaaS free trial length between 7 and 90 days, set by the seller when creating the free trial offer.


**How do I tell whether a subscription is a trial?** Read it from the marketplace signal. AWS Marketplace notifications include an` isFreeTrialTermPresent` flag, and trial buyers arrive at your registration URL with an` x-amzn-marketplace-offer-type=free-trial` token.


**What happens to active trials if I cancel the trial offer?** On AWS Marketplace, agreements already active under a cancelled free trial offer stay active until they expire. Cancelling stops new trials from starting; it does not end existing ones.


**Can the same customer trial my product twice?** Each AWS account can use a free trial for a product once. The allowance is not shared across linked accounts under one payer, so a large organisation can trial the same product from several accounts.


**When is the last moment to report usage on an expiring trial?** For usage-based SaaS products, AWS Marketplace gives sellers one hour after the` License Deprovisioned` event to submit final usage with` BatchMeterUsage` . After that, usage reporting is no longer accepted.


---


## **Takeaways**


- The trial’s end date exists from day one, in the marketplace’s own record. Nothing about the ending needs to be a surprise.
- Read the trial flag from the marketplace signal rather than inferring it from price, or trials and customers look identical in your systems.
- Final usage on a usage-based product has a one-hour window after deprovisioning. Nightly metering batches miss it.
- Revoking access is the seller’s job. The marketplace ends the entitlement; your product decides whether it notices.
- Split the cohort mid-trial: enterprise trials need a private offer and a human, self-serve trials need a path that does not involve one.


Suger reads marketplace subscription and entitlement events for AWS, Microsoft, Google Cloud and the other marketplaces it supports, and keeps the resulting agreement state in one place your CRM and your finance system can both see. If trials and paid agreements currently look the same in your reporting,[see how Suger handles entitlements](https://www.suger.io/platform/agreements/) or[talk to our team](https://www.suger.io/contact-us/) about your own trial cohort.
