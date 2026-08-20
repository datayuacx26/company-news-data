---
schema_version: "1.0.0"
document_id: "b0b5a889c20c4938eb1e69c53e62bb837013a35acfc79f36ac4f45e8626a6641"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/packaging-software-and-services-in-one-offer/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:e540e039435712ba76da14fa39104737b62bc9e23ccfec403e1ef3345e0bb016"
---

# Packaging Software and Services in One Offer

*An AWS Marketplace offer set is a container that groups two to seven private offers so a buyer can accept all of them in a single action. It is how a licence and the implementation work that makes it useful reach procurement as one decision — and the rules it enforces are almost all checked at release, not at creation.*


---


The deal your customer wants is usually not the deal your listing describes. They want the software and the people who will make it work: the licence, the migration, the twelve weeks of implementation, sometimes the premium support that follows.


Historically that meant two purchases. The software went through the marketplace, the services went through a statement of work and a separate procurement cycle, and the two arrived on different weeks with different paperwork. The customer, who had made one decision, experienced two.


Offer sets close that gap. They are worth understanding precisely, because their constraints are unusually rigid.


---


## **What is an offer set?**


**An offer set is a container that groups 2 to 7 private offers into a single transactable package, which a buyer accepts in one action.** AWS describes the outcome as “coordinated private offers where customers can review and accept all components with one-time approval.”


The important structural detail is what does *not* get merged:


> “Each offer within an offer set maintains its own distinct pricing, payment terms, duration, and End User License Agreement (EULA), while the offer set provides a unified discovery and acceptance experience for buyers.”


And after acceptance, the set dissolves into its parts. It “creates separate agreements for each product, allowing independent management after purchase.” So the buyer signs once and you manage several agreements — which is the right trade, but it means your downstream systems see three agreements, not one bundle.


---


## **Why services and software belong in the same set**


Professional services on AWS Marketplace are already private-offer-only. AWS’s model is that you publish a product describing the services, then “negotiate with customers on price, scope of work, and payment terms, then create a private offer for your services,” creating one “for each customer and project.”


That is the same motion as a negotiated software deal, which is why the two combine cleanly. Products must sit in one of the recognised categories — assessments, implementation, managed services, premium support, or training — so the shape of what you are selling is already defined.


There is a commercial reason to keep them as separate offers inside one set rather than wishing they were a single line. **The listing fees differ, substantially.**


What is sold AWS listing fee


SaaS, public offer 3%


Private offer under $1M 3%


Private offer $1M to under $10M 2%


Private offer $10M or more 1.5%


Any renewal 1.5%


Professional services, private offer 0.5%


Channel partner private offer +0.5% uplift


Fees are calculated on pre-tax total contract value. A regional uplift can apply on top — buyers in South Korea add 1%.


The consequence is worth stating plainly: services carried inside a software offer would be charged at the software rate. Kept as their own offer within the set, they are charged at 0.5%. The structure that gives the buyer one signature also happens to be the structure that prices correctly.


---


## **The rules, and when they are enforced**


This is the part that catches teams, because most of the validation happens at **release** , after everything has been built.


Before an offer set can be released, AWS requires that it contains between 2 and 7 offers, and that all associated offers are:


- In` Released` state
- Active
- Using the same currency
- Targeting the same buyer AWS account identifiers
- Carrying identical expiration dates


Plus two more: only one offer per product, and only buyer-targeted offers. Buyer notes are mandatory — releasing without them fails with` MISSING_BUYER_NOTES` .


The error codes are unusually legible, which helps:


Error What it means


` TOO_MANY_OFFERS` More than 7 offers


` MISSING_OFFERS` Fewer than 2


` INCONSISTENT_OFFER_CURRENCY_CODE` Offers disagree on currency


` INCONSISTENT_OFFER_AVAILABILITY_END_DATE` Offers disagree on expiry


` TOO_MANY_OFFERS_PER_PRODUCT` Two offers for the same product


` DRAFT_OFFERS` An offer was never released


` EXPIRED_OFFERS` An offer’s availability window has passed


