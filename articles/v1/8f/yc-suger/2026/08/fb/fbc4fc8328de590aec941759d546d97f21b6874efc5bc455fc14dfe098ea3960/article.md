---
schema_version: "1.0.0"
document_id: "fbc4fc8328de590aec941759d546d97f21b6874efc5bc455fc14dfe098ea3960"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-eulas-and-custom-terms/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:684e0d8cc5f15f220f5f71e41c818903cb38dd06d8818392df7f8378906e5e98"
---

# EULAs and Custom Terms in Marketplace Deals

*A marketplace EULA is the end user license agreement that governs how a buyer may use your product once they transact. Every marketplace offers a standard one, every marketplace lets you supply your own, and the choice you make sets your legal review time for years. This is not legal advice — it is the mechanics your counsel will need before they can give you any.*


---


The first private offer is where most teams discover they have a contract problem. Procurement asks which terms apply. Someone forwards the master services agreement from the website. The buyer’s counsel points out that the marketplace transaction is not covered by it. Two weeks disappear.


None of that is a legal disagreement. It is an unmade decision about which paper governs a marketplace deal, made under time pressure by whoever is nearest.


---


## **What is a marketplace EULA?**


**A marketplace EULA is the agreement between you and the buyer that governs use of your product, referenced by the listing and accepted at purchase.** The marketplace facilitates the transaction; it is not a party to that agreement. AWS states the position bluntly in its own documentation: “The EULA is between you and the buyer.”


That single sentence explains most of the mechanics that follow. Because the cloud is not a party, it cannot negotiate on your behalf, cannot waive your terms, and cannot resolve a dispute about them. What it can do is offer a template that most buyers have already accepted — which is where standard contracts come from.


Every seller therefore picks one of three paths:


Path What it is Best when


Standard contract The marketplace’s own template, pre-vetted by many buyers Self-service and mid-market deals where speed matters more than bespoke terms


Standard contract + amendments The template, plus your changes layered on You need a handful of specific carve-outs, not a rewrite


Your own EULA Your paper, hosted by you Regulated products, unusual liability profiles, or terms you cannot express as an amendment


---


## **How each cloud implements it**


### AWS


AWS gives sellers three named options: your own EULA, the **Standard Contract for AWS Marketplace (SCMP)** , or the **Reseller Contract for AWS Marketplace (RCMP)** for channel deals.


The SCMP “governs usage and defines the obligations of buyers and sellers for digital solutions” — AWS names server software, SaaS and AI/ML algorithms as examples — and “proactively defines common ground across key contractual clauses like use, warranty, indemnification, and governing law.”


Three optional addendums attach to it: an **Enhanced Security Addendum** for elevated data security requirements, a **HIPAA Business Associate Addendum** , and a **Federal Addendum** for US Government purchases. Each is a pre-drafted module rather than something you draft.


For private offers, AWS’s model is amendable: “buyers can request the SCMP template from the seller, and the terms can be amended to address custom transaction requirements as agreed upon by the parties.”


