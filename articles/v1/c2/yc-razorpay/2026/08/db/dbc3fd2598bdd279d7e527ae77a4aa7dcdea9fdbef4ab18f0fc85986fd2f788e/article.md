---
schema_version: "1.0.0"
document_id: "dbc3fd2598bdd279d7e527ae77a4aa7dcdea9fdbef4ab18f0fc85986fd2f788e"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/how-indian-it-and-services-companies-get-paid-from-german-clients-in-2026/"
published_at: "2026-08-17T13:24:07+00:00"
first_seen_at: "2026-08-17T16:16:20.169639+00:00"
fetched_at: "2026-08-17T16:16:22.287763+00:00"
content_hash: "sha256:5fca68538564f5bcd620df27bc39563b393b21b7afc7cc4ee6cf017dfadc7b3f"
---

# How Indian IT and Services Companies Get Paid from German Clients in 2026

Your German client emails: “Payment sent, EUR 8,000 as agreed.” Three days later your account shows closer to INR 7.1 lakh instead of the roughly INR 7.4 lakh you calculated. Where did the missing money go? The answer is not fraud, and it is not your client. It is the payment rail itself.


Most Indian IT firms assume a German bank transfer works like any other wire. It does not. India sits outside the SEPA zone that Germany pays into domestically, so every EUR invoice is forced onto SWIFT, where intermediary banks and forex spreads quietly skim 2 to 4 percent before the money reaches you.


India’s services exports reached USD 387.55 billion in FY 2024-25, with IT and business services forming the largest share, and Germany is one of the fastest-growing source markets in that story. This guide corrects the expensive assumption that a EUR payment from Germany behaves like a normal wire transfer. We walk you through exactly how EUR moves from a German bank to your Indian account, what it costs at each step, and what you need to stay compliant with RBI and GST.


### Key Takeaways


- German clients send EUR via SEPA within Europe, but India is outside the SEPA zone, so the transfer must re-route through SWIFT before reaching an Indian bank, unless you use a virtual European IBAN.
- There are four practical ways to receive EUR: SWIFT wire to your Indian bank, virtual IBAN accounts that accept SEPA and settle INR, card-based international payment gateways, and digital wallet platforms (limited use for businesses).
- Every inward EUR remittance must carry an RBI purpose code (P0802 for software and IT services is most common) and generate a FIRC or FIRA for GST and income tax compliance.
- The three hidden cost layers in a SWIFT transfer are the outgoing wire fee, correspondent bank deductions in transit, and your Indian bank’s forex markup, with independent estimates placing correspondent deductions at USD 10 to 25 and forex spreads around 1.5 to 2 percent.
- The RBI export realisation period was extended to 15 months effective 13 November 2025, giving Indian IT firms more time to chase delayed German client payments.
- Export of IT services from India to Germany is zero-rated under GST. File a Letter of Undertaking (LUT) to export without charging IGST.


## Why Is Receiving EUR from Germany Different from Other International Payments?


EUR from Germany travels on SEPA rails within Europe, but India is not a SEPA member. A German client’s “domestic bank transfer” cannot land directly in an Indian account. It is forced onto SWIFT instead, triggering international wire fees, correspondent bank deductions, and forex conversion, costs that stay invisible until you check what actually arrived.


### What Is SEPA and Why Does It Matter for Indian IT Exporters?


