---
schema_version: "1.0.0"
document_id: "c62cf55e990b697a68b6e27a1faf87bafa003ef0905efc74e02d5c63e175d71f"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/google-cloud-private-offers-setup-and-pitfalls/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:76b451db1696df3b0f74b566ee5d485a344c22754b96c18e41d7c9322cacb728"
---

# Google Cloud Private Offers: Setup and Pitfalls

*A Google Cloud Marketplace private offer is a custom-priced, non-public deal an ISV sends to one customer through Producer Portal. Most first attempts fail for reasons on the buyer’s side — a free-trial billing account, a hidden listing, or a value above the limit their account type allows.*


---


Sending your first Google Cloud private offer feels straightforward right up until the customer says they can’t accept it.


That’s the pattern worth planning for. The offer builds fine. The pricing is right. The failure surfaces at the moment of acceptance, in the buyer’s billing account, at the end of a negotiation — and by then it reads to the customer as your product being difficult to buy.


The prerequisites and blockers below are all knowable in advance. Check them before you build the offer, not after the customer clicks.


---


## **How do you create a private offer on Google Cloud Marketplace?**


You create a Google Cloud Marketplace private offer in **Producer Portal** , in five steps: create a new offer, add the customer and product details, set the pricing, specify the legal terms, then review and publish it to the customer.


The mechanics are unremarkable. What trips teams up is everything that has to be true *before* step one — permissions on your side, and account conditions on the buyer’s.


If you’re earlier than this and still getting the listing itself right,[the ISV seller’s guide to Google Cloud Marketplace](https://www.suger.io/resources/blog/google-cloud-marketplace-isv-seller-guide/) covers listing and Producer Portal setup first.


---


## **Prerequisites on your side**


**Permissions.** Creating and managing private offers in Producer Portal is role-gated. If you don’t have the roles, you’ll need a Project Owner to grant them — which is a two-day detour if you discover it on a Friday. Attaching a **custom end user licence agreement** needs more: the Commerce Price Management Private Offers Admin role, or equivalent permissions. Sort both out before your first deal, not during it.


**A live listing.** The product must be genuinely live on the marketplace. A listing that is still hidden in Producer Portal cannot carry a private offer, and this is the single most common self-inflicted blocker.


**Entitlements for SaaS.** For SaaS products, entitlements must be set up to track usage. Without them you have an offer that sells something your systems can’t provision or measure.


---


## **The buyer-side conditions that block acceptance**


These are the ones that surface late, because they sit inside an account you can’t see.


Condition What happens How to check


**Free trial billing account** Private offers cannot be accepted on a free trial account Ask the customer whether their billing account is still on a free trial before you build


**Listing hidden in Producer Portal** The offer has no live product to attach to Confirm the listing is published and publicly visible


**Prepay + Brazil-based billing account** Prepay payment types are unavailable to billing accounts based in Brazil Check the Private Offers tab in Producer Portal


**Self-serve or Online billing account over the limit** Private offers on self-serve or online billing accounts are capped at $250k Ask which account type the buyer holds, and the offer value, early


**Wrong billing account identified** The offer goes to an account the buyer’s procurement doesn’t control Get the billing account ID in writing, not verbally


The $250k ceiling is the one to internalise. A buyer on a self-serve billing account who wants a larger deal isn’t blocked from buying — they need to move to an account type that supports it, and that is a conversation with their Google account team. Discovering it at signature costs a week; discovering it at qualification costs nothing.


---


## **The pre-flight checklist**


Run this before you build the offer.


- The listing is live and publicly visible — not hidden in Producer Portal
- Your user has the Producer Portal roles to create and publish offers
- If a custom EULA is needed, you have the private offers admin permissions for it
- For SaaS, entitlements are configured to track usage
- You have the customer’s **billing account ID** , confirmed in writing
- The billing account is not on a free trial
- You know the account type — and if it’s self-serve or online, the deal is under $250k
- If using prepay, the billing account is not based in Brazil
- Pricing, term, and legal terms are internally approved before publishing


Nine checks, most of which take a single question to the customer. The offers that die at acceptance are almost always the ones where nobody asked.


---


## **Doing it more than a few times a quarter**


Producer Portal is fine for the first offer and tedious by the tenth. Google publishes a **Commerce Producer API** that provides programmatic access to private offer creation and publishing, replacing the manual Producer Portal steps — which matters once offer volume outgrows the patience of whoever owns the console.


The deeper problem isn’t clicks, though. It’s that a Google offer built in Producer Portal is invisible to the rest of your revenue operation: your CRM doesn’t know it exists, your forecast doesn’t include it, and your finance team learns about it at disbursement. Multiply that by each marketplace you sell on and the console-per-cloud model stops being viable — which is the argument in[how to choose a cloud marketplace platform](https://www.suger.io/resources/blog/how-to-choose-a-cloud-marketplace-platform/) .


Suger creates and tracks Google Cloud private offers alongside every other marketplace from one pipeline, with the offer synced back to the CRM opportunity it belongs to.[Private offer automation](https://www.suger.io/platform/private-offers/) covers the lifecycle, and the[Google Cloud Marketplace seller solution](https://www.suger.io/solutions/gcp-marketplace/) covers the Google-specific setup.


---


## **Frequently asked questions**


**How do I create a private offer on Google Cloud Marketplace?** In Producer Portal: create a new offer, add customer and product details, set pricing, specify legal terms, then review and publish. You need the right Producer Portal roles before you start.


**Why can’t my customer accept a Google Cloud private offer?** The usual causes are a billing account still on a free trial, a listing that isn’t publicly live, an offer above the limit for a self-serve or online billing account, or prepay on a Brazil-based billing account.


**Is there a limit on Google Cloud private offer amounts?** Yes. Private offers on self-serve or online billing accounts are capped at $250k. Larger deals require a billing account type that supports them, which the customer arranges with their Google account team.


**Can I attach a custom EULA to a Google Cloud private offer?** Yes, but it requires additional permissions — the Commerce Price Management Private Offers Admin role or equivalent — beyond the roles needed to create a standard offer.


**Do I need entitlements configured before sending an offer?** For SaaS products, yes. Entitlements must be set up to track usage, otherwise you can sell something your systems cannot provision or meter.


**Can private offers be created programmatically?** Yes. Google’s Commerce Producer API gives programmatic access to private offer creation and publishing, replacing manual Producer Portal operations once volume justifies it.


---


## **Takeaways**


- Most Google private offer failures are buyer-side account conditions, not pricing. Qualify them at the start of the deal.
- Confirm the billing account ID in writing, plus its type and whether it’s still on a free trial.
- The $250k ceiling on self-serve and online billing accounts is a deal-shaping constraint, not a footnote.
- Sort Producer Portal roles — including the extra permission a custom EULA needs — before your first offer.
- Once volume grows, move offer creation off the console: Google’s Commerce Producer API exists for exactly that.


---


An offer built in a console is invisible everywhere else. See how[private offer automation in Suger](https://www.suger.io/platform/private-offers/) creates Google Cloud offers and keeps them attached to the deal in your CRM.
