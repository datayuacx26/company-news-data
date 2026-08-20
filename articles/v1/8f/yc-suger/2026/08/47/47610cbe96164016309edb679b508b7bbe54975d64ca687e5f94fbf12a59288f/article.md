---
schema_version: "1.0.0"
document_id: "47610cbe96164016309edb679b508b7bbe54975d64ca687e5f94fbf12a59288f"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/aws-standard-contract-or-your-own-paper/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:df9e0cc5c7f5e1f772a0ea5e58dc0fae1723b41f0f000d4a4ec0328bd9029219"
---

# The AWS Standard Contract, or Your Own Paper?

*The Standard Contract for AWS Marketplace is a template AWS built with buyers and sellers that you can use as your EULA instead of your own. It costs nothing, most enterprise buyers have already accepted it, and you can withdraw from it at any time. Nothing here is legal advice — take the mechanics to your counsel and let them make the call.*


---


Counsel’s instinct, correctly, is to use your own paper. That instinct was formed in a world where every deal involved a negotiation, and it is the right instinct in most of them. Marketplace deals are the case where it is worth examining, because the alternative on offer is not “weaker terms” — it is “terms the buyer’s legal team has already read.”


Here is what is actually in the SCMP, what accepting it commits you to, and the cases where your own EULA is still the answer.


---


## **What is the Standard Contract for AWS Marketplace?**


**The SCMP is a contract template AWS developed with the buyer and seller communities that sellers can offer as the EULA for their listing.** AWS describes it as governing “usage and … the obligations of buyers and sellers for digital solutions,” naming server software, SaaS and AI/ML algorithms as examples of what it covers.


Its stated design goal is coverage of the clauses that get argued about: the SCMP “proactively defines common ground across key contractual clauses like use, warranty, indemnification, and governing law.”


You have three options on an AWS listing, and AWS names all three:


- Your own EULA.
- The **Standard Contract for AWS Marketplace (SCMP)** .
- The **Reseller Contract for AWS Marketplace (RCMP)** , for authorising channel partners.


On the product load form the EULA field defaults to a link to the standard agreement, and you can replace it with a link to your own. If you do, AWS requires the link to let customers download the EULA — an Amazon S3 link is the documented example. A terms page behind a login is not sufficient.


---


## **The three addendums**


The part sellers most often miss is that the SCMP is modular. Three optional addendums attach to it, for self-service or private offers:


Addendum Covers


Enhanced Security Addendum Transactions with elevated data security requirements


HIPAA Business Associate Addendum Transactions with HIPAA compliance requirements


Federal Addendum Software purchases involving the US Government


This matters because the most common reason teams reject a standard contract — “our buyers need security and compliance commitments the template doesn’t make” — is often already answered. The Enhanced Security Addendum and the HIPAA BAA exist precisely for the deals that would otherwise trigger your own paper, and they are pre-drafted rather than negotiated.


The Federal Addendum is the one to know about before your first public-sector conversation, not during it.


---


## **What opting in actually commits you to**


AWS states the trade clearly, and it is the single paragraph to put in front of counsel:


> “The EULA is between you and the buyer. Using the SCMP as your EULA is at your discretion. By applying the SCMP to your product listing, you are opting in to the SCMP program. Under this program, AWS may update the SCMP template periodically and may update product listings carrying the terms with the current version. You may withdraw from the SCMP program at any time by replacing the SCMP template with your own EULA.”


Three things follow from it.


**AWS is not a party.** Adopting the template does not put AWS between you and your customer. Your obligations are still yours.


**The template can change under you.** This is the real objection, and it is a legitimate one: AWS may update the template and update listings carrying it to the current version. For most software products that is a feature — the terms stay current without a project. For a product where a specific clause is load-bearing, it is a risk your counsel should price.


