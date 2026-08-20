---
schema_version: "1.0.0"
document_id: "b3a6b547f71be408df02f8e91dcd319747ffef1a8f8cb0e6f2df3955161ef930"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/resellers-on-google-cloud-marketplace/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T06:20:29.574702+00:00"
fetched_at: "2026-08-07T06:20:30.243690+00:00"
content_hash: "sha256:8d218f0118a1651185efe1e26674b69036d7dfd6e77e724ca86f9dec885574d5"
---

# Bringing Resellers Into Google Cloud Marketplace

*Google Cloud Marketplace supports reselling through **reseller private offer plans** — pre-approved templates an ISV publishes to a specific reseller, which the reseller then uses to create customer offers without coming back to you for each deal.*


---


A partner manager who has built a working reseller motion on AWS arrives at Google Cloud expecting to find CPPO with a different logo. It isn’t there. The concept survives — a partner transacts with your customer and takes a margin — but the mechanism is genuinely different, and the difference sits in a place that changes how you operate.


On AWS, you authorise a partner and then create a base offer per deal. On Google Cloud, you publish a **plan** the reseller accepts once, and the reseller creates offers from it. The per-deal work moves off your desk and onto theirs.


Here’s how to set it up, and an honest map of what transfers from your AWS motion and what has to be rebuilt.


---


## **How does reselling work on Google Cloud Marketplace?**


Reselling on Google Cloud Marketplace works through reseller private offer plans: the ISV turns on reselling for a published product, creates a plan naming a reseller and the commercial terms, and the reseller accepts it. From that point the reseller can create private offers for customers within the boundaries the plan sets.


The plan is the unit of control. It carries the product, the pricing model, the billing frequency, the fixed discount the reseller receives, the dates it is valid between, and the legal terms every offer created under it inherits. Once accepted, you are not in the per-deal path.


