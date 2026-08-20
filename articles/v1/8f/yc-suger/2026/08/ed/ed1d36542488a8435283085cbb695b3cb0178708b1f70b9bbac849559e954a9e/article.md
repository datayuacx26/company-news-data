---
schema_version: "1.0.0"
document_id: "ed1d36542488a8435283085cbb695b3cb0178708b1f70b9bbac849559e954a9e"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/cloud-marketplace-pricing-models/"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-10T02:28:43.687936+00:00"
fetched_at: "2026-08-10T02:28:45.704186+00:00"
content_hash: "sha256:88d4e86a9b0c49d64f6ed06bf3ee40fe01f2ebc69d5efc0a9722dd958c94babc"
---

# Cloud Marketplace Pricing Models: How to Choose

*A cloud marketplace pricing model is the billing structure a marketplace applies to your listing — subscription, usage-based, contract, per-user, or a combination. Each marketplace supports a different set, uses different names for similar things, and treats the choice as permanent once the offer is published.*


---


You can change your price on a cloud marketplace. You generally cannot change your pricing *model* .


That asymmetry is the most consequential thing an ISV learns too late. Microsoft: “After you publish your offer, you can’t change the pricing model.” AWS: “Once you create your listing and publish it to limited, you can’t change the pricing model.” The decision you make in a Partner Center form in week two governs how you can sell for the life of that listing.


What follows is the cross-cloud view — every model each marketplace actually supports, what each is called, and how to pick. Everything here comes from the marketplaces’ own published documentation.


---


## **What pricing models do cloud marketplaces support?**


The three major marketplaces support overlapping but non-identical model sets, and the vocabulary diverges more than the mechanics do.


Model shape AWS Marketplace Microsoft Marketplace Google Cloud Marketplace


Free SaaS free (all dimensions $0.00) Get it now (Free) listing Free


Pure consumption SaaS subscriptions (hourly metering) Not available as a standalone model Usage-based pricing


Fixed recurring fee — (use a monthly contract) Flat rate Subscription-based pricing


Per-seat A contract dimension priced per user Per user A usage metric priced per seat


Committed term, paid upfront SaaS contracts Flat rate or per user, with a contract duration Subscription-based pricing


Committed term plus overage SaaS contracts with pay-as-you-go Flat rate with metered dimensions Combined pricing


Three structural differences fall out of that table.


**Microsoft has the narrowest choice and the strictest rule.** SaaS offers use “one of two pricing models with each plan: either *flat rate* or *per user* ,” and “all plans in the same offer must use the same pricing model. For example, an offer can’t have one plan that’s flat rate and another plan that’s per user.” If you want to sell seats to one segment and site licences to another, that is two offers.


**Google Cloud names the hybrid explicitly.** Its combined model is defined as: “Customers pay a base subscription fee for using your software, and additional charges based on their usage.” Two constraints come with it — the base fee cannot be zero, and at least one usage tier must be paid rather than free.


