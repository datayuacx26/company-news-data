---
schema_version: "1.0.0"
document_id: "80eea648aefdbc4248886cf83922b8ba3671f478a898366683f224d35983410e"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/selling-on-snowflake-oracle-and-alibaba-cloud/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:956d2944df6f0f19eb5130d0ecc8bf45beaf136794757e21f61d43bc18787af1"
---

# Selling on Snowflake, Oracle and Alibaba Cloud

*Snowflake, Oracle Cloud and Alibaba Cloud each run a marketplace that transacts real software revenue, and none of them works the way AWS, Microsoft or Google Cloud does. The differences are not cosmetic: they change who may sell, how a customer pays, and — in Oracle’s case — how large a deal you are permitted to write.*


---


Most sellers evaluate the fourth marketplace the way they evaluated the second: assume it is the same shape, budget a sprint, discover otherwise.


The three covered here are genuinely different animals. One is a data cloud where the product is a share rather than a deployment. One caps the share of a customer’s commitment you are allowed to consume. One settles monthly and will not pay you at all until a finance form is complete.


Suger transacts on six marketplaces, and these are the three that surprise teams most. Here is what each actually asks of a seller.


---


## **What these marketplaces have in common**


**All three are commitment-adjacent marketplaces: the buyer is spending money they have already promised to that vendor, and your product is a way to spend it.** That is the same economic logic that makes AWS, Microsoft and Google Cloud marketplaces work.


What differs is the gate. On the big three, seller registration is essentially self-service and the friction arrives at listing review. On these three, a human at the provider decides whether you may sell at all — and on Oracle, how much.


---


## **Snowflake: approval before monetization**


Snowflake Marketplace lists data products and native applications rather than deployable software, so the unit being sold is access to a share.


**The account itself is a gate.** Snowflake requires a full Snowflake account — trial accounts and reader accounts cannot publish — plus the` ACCOUNTADMIN` role or equivalent provider privileges, and acceptance of the Snowflake Provider and Consumer Terms.


**Charging money is a second gate.** Before creating a paid listing, Snowflake’s guidance is to “contact your business development partner at Snowflake,” or open a case with Marketplace Operations if you do not have one. This is not a formality — it is the step that gets monetization approved, and it can include a review of go-to-market readiness.


**Payouts run through Stripe.** Providers must “set up a Stripe Express account associated with Snowflake” before revenue can reach them.


**Geography decides eligibility.** Paid listings are available only where the provider’s billing address sits in one of Snowflake’s supported countries — a list that includes Australia, Canada, France, Germany, Ireland, Israel, Italy, Japan, Mexico, the Netherlands, Singapore, the United Kingdom and the United States, among others. A company incorporated outside it can publish free listings and cannot charge.


Listings are then submitted for approval. If one is rejected, Snowflake sends instructions on what to correct, and the listing can be revised and resubmitted.


---


## **Oracle: the 25% rule that shapes your deal**


Oracle Cloud Marketplace sells “business apps and professional services that complement your existing Oracle Cloud implementation,” with three pricing models available to publishers: BYOL, pay-as-you-go, and private offers.


Private offers are where enterprise deals happen, and they carry two prerequisites that catch sellers out.


**You must be admitted to the program.** An active Oracle Partner Network membership and publisher registration are not sufficient. The publisher must also be qualified by Oracle’s marketplace and partnership team specifically for private offers, and request that their Partner Portal account be enabled for it.


**And there is a ceiling on the deal.** Oracle’s prerequisites state that “the customer’s Universal Credits commit credit spending for Marketplace partner products, including the prospective private offer, must be less than 25% of the customer’s active Universal Credits commit credits.”


Read that carefully, because it is a constraint on the *customer* , not on you, and it is cumulative. It counts every marketplace partner product that customer has bought, not just yours. A customer with a small Oracle commitment who has already bought from two other vendors may not have room for your deal at any price — and you will not discover this from your own console.


The practical move is to raise it during qualification rather than at close: ask what the customer’s active commit is and what else they have purchased through the marketplace. A deal that has to be resized after legal review is a deal that slips a quarter.


