---
schema_version: "1.0.0"
document_id: "71b961df2435a877078b4312490227a30a0943d76e14837d965a141737f0548c"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/how-indian-it-and-services-businesses-receive-cad-from-canadian-clients-in-2026-rails-firc-and-forex-reality/"
published_at: "2026-08-17T07:10:48+00:00"
first_seen_at: "2026-08-17T11:30:45.788177+00:00"
fetched_at: "2026-08-17T11:30:47.650474+00:00"
content_hash: "sha256:b80bab0036000d1bc6f60a012738bfc2253151d827e848977546018ef03431c9"
---

# How Indian IT and Services Businesses Receive CAD from Canadian Clients in 2026: Rails, FIRC, and Forex Reality

The Canadian client marks the invoice paid. A day later, your bank SMS lands, and the credited INR is noticeably lower than the invoice suggested. Then your CA calls asking for an FIRC.


Here is the correction most businesses learn the hard way: receiving the money is not the same as receiving it correctly. Using the wrong rail, missing the FIRC, or applying the wrong purpose code costs money and creates GST refund blockers.


This guide resolves three problems: which rail to use for receiving CAD, the real forex math behind that shortfall, and how to get the compliance document that unlocks your GST benefits.


### Key Takeaways


- Indian IT and services businesses can receive CAD via SWIFT bank wire, a local Canadian receiving account through a cross-border provider, or an international payment platform. Each carries a different cost, speed, and documentation outcome.
- SWIFT transfers from Canada to India typically take 3 to 5 business days and carry a combined forex-and-fee cost of roughly 2.5% to 4%.
- Local CAD receiving accounts can reduce the all-in cost to roughly 0.4% to 1%, per one provider’s published comparison. Treat this as a provider claim.
- FIRC (Foreign Inward Remittance Certificate) is the bank-issued proof that foreign currency was received. Without it, GST refunds on service exports cannot be claimed.
- Since the EDPMS rollout, banks issue an e-FIRA rather than a physical FIRC for most service-export receipts.
- The RBI purpose code attached to each inward payment classifies the transaction. The wrong code can delay or invalidate your FIRC and GST refund claim.
- Businesses can hold received CAD in an EEFC (Exchange Earners’ Foreign Currency) account instead of converting immediately.
- The export-realisation deadline is now 15 months from the date of export, extended from nine months under the FEMA amendment notified on 13 November 2025.


## What Actually Happens When a Canadian Client Pays You – The Full Payment Journey


Follow a single CAD payment from the client’s bank to the INR credit and the e-FIRA. Each step decides part of your cost and your paperwork.


### Step 1 – The Canadian Client Initiates the Payment


For a SWIFT wire, the client needs your SWIFT/BIC code, account number, bank name, and branch address. For a local rail, they need only a local Canadian account number from a cross-border provider.


Canadian banks treat India as an international SWIFT corridor by default. But Canadian businesses can pay India via domestic EFT or Interac inside Canada when you provide local account details.


### Step 2 – The Money Moves Through Correspondent Banks


With SWIFT, the originating Canadian bank routes the payment through one or two correspondent (intermediary) banks before it reaches your Indian AD (Authorised Dealer) bank. Each intermediary can deduct its own fee en route, which is why the amount arriving is often smaller than the amount sent.


Under SHA, fees are shared: the sender pays their bank’s fee, while intermediary fees are deducted in transit. Under OUR, the sender pays all fees, so you receive the full value.


### Step 3 – The Indian AD Bank Converts CAD to INR and Credits the Account


An AD Category-I bank is an RBI-authorised bank permitted to receive and convert foreign currency in India. The bank applies its own conversion rate, not the mid-market rate you see on Google. It then reports the transaction to RBI through EDPMS, the Export Data Processing and Monitoring System.


### Step 4 – The Purpose Code Gets Attached and the e-FIRA Is Generated


The purpose code assigned at credit classifies the nature of your inflow. Common codes include P0802 for software consultancy where SOFTEX is not required, and P0807 for off-site software exports covered by SOFTEX. Once recorded, the bank generates the e-FIRA you access on request.


