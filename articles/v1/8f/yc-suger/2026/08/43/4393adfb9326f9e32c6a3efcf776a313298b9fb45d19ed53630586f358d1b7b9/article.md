---
schema_version: "1.0.0"
document_id: "4393adfb9326f9e32c6a3efcf776a313298b9fb45d19ed53630586f358d1b7b9"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/aws-marketplace-professional-services-fee-cut/"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T10:32:29.919238+00:00"
fetched_at: "2026-08-19T10:32:31.370146+00:00"
content_hash: "sha256:e9c787a7517900d5600d4802c585b6e36903c2128e249529e7cb56000df8c46f"
---

# The AWS Marketplace Professional Services Listing Fee Cut

*The AWS Marketplace professional services listing fee is the percentage AWS keeps from a services private offer. In June 2026 AWS cut it from 2.5% to 0.5% of the pre-tax contract value — a five-fold reduction that changes the margin math on every services attach you route through the marketplace.*


---


If you model margin for an ISV or a consulting partner, a marketplace listing fee is a line you already know how to book: a percentage of contract value that AWS keeps before disbursing the rest to you. Until June 2026 that line read 2.5% on professional services. Now it reads 0.5%.


That is not a rounding change. On a services attach, the fee just got five times smaller, and the number you were subtracting from gross margin to justify transacting through the marketplace instead of on your own paper mostly went away.


Here is the exact before and after, which listings it applies to, and what it does to the model.


---


## What changed, and by how much


**In June 2026, AWS reduced the listing fee on professional services private offers from 2.5% to 0.5% of the pre-tax total contract value.** The fee is what AWS retains from the transaction before disbursing the balance to the seller; everything else about how a services offer is created, accepted and billed stayed the same.


Two details from the announcement matter for a finance model:


- **The reduction applies to new private offers going forward.** Offers and subscriptions already accepted before the change continue at their original 2.5% terms — so a mid-flight engagement does not re-rate, and your existing backlog keeps the fee it was booked with.
- **Sellers with existing professional services listings benefit automatically.** There is nothing to re-register or re-list; the next services private offer you send draws the 0.5% fee. The reduced rate applies across the AWS Regions where AWS Marketplace operates, and across pricing models and currencies.


This is the professional-services line of the standard AWS Marketplace fee schedule, not a promotion with an expiry date. It sits in the same seller-guide table as the software and data listing fees.


---


## Which listings the 0.5% fee applies to


