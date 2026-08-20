---
schema_version: "1.0.0"
document_id: "616bee834c1a92078e7718a0403a8efcc81efa302544a22976baa8f1f187b970"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/azure-private-offers-vs-private-plans/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:78b5489e35cb2ca5886082fd47f57035272941225bd716b1b85087bcc99e8602"
---

# Azure Marketplace Private Offers vs Private Plans

*An Azure Marketplace private offer is a custom, time-bound deal sent to one named customer. A private plan is a hidden pricing plan inside a public listing, visible only to tenants you specify. They solve different problems, and Microsoft supports them on different offer types.*


---


The first time a deal desk sends a custom-priced deal through Microsoft’s marketplace, somebody opens Partner Center and finds three things that all look like the answer: a private offer, a private plan, and — if a partner is involved — a multiparty private offer.


Picking the wrong one is not a formatting mistake. Each construct targets the customer differently, carries different legal terms, and is supported on a different set of offer types. Some deals simply cannot be done with the construct you reached for first.


Here is what each one is, and a decision framework for choosing between them.


---


## **What is an Azure Marketplace private offer?**


An Azure Marketplace private offer is a custom deal a publisher extends to one specific buyer, with negotiated pricing, terms, and an expiry date. It is created in Partner Center, sits outside the public listing entirely, and is built to align the customer’s procurement and IT teams around one agreed transaction.


Two properties make it the default for enterprise deals. It is **time-bound** — the offer has a deadline, which does useful work in a negotiation. And it can carry a **custom end user licence agreement** attached to that specific transaction, so a customer with redlines is not forced onto your standard public terms.


