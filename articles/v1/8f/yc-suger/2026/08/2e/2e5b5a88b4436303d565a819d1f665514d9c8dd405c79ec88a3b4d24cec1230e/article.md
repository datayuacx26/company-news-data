---
schema_version: "1.0.0"
document_id: "2e5b5a88b4436303d565a819d1f665514d9c8dd405c79ec88a3b4d24cec1230e"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/where-the-signed-marketplace-agreement-lives/"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T10:32:29.919238+00:00"
fetched_at: "2026-08-19T10:32:31.370146+00:00"
content_hash: "sha256:d9ec71f8f6723c0a88e326209f37174f2708a2316bda6c1ca03cfde9fbb048e8"
---

# Where a Signed AWS Marketplace Agreement Lives

*Across a marketplace deal, four different documents exist at four different stages — the offer, the EULA or contract terms, the buyer’s acceptance, and the executed agreement. On AWS Marketplace each one lives in a different place, and the “signed agreement” your legal team is asking for is the last of the four. This is a stage-by-stage checklist for finding it and downloading it as a PDF.*


---


Legal asks for “the executed marketplace agreement” the way they’d ask for a countersigned MSA — one file, with two signatures on the last page. A marketplace deal does not produce that file. There is no wet-ink signature block, no DocuSign envelope, and no single moment where both parties sign the same page.


What exists instead is a chain of documents, each created at a different stage, each living in a different view of a different console. The offer is one artifact. The terms attached to it are another. The buyer’s acceptance is an event, not a page. And the “executed agreement” is a fifth thing that AWS assembles from the offer plus the acceptance — retrievable, downloadable as a PDF, but only if you know which tab it sits in and whose login you need.


If you are the legal-ops owner handed that request, this is the map.


---


## What does “the signed agreement” actually mean on AWS Marketplace?


**On AWS Marketplace, the executed agreement is an offer that a buyer accepted — there is no separately signed contract file.** AWS states it plainly: “An agreement is formed when a buyer accepts an offer, public or private.” The offer carries the terms; acceptance is the signature-equivalent; the resulting agreement is what you retrieve.


One detail changes how you should frame this internally: AWS is not a party to the agreement. AWS documents that “AWS is not a party to any agreements” — the contract is between the seller (the ISV) and the buyer. AWS runs the catalog, the billing, and the record-keeping, but the legal relationship is yours and your customer’s. That is why the “signed” document has no third signature and why AWS provides it as a system-generated PDF rather than a countersigned original.


So when Legal asks for the executed agreement, the honest answer is: it is the accepted offer, and it can be produced as a PDF from either party’s console. The rest of this post is which document sits where, stage by stage.


---


## The four documents, mapped by deal stage


A marketplace deal generates four distinct documents. They are easy to conflate because the same words — “offer,” “terms,” “agreement” — get used for all of them in conversation. In the console they are separate objects with separate homes.


Deal stage Document What it is Where it lives on AWS Marketplace


1. Terms negotiated **The offer** A set of terms for buying the product: pricing model, duration, payment schedule, and the attached legal terms. Still just an offer — nobody has bought anything. Seller side: the **Private offers** page in AWS Partner Central. Downloadable as a PDF via **View details → Download PDF** .


2. Legal terms attached **The EULA / contract terms** The legal document governing use: the seller’s public EULA, the Standard Contract for AWS Marketplace (SCMP), or custom legal terms. On a private offer, custom terms can be up to five files, merged into one document. Public EULA: on the product listing page. Private-offer terms: inside the offer itself, carried into the agreement on acceptance.


3. Buyer accepts **The acceptance** The event that converts the offer into a binding agreement. Not a separate downloadable page — it is the act that AWS timestamps and records. Recorded by AWS. The accepted offer then “shows up as an agreement in the **Agreements** tab.”


4. Deal is live **The executed agreement** The offer plus the acceptance: the contract of record, with terms, pricing, dates, and status. This is what Legal means by “the signed agreement.” Seller: the **Agreements** page (Partner Central). Buyer: **Manage subscriptions → agreement detail** . Both can **Download PDF** .


