---
schema_version: "1.0.0"
document_id: "1e630ad537a3c1fe9b236cb79268af1db19db862870d7289a0307c2f998b4566"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/one-gtm-motion-across-aws-azure-google-cloud/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T12:03:27.187135+00:00"
fetched_at: "2026-08-16T12:03:28.828961+00:00"
content_hash: "sha256:485cb831c7757a7d5746433f9cdaa4529e6ac1712ada6a330940b94298c8d954"
---

# One GTM Motion Across AWS, Azure and Google Cloud

*Running one go-to-market motion across AWS, Microsoft and Google Cloud means standardising the parts that are genuinely the same — the deal record, the entitlement state, the revenue trail — and refusing to abstract the parts that are not. Teams that get the boundary wrong in either direction pay for it, just differently.*


---


Adding the second marketplace usually costs about what the first did. That is expected, and forgivable.


Adding the third costing the same is the signal that something structural is wrong. By then a team has seen enough of the pattern to have generalised it, and if the cost is flat it means nothing was generalised — each marketplace is a separate programme with its own spreadsheet, its own definition of “active customer,” and its own person who knows how it works.


The opposite failure is quieter and worse: a single abstraction that pretends the three clouds are one, and produces offers that are subtly wrong on two of them.


The useful question is not “can this be unified” but “which layer is actually common.”


---


## **What is the same everywhere**


**Three things generalise cleanly across every marketplace, because they describe your business rather than the provider’s process.**


**The deal record.** A negotiated deal has a customer, a value, a term, a set of entitlements and an approval state. None of that is cloud-specific. If your CRM holds one shape of marketplace deal, every marketplace can be generated from it.


**Entitlement state.** What a customer is entitled to right now is a property of your product, not of the marketplace that sold it. Every cloud gives you a way to read authoritative state; the answer they return means the same thing. Build one internal model and populate it from three sources.


**The revenue trail.** Booked, invoiced, disbursed, reconciled. The vocabulary and the timing differ per cloud, but the questions finance asks do not. One normalised ledger, three adapters.


Standardise these and the third marketplace genuinely is cheaper than the first, because the only new work is an adapter.


---


## **What never abstracts**


**The acceptance path is different on each cloud, in ways that change what you promise a customer.** This is where a single abstraction quietly lies.


**AWS: the offer is the unit, and change means replacement.** An agreement is an accepted offer. Changing one means an amendment, and “an amendment replaces a buyer’s current subscription” — remaining scheduled payments are cancelled and replaced by whatever schedule the amendment carries. There is one buyer-facing act: accept.


**Microsoft: four acts, four failure points.** Accept, purchase, subscribe, activate. Each needs different permissions. Purchasing requires “an Azure subscription owner or contributor role for a subscription under the billing account of the private offer,” and the offer itself “aligns to your billing account.” There is a real delay in the middle — 15 to 60 minutes after acceptance before the Purchase button enables — and a real deadline at the end, with buyers asked to ensure “the partner activates your SaaS subscription within 30 days after purchase.”


**Google Cloud: the purchase sits alongside the customer’s cloud spending.** “For most configurations, your customers receive one bill for all of your products and services, as well as the Google Cloud services that they use,” across SaaS, VM images, Kubernetes apps, data products and professional services.


Two defaults differ enough to matter commercially. Microsoft SaaS subscriptions have auto-renew **off** by default. AWS public offers auto-renew, but “if you amend an accepted public offer, it becomes a private offer and no longer auto-renews.” A renewal forecast built on one cloud’s default will be wrong on the other.