> **DID YOU KNOW:** A Canadian client may never need to trigger an international SWIFT wire at all. If you provide local Canadian bank account details through a cross-border receiving account, the payment can move as a domestic Canadian EFT or Interac transfer.


## The Three Rails for Receiving CAD in India – A Plain-English Comparison


The rail you give your client decides your cost, speed, and paperwork before the money even moves.


### Rail 1 – SWIFT Bank Wire (The Default, and Often the Costliest)


The Canadian bank sends an international wire to your Indian bank’s SWIFT address. It needs no special setup. The timeline is 3 to 5 business days, and the all-in cost is roughly 2.5% to 4%. Sender-side fees at traditional banks often run CAD 15 to CAD 80. Use SWIFT for large one-time payments.


### Rail 2 – Local CAD Receiving Account via a Cross-Border Platform


You get a local Canadian account number from a cross-border provider, and your client pays domestically. The timeline is typically 1 to 2 business days. The all-in cost is roughly 0.4% to 1%, per one provider’s published comparison. The provider facilitates an e-FIRA accepted for RBI and GST purposes.


### Rail 3 – International Payment Platforms and Aggregators


Your client pays via card or platform, and the funds route through the platform before settling to your Indian bank account. The typical cost is roughly 3% to 5%. It makes sense for smaller amounts and one-off projects. The AD bank still issues the e-FIRA against the final INR credit.


Rail Typical Timeline Approximate All-in Cost FIRC / e-FIRA Issued By


SWIFT Bank Wire 3 to 5 business days 2.5% to 4% Indian AD bank


Local CAD Account (Cross-Border Platform) 1 to 2 business days 0.4% to 1% Platform or partner AD bank


International Payment Platform 1 to 3 business days 3% to 5% Indian AD bank on final credit


> **PRO-TIP:** If your cross-border account provider gives you a local Canadian account number, share those details instead of your SWIFT details. The client pays domestically, you receive faster and at a lower forex cost, and both sides avoid international wire fees.


## The Forex Reality – Why the INR You Receive Is Never Equal to the Invoice


The Google rate and the bank credit are two different numbers, and the gap is a stack of predictable deductions.


### The Three Layers of Cost Between Your CAD Invoice and the INR Credit


- Layer 1: the correspondent bank fee, a flat deduction before the payment reaches India.
- Layer 2: the Indian AD bank’s forex markup, the spread between the mid-market rate and the bank’s buy rate.
- Layer 3: the Indian bank’s incoming-wire handling charge.


### A Real-Money Example Using a CAD 5,000 Invoice


Assume a mid-market rate of about 1 CAD = 68.4 INR. At that rate, CAD 5,000 equals roughly Rs 342,000 at mid-market. After a correspondent flat fee, a smaller CAD amount arrives. After the bank applies its roughly 2% to 3% markup, the value drops again, and a fixed processing charge follows.


The total loss versus the expected amount lands around Rs 8,500 to Rs 13,500 on a CAD 5,000 SWIFT payment, roughly 2.5% to 4%. The same invoice via the local rail costs closer to Rs 1,400 to Rs 3,400.


### What the Mid-Market Rate Is and Why Banks Never Use It


The mid-market rate is the interbank rate that data providers display. AD banks never pass it to you; they apply a buy-sell spread that is their margin. To check your real cost, compare the rate on your bank statement against that day’s mid-market CAD/INR.


### The EEFC Account Option – Holding CAD Without Converting