SEPA is the Single Euro Payments Area, the system that lets EUR move cheaply and instantly across Europe.[SEPA includes 41 countries](https://www.europeanpaymentscouncil.eu/document-library/other/epc-list-sepa-scheme-countries) , covering all EU member states plus others such as the UK, Switzerland, and Norway.


Within SEPA, a EUR transfer costs cents, settles overnight, and needs only an IBAN. Indian banks are not SEPA members. When a German client’s bank initiates a “bank transfer” to India, their system flags the destination as non-SEPA and re-routes the payment via SWIFT automatically, often without the client even knowing.


Did you know: India is the world’s largest recipient of inward remittances at about USD 129 billion in 2024, yet it cannot directly receive a SEPA transfer. See our plain-English guide on how to[receive EUR from European clients](https://razorpay.com/blog/how-indian-businesses-receive-eur-from-european-clients-a-plain-english-guide-to-sepa-and-swift/) .


### What Actually Happens When Your German Client Clicks “Send Payment”?


1. German client initiates a EUR transfer from their corporate bank.
2. Their bank identifies the destination as non-SEPA and re-routes it as a SWIFT international wire.
3. The SWIFT message passes through one to three correspondent banks, each potentially deducting a transit fee.
4. The payment arrives at your Indian AD Category-I bank in EUR or USD.
5. Your Indian bank converts to INR at its own rate, typically below the mid-market rate.
6. You receive INR, often several business days after the transfer.
7. You then request a FIRC to document the inward remittance for GST and tax compliance.


Net result: on a EUR 10,000 invoice, a forex markup of even 1.5 to 2 percent quietly costs you thousands of rupees before you count a single SWIFT fee.


## Which Payment Methods Do German Clients Actually Use to Pay Indian IT Companies?


German corporate clients pay Indian IT companies almost exclusively by bank transfer (Bankuberweisung, meaning a standard bank credit transfer). SEPA Credit Transfer is standard within Europe, but for India it converts to SWIFT. Large German enterprises (GmbH, AG) typically work on Net 30 to Net 60 payment terms (Zahlungsziel, meaning the payment deadline) and require fully compliant invoices before initiating payment.


### German Corporate Payment Culture – What Indian IT Firms Need to Know


German businesses operate on formal invoice approval cycles. Payment rarely releases without a compliant invoice sitting in their ERP system. Standard payment terms are Zahlungsziel 30 or 60 days. German mid-market (Mittelstand) companies rarely use credit cards for B2B service payments. Bank transfer is the default.


German accounting systems require specific invoice fields before payment can be approved: Rechnungsnummer (invoice number), Leistungszeitraum (the service period), your company’s full address, and the reverse charge notation under EU VAT rules. Missing any required field can delay payment by a full billing cycle, because their accounts payable system will reject the invoice outright.


### Why German Clients Cannot Just “Send EUR Directly” to an Indian Bank Account via SEPA


SEPA Credit Transfers require the destination account to hold a valid European IBAN. Indian bank accounts use IFSC codes, not IBANs. A German client who tries to enter an Indian account number into a SEPA payment form will receive an error. This is exactly why the virtual IBAN solution, a European IBAN that forwards funds and settles INR in India, is the most friction-free option for German payers.


## The 4 Ways Indian IT Companies Can Receive EUR from German Clients


Indian IT companies have four practical routes: (1) SWIFT wire directly to an Indian bank account, (2) virtual European IBAN via a fintech platform that accepts SEPA and settles INR, (3) card-based international payment gateway, and (4) digital wallet platforms. The virtual IBAN route delivers the best combination of speed, cost, and automatic FIRC generation for most IT exporters.


### Comparison Table: EUR Receipt Methods for Indian IT Exporters


Method Speed Client-Side Cost Your Receiving Cost Forex Markup FIRC/FIRA Best Suited For


SWIFT to Indian bank 3-7 business days Outgoing wire fee Correspondent deductions in transit Hidden markup below mid-market Manual, may cost extra Large, infrequent B2B transactions


Virtual IBAN via SEPA fintech 1-2 business days Near zero (domestic SEPA) Low flat fee or low % Transparent, often near mid-market Automatic e-FIRC Regular EUR exporters, agencies, SaaS


International PG – card-based 0-2 business days None Per-transaction fee including forex Included in stated fee Varies by provider SaaS, e-commerce, one-off invoices


Digital wallet platform Minutes to 3 days Low Wallet plus withdrawal fee Variable Often manual, not auto-generated Small occasional freelance payments only


### Method 1 – SWIFT Wire Transfer to Your Indian Bank Account


**How it works:** You share your Indian account number, IFSC, and SWIFT/BIC code. The German client initiates an international wire that routes via SWIFT through correspondent banks to your Indian AD bank.


**Speed:** 3 to 7 business days.


**The three cost layers:** (a) the German client pays an outgoing wire fee; (b) correspondent banks deduct their charges in transit, often around USD 10 to 25; (c) your Indian bank applies a forex markup below the mid-market rate.


**Best for:** Large, infrequent contracts (EUR 50,000 and above) where the per-transaction loss is proportionally smaller.


**Key risk:** You may receive less than invoiced with no clear explanation. See our guide on[how to receive money from abroad in India](https://razorpay.com/blog/receive-money-from-abroad-in-india/) for the full SWIFT documentation checklist.


Pro-tip: Add a bank charges clause to your invoice: “All banking charges outside India to be borne by the remitter (OUR charges).” This tells the client’s bank to collect all fees upfront. Some intermediary banks may still deduct a fee even with OUR selected, so it reduces the risk rather than eliminating it.


### Method 2 – Virtual European IBAN via Fintech Platform (Recommended for Most Indian IT Exporters)


**How it works:** A fintech platform gives you a European IBAN with a real EU bank’s BIC. Your German client sends a cheap, instant domestic SEPA Credit Transfer to this account. The platform converts EUR to INR and credits your Indian bank account.


**Speed:** Typically 1 to 2 business days from SEPA receipt to INR credit.


**Client cost:** Near zero. This is a domestic SEPA transfer for the German client, identical to paying a local vendor.


**Your cost:** Typically a low flat fee or a transparent percentage with no hidden correspondent deductions.


**FIRC:** The best platforms auto-generate an e-FIRC per transaction, downloadable from your dashboard.


**Best for:** IT agencies, consulting firms, SaaS exporters, and freelancers with recurring EUR invoices.


To hold EUR before converting, read about[what is an EEFC account](https://razorpay.com/blog/what-is-an-eefc-account/) . Some virtual account platforms also allow you to hold EUR balances. This route eliminates the SWIFT routing problem and removes the correspondent bank fee layer.


### Method 3 – International Payment Gateway (Card-Based EUR Receipts)


**How it works:** You generate a Payment Link or hosted payment page. Your German client pays using Visa, Mastercard, or Amex in EUR. The gateway converts and settles INR.


**Speed:** 0 to 2 business days.


**Cost:** Typically a percentage per transaction including FX conversion, stated upfront.


**Limitation:** German B2B clients rarely prefer to pay invoices by card. This method works better for SaaS subscriptions, digital product sales, or smaller one-time invoices.


**FIRC:** Varies by provider. Look for platforms that auto-generate e-FIRC per settlement. Razorpay’s[International Payment Gateway](https://razorpay.com/accept-international-payments/) supports EUR card payments in 130+ currencies with RBI-compliant settlement and automatic FIRC.


### Method 4 – Digital Wallet Platforms (Proceed with Caution)


**How it works:** You hold a EUR wallet balance and withdraw to your Indian bank.


**Speed:** Minutes to 3 days for wallet credit, plus 1 to 3 additional days for INR withdrawal.


**Key limitation:** Most wallet platforms do not auto-generate FIRC or FIRA, which creates friction during GST audits.


**FEMA note:** Inward remittances via wallet platforms still need to route through an AD Category-I bank to be FEMA-compliant. Verify your platform’s banking partner first.


**Verdict:** Acceptable for individual freelancers receiving small, occasional payments. Not recommended as the primary channel for registered Pvt Ltd or LLP entities with regular German client billing.


## How Razorpay Helps Indian IT Companies Receive EUR from German Clients


Indian IT exporters face two distinct payment receipt scenarios: recurring project-based bank transfers and one-off or subscription card payments. Razorpay addresses card-based receipts and the compliance documentation that follows.


Razorpay’s[International Payment Gateway](https://razorpay.com/accept-international-payments/) lets Indian IT companies accept EUR card payments in 130+ currencies with real-time INR conversion.


Feature What It Means for You


Card acceptance Accept Visa, Mastercard, and Amex in EUR from German clients


130+ currencies Real-time conversion to INR at settlement


Payment Links Share a payment page via email or WhatsApp, no website required


Automatic e-FIRC Compliance documentation generated per transaction, downloadable from your dashboard


RBI-compliant settlement Settled via AD Category-I banking partners, FEMA-compliant end to end


Pro-tip: For card-based EUR receipts, the real cost is often the declined transaction, not the stated fee. Choose a gateway with intelligent routing and retry logic to recover otherwise-lost revenue.


## What Should You Put on a EUR Invoice for a German Client?


Yes, Indian IT exporters can and should invoice German clients in EUR. German corporate accounting requires specific fields on every invoice before payment is approved: a sequential invoice number (Rechnungsnummer), the service period (Leistungszeitraum), full company addresses on both sides, a reverse charge notation under EU VAT rules, and your EUR bank details or IBAN.


### Mandatory Fields on a EUR Invoice for German Clients


**Indian exporter side:**
– Your full legal company name and registered address
– Your PAN and GST registration number
– Your EUR receiving details: IBAN or SWIFT/BIC plus Indian account number
– Invoice number, sequential (Rechnungsnummer), and invoice date
– RBI Purpose Code (for example, P0802, see our[RBI purpose code guide](https://razorpay.com/blog/rbi-purpose-code-remittance-compliance-guide/) )


**Service description:**
– Service period (Leistungszeitraum), the date range of services rendered
– Clear description: “Software Development Services – \[Project Name\] – \[Month/Year\]”
– Invoice currency: EUR, stated explicitly


**GST and VAT fields:**
– GST note: “Export of Services under LUT – IGST not charged (Zero-Rated Supply under IGST Act)”
– EU reverse charge notation: “Reverse Charge – VAT to be accounted for by the recipient”
– Your German client’s VAT ID (Umsatzsteuer-Identifikationsnummer, the German VAT identification number)


**Bank charges:** “All banking charges outside India to be borne by the remitter (OUR)”


### In Which Currency Should You Invoice – EUR or USD?


EUR is strongly preferred. German corporate finance teams work in EUR, and triggering a USD conversion on their side adds complexity. Invoicing in EUR also gives you cleaner FEMA compliance, as the purpose code and FIRC match the currency stated on your invoice.


For large contracts, some Indian firms invoice in USD by agreement. This is acceptable under FEMA but requires clarity on which party bears the conversion cost. Invoicing in INR almost never works for German corporate accounts payable systems and usually requires reissuing the invoice.


Pro-tip: Include your service period (Leistungszeitraum) on every invoice even if not explicitly asked. German enterprise ERP systems flag invoices without a service period for manual review, so adding this single field can meaningfully shorten your payment cycle.


## RBI and FEMA Compliance for EUR Payments from German Clients


Yes, every EUR payment from a German client requires FIRC or FIRA documentation under FEMA. It also requires an RBI purpose code on the remittance instruction. For Indian IT and software services companies, the most commonly applicable code is P0802. Without a FIRC, you cannot claim GST export refunds or defend the receipt during a tax audit.


### What RBI Purpose Code Should an Indian IT Company Use for German Client Payments?


Purpose Code Applies To Notes


P0802 Software implementation, IT consulting, professional services Most common for Indian IT services companies billing German clients


P0807 Computer software, packaged and off-the-shelf Use for exported software products; triggers SOFTEX requirement


P0801 Hardware consultancy Only if your service is hardware-specific


P1004 Management consulting and advisory For strategy or business consulting firms


P0603 Advertising and market research Digital marketing agencies billing German clients


A wrong or missing purpose code causes your bank to hold the remittance pending a declaration letter, delaying settlement by a few business days. Declare the code at the time of receiving the remittance, not afterwards. For more, see our[P0802 purpose code guide](https://razorpay.com/blog/p0802-purpose-code-guide/) .


### What Is FIRC/FIRA and How Do You Get One for a German Client Payment?


FIRC (Foreign Inward Remittance Certificate) documents an inward remittance. FIRA (Foreign Inward Remittance Advice) is the lighter digital proof of credit that fintech platforms often provide. Since 2016, exporters generally receive a system-generated electronic e-FIRC reported to RBI rather than a physical certificate.


You need it for GST export refund claims, income tax filings, export incentive scheme applications, and audit defence. Traditional Indian banks may[charge around INR 200 to 500 per FIRC](https://razorpay.com/blog/bank-charges-inward-remittance-india/) and may require a branch visit. Modern fintech platforms that process SEPA-to-INR settlements auto-generate e-FIRC or FIRA from your dashboard. Read the full[FIRC certificate guide](https://razorpay.com/blog/firc-certificate/) for documentation requirements.


### What Is the RBI Export Realisation Period and Why Does It Matter?


Under FEMA, every Indian exporter of services must realise payment within a set window from the invoice date. The RBI extended this window from 9 months to 15 months,[effective 13 November 2025](https://leatherindia.org/wp-content/uploads/2025/11/FE-amendments-RBI-NOTIFICATION-DATED-13.11.25.pdf) , giving Indian IT exporters more time before a delayed German client payment becomes a FEMA non-compliance issue.


If a German client delays beyond 15 months, you must apply for an extension with your AD Category-I bank, supported by correspondence showing you have actively chased the payment. A payment that exceeds the realisation window is a reportable compliance event under FEMA, so treat overdue German invoices as a legal matter, not just a collections matter. See the[RBI circular on export realisation](https://razorpay.com/blog/rbi-circular-on-export-realisation/) for extension procedures.


### Does SOFTEX Filing Apply to Indian Software Exporters Billing German Clients?


SOFTEX is a declaration form filed with the Software Technology Parks of India (STPI) or Special Economic Zones for export of computer software. It applies if you are exporting products rather than services.


If you are a service-based IT company billing for development hours, consulting, or managed services, SOFTEX may not apply, and P0802 is typically used. If you are exporting a software product, SOFTEX filing may apply and the correct purpose code shifts to P0807. Flag this with your CA and AD bank before filing, and see our[P0807 purpose code guide](https://razorpay.com/blog/p0807-purpose-code-software-exports/) .


## The Indo-German DTAA – Does Germany Withhold Tax Before Paying Your Invoice?


Germany can withhold tax at source on payments for Fees for Technical Services (FTS) under the Indo-German Double Taxation Avoidance Agreement. The DTAA caps this withholding at 10 percent of the gross payment. Indian IT companies can prevent over-withholding by providing their German client with a Tax Residency Certificate (TRC) issued by the Indian Income Tax Department.


### Which Indian IT Services Attract Withholding Tax in Germany?


Under Article 12 of the India-Germany DTAA, the maximum withholding tax rate on royalties and fees for technical services is 10 percent of the gross amount. “Technical services” includes software development, engineering consulting, data processing, and similar IT-enabled services. The India-Germany treaty has no “make available” test for FTS, so assuming your fees escape FTS classification does not hold here.


If your German client pays EUR 10,000 for IT services, they may legally withhold EUR 1,000 and remit EUR 9,000 to you, then pay the EUR 1,000 to the German tax authority. You then claim a credit for that EUR 1,000 against your Indian income tax liability using Form 67 and the TRC.


### How to Prevent German Clients from Withholding Tax on Your Invoice


Provide a Tax Residency Certificate (TRC) from the Indian Income Tax Department to your German client. This proves you are an Indian tax resident and triggers the DTAA benefit. Apply for a TRC using[Form 10FA](https://www.incometaxindia.gov.in/w/form_10fa) with your Assessing Officer. The certificate is issued in Form 10FB.


Some German clients withhold tax out of procedural caution even when not required. A TRC, with a brief letter referencing the Indo-German DTAA, usually resolves this. If your client is paying for generic business process outsourcing rather than technical services, the FTS article may not apply. Confirm with your CA.


## What Banking Details Should You Send to Your German Client?


Give your German client one of two sets of details depending on your setup. For SWIFT: your beneficiary name, Indian bank name and address, account number, SWIFT/BIC code, IFSC code, and invoice reference. For virtual IBAN: the European IBAN and BIC provided by your fintech platform, the platform’s European bank name, and a note that this is a SEPA Credit Transfer.


### SWIFT Details Checklist for German Client Payments to India


- Beneficiary name exactly as it appears on the account (registered company name, not a trading name)
- Beneficiary bank name and full branch address
- Account number (a current account is required for business export receipts)
- SWIFT/BIC code of your Indian bank
- IFSC code (include even if the client’s system does not ask)
- Invoice number and currency (EUR) in the payment reference field
- Purpose of payment stated clearly


### Virtual IBAN Details Checklist for German Client SEPA Payments


- Beneficiary name as registered on the fintech platform (must match your business name exactly)
- IBAN: the European IBAN assigned by your platform
- BIC/SWIFT of the platform’s European banking partner
- Bank name and European address of the banking partner
- Payment reference: invoice number (critical for reconciliation)
- A note: “This is a domestic SEPA Credit Transfer – no international wire fees apply on your side.”


### What to Tell Your German Client Before They Initiate the Transfer


Specify SEPA Credit Transfer explicitly if using a virtual IBAN. Do not say “bank transfer” generically or your client may default to SWIFT. German accounting teams often need a signed declaration for first-time international vendor setup, and enterprises using ERP systems may need your details entered as a foreign vendor, requiring your PAN or business registration number.


## GST Treatment for Indian IT Companies Receiving EUR from German Clients


IT services provided from India to a German client qualify as export of services under GST and are zero-rated, meaning 0 percent GST. Indian IT companies can export without collecting IGST by filing a Letter of Undertaking (LUT) annually on the GST portal. Without an LUT, you must charge 18 percent IGST and claim a refund later, an unnecessary cash flow burden.


### What Is a Letter of Undertaking (LUT) and Why Every Indian IT Exporter Needs One?


An LUT is a declaration filed on the GST portal (Form RFD-11) stating that you will export services without collecting IGST, and that export proceeds will be received within the prescribed RBI period. The LUT is furnished in Form GST RFD-11 before affecting such supply and must be filed for each financial year.


Without an LUT, your invoice must include 18 percent IGST, which you pay upfront and claim back later, tying up working capital. With an LUT, your invoice shows 0 percent IGST and you collect the full EUR amount. File the LUT before the first export invoice of each financial year. See our guides on[export of services under GST](https://razorpay.com/blog/export-services-gst-conditions-guide/) and LUT under GST.


### Does the German Client Pay German VAT on Your Invoice?


No. You do not charge German VAT. You are an Indian service provider, not a German-registered business. Under EU VAT rules, B2B services from a non-EU supplier to a German business fall under the reverse charge mechanism, so the customer accounts for the VAT. Your invoice must carry “Reverse Charge – VAT to be accounted for by the recipient.”


This is separate from Indian GST. If your German client asks whether you are VAT-registered, clarify that you are GST-registered in India but not VAT-registered in Germany.


## How to Set Up Your EUR Receiving Infrastructure – A Step-by-Step Process


### Step 1 – Choose Your Primary Method Based on Business Profile


- IT agency or SaaS with recurring EUR invoices: virtual IBAN account
- SaaS subscriptions or digital products: international payment gateway
- Large-value B2B exporter with occasional contracts: SWIFT to Indian bank
- Freelancer or consultant: virtual IBAN for ease; wallet as a secondary channel only


### Step 2 – Register and Complete KYC


- Documents: business registration, GST certificate, PAN, bank details, and signatory KYC
- Fintech virtual IBAN platforms complete online KYC, typically in 1 to 5 business days
- All EUR receipts must route through an AD Category-I bank


### Step 3 – Update Your Invoice Template


- Add your EUR receiving details, the RBI purpose code, the bank charges clause, and your GST/LUT status
- Add the Leistungszeitraum and Rechnungsnummer fields
- Add a note instructing the client to use SEPA Credit Transfer if paying to your virtual IBAN


### Step 4 – Confirm the First Payment and Save Your FIRC


- Download or save your FIRC or FIRA immediately after the first receipt
- Verify the purpose code was applied correctly
- Store FIRCs by financial year for GST returns and export incentive claims


## Common Mistakes Indian IT Companies Make When Receiving EUR from Germany


- **Sharing the wrong bank details:** Giving a SWIFT account when the client expected an IBAN, causing returns or expensive routing.
- **Missing or wrong purpose code:** Causes your bank to hold funds pending a declaration letter.
- **Not instructing the client on transfer method:** A client who does not know SEPA from SWIFT will default to the expensive option.
- **Ignoring the forex markup:** Choosing a method on stated fees alone while ignoring the silent spread.
- **Failing to collect FIRC:** Missing FIRCs make GST refund and incentive claims difficult during audits.
- **Omitting German invoice fields:** A missing Leistungszeitraum or Rechnungsnummer can delay payment by a full billing cycle.


## Frequently Asked Questions


**Q1: Can my German client send EUR directly to my Indian bank account?**


Yes, but it routes as a SWIFT wire, not a SEPA transfer, because India is outside the SEPA zone. Your client pays an outgoing fee, and you may lose to forex markup plus correspondent deductions. A virtual European IBAN lets your client send a cheap domestic SEPA transfer that converts and settles in INR.


**Q2: What is the correct RBI purpose code for an IT company receiving EUR from Germany?**


For software development, IT consulting, and professional services, the most common code is P0802. For exported software products requiring SOFTEX filing, use P0807. Always confirm with your AD Category-I bank.


**Q3: Does the Indo-German DTAA mean Germany withholds tax before paying me?**


Germany can withhold up to 10 percent under the DTAA on fees for technical services. Provide a Tax Residency Certificate (TRC) to trigger the treaty benefit. Any tax withheld can be credited against your Indian tax liability using Form 67.


**Q4: Do I need to charge German VAT on my invoice?**


No. Under the EU reverse charge mechanism, your German B2B client self-accounts for German VAT. Add the notation “Reverse Charge – VAT to be accounted for by the recipient” on your invoice. This is separate from Indian GST.


**Q5: Can I hold EUR in India instead of converting immediately?**


Yes. An EEFC (Exchange Earners’ Foreign Currency) account lets Indian exporters hold EUR before converting to INR. Read our[EEFC account guide](https://razorpay.com/blog/what-is-an-eefc-account/) for eligibility and benefits.


**Q6: What happens if my German client does not pay within 15 months?**


The RBI export realisation window is 15 months from the invoice date, effective 13 November 2025. If a client delays beyond this, apply for an extension with your AD Category-I bank, supported by evidence you actively chased the payment. Treat overdue invoices as a FEMA compliance matter.
