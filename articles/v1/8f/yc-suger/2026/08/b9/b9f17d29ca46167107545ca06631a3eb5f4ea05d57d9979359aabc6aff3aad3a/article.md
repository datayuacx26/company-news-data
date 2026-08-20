---
schema_version: "1.0.0"
document_id: "b9f17d29ca46167107545ca06631a3eb5f4ea05d57d9979359aabc6aff3aad3a"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:898f90bb7931b71c1bced693334a96b385afd1f394a9566434b8fc1a62fd6bbb"
---

# CPPO vs MPO: Reselling on the Cloud Marketplaces

*A multiparty private offer is any marketplace construct that lets a channel partner — not the software vendor — transact with the end customer. AWS calls it a Channel Partner Private Offer (CPPO), Microsoft calls it a multiparty private offer (MPO), Google Cloud does it through reseller private offer plans.*


---


An alliances leader with a working AWS channel gets asked the same question every year: can we run this on the other clouds too?


The answer is yes, and the trap is assuming that means running the *same* process three times. The commercial idea is identical everywhere — a partner sells, takes a margin, and the customer transacts through a cloud they already buy from. The mechanics are not identical anywhere. One cloud wants a base offer per deal, one wants a pre-approved plan, and one has a separate reseller program that isn’t a marketplace motion at all.


Nobody in the category has published this side by side, so here it is.


---


## **What is a multiparty private offer?**


A multiparty private offer is a marketplace transaction with three parties instead of two: the software vendor, a channel partner, and the end customer. The vendor authorises the partner and sets the economics; the partner sells to the customer at their own price; the customer buys through the cloud marketplace and draws down committed spend as usual.


The point of the construct is that it keeps two things simultaneously true that are normally in tension: the partner owns the customer relationship, and the transaction still runs on marketplace rails. The customer gets committed-spend drawdown and a procurement path they already approved. The partner gets margin. You get a channel that doesn’t require you to build reseller billing.


---


## **The three mechanisms, side by side**


**AWS — CPPO** **Microsoft — MPO** **Google Cloud — reseller plans**


**Name** Channel Partner Private Offer Multiparty private offer Reseller private offer plan


**What the vendor creates** A base offer, per deal An offer with the partner included A plan the reseller sells from


**Per-deal vendor work** Yes — a base offer each time Yes None after the plan is accepted


**Who creates the customer offer** The channel partner, from your base offer The partner, within the multiparty offer The reseller, from the accepted plan


**Where margin is set** On the base offer In the offer’s partner terms Fixed discount on the plan


**Reuse across customers** Per deal Per deal Multi-use plans cover many customers


**Who bills the customer** AWS Microsoft Google Cloud


**Extra marketplace fee** +0.5% listing fee uplift on CPPO — —


**Committed spend drawdown** Yes (EDP) Yes (MACC) Yes


The columns diverge in one place that matters more than the rest: **where the per-deal work lives.** On AWS and Microsoft, the vendor is in the loop for every transaction. On Google Cloud, the vendor is in the loop once, and the partner is autonomous after that.