Oracle notes the limit can be increased with justification and approval, so a large deal is not automatically dead — it is automatically a conversation.


---


## **Alibaba Cloud: settlement is the hard part**


Alibaba Cloud Marketplace is the most straightforward of the three to list on and the most likely to leave you unpaid on a technicality.


Settlement is monthly and automated: after a customer completes an order, earnings are processed through a monthly settlement cycle. The condition attached to it matters more than the cycle. Sellers must update their financial information in the Finance Center of the Seller Portal, because that information “is used for invoice generation and payment settlements” — and incomplete or pending financial information delays the settlement timeline.


In other words, the payout clock does not start when the customer pays. It starts when your finance details are complete. Teams that treat Finance Center as post-launch housekeeping find their first settlement sitting a month behind their first sale.


One more timing rule worth knowing: changes to the listing fee through an updated agreement take effect in the following month’s settlement, not the current one.


---


## **Choosing where to go next**


Snowflake Oracle Alibaba Cloud


What you list Data products, native apps Apps and professional services Software and service products


Gate to sell Full account, ACCOUNTADMIN, terms accepted OPN membership plus publisher registration ISV registration


Gate to charge Business development approval, Stripe payout Private offer program enablement Finance Center completion


Deal-size constraint None published Under 25% of the customer’s active commit None published


Payout Stripe Per Oracle partner agreement Monthly settlement


The sequencing question is usually not “which is biggest” but “where do our customers already have money committed.” A data product belongs where the data already sits. An application that complements an Oracle estate belongs where that estate is. Both beat picking by marketplace size.


If you are still deciding between the larger three, the[cloud marketplaces guide](https://www.suger.io/resources/guides/cloud-marketplaces/) compares them directly, and[one GTM motion across AWS, Azure and Google Cloud](https://www.suger.io/resources/blog/one-gtm-motion-across-aws-azure-google-cloud/) covers the operating model that lets a fourth marketplace cost less than the third did.


---


## **Frequently asked questions**


**Can any company sell a paid listing on Snowflake Marketplace?** No. Paid listings require a billing address in one of Snowflake’s supported countries, a Stripe Express account for payouts, and monetization approval obtained through a Snowflake business development partner or Marketplace Operations.


**What is the limit on an Oracle Cloud Marketplace private offer?** Oracle requires that the customer’s Universal Credits commit spending on marketplace partner products, including the proposed offer, be less than 25% of their active Universal Credits commit credits. It is cumulative across vendors.


**Can the Oracle 25% limit be raised?** Oracle documents that the amount can be increased with justification and approval. Treat it as a conversation to start during qualification, not at signature.


**Why has an Alibaba Cloud settlement not arrived?** Most often because the Finance Center information in the Seller Portal is incomplete. Alibaba Cloud states that incomplete or pending financial information delays the settlement timeline.


**Do these marketplaces support private or negotiated pricing?** Oracle supports private offers as a documented pricing model, and Snowflake supports paid private listings alongside public ones. Terms and approval paths differ from the hyperscaler equivalents.


**Is a Snowflake listing a deployment?** No. Snowflake listings share data products and native applications inside the consumer’s own account rather than provisioning infrastructure, which changes both the integration work and the support model.


---


## **Takeaways**


- These are not smaller versions of the hyperscaler marketplaces. Each has its own gate, and on two of them a human decides whether you may charge.
- Snowflake separates permission to publish from permission to monetize, and paid listings are restricted by the provider’s country of billing.
- Oracle caps a customer’s marketplace partner spending at under 25% of their active Universal Credits commit — cumulative across every vendor, not just yours.
- Qualify the Oracle constraint early. It is a property of the customer’s account that no seller console will show you.
- Alibaba Cloud settles monthly, and the clock starts at Finance Center completion rather than at the sale.
- Pick the next marketplace by where your customers have already committed spend, not by which one is largest.


---


Adding a fourth marketplace should not cost what the second did. See how Suger’s[product listing and offer management](https://www.suger.io/platform/product-listing/) runs the same catalogue, offers and entitlements across every marketplace you sell on.