**And one constraint has to be right before you start.** The` OfferSetId` on an individual offer is immutable and can only be set when the offer is created. AWS is explicit: “If you need to include an existing offer that doesn’t have the correct` OfferSetId` , you must create a new offer with the correct` OfferSetId` specified.”


You cannot assemble a set from offers you already made. You create the set first, then create every offer against it. A team that builds the software offer on Monday and decides to add services on Thursday is rebuilding the software offer.


---


## **After release, you rebuild rather than edit**


There is no in-place amendment. When a buyer asks for a change to a released offer set, AWS documents a recreation workflow: create a new offer set, clone the offers that are unchanged, create new offers for the ones that changed with the new` OfferSetId` , associate them all, release the new set, then expire the original by setting the availability end date on its offers.


Two related behaviours follow from the same design:


**The set expires as early as its earliest offer.** “The effective expiration of an offer set is calculated as the earliest expiration date among all associated offers.” A services offer with a short window silently shortens the whole package.


**Disassociating hides rather than frees.** A disassociated offer “becomes hidden from buyer discovery until re-associated with the same offer set,” and it cannot be moved to a different one. The` OfferSetId` sticks.


Compare this with a plain private offer, which[can be amended in place](https://www.suger.io/resources/blog/co-terming-and-expanding-marketplace-agreements/) — and note that professional services products do not support amendments at all. Bundling buys the buyer one signature at the cost of your own flexibility later.


---


## **A working sequence**


1. Agree the full commercial package — licence, services, terms, and one shared expiry date — before touching the catalog.
2. Create the offer set entity. Note the` offerset-` identifier.
3. Create every offer with that` OfferSetId` set at creation. Same currency, same buyer accounts, same expiration date.
4. Release each individual offer.
5. Write buyer notes. They are required, and they are what the buyer reads to understand the package.
6. Associate the offers, then release the set.
7. Send it, and give the buyer a window long enough for their approval process — a set that expires mid-approval has to be rebuilt, not extended.


Channel partners are not excluded from any of this. AWS allows ISVs, channel partners and consulting partners to authorise others to resell professional services through channel partner private offers, at the CPPO fee uplift — which is how a systems integrator’s implementation ends up in the same package as your software.[CPPO and multiparty private offers](https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/) covers how those authorisations are structured.


---


## **Frequently asked questions**


**What is an AWS Marketplace offer set?** A container grouping 2 to 7 private offers into one transactable package. The buyer accepts all components in a single action, and each offer keeps its own pricing, terms, duration and EULA.


**Can software and professional services be sold in one offer?** They can be sold in one offer *set* , as separate offers accepted together. That is also the arrangement that charges services at their own 0.5% listing fee rather than the software rate.


**What does AWS charge for professional services?** AWS states that all professional service offerings have a 0.5% listing fee for private offers, calculated on pre-tax total contract value.


**Can an existing offer be added to an offer set?** No. The` OfferSetId` is immutable and can only be set at offer creation, so an existing offer must be recreated to join a set.


**How is an offer set changed after release?** It is not changed, it is recreated. AWS documents creating a new set, cloning unchanged offers, creating new ones for changes, releasing the new set, then expiring the original.


**When does an offer set expire?** On the earliest expiration date among its associated offers, so one short-dated offer shortens the entire package.


---


## **Takeaways**


- An offer set groups 2 to 7 private offers into one buyer acceptance, then dissolves into separate agreements you manage independently.
- Keeping services as their own offer inside the set is what charges them at 0.5% rather than the software rate.
- The` OfferSetId` is set at offer creation and never changes. Create the set before you create a single offer.
- Currency, buyer targeting and expiration date must match across every offer, and this is checked at release.
- There is no in-place edit after release — changes mean rebuilding the set and expiring the old one.
- The package expires as early as its earliest offer, so align expiry dates deliberately rather than by accident.


---


Bundled deals fail on coordination, not on commercial terms. See how Suger’s[private offer and agreement management](https://www.suger.io/platform/agreements/) keeps every offer in a package aligned on currency, targeting and dates before it reaches a buyer.