Private offers are also the construct that carries the full deal-making surface: negotiated price, term, and the paperwork enterprise procurement expects. If you are new to selling through Microsoft,[how to sell on Azure Marketplace](https://www.suger.io/resources/blog/how-to-sell-on-azure-marketplace-isv-guide/) covers where offers sit in the wider listing and Partner Center flow.


---


## **What is a private plan?**


A private plan is a pricing plan inside an existing offer that only nominated customers can see. The listing stays public; the plan does not. Publishers use it to give a specific customer — or a small set of them — custom pricing without building a separate deal each time.


The differences from a private offer are precise and worth memorising:


- **Targeting.** A private plan is scoped by **tenant ID** . A private offer is scoped to the customer’s billing account. That distinction decides which one your customer’s procurement structure can even accept.
- **Legal terms.** A private plan inherits the EULA on your public listing. Only a private offer lets you attach a custom EULA for that transaction.
- **Supported offers.** Private plans are only available for **paid products that can be purchased and installed directly from the Azure portal** . Publishers cannot create private plans for consulting services, for any offer whose call to action is “Contact me,” or for any free service — regardless of whether it installs from the portal.


That last constraint is where most teams discover the difference the hard way. A services offer or a “Contact me” listing has no private-plan path at all.


---


## **And multiparty private offers**


A multiparty private offer (MPO) works like a private offer, except a **channel partner** sells it to the customer. The customer buys each included product through Microsoft Marketplace, and Microsoft invoices and collects payment from the customer under their existing billing terms with Microsoft.


Use it when the transacting party is not you. If your motion is direct, an MPO adds a party and a margin conversation for no benefit. If your motion runs through resellers, it is the only construct that puts them in the transaction properly —[CPPO vs MPO vs reseller offers across the clouds](https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/) compares it with the AWS and Google Cloud equivalents.


---


## **Which construct for which deal shape**


Deal shape Use Why


One enterprise customer, negotiated price and term **Private offer** Time-bound, billing-account scoped, full deal terms


Customer requires redlined or custom legal terms **Private offer** The only construct that attaches a custom EULA to the transaction


A few known tenants on standing custom pricing **Private plan** No per-deal build; pricing lives in the listing


Consulting or professional services offer **Private offer** Private plans are not supported for consulting services


Offer with a “Contact me” call to action **Private offer** Private plans require a directly purchasable, installable product


Free offer with a custom arrangement Neither — restructure Private plans exclude free services


A reseller transacts with the customer **Multiparty private offer** Puts the partner in the transaction; Microsoft bills the customer


Customer wants the spend to count against committed Azure spend **Private offer** Enterprise buyers are usually optimising for MACC drawdown — see below


Read the table top to bottom and the rule of thumb emerges: **private offers are for deals, private plans are for standing arrangements.** If the pricing is the outcome of a negotiation, it is an offer. If the pricing is a policy you apply to a known tenant, it is a plan.


---


## **The reason the customer cares at all**


Most enterprise buyers are not asking for a private offer because they like the mechanism. They are asking because purchases made through Microsoft’s marketplace can draw down their **Microsoft Azure Consumption Commitment (MACC)** — money they have already promised Microsoft they will spend.


That is the actual centre of gravity in an Azure deal, and it changes which internal conversation the customer has to win.[What is a MACC?](https://www.suger.io/resources/blog/what-is-a-macc-azure-committed-spend-explained/) explains the drawdown mechanics and why it moves procurement timelines.


Which is also why the construct choice matters more than it looks. Reaching for a private plan because it seemed simpler, on an offer type that does not support one, restarts a procurement conversation that was nearly finished.


---


## **Operating all this without a Partner Center tab open**


The constructs are Microsoft’s; the operational load is yours. Each private offer has an expiry to track, each private plan has a tenant list to maintain, and every accepted deal has to reconcile back to the opportunity in your CRM and the revenue in your books.


Suger creates and tracks private offers across every connected marketplace from one place — including Microsoft — so a deal desk works one queue instead of one console per cloud, and accepted offers sync back to Salesforce or HubSpot without re-keying.[Private offer automation](https://www.suger.io/platform/private-offers/) covers the offer lifecycle, and the[Azure Marketplace seller solution](https://www.suger.io/solutions/microsoft-marketplace/) covers the Microsoft-specific setup.


---


## **Frequently asked questions**


**What is the difference between an Azure private offer and a private plan?** A private offer is a time-bound custom deal sent to one customer’s billing account, and can carry a custom EULA. A private plan is a hidden plan inside a public listing, scoped by tenant ID, that inherits the public listing’s EULA.


**Can I create a private plan for a consulting services offer?** No. Private plans are only available for paid products that can be purchased and installed directly from the Azure portal. Consulting services, “Contact me” offers, and free services are excluded.


**Which one lets me attach custom legal terms?** Only a private offer. When you create one, you can attach a custom end user licence agreement for that specific transaction. A private plan uses the EULA already on your public listing.


**How does a customer get targeted for each?** A private plan uses the customer’s tenant ID. A private offer is aligned to the customer’s billing account. Confirm which identifier your customer can provide before you build the offer.


**What is a multiparty private offer?** A multiparty private offer works like a private offer, except a channel partner sells it to the customer. The customer buys each product through Microsoft Marketplace, and Microsoft invoices and collects payment.


**Do private offers count toward a customer’s MACC?** Marketplace purchases are what enterprise buyers use to draw down a Microsoft Azure Consumption Commitment. It is usually the reason they asked for a marketplace transaction in the first place.


---


## **Takeaways**


- Private offers are for negotiated deals; private plans are for standing custom pricing on known tenants.
- Only a private offer can carry a custom EULA. If the customer has redlines, the choice is already made.
- Private plans don’t exist for consulting services, “Contact me” offers, or free services — check the offer type before promising one.
- Confirm whether the customer can give you a tenant ID or a billing account. That identifier decides the construct.
- Use a multiparty private offer when a partner transacts, not when you do.


---


Three constructs, one deal desk. See how[private offer automation in Suger](https://www.suger.io/platform/private-offers/) builds and tracks Microsoft offers alongside every other marketplace you sell on.