The request for “the executed agreement” is a request for row 4. Rows 1 through 3 are the paper trail that produced it — worth understanding, because if row 4 is missing or wrong, the answer is almost always in one of them.


---


## Stage 1 — the offer: where the terms start


**The offer is a set of terms for purchasing a product, and on AWS Marketplace it exists before any contract does.** AWS defines a private offer as “negotiated terms used to purchase a product from AWS Marketplace,” which “can involve a custom pricing plan, end user license agreement (EULA), or custom solutions.”


For legal-ops purposes, the offer is where you confirm what was *proposed* — the pricing, the duration, the payment schedule, and which legal terms are attached. A seller finds it on the **Private offers** page in AWS Partner Central, and can pull it down as a standalone document: select the offer, choose **View details** , then **Download PDF** .


The offer is not the executed agreement, and it is a common mix-up. An offer can sit unaccepted, be edited, expire, or be cancelled. Only once a buyer accepts it does a contract exist. If someone hands you an “offer PDF” and calls it the signed contract, check the status — a draft or active offer has not been signed by anyone.


---


## Stage 2 — the EULA and contract terms: the legal document itself


**The EULA or contract terms are the legal document that governs the buyer’s use of the product, and on AWS Marketplace they come in three flavors.** When a seller configures a private offer, AWS presents exactly three legal-terms options: use the public offer EULA, use the Standard Contract for AWS Marketplace (SCMP), or upload custom legal terms.


Where each one lives depends on which was used:


- **Public EULA** — the standard terms for a publicly listed product appear on the product listing page in AWS Marketplace.
- **Standard Contract for AWS Marketplace (SCMP)** — AWS’s own standardized contract, which many sellers use as a default and can amend for private offers.
- **Custom legal terms** — on a private offer, a seller can upload up to five files (legal terms, a statement of work, a bill of materials, a pricing sheet, or other addendums), and AWS “will be merged into one document when the offer is created.”