If Google Cloud Marketplace itself is new territory,[the ISV seller’s guide to Google Cloud Marketplace](https://www.suger.io/resources/blog/google-cloud-marketplace-isv-seller-guide/) covers listing, Producer Portal, and the direct private-offer motion first.


---


## **Setting it up: what the ISV configures**


Four things happen before a reseller can sell anything.


**1. Turn on reselling for the product.** Reselling is enabled per published product, not account-wide. A product that is not enabled has no reseller path regardless of what your partner agreement says.


**2. Configure access control.** Viewing, creating, and managing reseller discounts in Producer Portal is permission-gated. Sort the roles out before the first plan, because discovering it mid-deal means going to a project owner for access while a reseller waits.


**3. Choose the plan’s scope.** This is the decision with the longest tail:


- **Single-use** — one reseller offer to one specific customer. The plan targets that customer’s Cloud Billing subaccount.
- **Multi-use** — the same reseller can create multiple offers to multiple customers. The plan targets the reseller’s main billing account.


Single-use gives you approval on every deal. Multi-use gives the reseller autonomy and gives you a channel that scales without you. Most programs start single-use and never revisit it, which quietly caps the motion.


**4. Set the commercial and legal terms.** The plan specifies the product, pricing model, billing frequency, and — crucially — the **fixed reseller discount** for that plan. It also carries the end user licence agreement that applies to every offer created under it. Both are set at plan level, not per deal.


The reseller then accepts. Google provisions the plan and confirms acceptance by email, and the plan moves through its own lifecycle: draft, ready to accept, accepted, ready to use, used, and then expired, completed, or rejected. Those statuses are your pipeline view of the channel — if nobody is watching them, plans sit in “ready to accept” indefinitely and everyone assumes the other side is blocked.


---


## **What transfers from AWS CPPO, and what doesn’t**


AWS CPPO Google Cloud reseller private offer plans


**Who authorises the partner** ISV ISV


**Per-deal ISV work** Create a base offer for each deal None once the plan is accepted


**Unit of control** The individual base offer The plan


**Where margin is set** Per offer Fixed discount on the plan


**Reusability** Per-deal by default Multi-use plans cover many customers


**Legal terms** Attached per offer Attached to the plan; inherited by every offer under it


**Who the plan targets** The partner Customer billing subaccount (single-use) or reseller main account (multi-use)


**Partner acceptance step** Partner accepts the base offer Reseller accepts the plan, once


**Visible lifecycle** Offer status Plan status: draft → ready to accept → accepted → ready to use → used


**What transfers unchanged:** your partner agreements, your margin policy, your deal registration process, and your reconciliation obligations. Nothing about the commercial relationship changes.


**What has to be rebuilt:** the operating cadence. A CPPO motion is a per-deal workflow with a deal desk in the middle. A Google plan motion is a *catalogue* of standing authorisations you maintain — plans that expire, discounts that need revising, and resellers who need a new plan when terms change. If your team’s channel process is “wait for a partner to ask for an offer,” there is nothing on the Google side for that process to attach to.


For the wider cross-cloud picture,[CPPO vs MPO vs reseller offers](https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/) sets all three mechanisms side by side, and[accelerating resell growth with CPPO](https://www.suger.io/resources/blog/accelerating-resell-growth-with-cppo-in-the-cloud-marketplace/) covers the AWS motion in depth.


---


## **The four things that break a reseller program here**


- **Plans that expire quietly.** Every plan has a validity window. An expired plan looks to the reseller like your product stopped being sellable.
- **Discounts set once, never revisited.** The reseller discount is fixed on the plan. If your margin policy changes, existing plans do not follow — you issue new ones.
- **Single-use by default.** Every deal needing a fresh plan means your channel throughput equals your deal desk’s throughput, which is exactly what a channel is supposed to fix.
- **No link to the transaction.** A reseller closes a customer; your CRM shows nothing. Partner-sourced revenue then can’t be measured, and commissions settle on estimates instead of transactions.


That last one is the structural problem, and it is the same one on every cloud: the deal your partner registered and the transaction that fulfils it sit in different systems.[Deal registration software for ISVs](https://www.suger.io/resources/blog/deal-registration-software-for-isvs/) maps where those links break.


Suger manages resale offers across marketplaces from one place, so a Google reseller plan, an AWS CPPO, and a Microsoft multiparty offer are the same object in your pipeline — with the partner, the margin, and the closing transaction on one record.[Resale offer management](https://www.suger.io/platform/resale-offers/) covers the mechanics; the[Google Cloud Marketplace seller solution](https://www.suger.io/solutions/gcp-marketplace/) covers the Google side end to end.


---


## **Frequently asked questions**


**Does Google Cloud Marketplace support resellers?** Yes. ISVs turn on reselling for a published product and publish reseller private offer plans. Once a reseller accepts a plan, they create private offers for customers within the terms the plan sets.


**What is a reseller private offer plan?** It’s a pre-approved template an ISV creates for a specific reseller. It carries the product, pricing model, billing frequency, fixed reseller discount, validity dates, and the EULA that applies to every offer created under it.


**What’s the difference between single-use and multi-use plans?** A single-use plan covers one reseller offer to one customer and targets that customer’s billing subaccount. A multi-use plan lets the reseller create offers for multiple customers and targets the reseller’s main billing account.


**Is Google’s reseller model the same as AWS CPPO?** No. On AWS you build a base offer per deal. On Google Cloud you publish a plan the reseller accepts once and then sells from, so per-deal work moves to the reseller.


**Where is the reseller’s margin set?** On the plan, as a fixed reseller discount. It applies to every offer created under that plan, so changing your margin policy means issuing new plans rather than editing existing ones.


**How do I know a reseller has accepted a plan?** Google provisions the plan on acceptance and sends a confirming email. The plan status also moves from “ready to accept” to “accepted,” which is worth monitoring as a channel pipeline stage.


---


## **Takeaways**


- Google’s reseller motion runs on plans, not per-deal base offers. Publish once, and the reseller sells from it.
- Choose scope deliberately: single-use keeps you in every deal, multi-use lets the channel scale past your deal desk.
- Margin and legal terms are set at plan level. Policy changes mean new plans, not edits.
- Watch plan statuses as a pipeline. Plans stuck at “ready to accept” are a channel stalled on nobody.
- What transfers from AWS is the commercial relationship. What must be rebuilt is the operating cadence.


---


One channel, three mechanisms. See how[resale offer management in Suger](https://www.suger.io/platform/resale-offers/) keeps Google reseller plans, AWS CPPOs, and Microsoft multiparty offers in a single partner pipeline.