**The 0.5% fee applies to professional services offerings sold as private offers on AWS Marketplace** — assessments, implementation and integration work, training and enablement, and managed or support services. These are the human-delivered engagements that are scoped and quoted per customer, then transacted as a private offer rather than a self-serve subscription. For the mechanics of what qualifies and how the listing is built, see[selling professional services on AWS Marketplace](https://www.suger.io/resources/blog/selling-professional-services-on-aws-marketplace/) .


What the reduced fee does *not* touch is your software. A SaaS or AMI listing carries its own fee schedule, and the cut does not change it. That distinction is the whole point when you model a bundle: the software portion and the services portion of the same deal are metered and charged at different rates, so a blended assumption will misstate both.


The fee is calculated on the **pre-tax total contract value (TCV)** of the services offer. If you send a $200,000 implementation engagement, the fee is 0.5% of $200,000 — $1,000 — and AWS disburses the remaining $199,000 to you on the marketplace’s normal schedule. Under the old rate, that same offer carried a $5,000 fee.


---


## Before and after: the fee treatment side by side


Here is the professional services listing fee before and after the change, alongside the software private-offer fees that did not move, so you can see where a services attach now sits relative to the software it rides on.


Listing type Fee before June 2026 Fee after June 2026 Basis


**Professional services** (private offer) 2.5% **0.5%** Pre-tax TCV of the services offer


SaaS software (private offer, <$1M) 3% 3% (unchanged) Pre-tax TCV


SaaS software (private offer, $1M–<$10M) 2% 2% (unchanged) Pre-tax TCV


SaaS software (private offer, ≥$10M or any renewal) 1.5% 1.5% (unchanged) Pre-tax TCV


SaaS software (public offer) 3% 3% (unchanged) Pre-tax TCV


Two things stand out. First, professional services is now the cheapest line in the schedule to transact — at 0.5% it is below every software tier. Second, because the software tiers step down as contract value rises but services stays flat at 0.5%, the *relative* cost of routing services through the marketplace only falls further on larger deals.


---


## What the fee cut does to a services attach


**On a services attach, the fee cut returns two cents of every dollar of contract value straight to gross margin.** Going from 2.5% to 0.5% is a 2.0-percentage-point reduction in the marketplace’s take on the services line — and on an attach with meaningful services content, that flows through to the blended margin of the whole deal.


Model it on a bundle. Say you sell $500,000 of SaaS with a $200,000 implementation engagement attached, both through AWS Marketplace as private offers under $1M:


Line Contract value Fee rate Fee Old fee (services at 2.5%)


SaaS software $500,000 3% $15,000 $15,000


Professional services $200,000 0.5% $1,000 $5,000


**Total** **$700,000** — **$16,000** **$20,000**


The software fee is untouched. The services fee drops from $5,000 to $1,000, so the total marketplace fee on the bundle falls from $20,000 to $16,000 — a $4,000 improvement to gross margin on a single deal, from a schedule change that required no renegotiation. The heavier your services mix, the larger the swing: on a services-led engagement the 2.0-point cut is nearly the entire fee.


There is a strategic consequence hiding in the arithmetic. The old 2.5% was often cited as the reason to keep services off the marketplace and on a direct MSA — the fee ate into already-thin services margin. At 0.5%, that objection mostly disappears, and the marketplace’s own advantages come back into view: the engagement can draw down the customer’s committed cloud spend, it clears procurement the buyer has already run, and it lands on the same consolidated bill as the software. You get those benefits for a fee that is now a fifth of what it was. If services routing was a close call in your model at 2.5%, it is not a close call at 0.5%.


The one thing not to over-model: the reduction applies to *new* offers, so your in-flight backlog does not re-rate. The improvement shows up on deals you send from here on, not retroactively on the pipeline you already closed.


---


## How to keep the two rates straight in your model


**Model the software line and the services line separately, because they carry different fees.** The single most common error after a change like this is applying one blended marketplace-fee assumption to a mixed deal — which either overstates the fee on the services portion or understates it on the software portion. Split the contract value by listing type, apply 0.5% to services and the correct software tier to software, and the blended rate falls out correctly.


For teams reconciling many of these at scale, the fee is a field on every marketplace transaction, and the disbursement you actually receive is contract value minus that fee. Reconciling booked revenue against disbursed cash — across software tiers, services, and any channel-partner uplift — is exactly the kind of work that gets error-prone in a spreadsheet and belongs in a system that reads the transactions directly. A[marketplace billing and metering](https://www.suger.io/platform/billing-metering/) platform stores the fee and the disbursement per transaction, so the services line rates at 0.5% and the software line rates at its own tier without a human remembering which is which.


---


## Frequently asked questions


**What is the new AWS Marketplace professional services listing fee?** It is 0.5% of the pre-tax total contract value on professional services private offers, reduced from 2.5% in June 2026. AWS keeps 0.5% of the contract value and disburses the balance to the seller on the marketplace’s normal schedule.


**When did the professional services fee reduction take effect?** AWS announced the reduction in June 2026, and it applies to new private offers going forward. Offers and subscriptions accepted before the change continue at their original 2.5% terms, so mid-flight engagements do not re-rate.


**Which listings does the 0.5% fee apply to?** Professional services offerings sold as private offers — assessments, implementation, training and enablement, and managed or support services. It does not change the fees on SaaS or AMI software listings, which keep their own separate fee schedule.


**Do I need to re-list to get the lower fee?** No. Sellers with existing professional services listings benefit automatically; the next services private offer you send draws the 0.5% fee. There is nothing to re-register, and it applies across AWS Regions, pricing models and currencies.


**How does the cut change margin on a services attach?** It returns two percentage points of the services contract value to gross margin. On a $200,000 engagement, the fee drops from $5,000 to $1,000 — a $4,000 improvement on that line alone. The software portion of a bundle is unaffected.


**Is the professional services fee lower than the software fee now?** Yes. At 0.5%, professional services is the cheapest line in the AWS Marketplace fee schedule — below the software private-offer tiers of 3%, 2% and 1.5%. The gap is widest on smaller deals, where the software rate is highest.


---


## Takeaways


- AWS cut the professional services listing fee from **2.5% to 0.5%** of pre-tax total contract value in June 2026 — a five-fold reduction.
- It applies to **professional services private offers only** — assessments, implementation, training, and managed services. Software fees are unchanged.
- The cut applies to **new offers going forward** ; already-accepted offers keep their original 2.5% terms, so your backlog does not re-rate.
- On a services attach, the change returns **two points of contract value to gross margin** — $4,000 on a $200,000 engagement — and makes services the cheapest line in the fee schedule.
- Model the software and services lines separately; a single blended fee assumption will now misstate a mixed deal.


---


Suger stores the listing fee and disbursement on every marketplace transaction, so a services line rates at 0.5% and a software line at its own tier without a spreadsheet in between. See how it reconciles marketplace revenue on the[Cloud GTM platform](https://www.suger.io/platform/) .