**AWS separates the model by which API you integrate** , not by how the invoice looks. Its contract model verifies entitlements and never meters; its subscription model meters hourly and bills in arrears. That distinction, and the engineering it implies, is covered in[AWS Marketplace contract pricing vs usage pricing](https://www.suger.io/resources/blog/aws-marketplace-contract-vs-usage-pricing/) .


---


## **Term length is a pricing decision too**


Once you have chosen a model, the term structure is the next irreversible-ish choice, and the marketplaces differ sharply.


**Microsoft** offers contract durations of 1-month, 1-year, 2-year, 3-year, 4-year, and 5-year, each with a defined billing frequency: one-time upfront, monthly equal payments, annual equal payments, or a flexible schedule. Three caveats worth knowing before you fill the form in:


- “A given plan supports only one billing frequency for a contract duration. To offer an additional billing frequency for the same contract duration, create another plan.” Monthly *and* upfront on the same one-year term means two plans.
- Flexible billing schedules are “applicable with a private offer only,” and only for durations of one year or longer.
- “Private plans don’t support 4-year and 5-year contract durations.”
- “The contract durations and billing frequencies made available cannot be removed later” — you can add options, not withdraw them.


**AWS** offers monthly, 1-year, 2-year, and 3-year contracts publicly, and custom durations in months up to 144 months on a private offer.


**Google Cloud** prorates partial months on subscription pricing and lets sellers update pricing after 30 days of publication, with “price decreases effective immediately upon approval and price increases taking an additional 45 days.” Plan a price rise a quarter ahead, not a fortnight.


---


## **How to choose: a decision framework**


Work through these in order. The first four are about the product; the last is about your team.


**1. Does the customer’s usage vary enough that they would refuse a commitment?** If yes, you need a usage-based model — AWS SaaS subscriptions, Google Cloud usage-based, or Microsoft flat rate with metered dimensions. If no, a committed term will close faster and forecast better.


**2. Can you measure the thing you want to charge for, hourly, reliably?** This is the question that should stop most usage-based ambitions. AWS bills subscriptions purely from records you transmit, and unreported usage is unbilled revenue. If your telemetry is best-effort, price on something you can count with certainty — seats, environments, tiers.


**3. Does the buyer need the deal to draw down committed cloud spend?** Committed-spend programs reward a single, large, predictable transaction. A committed term paid upfront maps cleanly onto an[AWS EDP](https://www.suger.io/resources/blog/aws-edp-what-sellers-need-to-know/) or a[MACC](https://www.suger.io/resources/blog/what-is-a-macc-azure-committed-spend-explained/) drawdown in a way that a variable monthly usage bill does not.


**4. Do you need a floor and an upside?** Then you want the hybrid: AWS contracts with pay-as-you-go, Microsoft flat rate with metered dimensions, or Google Cloud combined pricing. Every marketplace supports the shape; each meters it differently, and the overage logic is where the billing bugs are.


**5. Will you sell on more than one marketplace?** This is the one people skip. If you list on AWS, Microsoft, and Google Cloud, you have chosen three permanent, differently-named, differently-metered pricing models that your finance team has to reconcile into one revenue number. Aligning the *commercial* shape across all of them — even where the marketplace vocabulary differs — is what keeps that reconciliation possible.


---


## **The mistakes that cost the most**


**Picking per-user on Microsoft because the sales deck says per-user.** Per user locks the whole offer. If an enterprise later wants an unlimited site licence, that is a second offer, a second listing, and a second procurement conversation.


**Assuming a hybrid is “just” a contract plus metering.** In a hybrid, two billing paths run against one customer simultaneously. Meter total usage rather than the delta above entitlement and you bill twice for what the customer already paid.


**Treating a free trial as a pricing model.** It is a plan setting. Microsoft supports trials of 1 to 180 days on transactable plans, and “the trial subscription automatically converts to a paid subscription unless the customer cancels before the trial period ends or disables auto-renew for the subscription.” What happens at conversion is covered in[free trials on cloud marketplaces](https://www.suger.io/resources/blog/free-trials-on-cloud-marketplaces/) .


**Forgetting the marketplace fee sits on top.** Microsoft describes an agency model “whereby publishers set prices, Microsoft bills customers, and Microsoft pays revenue to publishers while withholding an agency fee.” Fee treatment varies by marketplace, offer type, and deal size —[what a marketplace dollar actually costs you](https://www.suger.io/resources/blog/what-a-marketplace-dollar-costs-you/) works through the arithmetic.


---


## **Frequently asked questions**


**What is a cloud marketplace pricing model?** It is the billing structure a marketplace applies to your listing — free, usage-based, subscription or flat rate, per user, committed contract, or a combination of a committed fee plus metered overage. Each marketplace supports its own set.


**Can I change a marketplace pricing model after publishing?** Generally no. Microsoft states the pricing model can’t be changed after the offer is published, and AWS states it can’t be changed once the listing is published to limited. Changing means a new listing.


**What pricing models does Microsoft Marketplace support for SaaS?** Two: flat rate and per user. Flat rate plans can add metered dimensions through the Marketplace metering service. All plans in the same offer must use the same pricing model.


**What pricing models does Google Cloud Marketplace support?** Four: free, subscription-based (a flat monthly rate, prorated for partial months), usage-based (billed on metrics you define), and combined (a base subscription fee plus usage charges).


**Which pricing model is best for enterprise deals?** Usually a committed term paid upfront or on a schedule. It gives procurement a fixed number, forecasts cleanly, and draws down committed cloud spend in a single predictable transaction.


**Do I have to use the same pricing model on every marketplace?** No, and you often can’t — the supported models differ. Aim to keep the commercial shape consistent even where the marketplace terminology and metering differ, so revenue still reconciles.


---


## **Takeaways**


- Price is editable; the pricing model is not. AWS and Microsoft both freeze it at publication.
- Microsoft SaaS offers pick flat rate *or* per user, and every plan in the offer must match.
- Google Cloud names the hybrid “combined pricing” — base fee plus usage, with a non-zero base and at least one paid usage tier.
- Term structure carries its own locks: Microsoft billing frequencies can be added but not removed, and private plans exclude 4- and 5-year durations.
- Choose usage-based only if you can meter reliably every hour. Otherwise price on something you can count.
- Selling across marketplaces means operating several different models at once. Keep the commercial shape aligned so revenue reconciles.


---


Pricing models are a per-marketplace decision with a company-wide consequence. See how[product listing in Suger](https://www.suger.io/platform/product-listing/) keeps offers, dimensions, and terms consistent across every marketplace you sell on — from one definition rather than three consoles.