That single difference decides how each channel scales. A CPPO motion scales with your deal desk’s capacity. A Google plan motion scales with your partners’ capacity — provided you set the plans up as multi-use, which most teams don’t.[Bringing resellers into Google Cloud Marketplace](https://www.suger.io/resources/blog/resellers-on-google-cloud-marketplace/) covers that setup in detail.


---


## **CSP is not a marketplace resell motion**


Microsoft’s **Cloud Solution Provider (CSP)** program comes up in every one of these conversations, and it is a different thing. In CSP, the partner buys from Microsoft and resells to the customer, owning the billing and support relationship directly. The customer’s invoice comes from the partner.


In a multiparty private offer, the customer’s invoice comes from **Microsoft** , under the customer’s existing billing terms with Microsoft. That is the whole reason the deal draws down their committed spend.


The practical distinction for an ISV:


- **Choose an MPO** when the customer’s motivation is marketplace procurement — committed spend, an approved vendor path, one bill from Microsoft.
- **Choose CSP** when the partner’s managed-services relationship is the product, and the customer wants one throat to choke for the whole cloud estate.


Whether your specific offer type can be transacted through CSP is a Partner Center question, and it varies by offer. Confirm it there rather than assuming; the answer changes what you can promise a partner.


---


## **Build one motion or three?**


Build one commercial motion and accept three mechanical implementations. Concretely:


**Keep identical across clouds:** the partner agreement, the margin policy and its approval thresholds, deal registration and conflict rules, the commission plan and its clawback window, and the definition of partner-sourced versus partner-influenced revenue. None of these are cloud-specific, and letting them fork per cloud is how a channel program becomes three channel programs.


**Accept as different:** the offer construct, the authorisation step, where margin is configured, whether the vendor is in the per-deal path, and the fee treatment. On AWS, note that CPPO products carry a **0.5% uplift on the listing fee regardless of offer type or deployment method** — so a SaaS private offer under $1M in total contract value that would be 3% direct becomes 3.5% through the channel. That is a margin-model input, not a surprise to discover at disbursement.[What a marketplace dollar actually costs you](https://www.suger.io/resources/blog/what-a-marketplace-dollar-costs-you/) works through the full cost stack.


**Watch the same failure everywhere:** the registered deal and the closing transaction living in different systems. Every cloud has its own resell construct, and none of them tells your CRM that a partner closed a customer. That reconciliation is yours, and it’s where commission disputes come from.


Suger runs resale offers across every connected marketplace as one object — the partner, the terms, the margin, and the transaction on a single record — so an alliances team operates one channel pipeline rather than one per cloud.[Resale offer management](https://www.suger.io/platform/resale-offers/) covers how, and[PRM in Suger](https://www.suger.io/prm/) covers the partner side: registration, commission plans, and the partner portal.


---


## **Frequently asked questions**


**What is a multiparty private offer?** A marketplace deal with three parties: the software vendor, a channel partner, and the end customer. The partner sells and takes margin; the customer buys through the cloud marketplace and can draw down committed spend.


**What is the difference between CPPO and MPO?** They’re the same idea on different clouds. CPPO is AWS’s Channel Partner Private Offer, built from a vendor’s base offer per deal. MPO is Microsoft’s multiparty private offer, where Microsoft invoices the customer directly.


**Does Google Cloud Marketplace have a CPPO equivalent?** Functionally yes, mechanically no. Google uses reseller private offer plans: the ISV publishes a plan a reseller accepts once, and the reseller creates customer offers from it without per-deal vendor involvement.


**Is CSP the same as a multiparty private offer?** No. In CSP, the partner buys from Microsoft and bills the customer directly. In a multiparty private offer, Microsoft invoices the customer, which is what lets the purchase draw down their committed spend.


**Does reselling cost more on AWS?** Yes. CPPO products carry a 0.5% uplift on the listing fee regardless of offer type or deployment method, so a SaaS private offer under $1M in total contract value moves from 3% to 3.5%.


**Should we build one channel program or one per cloud?** One commercial program — agreements, margin policy, registration, commissions — with three mechanical implementations. Letting the commercial rules fork per cloud is what turns a channel into three channels.


---


## **Takeaways**


- The commercial idea is the same on every cloud; the construct is not. Map CPPO, MPO, and reseller plans before promising a partner anything.
- Google is the outlier that matters: plans remove the vendor from the per-deal path, which is the only version of this that scales past your deal desk.
- CSP is a reseller program, not a marketplace motion. Who invoices the customer is the tell.
- Price the CPPO uplift into your margin model — 0.5% on top of the standard listing fee, on every channel deal.
- Keep agreements, margin policy, registration, and commissions identical across clouds. Only the mechanics should differ.


---


Three constructs, one channel. See how[resale offer management in Suger](https://www.suger.io/platform/resale-offers/) puts every partner-transacted deal — CPPO, multiparty, or reseller plan — into a single pipeline with the commission that settles it.