The opt-in carries one consequence worth reading before you take it: by applying the SCMP you enter the SCMP program, under which “AWS may update the SCMP template periodically and may update product listings carrying the terms with the current version.” You can withdraw at any time by replacing it with your own EULA.[The AWS standard contract, or your own paper?](https://www.suger.io/resources/blog/aws-standard-contract-or-your-own-paper/) works through that decision in detail.


If you use your own EULA on a server listing, AWS requires the link to be downloadable by customers — an S3 link is the documented example, not a page behind a login.


### Microsoft


Microsoft offers a **Standard Contract for Microsoft Marketplace** that “customers only need to vet and accept one time,” and supports it across Azure Applications, Azure Containers, Container Apps, Virtual Machines, SaaS and the AI Apps and Agents category.


Its amendment model is the most structured of the three, and the most useful to understand:


- **Universal amendments** apply to every customer of the offer and appear in every purchase flow.
- **Custom amendments** are targeted at specific customers by Azure tenant ID. Only customers in that tenant see them.
- The two **stack** : a targeted customer gets the universal amendment as well as their own.


Two hard limits: amendments are capped at **4,000 characters including spaces** , and you can add universal terms plus up to **10 custom amendments** .


And one rule that is genuinely irreversible: “Once you publish an offer using the Standard Contract for Microsoft Marketplace, you won’t be able to use your own custom terms and conditions. You either offer your solution under the Standard Contract *or* your own terms and conditions.” There is no migrating later.


### Google Cloud


Google Cloud Marketplace follows the same shape — a standard set of terms for self-service purchases, with negotiated terms attaching to private offers. Confirm the current requirements in Google’s own producer documentation before you commit a listing to either path, because the terms structure is the part of a marketplace that changes least loudly.


---


## **What custom terms actually cost you**


Not the drafting. The drafting is a week of counsel’s time and then it is done. The recurring costs are the ones that show up in the pipeline:


**Every buyer reviews from scratch.** A standard contract has already passed through the legal team of many of your prospects. Your paper has not. That is a review cycle per deal, and the review sits between the verbal yes and the revenue.


**Self-service stops working.** A buyer who has to route your EULA through counsel is not clicking subscribe. If any part of your motion is meant to be self-service, custom terms remove it.


**Amendments outlive the people who wrote them.** Three years in, nobody remembers why clause 7 was struck for one customer, and the renewal has to reconstruct it. This is an argument for expressing changes as amendments to a known base rather than as a bespoke document per deal — the diff stays visible.


**The channel multiplies it.** Reseller deals mean your terms have to flow through a partner to an end customer. AWS’s RCMP exists precisely to standardise that layer, and it is worth noting that buyers can’t view the RCMP — it governs you and the channel partner, not the end customer.


---


## **A workable default**


For most ISVs, the shape that holds up is:


1. **Standard contract on the public listing.** Self-service stays self-service.
2. **Standard contract plus amendments for mid-market private offers.** Small, named deltas rather than a different document.
3. **Your own paper only where a real obligation requires it** — a regulated data commitment, an unusual indemnity, a term the amendment mechanism genuinely cannot express.
4. **A decision rule written down** , so a deal desk owner knows which path a deal takes without escalating to counsel. That rule is what turns a two-week question into a two-minute one.


Signature mechanics are a separate question from which paper governs —[when you need a signature on legal documents in cloud marketplace deals](https://www.suger.io/resources/blog/when-do-you-need-a-signature-on-legal-documents-in-cloud-marketplace-deals/) covers what marketplace acceptance does and does not replace.


---


## **Frequently asked questions**


**What is a EULA on a cloud marketplace?** The end user license agreement between you and the buyer, referenced by your listing and accepted at purchase. The marketplace facilitates the transaction but is not a party to the agreement.


**Can I use my own terms instead of the marketplace standard contract?** Yes, on every major marketplace. On Microsoft the choice is one-way: once an offer is published under the Standard Contract, you can’t switch to your own terms and conditions.


**How many amendments can I add to Microsoft’s standard contract?** Universal amendment terms plus up to 10 custom amendments targeted by Azure tenant ID. Each amendment is limited to 4,000 characters including spaces.


**Can standard contract terms be negotiated on a private offer?** On AWS, yes. Buyers can request the SCMP template from the seller and the terms can be amended to address custom transaction requirements as agreed by the parties.


**What is the Reseller Contract for AWS Marketplace?** A standardized template ISVs use when authorizing channel partners to resell to AWS Marketplace buyers. It governs the ISV and the partner — buyers can’t view it.


**Does using a standard contract mean AWS is a party to my agreement?** No. AWS states that the EULA is between you and the buyer. Using the SCMP is at your discretion, and you can withdraw by replacing it with your own EULA.


---


## **Takeaways**


- The EULA is between you and the buyer. The marketplace facilitates the transaction and is not a party to the agreement.
- Three paths exist everywhere: the standard contract, the standard contract with amendments, or your own paper.
- AWS’s SCMP is amendable on private offers and carries pre-drafted security, HIPAA and federal addendums. Opting in means AWS may update the template on your listing.
- Microsoft’s amendments are structured — universal for everyone, custom by tenant ID, stacking, capped at 4,000 characters, up to 10 custom.
- Microsoft’s choice is one-way. Publish under the Standard Contract and you cannot later use your own terms for that offer.
- The cost of custom terms is not drafting. It is a legal review per deal, and the loss of any self-service motion.


---


Terms are only half the problem; keeping track of which version a given customer accepted is the other half. See how Suger’s[agreement management](https://www.suger.io/platform/agreements/) holds the offer, the accepted terms and the resulting contract as one record across every marketplace you sell on.