An EEFC account is a zero-interest current account held in foreign currency at an Indian AD bank. It lets you hold CAD without immediate conversion and convert when the rate is more favourable. Read more in this[EEFC account guide](https://razorpay.com/blog/what-is-an-eefc-account/) . The trade-off is that no interest accrues.


> **DID YOU KNOW:** On a CAD 5,000 SWIFT payment, intermediary fees plus the bank’s forex markup plus processing charges can quietly reduce the final INR credit by several thousand rupees versus the mid-market rate.


## What Is FIRC (Foreign Inward Remittance Certificate) and Why Indian IT Exporters Cannot Ignore It


### FIRC Defined – Without the Jargon


An FIRC is an official bank document confirming that a specific foreign-currency payment was received in your account from abroad. It contains beneficiary details, the amount in foreign currency and INR, the date of credit, the exchange rate, the purpose code, and the transaction reference (UTR). Only RBI-authorised AD Category-I banks can issue a valid FIRC or e-FIRA. See this[FIRC certificate guide](https://razorpay.com/blog/firc-certificate/) .


### Physical FIRC vs e-FIRA – What Changed and What It Means Now


With EDPMS, RBI directed banks to stop issuing paper FIRCs for most export receipts, via A.P. (DIR Series) Circular No. 74 dated 26 May 2016. What you get now is an e-FIRA, a digitally issued PDF accepted by GST authorities and the RBI. A physical FIRC is still issued for FDI and FII inflows. Many still say “FIRC” when they mean the e-FIRA, though you can see the[difference between BRC and FIRC](https://razorpay.com/blog/difference-between-brc-and-firc/) .


### Why FIRC Is Not Optional for IT and Service Exporters


- GST zero-rating: export of services is zero-rated only where payment is received in convertible foreign exchange and proven via FIRC or e-FIRA.
- GST refund eligibility: authorities require the FIRC as evidence of realisation.
- RBI compliance: every inward remittance is reported via EDPMS, and the e-FIRA is your readable output.
- Audit readiness: FIRCs create a clean trail linking bank credits to specific foreign invoices.


> **DID YOU KNOW:** Under Rule 96A of the CGST Rules, a service exporter under an LUT must pay the un-charged IGST plus 18% interest if payment is not realised within one year from the invoice date, or the FEMA period including any RBI extension, whichever is later.


## How to Get the FIRC (or e-FIRA) for Your CAD Payment from Canada – Step by Step


### Step 1 – Confirm the Credit and Note the UTR


A UTR is the unique transaction reference number that identifies your inward remittance. You will find it on your bank statement against the CAD receipt. You need it before requesting the FIRC, because it is how the bank locates the transaction.


### Step 2 – Contact Your AD Bank and Submit the FIRC Request


Banks handle FIRC requests through a written application, a branch visit, or a net-banking request. The bank asks for your account details, the UTR or SWIFT reference, the remitter name and country, the amount, and the purpose of remittance. A typical fee runs around Rs 200 to Rs 800 per e-FIRA plus GST.


### Step 3 – Provide the Correct Purpose Code


The purpose code is the most important field, because it must match your invoice description. Common codes include:


- P0802 – software implementation and consultancy where SOFTEX is not required. See the[P0802 purpose code explainer](https://razorpay.com/blog/p0802-purpose-code-guide/) .
- P0807 – off-site software exports covered by SOFTEX. See the[P0807 purpose code guide](https://razorpay.com/blog/p0807-purpose-code-software-exports/) .
- Codes for other information services and business consultancy.


With the wrong code, the bank may pause the credit or issue an FIRC that conflicts with your GST records.


**SOFTEX note (2026):** Under the new framework, SOFTEX is being consolidated into a monthly Export Declaration Form (EDF), effective 1 October 2026. As of writing, SOFTEX is still the operative filing. See the[SOFTEX filing resource](https://razorpay.com/softex-filing) .


### Step 4 – Wait for the e-FIRA and Verify Every Field


Turnaround is typically 2 to 7 working days. On arrival, check your name, account number, the foreign-currency amount, the INR equivalent, the date, the purpose code, and the remitter details. If there is an error, contact the bank with the invoice and contract and request an amended certificate.


### Step 5 – File and Organise the e-FIRA Alongside the Invoice


You get one e-FIRA per inward remittance, so 12 CAD payments means 12 e-FIRAs. Keep a register that links each sales invoice to its e-FIRA and UTR. Retain export documentation for at least 72 months (6 years).


> **PRO-TIP:** Set a 48-hour rule and request the e-FIRA within 48 hours of the UTR appearing on your statement. Banks issue it easily while the transaction is fresh.


## RBI Purpose Codes for Canadian CAD Payments – The Classification That Changes Everything


Purpose codes are not a formality for IT exporters. They decide whether your FIRC lines up with your GST records.


### Why Purpose Codes Are Not a Formality


RBI assigns alphanumeric purpose codes to every inward remittance. The code determines classification in EDPMS, whether the FIRC aligns with your GST invoice description, and your eligibility for certain export benefits. See this[RBI purpose code guide](https://razorpay.com/blog/rbi-purpose-code-remittance-compliance-guide/) .


### The Purpose Code Reference Table for Indian IT and Services Exporters


Descriptions are drawn from the RBI Annexure II purpose-code list.


Purpose Code Description (from RBI annexure) Typical Use for IT/Services SOFTEX/EDF Applicable?


P0802 Software implementation/consultancy (other than those covered in SOFTEX form) Software consultancy where SOFTEX not filed No


P0807 Off-site software exports covered by SOFTEX Remote software development, SOFTEX/EDF filed Yes


P0806 Other information services Content, design, research services No


P1006 Business and management consultancy and public relations services Business consultancy No


### What to Do If the Bank Applies the Wrong Code


Check the purpose field on the e-FIRA against your invoice. Correct it with a written request to the bank’s forex operations team, attaching the invoice as evidence. Corrections can take 1 to 3 weeks.


## How Razorpay Helps Indian IT and Services Businesses Collect International Payments from Canadian Clients


Indian IT businesses exporting to Canada face a recurring problem. The payment lands, but the compliance trail is fragmented, the forex cost is higher than expected, and documentation arrives late. Razorpay’s International Payments solution is built for the export side of this equation.


### Accept Payments from Canadian Clients via International Payment Gateway


Capability What it means for you


Card network acceptance Accept payments via major card networks through[Accept International Payments via Cards](https://razorpay.com/accept-international-payments/cards)


Multi-currency support Accept payments in 130+ currencies including CAD


Upfront visibility Real-time conversion with charges shown before settlement


Regulated routing Routed through Razorpay’s RBI-authorised aggregator infrastructure


Explore[Razorpay International Payments](https://razorpay.com/accept-international-payments) and the International Payment Gateway India.


### Clean Settlement and Compliance Documentation


- Every settled international payment generates a traceable record for e-FIRA purposes.
- A unified dashboard view of payments, invoices, and settlement data cuts reconciliation time.
- Purpose-code alignment is captured at onboarding. See[Razorpay Settlement](https://razorpay.com/settlement) .


### Why RBI-Authorised Infrastructure Matters


Razorpay holds RBI cross-border payment aggregator authorisation. Using an RBI-authorised aggregator means the inward payment flows through a regulated channel with proper EDPMS reporting.


## Compliance Rules Every Indian IT Business Receiving CAD Must Know in 2026


### Two Separate Clocks – the FEMA Realisation Clock and the GST/Rule 96A Trigger


- **FEMA realisation obligation:** you must realise and repatriate export proceeds within 15 months from the date of export, extended from nine months under the notification dated 13 November 2025. Missing it is a FEMA contravention with penalties. See the export realisation rules explainer.
- **GST/Rule 96A trigger:** under a LUT, if you do not realise payment within one year from the invoice date, or the FEMA-allowed period including RBI extensions, whichever is later, you must pay the uncharged IGST plus 18% interest.
- **Practical rule:** track the FEMA clock first, because it bites earliest, and treat the GST IGST reversal as the second trigger.


### Letter of Undertaking (LUT) – The Prerequisite for Zero-Rated Service Exports


An LUT is a declaration on the GST portal (Form GST RFD-11) that lets you export services without charging IGST upfront. File it at the start of each financial year, before your first zero-rated invoice. There is no fee. See the[export of services under the GST guide](https://razorpay.com/blog/export-services-gst-conditions-guide/) .


### EEFC Account Rules for CAD Receipts


You can credit up to 100% of eligible forex earnings to an EEFC account for operational use. Balances are for business forex expenditure, not indefinite parking. CAD held in EEFC and later converted uses the rate at conversion, not at receipt.


### Documentation and FEMA Reporting for Inbound CAD


The AD bank handles EDPMS reporting on inbound export remittances automatically. Your job is to keep supporting trade documents available. The[inbound receiving checklist](https://www.karboncard.com/blog/receive-cad-payments-from-canada) shows banks want the invoice or contract, recipient bank details, and compliance data up front. Very large or unusual credits may attract additional scrutiny under standard KYC and AML processes.


> **DID YOU KNOW:** RBI’s PA-CB licensing framework now lets authorised aggregators facilitate regulated inward and outward flows, reducing reliance on traditional SWIFT correspondent chains for some corridors.


## Common Mistakes Indian IT Businesses Make When Receiving CAD – And How to Avoid Them


- Sharing SWIFT details when a local CAD account is available. Check for a local receiving account first.
- Not requesting the e-FIRA until the CA asks. Chasing certificates at filing season invites delays.
- Letting the bank auto-assign the purpose code. Confirm it matches your invoice.
- Mixing personal and business inward remittances in one account.
- Ignoring the realisation clock. Track invoice age against the 15-month FEMA window.


> **PRO-TIP:** Build a tracker with columns: Invoice Date, CAD Amount, e-FIRA Received (Y/N), Age vs FEMA clock. Review it monthly.


## How Razorpay International Payments Fits Into Your Canada-to-India Payment Stack


Problem How Razorpay addresses it


The rail problem An RBI-compliant way to accept CAD via international card networks, with no dependency on the client knowing a SWIFT code


The forex problem Exchange rate and charges shown upfront before settlement, routed through Razorpay’s RBI-authorised aggregator


The FIRC problem Each settled payment generates a traceable record your AD bank can reference, inside a searchable dashboard linked to invoices


Ready to accept CAD from your Canadian clients with transparent forex and clean compliance records?[Explore Razorpay International Payments](https://razorpay.com/accept-international-payments) .


## Frequently Asked Questions


### What is FIRC, and do I need one for every CAD payment from Canada?


FIRC is the bank-issued proof that a foreign-currency payment was received from abroad. You need one e-FIRA per inward remittance for export compliance. It is the document your CA uses to support GST refunds.


### How do I receive payment from a Canadian client into my Indian bank account?


You can use three rails: a SWIFT bank wire, a local Canadian account through a cross-border provider, or an international payment platform. For SWIFT, share your SWIFT/BIC code, account number, and branch address. For a local account, share the Canadian account number your provider gives you.


### How do I get an FIRC for my export of services?


Confirm the credit and note the UTR, then contact your AD bank. Provide the correct purpose code plus your invoice, pay the issuance fee of around Rs 200 to Rs 800, and verify every field when it arrives.


### Is FIRC mandatory for service exports even if I am not claiming a GST refund?


The AD bank reports every inward remittance via EDPMS regardless, but you must retain the e-FIRA yourself. It becomes essential evidence if a refund is ever claimed, and it supports your FEMA and audit trail.


### How does a SWIFT transfer from Canada to India work, and how long does it take?


The Canadian bank routes the payment through one or two correspondent banks before it reaches your Indian AD bank, which converts CAD to INR. It typically takes 3 to 5 business days.


### What are the actual forex charges on a CAD inward remittance?


Three layers apply: the intermediary flat fee, the bank forex markup of roughly 2% to 4%, and the handling charge. On a CAD 5,000 SWIFT payment at about Rs 68.4 per CAD, the total loss lands around Rs 8,500 to Rs 13,500, roughly 2.5% to 4%.


### What is the difference between FIRC and e-FIRA?


FIRC is the traditional paper certificate, still issued mainly for FDI and FII inflows. e-FIRA is the current digital equivalent for service-export receipts. Both are accepted for GST and RBI purposes.


### What RBI purpose code should an Indian IT company use for CAD payments?


Use P0802 for software or IT consultancy where SOFTEX does not apply, and P0807 for off-site software exports covered by SOFTEX or the new EDF. Always match the code to your invoice description.