That merge is the detail worth writing down. If your deal used custom terms, the “contract terms” are one composite document that AWS assembled from your uploads at offer-creation time — not five loose attachments. Which of the three flavors governs a given deal, and what each option means for your paper, is the subject of[EULAs and custom terms in marketplace deals](https://www.suger.io/resources/blog/marketplace-eulas-and-custom-terms/) and the AWS-specific decision in[the standard contract versus your own paper](https://www.suger.io/resources/blog/aws-standard-contract-or-your-own-paper/) .


---


## Stage 3 — the buyer’s acceptance: the signature that isn’t a signature


**The buyer’s acceptance is the event that turns an offer into a binding agreement, and it is a click, not a countersigned page.** There is no separate “acceptance document” to retrieve. Acceptance is the moment AWS records, and the proof of it is the agreement that appears afterward.


AWS is explicit about the transition: “After the offer is accepted, it will show up as an agreement in the **Agreements** tab. The status of the offer won’t change.” So the offer stays as-is in its own list, and a new object — the agreement — is created alongside it. That new object is your evidence that acceptance happened, who accepted, and when.


For legal-ops, this reframes a question that trips people up: “who signed it and when?” The answer is not on a signature page. It is the agreement’s creation and its start date, both visible on the agreement detail. If you need to prove execution, you are proving that an agreement exists in an active status with a start date — that is what “signed” means here.


---


## Stage 4 — the executed agreement: how to retrieve and download it


**The executed agreement is the accepted offer, and both the seller and the buyer can download it as a PDF from their own console.** This is the document Legal asked for. Which login you use depends on which side you sit on.


**If you are the seller (the ISV):**


1. Sign into AWS Partner Central and choose **Agreements** . The **Agreements** table shows the contracts for products you sell.
2. Find the agreement — filter by **Agreement ID** if you have it, or sort by end date.
3. Choose the link in the **Agreement ID** column, or select the row and choose **View details** .
4. On the agreement detail page, choose **Download PDF** .


**If you are the buyer (or need to walk your customer through it):**


1. Open the AWS Marketplace console and go to the **Manage subscriptions** page.
2. Choose the **product name** to open the **subscription detail** page, which lists all agreements for that product.
3. Choose the **Agreement ID** to open the **agreement detail** page. It shows agreement terms and conditions, pricing, charge summary, and — per AWS — an “Option to download the agreement as a PDF.”
4. Download the PDF.


Both paths produce a PDF of the contract of record. Note the statuses so you retrieve the right one: an agreement can be **Active** , **Expired** , **Renewed** , **Cancelled** , **Replaced** , or **Terminated** on the seller side; the buyer view adds **Pending** and **Archived** . A **Renewed** or **Replaced** agreement means the version Legal wants may be the newer one that superseded it — or the historical one, if the request is about a specific prior term.


One structural reason retrieval is harder than it should be: the seller sees agreements in Partner Central, the buyer sees them under Manage subscriptions, and neither can see the other’s console. If you are on the seller side and the request is really about proving terms to the buyer, the buyer often has to pull their own copy. Centralizing every marketplace document — offers, terms, and executed agreements across AWS, Microsoft, Google Cloud, and other marketplaces — in one place is exactly the gap a marketplace-native platform like Suger closes; you can see how[marketplace agreement management](https://www.suger.io/platform/agreements/) fits the broader[Cloud GTM platform](https://www.suger.io/platform/) .


---


## Frequently asked questions


**Where is the signed agreement on AWS Marketplace?** It is on the **Agreements** page in AWS Partner Central for sellers, and under **Manage subscriptions → agreement detail** for buyers. On AWS Marketplace the “signed agreement” is an offer the buyer accepted, and either side can download it as a PDF from that view.


**How do I download an AWS Marketplace agreement as a PDF?** Sellers: in AWS Partner Central, open **Agreements** , select the agreement, choose **View details** , then **Download PDF** . Buyers: open the AWS Marketplace console, go to **Manage subscriptions** , choose the product, choose the **Agreement ID** , and use the option to download the agreement as a PDF.


**Is there a countersigned contract file for a marketplace deal?** No. There is no wet-ink or countersigned file. AWS forms the agreement when the buyer accepts the offer, and AWS is not a party to it. The executed agreement is a system-generated PDF of the accepted offer, not a document with two signature blocks.


**What is the difference between the offer and the agreement?** The offer is a set of proposed terms — pricing, duration, and attached legal terms — that nobody has bought yet. The agreement is that offer after a buyer accepts it. An unaccepted offer is not a contract; the agreement in the **Agreements** tab is.


**Where do the EULA or contract terms live?** A public product’s EULA is on the product listing page. Private-offer terms are inside the offer and carried into the agreement on acceptance. If custom legal terms were used, AWS merges up to five uploaded files into one document at offer creation, and that composite is the governing paper.


**Can the buyer and seller both retrieve the executed agreement?** Yes, but from different consoles. The seller retrieves it from the **Agreements** page in AWS Partner Central; the buyer retrieves it from **Manage subscriptions** in the AWS Marketplace console. Neither can see the other’s view, so each party downloads its own copy.


---


## Takeaways


- The “executed agreement” on AWS Marketplace is an offer the buyer accepted — there is no countersigned file, and AWS is not a party to it.
- Four documents exist across the deal: the offer, the EULA/contract terms, the acceptance event, and the executed agreement. Only the last is what Legal usually means.
- Retrieve it as a PDF from **Agreements** in AWS Partner Central (seller) or **Manage subscriptions → agreement detail** (buyer). Both sides can download; neither can see the other’s console.
- Check the agreement status before you hand it over. **Renewed** or **Replaced** means a newer version may be the one that governs today.
- If custom legal terms were used, the governing paper is the single merged document AWS assembled from your uploads — not the loose files you sent.


---


When the request is “produce the executed agreement” and the answer spans two consoles you can’t both see, the fix is to keep every offer, term, and signed agreement in one place. See how Suger handles[marketplace agreement management](https://www.suger.io/platform/agreements/) across AWS, Microsoft, and Google Cloud, and how it connects to partner-sourced deals in[Suger’s PRM](https://www.suger.io/prm/) .
