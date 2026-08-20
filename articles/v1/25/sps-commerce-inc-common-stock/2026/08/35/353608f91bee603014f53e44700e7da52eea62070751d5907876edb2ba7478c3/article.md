---
schema_version: "1.0.0"
document_id: "353608f91bee603014f53e44700e7da52eea62070751d5907876edb2ba7478c3"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/ach-remittance-explained-for-suppliers"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-12T08:09:55.066752+00:00"
fetched_at: "2026-08-12T08:09:56.139527+00:00"
content_hash: "sha256:6298cad443a39fef1e6dfd1cd26ad6c6d9a66a2fd4ed2a9cdedf8d8b2d416a1c"
---

# ACH Remittance Explained for Suppliers

# ACH Remittance Explained for Suppliers


By[Jacqueline Nance](https://www.spscommerce.com/community/profile/jacqueline-nance) , *Sr. Content Marketing Manager*


Last Updated August 11, 2026


8 min read


In this article, learn about:


-


Why an ACH deposit and its remittance information often arrive separately


-


How suppliers enroll for electronic payments and EDI remittance data


-


Why ACH payments don't always match invoice totals, and what the EDI 820 does to explain them


---


Most suppliers have no trouble receiving an ACH payment. The challenge typically begins after the money reaches the bank account.


A deposit often arrives as a single net amount with little explanation of which invoices were paid, which deductions were taken, or why the total doesn't match your open receivables. That payment detail, known as the remittance, typically arrives through a separate channel, sometimes hours later, sometimes days later, and occasionally not at all.


That's because ACH only moves the money. The information explaining what the payment settles travels separately, often as an EDI 820 remittance advice or another remittance document. When those two pieces arrive out of sync, the reconciliation work falls to accounts receivable (AR), turning what should be an automatic payment into a manual investigation.


This guide explains how ACH payments work from the supplier's perspective, why payment and remittance are separate processes, and how EDI remittance data helps finance teams reconcile cash faster and more accurately.


## What Is ACH, and Why Does the Remittance Matter More Than the Transfer?


ACH (Automated Clearing House) is the electronic network that moves money between US bank accounts. It runs under rules set by[Nacha](https://www.nacha.org/content/abcs-ach) , a private rule-making body, not a government agency. The Treasury's Bureau of the Fiscal Service separately folds Nacha's rules into federal regulation ([31 CFR Part 210](https://www.ecfr.gov/current/title-31/subtitle-B/chapter-II/subchapter-A/part-210) ) for its own agency payments, but that's a separate track from standard B2B payments.


The transfer itself is the easy half. What determines whether a supplier can close their books is the remittance: the structured explanation of which invoices a payment covers, and what, if anything, got deducted along the way. That explanation doesn't automatically ride inside the ACH transaction. It depends on which format, or SEC code, the retailer's bank uses to send the payment:


-


**Cash Concentration or Disbursement (CCD):** The standard business-to-business format that carries the payment with no remittance detail attached.


-


**CCD+:** The same format as CCD with one addenda record added. That's enough for a single invoice reference, but not much more.


-


**Corporate Trade Exchange (CTX):** Built to carry structured remittance data, including a full EDI 820 Payment Order/Remittance Advice, across multiple addenda records.


A CCD payment tells a supplier that money was deposited, but it may not provide enough detail to explain what the payment covers. That explanation comes from the remittance data. An EDI 820 can provide invoice numbers, deduction details, and other information the AR team needs to match the payment to open invoices. Depending on the trading partner’s setup, that information may accompany a CTX payment or arrive separately from the ACH transaction.


## What Forms Do Suppliers Actually Submit To Get Paid by ACH?


Getting set up for electronic payments from a retail partner usually means submitting a handful of documents.


Most commonly, that includes:


-


**Vendor banking or EFT enrollment:** Authorizes the retailer to deposit funds into a specific account


-


**A W-9:** Confirms tax identification


-


**A voided check or bank verification letter:** Validates the account on file


-


**Account validation:** Confirmation step some retailers run before the first live payment


The forms establish the payment process, but the remittance detail is handled separately.


Enrolling for EDI 820 remittance is often a separate request to a separate team. Skip that second step, and a supplier ends up receiving payments with no readable detail behind them.[The Home Depot is a clear example of this pattern:](https://www.spscommerce.com/community/articles/remittance-advice-for-the-home-depot) suppliers are directed to a dedicated B2B team specifically for 820 enrollment, apart from standard vendor banking setup.


**Related Reading** :[What Is Remittance Advice and Why Is It Important for Suppliers?](https://www.spscommerce.com/community/articles/what-is-remittance-advice-and-why-is-it-important-for-suppliers)


## Why Doesn't My ACH Deposit Match My Invoice?


A retailer's ACH credit typically arrives as one net number covering many invoices, minus allowances, or chargebacks that the retailer applied along the way. On paper, the EDI 820 is supposed to explain that gap using standardized X12 adjustment reason codes, one per deduction, tied back to the specific invoice it affected.


In practice, what actually lands in a supplier's inbox varies widely. Some deductions arrive netted into the deposit with a usable reason code attached.


Others show up as a separate claim document days later. Some don't get explained at all until someone on the AR team goes looking for the reason.


## How Does ACH Compare To Checks, Wires, and Cards?


**Payment Type**


**Median Cost Per Payment**


**Settlement Time**


**Remittance Richness**


ACH (standard)


$0.26 to $0.50 to initiate/receive


1 to 2 business days


High when sent as CTX/820; none by default with plain CCD


Same Day ACH


Included in ACH cost range above


Same business day


Same as standard ACH; format determines remittance detail


Paper check


$2.01 to $4.00 to issue


Days, plus mail and deposit time


Comes with the check itself, but requires manual entry


Wire transfer


Typically $15 to $50+ per wire


Same day, often within hours


Minimal; usually just a reference line


Card payment


Percentage-based interchange fees, often 1.5% to 3%+


1 to 2 business days to settle


Limited; depends on the processor's reporting


The[2025 AFP Digital Payments Survey](https://www.nacha.org/news/over-21-years-massive-drop-b2b-check-payments-study-finds) , cited by Nacha, found that just 26% of US and Canadian B2B payments are still made by check, down from 81% in 2004. B2B volume on the ACH Network grew almost 10% in 2025 to roughly 8.1B payments. That shift is happening on the retailer's timeline, not the supplier’s. That's exactly why a growing number of AR teams are receiving netted electronic deposits without ever being taught how to read one.


## What Has To Happen Before an EDI 820 Can Auto-Apply?


An 820 only auto-applies cleanly when the documents upstream of it were accurate. If the original invoice ([EDI 810](https://www.spscommerce.com/community/articles/explaining-edi-810-invoice) ) or the shipment notice ([EDI 856](https://www.spscommerce.com/community/articles/edi-856-ship-notice-manifest) ) had errors, the retailer's system carries that mismatch forward into the payment and remittance data. Getting the EDI 820 right starts well before the payment is sent.


**Related Reading:**[How EDI Payments Improve Cash Flow](https://www.spscommerce.com/community/articles/how-edi-payments-improve-cash-flow)


## How Do You Know if Your Remittance Data Is Actually Working?


Three numbers separate a supplier who has this under control from one still working off a spreadsheet:


### Unapplied Cash Rate


The formula to calculate unapplied cash rate is: (unapplied cash balance ÷ total cash received in the period) × 100. This is the share of incoming payments an AR team hasn't been able to match to an invoice yet. A high or rising rate usually points to missing or unusable remittance data, not a collections problem.


### Remittance Match Rate


The formula to calculate remittance match rate is: (deductions received with a usable adjustment reason code ÷ total deductions received) × 100. This measures how often the EDI 820 actually explains itself, versus how often someone must dig for the reason.


### Days to Deduction Identification


This is the average time between the deposit date and the date the AR team can name the specific cause of a deduction. Shorter is better. It's also a different clock than the retailer's dispute deadline, which is worth confirming separately for each trading partner.


These metrics aren't meant to replace days sales outstanding (DSO). They answer a different question: Is the remittance data arriving complete, accurate, and on time? When those numbers trend in the wrong direction, the issue is often the quality of the payment data rather than the speed of cash collection.


**Related Reading:**[How to Dispute Deductions and Recover Revenue](https://www.spscommerce.com/community/articles/how-to-dispute-deductions-and-recover-revenue)


## Frequently Asked Questions Regarding ACH


### Is ACH remittance the same thing as an ACH payment?


No. The ACH payment is the transfer of funds. The remittance is the separate data that explains which invoices the payment covers and what was deducted. They can travel together (as a CTX transaction) or separately (as a standalone EDI 820), depending on how the retailer's bank sends the payment.


### Why did I get an ACH deposit with no remittance detail?


Most likely the payment was sent as a plain CCD entry, which carries no addenda record, or your EDI 820 enrollment with that retailer was never completed separately from your banking setup. Check with the retailer's B2B or vendor payments team to confirm both are in place.


### What's the difference between CCD, CCD+, and CTX?


CCD carries the payment with no remittance detail. CCD+ adds one addenda record, enough for a single reference like an invoice number. CTX is built for structured remittance data across multiple addenda records, including a full EDI 820.


### Does Same Day ACH change the remittance data I receive?


No. Same Day ACH only changes settlement speed. The remittance detail you receive still depends on whether the payment was sent as CCD, CCD+, or CTX, not on how fast it settled.


Reading the deposit can be a data problem before it's ever a cash-flow problem. Getting the remittance format right with each retail partner determines whether an ACH payment closes the loop on an invoice or just adds another unexplained line to the reconciliation pile.


## A Payment Is Only as Useful as the Data Behind It


Receiving an ACH payment is only part of the process. AR teams still need enough information to identify what the payment covers, match it to the right invoices, and understand any deductions or adjustments.


Complete, structured remittance data shortens that path. When the information is missing or incomplete, teams may have to research the payment manually before they can apply the cash or investigate a deduction.


For suppliers managing ACH payments at scale, the goal is not simply faster payment. It is a cleaner connection between the funds received and the transaction data needed to reconcile them.[SPS Commerce Fulfillment](https://www.spscommerce.com/products/fulfillment/) helps suppliers automate that exchange, giving finance teams more consistent payment and remittance data to work from.
