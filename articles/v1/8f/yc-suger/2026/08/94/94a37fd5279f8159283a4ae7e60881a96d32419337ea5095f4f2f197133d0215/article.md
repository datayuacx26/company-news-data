---
schema_version: "1.0.0"
document_id: "94a37fd5279f8159283a4ae7e60881a96d32419337ea5095f4f2f197133d0215"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/co-terming-and-expanding-marketplace-agreements/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:ff3474f84ec6ea97dd3b2dc261371474a97454c59bfb1184862eccf53952a0bb"
---

# Co-Terming and Expanding Marketplace Agreements

*Co-terming means bringing a mid-term expansion onto the same end date as the original agreement, so a customer has one renewal conversation instead of two. On AWS Marketplace it is done with an amendment, which does not extend the agreement — it replaces it. Knowing what that replacement takes with it is most of the job.*


---


Six months into a contract, the customer wants more. More seats, another module, a second environment. Commercially this is the easiest conversation of the year.


Operationally it forks immediately. Do you amend the existing agreement so everything ends together, or write a second agreement alongside it? Until recently AWS made that choice for you — one agreement per product per account, so amendment was the only path. That has changed, and the choice is now real.


---


## **What an agreement is, and what amending one does**


**An agreement is an offer a buyer accepted.** AWS is precise about the pair: “An *offer* is a set of terms for the use of a product… An **agreement** is an offer that a buyer accepted.” AWS itself is not a party to it.


An amendment is therefore not an edit. It is a new offer whose acceptance retires the old agreement. AWS states the consequence directly: “An amendment replaces a buyer’s current subscription.”


The mechanics of that replacement are what matter for a co-term:


- On acceptance, “the new agreement takes effect immediately.”
- “Any remaining scheduled payments from previous agreements are cancelled.”
- “Existing invoices remain unchanged. However, the payment schedule in the amendment replaces pending invoices from the previous subscription.”
- If the buyer never accepts, “the original agreement will continue unchanged.”


So the buyer keeps what they have already been invoiced for, and everything still pending is replaced by whatever schedule you wrote. **The new schedule must therefore contain the remaining value of the old agreement plus the expansion** — not just the expansion. Writing an amendment that prices only the new seats is the single most expensive mistake available here, because it silently cancels the rest of the original contract’s payments.


---


## **The co-term itself**


AWS lists what an amendment can change: “you can grant new entitlements, offer pricing discounts, adjust payment schedules, or change the end user license agreement (EULA)… You can also change the number of units and payment schedule and **add a custom end date** .”


That custom end date is the co-term. You set the amended agreement to finish when the original would have, price the expansion for the remaining months, and the customer arrives at one renewal with one number.


On the amendment form itself you can adjust service dates, product dimensions, offer currency, payment schedule, usage dimensions, renewal status, EULA, and the offer expiration date.


Two constraints bound this:


**No future dating.** “Amended offers cannot have a future agreement start date. Amended offers only target active agreements, and once accepted, the amended agreements immediately become active making future dating impossible.” An expansion that should begin next quarter cannot be pre-loaded as an amendment.


**The seller of record is fixed.** “You can’t amend an agreement to specify a seller of record that’s different from the seller of record from the original agreement.” A deal that moves from direct to a channel partner mid-term is not an amendment. For channel partner private offers, the amendment must also use the currency defined in the selling authorisation.


---


## **What cannot be amended at all**


This is the constraint most likely to end the conversation before it starts. Only two product types support offer amendments:


- SaaS contracts
- SaaS contracts with consumption


AWS lists what appears on the Agreements tab but cannot be amended: SaaS usage-based products, AMI-based products, container-based products, server contracts, and **professional services products** .