The co-sell layer diverges further still —[how co-sell works](https://www.suger.io/resources/blog/how-co-sell-works/) covers the three programmes separately for exactly that reason.


---


## **Where to put the seams**


The design that holds is a common core with thin, honest adapters:


Layer Common or per-cloud


Deal record and approval Common


Offer construction Common inputs, per-cloud rendering


Acceptance and activation Per-cloud, always


Entitlement state Common model, per-cloud source


Metering submission Per-cloud, with common validation


Revenue and reconciliation Common ledger, per-cloud adapter


Renewal tracking Common, with per-cloud defaults encoded


The rule that keeps this honest: **an adapter may translate, but it may never invent.** If Microsoft requires a billing account identifier and your common deal record has no field for it, the fix is a field on the deal record — not a default inside the adapter. Defaults inside adapters are how a team ends up with offers targeted at the wrong tenant and no idea which layer chose it.


---


## **The four questions that reveal whether you have one motion**


Ask these of your own team. Each takes a minute to answer if the model is unified and a week if it is not.


1. **How many marketplace agreements end in the next 90 days, across everything?** If the answer requires three exports, renewals are three programmes.
2. **What is the total value of what we sold through marketplaces last quarter?** If each cloud reports a different definition of “sold,” you have three ledgers.
3. **Which customers are entitled to the enterprise tier right now?** If the answer depends on which cloud they bought through, the entitlement model is per-cloud.
4. **Who owns the fourth marketplace?** If nobody can start one without a new headcount, the model has not generalised.


Suger transacts on six marketplaces, and the reason the sixth is not six times the work is entirely this: one deal record, one entitlement model, one ledger, and adapters that translate rather than decide.


---


## **The sequencing that works**


**Normalise before you automate.** Automating three different processes gives you three automations to maintain. Agree what a marketplace deal *is* first.


**Make entitlement state authoritative before anything reads it.** Every downstream consumer — provisioning, support, renewals, finance — should read one model.[Entitlement management on cloud marketplaces](https://www.suger.io/resources/blog/marketplace-entitlement-management/) covers the state machine underneath it.


**Encode the per-cloud defaults explicitly.** Auto-renew behaviour, activation deadlines and permission requirements belong in code with a comment naming the cloud, not in a person’s memory.


**Reconcile across all of them on the same schedule.** Drift found per-cloud is three investigations; drift found against one ledger is one.[Marketplace revenue reconciliation](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) covers the mechanics.


---


## **Frequently asked questions**


**Can one process really cover every cloud marketplace?** The deal record, entitlement model and revenue ledger can be common. Acceptance, activation and metering cannot — each provider implements them differently, and pretending otherwise produces offers that are wrong on at least one cloud.


**What is the most common mistake when unifying?** Putting defaults in the adapter. When a cloud requires a field the common model lacks, add it to the model rather than letting the adapter choose a value nobody can trace.


**Do renewals behave the same across clouds?** No. Microsoft SaaS subscriptions default to auto-renew off, and on AWS an amended public offer becomes a private offer and stops auto-renewing. Encode each default rather than assuming one.


**Why does the third marketplace cost as much as the first?** Because nothing was generalised after the first two. If each marketplace has its own spreadsheet and its own definition of an active customer, every addition is a new programme.


**What should be standardised first?** Entitlement state. It has the most downstream consumers — provisioning, support, renewals and finance — so unifying it pays back fastest.


**How do you tell whether the motion is really unified?** Ask how many agreements end in the next 90 days across every marketplace. If answering needs three exports, it is not one motion.


---


## **Takeaways**


- The deal record, entitlement state and revenue ledger generalise cleanly. Standardise those and the next marketplace is an adapter.
- Acceptance and activation never generalise. AWS has one buyer act, Microsoft has four with different permissions, Google Cloud folds the purchase into the customer’s cloud bill.
- Renewal defaults differ by cloud, so a single forecast assumption will be wrong somewhere.
- An adapter may translate but must never invent. Missing fields belong on the common model.
- Unify entitlement state first — it has the most downstream consumers.
- If answering “what ends in 90 days” takes three exports, you have three programmes rather than one motion.


---


The point of one motion is that the next marketplace is a decision, not a project. See how[the Suger platform](https://www.suger.io/platform/) runs listings, offers, entitlements and revenue across every marketplace from one model, with the per-cloud differences handled where they belong.