**It is reversible.** Unlike Microsoft’s standard contract, which cannot be swapped for your own terms once an offer is published under it, the SCMP can be withdrawn at any time by replacing it with your own EULA. That asymmetry is worth knowing if you sell on both —[EULAs and custom terms in marketplace deals](https://www.suger.io/resources/blog/marketplace-eulas-and-custom-terms/) covers how the clouds differ.


---


## **Private offers: the standard contract is a starting point, not a ceiling**


The assumption that adopting the SCMP means accepting it verbatim on every deal is wrong. AWS’s own description of the private offer path: “buyers can request the SCMP template from the seller, and the terms can be amended to address custom transaction requirements as agreed upon by the parties.”


That gives you a genuinely useful default: the SCMP as the base for self-service and standard deals, amended where a specific enterprise deal requires it. The negotiation starts from a document both sides already know rather than from your paper and their redlines.


Practically, this is the shape that shortens cycle time most. The buyer’s counsel is reviewing a diff, not a document.


---


## **When your own EULA is still the answer**


Four cases where the standard contract is the wrong choice, and they are narrower than most teams assume:


**Your product is not really software.** The SCMP is drafted for digital solutions. A listing that is substantially a service engagement has obligations the template does not contemplate.


**A clause is genuinely load-bearing for your business.** An unusual liability cap, a specific IP arrangement, an export control commitment. If a clause is the reason your product can be sold at all, do not put it on a template that may be updated.


**You have an existing paper relationship that must govern.** Where an enterprise customer already has a negotiated master agreement with you, the marketplace transaction usually needs to reference it rather than replace it.


**Regulatory obligations beyond the addendums.** The addendums cover elevated security, HIPAA and federal. Sectors with their own regimes may need terms none of them provide.


Note what is *not* on that list: “our lawyers prefer our paper,” and “we’ve always used our own EULA.” Both are real, and neither survives contact with a self-service motion where the buyer never speaks to a human.


---


## **The channel case: the RCMP**


If you resell through partners, there is a third template. The **Reseller Contract for AWS Marketplace** is a standardized reseller contract ISVs use “when authorizing channel partners to resell ISV products to AWS Marketplace buyers,” and AWS positions it as reducing redundant legal review when entering a reseller relationship or using the terms for a channel partner private offer.


The mechanics: the ISV uploads the contract to the resale opportunity, and the channel partner views and accepts it. One detail worth internalising — **AWS Marketplace buyers can’t view the RCMP.** It governs you and the partner, not you and the end customer, so it does not answer the end customer’s terms question.[CPPO vs MPO](https://www.suger.io/resources/blog/cppo-vs-mpo-multiparty-private-offers/) covers how the resale constructs differ.


Like the SCMP, it is optional: ISVs “can either attach the RCMP or their own customized contract terms — existing or pre-negotiated — when creating an opportunity.”


---


## **A decision rule you can hand to a deal desk**


Deal shape Path Why


Self-service, public listing SCMP Any legal review kills the self-service motion


Mid-market private offer SCMP, amended if needed Buyer reviews a diff, not a document


Regulated data, security review SCMP + Enhanced Security or HIPAA addendum Pre-drafted, not negotiated


US Government SCMP + Federal Addendum Built for the procurement path


Existing negotiated MSA Your own paper, referencing it The relationship already has terms


Load-bearing bespoke clause Your own paper Do not put it on a template that may update


Channel resale RCMP or your own reseller terms Governs the partner, not the end buyer


Write the rule down once. The value is not in any single row — it is that a deal desk owner can route a deal without escalating, which is where the cycle-time saving actually comes from.


---


## **Frequently asked questions**


**What is the SCMP in AWS Marketplace?** The Standard Contract for AWS Marketplace — a template AWS developed with buyers and sellers that you can offer as your product’s EULA. It covers use, warranty, indemnification and governing law.


**Is AWS a party to my SCMP agreement?** No. AWS states that the EULA is between you and the buyer. Using the SCMP is at your discretion, and it does not put AWS into your customer agreement.


**Can I stop using the standard contract later?** Yes. AWS states you may withdraw from the SCMP program at any time by replacing the SCMP template with your own EULA. Microsoft’s equivalent choice is not reversible after publishing.


**Can SCMP terms be changed for a specific customer?** On private offers, yes. Buyers can request the template from the seller and the terms can be amended to address custom transaction requirements as agreed by the parties.


**Does the SCMP cover HIPAA or government purchases?** Through addendums. AWS publishes an Enhanced Security Addendum, a HIPAA Business Associate Addendum and a Federal Addendum that attach to the SCMP for self-service or private offers.


**What is the RCMP?** The Reseller Contract for AWS Marketplace — an optional standardized template for authorizing channel partners to resell your product. Buyers can’t view it; it governs you and the partner.


---


## **Takeaways**


- The SCMP covers use, warranty, indemnification and governing law, and AWS built it with both buyers and sellers.
- Three pre-drafted addendums — enhanced security, HIPAA and federal — answer the objections that usually push teams onto their own paper.
- Opting in means AWS may update the template and update listings carrying it. That is a benefit for most products and a risk for a few.
- The choice is reversible on AWS: withdraw at any time by replacing the SCMP with your own EULA.
- On private offers the SCMP is a starting point. Terms can be amended by agreement, so the buyer reviews a diff rather than a new document.
- The RCMP standardises the ISV–partner layer for resale. Buyers can’t see it, so it does not answer the end customer’s terms question.


---


Which paper governs is one decision; knowing which version a given customer accepted, two renewals later, is another. See how Suger’s[agreement management](https://www.suger.io/platform/agreements/) keeps the offer, the accepted terms and the resulting contract together on every AWS Marketplace deal.