If you sell usage-based SaaS, there is no amendment path. If you bundled implementation services into the deal, that component cannot be amended either — which is worth knowing before you promise a customer a single co-termed expansion covering both.[Packaging software and services in one offer](https://www.suger.io/resources/blog/packaging-software-and-services-in-one-offer/) covers how those components are structured in the first place.


---


## **The other option: a second agreement**


AWS Marketplace now supports **Concurrent Agreements** , described in its own documentation as “enabling buyers to make multiple purchases per AWS account.” Multiple active agreements for the same product in one account, each with independent terms and pricing.


Before this, a one-agreement-per-account limit pushed sellers into workarounds — agreement-based offers, duplicate product listings, or cancel-and-reissue cycles — all of which existed to simulate a second purchase. Those workarounds are now unnecessary.


This changes the expansion decision into a genuine trade:


Amend Add a concurrent agreement


Existing agreement Replaced Untouched


Pending invoices Replaced by the new schedule Unaffected


End dates Can be aligned with a custom end date Independent unless you match them


Renewal conversations One One per agreement


Risk Mispricing cancels remaining payments Drift between agreements over time


Product types SaaS contracts only Broader, including professional services


**Amend when the customer wants one contract and one renewal** , and you can price the remainder correctly.


**Add a second agreement when the expansion has its own commercial logic** — a different business unit, a different term length, a pilot that may not renew — or when the product type leaves you no choice.


There is an integration prerequisite. Concurrent Agreements requires identifying entitlements per licence rather than per customer: from June 1, 2026, new SaaS products must use` CustomerAWSAccountId` instead of` CustomerIdentifier` , and` LicenseArn` instead of` ProductCode` . A product that still assumes one entitlement per customer will merge two agreements into one and meter them incorrectly.[Entitlement management on cloud marketplaces](https://www.suger.io/resources/blog/marketplace-entitlement-management/) covers the state model that has to hold.


---


## **Operational details worth planning around**


**Amended offers take about 45 minutes to appear.** AWS notes an amended offer “will appear on the **Private Offer** page within approximately 45 minutes.” Do not tell the buyer to look immediately.


**The buyer can decline and keep what they have.** On the Private Offer page “the buyer has the option to accept the offer or to continue the original agreement.” An amendment is a proposal, not a change.


**Amending a public offer costs auto-renewal.** “If you amend an accepted public offer, it becomes a private offer and no longer auto-renews.” A self-serve customer who accepts a one-time expansion has quietly become a manual renewal. Put the renewal date in your own system before you send it.


**Your integration gets a signal.** For SaaS notifications, “a` subscribe-success` message is sent with the new` offer-identifier` when the buyer accepts the amendment” — treat it as a prompt to re-read entitlement state rather than as a description of what changed.


**Name the offer for a human.** Custom offer names are visible to buyers, take up to 150 characters, and AWS recommends including identifying details such as your own reference or a purchase order number, and no personal data. In reporting, the daily customer subscriber report carries a` Previous offer ID` field, which is what lets finance connect an amendment to what it replaced.[Marketplace revenue reconciliation](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) covers the rest of that trail.


---


## **Frequently asked questions**


**What does co-terming mean on a cloud marketplace?** Aligning a mid-term expansion to the original agreement’s end date so the customer has one renewal. On AWS it is done by amending the agreement and setting a custom end date.


**Does amending extend an existing agreement?** No. An amendment replaces the buyer’s current subscription with a new agreement that takes effect immediately on acceptance, and cancels any remaining scheduled payments from the previous one.


**What must the amended payment schedule contain?** The remaining value of the original agreement plus the expansion. Pending invoices from the previous subscription are replaced, so pricing only the expansion cancels the rest.


**Which products can be amended?** Only SaaS contracts and SaaS contracts with consumption. AMI, container, server contract, usage-based SaaS and professional services products do not support amendments.


**Can a second agreement be added instead?** Yes. Concurrent Agreements allow multiple active purchases of the same product within one AWS account, each with independent terms, provided the integration identifies entitlements per licence.


**What happens if the buyer ignores the amendment?** The original agreement continues unchanged. An amendment is an offer, and letting it expire is a valid buyer response.


---


## **Takeaways**


- An amendment replaces the agreement rather than extending it, and cancels every remaining scheduled payment.
- Price the amendment as remaining value plus expansion, or you cancel revenue you had already contracted.
- The custom end date is the co-term mechanism. Future start dates are not possible, and the seller of record cannot change.
- Only SaaS contracts and SaaS contracts with consumption can be amended at all — professional services cannot.
- Concurrent Agreements make a second agreement a real alternative, but require per-licence entitlement identification.
- Amending an accepted public offer converts it to a private offer and ends auto-renewal. Track the renewal yourself from then on.


---


Expansion should not be a reconstruction. See how Suger’s[agreement management](https://www.suger.io/platform/agreements/) keeps amendments, renewals and concurrent agreements in one view across every marketplace, so a co-term is a decision rather than an archaeology project.
