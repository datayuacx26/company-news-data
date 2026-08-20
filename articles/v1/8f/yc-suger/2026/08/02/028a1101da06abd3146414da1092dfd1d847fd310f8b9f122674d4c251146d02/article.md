---
schema_version: "1.0.0"
document_id: "028a1101da06abd3146414da1092dfd1d847fd310f8b9f122674d4c251146d02"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/marketplace-seller-of-record/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:3d3c295bd826f71a85266c340c4bea7072bbb5514d4c802399f3c1a0f009b695"
---

# Seller of Record: Why It Changes Your Books

*Seller of record is the party that legally makes the sale to the buyer — the name on the invoice, the entity that collects the money, and usually the one carrying the tax obligation. On a cloud marketplace it is frequently not you, and every downstream number in your finance stack changes shape as a result. This is not tax or accounting advice; it is the mechanics your controller and auditor will need.*


---


The first marketplace close looks like a normal one until month-end. Sales booked a $180,000 contract. Finance goes looking for the invoice to the customer and there isn’t one. There is an invoice from a cloud provider to that customer, and eventually a payment from the cloud provider to you, minus a fee, on a schedule nobody in the room set.


Nothing has gone wrong. The company simply sold through a channel where somebody else is the seller of record, and the accounting has to catch up with that.


---


## **What is seller of record on a cloud marketplace?**


**The seller of record is the entity that transacts with the buyer: it issues the invoice, collects payment, and typically bears the obligation to charge and remit transaction taxes.** In a direct sale that is you. In a marketplace sale, the cloud provider’s commerce platform sits in the middle, and the answer depends on the marketplace and sometimes on the individual transaction.


Two facts make this concrete.


**Microsoft states its model explicitly.** “Microsoft Marketplace operates on an agency model, whereby publishers set prices, Microsoft bills customers, and Microsoft pays revenue to publishers while withholding an agency fee.” You set the price; Microsoft bills and collects; Microsoft pays you the remainder.


**AWS reports it per transaction.** The AWS taxation dashboard carries an **AWS seller of record** field, described as “an identifier of the business entity that facilitated the transaction” — AWS operates through several entities depending on geography. It also carries a **Tax liable party** field with two possible values,` AWS` or` Seller` : “If the seller is the tax liable party, taxes are collected. If AWS is the tax liable party, sales tax is collected and remitted by AWS.”


The consequence of that second field is the one most teams miss. Tax liability is not a single global answer for your business — it varies by transaction, and AWS says so directly: “The seller must determine whether some taxes were collected for each invoice, as the seller is liable for tax collection.”


---


## **The money path, end to end**


Step Who acts What your systems see


Buyer accepts an offer Buyer An entitlement appears; a booking exists


Invoice issued to the buyer The marketplace Nothing in your AR ledger


Buyer pays Buyer → marketplace Nothing yet


Listing fee deducted The marketplace A fee you did not invoice


Disbursement to you Marketplace → your bank Cash, in aggregate, later


Tax collected and remitted AWS or you, per transaction A field on a report you have to read


Two gaps in that table cause most of the pain.


**Between “booking exists” and “cash arrives” there is no receivable you control.** You cannot chase the customer, because they do not owe you anything — they owe the marketplace. Collections stops being a customer conversation and becomes a reconciliation exercise.


**Disbursement is aggregated and scheduled by the marketplace, not by your invoice terms.** Net-30 is not a thing you set. Confirm the current schedule and settings in each provider’s own disbursement documentation rather than from a summary — the rules differ per marketplace and change.


---


## **What actually changes in your books**


**1. Accounts receivable moves counterparty.** Your receivable is from the cloud provider, not the end customer. Credit exposure improves considerably. Ageing reports that group by customer stop reflecting collection risk.


**2. Gross versus net becomes a live question.** Whether you recognise the full contract value with the marketplace fee as an expense, or only the net you receive, is a principal-versus-agent judgement under the revenue standard your auditor applies. It is not a preference and it is not settled by how the marketplace describes itself. Take the facts — who sets price, who bears credit risk, who is responsible to the customer for the product — to your auditor early, because restating it later is expensive.[Bridging the GAAP on marketplace revenue](https://www.suger.io/resources/blog/finance-101-bridging-the-gaap-on-marketplace-revenue/) covers the wider recognition picture.


**3. Tax stops being one policy.** AWS reports a tax liable party per transaction, with tax types including sales, seller use, VAT, IGST and others. A single global “the marketplace handles tax” assumption will be wrong somewhere in your customer base.


**4. Displayed prices are tax-exclusive.** AWS notes that “all AWS Marketplace offerings are exclusive” for price taxability. Your listed price is the price before tax, so a buyer’s invoice will exceed your list price and their procurement team may ask why.


**5. Your revenue report and the marketplace’s report will not match on the first attempt.** Bookings, billings, collections and disbursements are four different numbers with four different dates.[Marketplace revenue reconciliation](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) covers why the consoles disagree and what to reconcile against what.


---


## **Questions worth answering before the first close**


- **For each marketplace we sell on, who is the seller of record, and does it vary by region or transaction?**
- **Who is the tax liable party on our transactions, and where do we read that per invoice?**
- **Are we recognising gross or net, and who signed off on that judgement?**
- **What is our disbursement cadence per marketplace, and how do we tie a payment back to the agreements it covers?**
- **Which report is authoritative when sales, finance and the marketplace console disagree?**


The last one is the question that saves the most time. Pick the authoritative source deliberately, before a disagreement, and write it down.


---


## **Frequently asked questions**


**What does seller of record mean?** The entity that legally makes the sale to the buyer — it issues the invoice, collects payment, and usually carries the obligation to charge and remit transaction tax. On a marketplace it is often the cloud provider rather than the software vendor.


**Who invoices my customer on a cloud marketplace?** The marketplace. Microsoft states that publishers set prices while Microsoft bills customers and pays out revenue less an agency fee. Your receivable is from the provider, not the end customer.


**Who pays sales tax on a marketplace transaction?** It varies by transaction. AWS reports a tax liable party of either` AWS` or` Seller` per invoice, and states that the seller must determine whether taxes were collected, because the seller is liable for tax collection.


**Do marketplace prices include tax?** On AWS, no. AWS reports that all AWS Marketplace offerings are tax-exclusive, so the buyer’s invoice will exceed your listed price by the applicable tax.


**Should I recognise marketplace revenue gross or net?** That is a principal-versus-agent judgement under your revenue standard, decided on facts like who sets price and who bears credit risk. Settle it with your auditor before the first close, not after.


**Why doesn’t my disbursement match my bookings?** Because bookings, billings, collections and disbursements are four different events on four different dates, and disbursements arrive aggregated on the marketplace’s schedule rather than your invoice terms.


---


## **Takeaways**


- Seller of record is whoever invoices the buyer and collects the money. On a marketplace that is usually the cloud provider, not you.
- Microsoft describes its marketplace as an agency model: publishers set prices, Microsoft bills and collects, Microsoft pays out less an agency fee.
- AWS reports both a seller-of-record entity and a tax liable party per transaction, with values of` AWS` or` Seller` .
- Your receivable moves from the customer to the provider. Credit risk falls; collections becomes reconciliation.
- Gross versus net is an auditor judgement, not a preference. Settle it before your first marketplace close.
- AWS Marketplace prices are tax-exclusive, so a buyer’s invoice will be larger than your listed price.


---


Once the marketplace is the seller of record, your close depends on tying four different records together across every cloud you sell on. See how Suger’s[billing and revenue operations](https://www.suger.io/platform/billing-metering/) connect agreements, metered usage, invoices and disbursements into one reconcilable set.
