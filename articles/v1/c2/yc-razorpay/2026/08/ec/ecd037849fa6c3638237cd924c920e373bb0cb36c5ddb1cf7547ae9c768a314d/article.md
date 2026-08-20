---
schema_version: "1.0.0"
document_id: "ecd037849fa6c3638237cd924c920e373bb0cb36c5ddb1cf7547ae9c768a314d"
company_key: "yc-razorpay"
company: "Razorpay"
source_id: "yc-razorpay-rss-1480baa13d4e"
canonical_url: "https://razorpay.com/blog/multi-currency-card-payments-for-indian-businesses-currencies-rates-conversion/"
published_at: "2026-08-19T12:49:01+00:00"
first_seen_at: "2026-08-19T13:07:18.795010+00:00"
fetched_at: "2026-08-19T13:07:19.524154+00:00"
content_hash: "sha256:423880d6932eae179e3603e2f0cafe5102fbf2bc4e8f658d3d91f07a3e7cbf67"
---

# Multi-Currency Card Payments for Indian Businesses (Currencies, Rates, Conversion)

You just landed your first overseas order. The customer reached checkout, the excitement was real, and then the international card was declined. If you searched “why do international cards fail on my Indian checkout” or “how to accept payments from foreign clients,” you are in the right place.


The real question underneath all of this is simple: how do I actually accept payments from customers abroad, and what will it truly cost me?


Many founders assume their existing domestic gateway handles international cards “somehow.” It does not. A domestic gateway may attempt to charge the card, but it does not natively handle multi-currency presentment, real-time conversion, FIRC generation, or FEMA compliance.


That gap is exactly where orders die and money quietly leaks.


This guide resolves the whole picture: the two-directional flow of exports and imports, the true layered fee breakdown, the settlement and compliance mechanics, and how to choose the right setup for your business type.


### Key Takeaways


- A multi-currency payment gateway in India lets businesses accept card payments in 100+ currencies and settle in INR, with the gateway handling conversion, compliance documentation (FIRC/e-FIRS), and RBI-mandated reporting.
- The true all-in cost of an international card transaction includes MDR (typically 3 to 3.5%), a card network cross-border assessment fee (0.5 to 1%), and a provider FX markup (0.5 to 1.5%), three separate charges, not one.
- Indian businesses accepting foreign payments must comply with FEMA realisation timelines, RBI guidelines, and GST zero-rating rules, and must ensure their gateway auto-generates FIRC or e-FIRS for every inward remittance.
- There are two distinct flows: the Export Flow (Indian business receives from a foreign client) and the Import Flow (a foreign business collects from an Indian consumer).
- Around 60 percent of international shoppers prefer to pay in their home currency, and local currency display can lift international conversion meaningfully.


## What Is a Multi-Currency Payment Gateway – and Why Does the Indian Definition Differ?


A multi-currency payment gateway is a payment processing system that lets Indian businesses accept card payments from customers in their home currencies (USD, EUR, GBP, AED, SGD, and others) while settling funds into the merchant’s Indian bank account in INR. It handles real-time currency conversion, fraud checks, authorisation, and generates mandatory compliance documents like FIRC.


The definition sounds global, but the Indian version carries extra weight. Here, the gateway is not just a conversion tool. It is your compliance layer.


### Why a domestic gateway is not the same thing


- A standard Indian gateway accepts INR transactions. It may allow an international card to be charged, but it does not handle multi-currency presentment, DCC, or automated FIRC generation. Read the[international payment gateway guide](https://razorpay.com/blog/what-is-a-international-payment-gateway/) for the deeper distinction.
- When an international card is declined at an Indian checkout, the most common cause is not fraud. It is a missing multi-currency capability, or a failure to navigate India’s 2FA/OTP requirement.


### The two flows Indian businesses need to understand


- **Export Flow:** An Indian business (IT agency, SaaS company, D2C exporter, freelancer) collects payment from a foreign customer. The foreign customer pays in their currency; the Indian business receives INR.
- **Import Flow:** A global business (an overseas platform, university, or foreign SaaS tool) collects payment from an Indian consumer. The global business wants to accept UPI, RuPay, and local Indian payment methods, and receive settlement in its home currency.


Think of it as two arrows pointing in opposite directions. Each direction has its own rails, its own compliance, and its own ideal product. Getting them straight is the fastest way to choose correctly.


## How Does a Multi-Currency Payment Gateway Work? (Step-by-Step for Indian Businesses)


When an international customer pays on an Indian merchant’s site, the gateway detects the card’s country of issue via BIN lookup, presents the price in the buyer’s local currency, processes the transaction through global card networks, converts the amount at a live FX rate, deducts fees, and credits INR to the merchant’s Indian bank account within T+2 to T+7 days.


### Step 1 – Currency detection via BIN lookup


- The gateway reads the first six digits of the card number (Bank Identification Number) to identify the issuing country and currency, triggering the multi-currency flow automatically.


### Step 2 – Dynamic currency display or multi-currency presentment


- The checkout shows the buyer their price in their local currency (for example, USD 49 instead of INR 4,100).
- Dynamic Currency Conversion (DCC) offers the buyer a choice to pay in their home currency (at a rate that includes a DCC markup of 3 to 5%) or in INR.


### Step 3 – Authorisation through international card networks


- The gateway routes the transaction to global card networks (Visa, Mastercard, Amex).
- Because Indian regulations require 2-factor authentication (2FA/OTP), the gateway must support 3D Secure 2.0 to avoid a decline.


### Step 4 – Currency conversion and fee deduction


- At settlement, the gateway converts the foreign currency to INR using the prevailing exchange rate plus its FX markup.
- Three distinct charges are deducted: MDR, card network cross-border assessment, and FX markup.


### Step 5 – INR settlement to the Indian merchant’s bank account


- Funds are credited to the merchant’s current account in INR.
- Standard settlement: T+2 to T+7 business days for international transactions versus T+2 for domestic.
- The gateway generates an e-FIRS or FIRC for each transaction.


### Step 6 – Reporting and reconciliation


- The dashboard shows each transaction in the original foreign currency alongside the settled INR amount, the exchange rate, and fees deducted. This data is critical for GST filing and FEMA reporting.


> **PRO-TIP:** Enable 3D Secure 2.0 on your international gateway from day one. Most international card declines on Indian checkouts are caused by authentication failures, not insufficient funds.


## How Razorpay’s International Payment Gateway Works for Indian Businesses


Everything above is the map. Here is the vehicle. Razorpay’s[International Payment Gateway](https://razorpay.com/accept-international-payments/) is built for both directions of the cross-border flow – the Export Flow and the Import Flow via[Razorpay’s inbound international product](https://razorpay.com/accept-international-payments-from-india/) .


### Export Flow – Indian businesses collecting from foreign clients


Capability What it does for you


Currency coverage Receive payments in 135 currencies from customers in 180+ countries


Transaction fee 3% for international card payments (flat rate with forex included)


Security PCI DSS Level 1 compliant, with 3DS 2.0 authentication


Documentation e-FIRS available through the dashboard for every inward remittance


No-code collection Payment Links in foreign currencies for freelancers and agencies


Recurring billing International card mandates via Subscriptions for SaaS billing


### Import Flow – Global businesses collecting from Indian consumers


- Accepts UPI, RuPay, Indian net banking, and domestic debit/credit cards.
- Enables global platforms to accept payments from Indian customers in INR.
- Handles RBI’s tokenisation and data localisation requirements.


### Why our RBI-authorised license matters


- We hold the RBI Payment Aggregator Cross Border (PA-CB) licence, covering both inward and outward cross-border payments.
- As an RBI-authorised aggregator, we handle funds directly, supporting regulatory certainty for every international transaction.


### Integration for international payments


- APIs, SDKs (iOS, Android, React Native, Flutter), and plugins for Shopify, WooCommerce, Magento.
- Payment Pages and Payment Links for no-code collection.
- n8n native integration enables[AI-powered payment workflows](https://razorpay.com/blog/introducing-the-razorpay-node-for-n8n-ai-powered-payments-for-every-builder/) .


## What Are the Real Fees for International Card Payments in India? (The Three-Layer Breakdown)


International card payments in India carry three separate fee layers: (1) the Merchant Discount Rate (MDR), typically 3 to 3.5% for international cards; (2) a card network cross-border assessment fee of 0.5 to 1%; and (3) an FX markup of 0.5 to 1.5% applied during currency conversion. GST at 18% applies on the forex conversion service component.


### Fee Layer 1 – Merchant Discount Rate (MDR)


- Standard Indian domestic MDR: around 2%. International card MDR: 3 to 3.5%.
- This is the fee the gateway keeps for processing. See the[Merchant Discount Rate explained](https://razorpay.com/blog/merchant-discount-rate-mdr/) guide.


### Fee Layer 2 – Card Network Cross-Border Assessment Fee


- Card networks charge an additional 0.5 to 1% when the issuing and acquiring banks sit in different countries.
- This fee is rarely shown separately and is typically embedded in the quoted MDR.


### Fee Layer 3 – FX Markup (the Hidden Erosion)


- The gateway converts foreign currency to INR using the mid-market rate plus a markup of 0.5 to 1.5% for transparent providers.
- Traditional banks embed 3 to 5% FX markups, meaning gateways with transparent FX pricing save merchants 1 to 2% per international transaction.


### The GST add-on most merchants miss


- GST at 18% applies on the forex conversion service fee, not the full transaction value.
- Example: On a USD 1,000 receipt with a 1% FX markup (around Rs 840), GST adds about Rs 151.


### True all-in cost – an honest example


All figures below are indicative for illustration only.


Fee Component Rate On USD 1,000 (approx. INR 84,000)


MDR (international card) 3% Rs 2,520


Cross-border assessment 0.75% Rs 630


FX Markup 1% Rs 840


GST on FX Markup 18% of markup Rs 151


**Total cost** **~4.75 to 5%** **~Rs 4,141**


**You receive** **~Rs 79,859**


For a broader view, see the full[payment gateway charges guide](https://razorpay.com/blog/convenience-fee-tdr-mdr-platform-fee-amc-setup-fee-technology-fee-of-payment-gateway/) .


> **DID YOU KNOW:** Transparent gateways with FX markups of 0.5 to 1.5% save Indian businesses 1 to 2% per international transaction. On Rs 1 crore of annual export revenue, that difference is Rs 1.5 lakh to Rs 3.5 lakh staying in your business.


## What Currencies Can Indian Businesses Accept Through a Multi-Currency Gateway?


Leading multi-currency gateways support 100 to 150+ currencies. The most commercially important for Indian exporters are USD, EUR, GBP, AED, SGD, AUD, CAD, and JPY. Card networks supported include Visa, Mastercard, Amex, and Diners Club. Actual availability depends on the gateway product and merchant category.


### Priority currencies for Indian exporters (by trading volume)


- **USD** – USA and global SaaS clients; highest volume for Indian IT, consulting, and D2C exporters.
- **EUR** – European Union clients; growing for software and services exports.
- **GBP** – UK-based clients; important for freelancers and agencies.
- **AED** – UAE buyers; significant for D2C brands selling into the Gulf.
- **SGD** – Singapore corporate clients and investors.
- **AUD and CAD** – Growing markets for SaaS and education exports.


### Card networks accepted


- Visa, Mastercard, American Express (Amex), and Diners Club.


### What currencies cannot be accepted


- Currencies from RBI-restricted or sanctioned jurisdictions cannot be processed.
- The gateway’s own approved currency list also applies. Confirm before signing up.


### UPI for the Import Flow


- For the Import Flow, the gateway must support UPI, RuPay, and Indian net banking, not just international cards.
- UPI reached 24,161.69 crore transactions worth Rs 314.23 lakh crore in FY26, so a global business not accepting UPI is missing India’s dominant payment rail.


## What Happens to Your Money After an International Payment? (Settlement, Timelines, and the FEMA Realisation Window)


After an international card payment is authorised, the gateway converts the foreign currency to INR and credits your Indian bank account within T+2 to T+7 business days. Under FEMA, export proceeds must currently be realised within 9 months for shipments dated before 1 October 2026, moving to a 15-month standard afterwards.


### Settlement timeline by payment type


Payment Type Typical Settlement to Indian Bank


International card via gateway T+2 to T+5 business days


Bank wire (SWIFT) T+3 to T+7 business days


Domestic UPI / cards T+2 business days


### What the FEMA realisation window means for you


- The June 2026 amendment restored a 9-month realisation period, effective 05 June 2026.
- The new FEMA Export and Import Regulations, 2026 create a 15-month standard, and 18 months for INR-invoiced exports, effective 1 October 2026.
- Practical reading: a shipment in July 2026 stays on the 9-month clock; only shipments dated on or after 1 October 2026 fall under the new standard. Razorpay’s[RBI circular on export realisation](https://razorpay.com/blog/rbi-circular-on-export-realisation/) breaks this down further.
- Failure to realise requires an application to your Authorised Dealer (AD) bank and may attract RBI scrutiny.


> **DID YOU KNOW:** India remained the world’s largest recipient of remittances, with inflows reaching USD 135.4 billion in FY25. Despite this, most business owners do not know their FEMA realisation deadline.


### The difference between FIRC and e-FIRS


- **FIRC (Foreign Inward Remittance Certificate):** A certificate confirming a foreign currency payment was received. Required for GST zero-rating and export promotion schemes. The[FIRC certificate guide](https://razorpay.com/blog/firc-certificate/) covers the format.
- **e-FIRS (Foreign Inward Remittance Statement):** The digital equivalent, issued by banks and authorised payment aggregators. Most modern gateways auto-generate e-FIRS at the transaction level.
- Your CA needs either document for every international receipt. Choose a gateway that generates this automatically.


> **PRO-TIP:** Before signing up, ask: “Do you auto-generate e-FIRS at the transaction level and make them downloadable from the dashboard?” If the answer is unclear, your compliance burden increases every time money comes in.


## What Are the RBI, FEMA, and GST Compliance Rules for Accepting International Payments in India?


Indian businesses must comply with four regulatory layers: (1) RBI Payment Aggregator guidelines; (2) FEMA rules on forex realisation timelines and documentation; (3) GST zero-rating via a Letter of Undertaking (LUT) filed annually; and (4) KYC and Import Export Code (IEC) requirements before a gateway activates international payments.


### Compliance Layer 1 – Use only an RBI-authorised Payment Aggregator


- Any entity processing payments for third-party merchants must hold an RBI Payment Aggregator (PA) license.
- Using an unlicensed aggregator exposes you to transaction failures, frozen settlements, and regulatory risk.


### Compliance Layer 2 – Documents required to activate international payments


- Business registration certificate (GST registration or incorporation documents)
- Import Export Code (IEC), mandatory for goods exporters
- PAN of the business entity
- KYC documents as required by the gateway
- A bank account in the business’s name for settlement


### Compliance Layer 3 – GST zero-rating for service exports


- If you export services, your export is a “zero-rated supply” under GST. No GST is charged to the foreign client, and you can claim input tax credit.
- File a **Letter of Undertaking (LUT)** with your GST jurisdiction officer before each financial year begins.
- Missing the LUT means you either pay 18% GST and claim a refund later or risk non-compliance.


### Compliance Layer 4 – FIRC / e-FIRS for every inward remittance


- Every international payment must be backed by a FIRC or e-FIRS, required for GST export exemption proof, DGFT schemes, income tax assessments, and audit.


> **DID YOU KNOW:** Export of services from India is generally zero-rated under GST, but zero-rating is not automatic. You must file a Letter of Undertaking (LUT) before the start of each financial year. Many founders miss this step and lose the benefit for the entire year.


## What Are the Proven Benefits of a Multi-Currency Payment Gateway for Indian Businesses?


A multi-currency payment gateway increases international conversion by showing buyers prices in their home currency, reduces hidden FX losses by replacing bank-embedded 3 to 5% markups with transparent 0.5 to 1.5% rates, automates FIRC generation, and makes reconciliation manageable.


### Benefit 1 – Higher international conversion rates


- Around 60% of international shoppers prefer to pay in their home currency; forcing them to calculate the INR equivalent drives abandonment.


### Benefit 2 – Transparent FX savings


- Modern gateways charge 0.5 to 1.5% versus 3 to 5% embedded by traditional bank wires.
- On Rs 50 lakh of annual international revenue, the saving is Rs 75,000 to Rs 1.75 lakh per year.


### Benefit 3 – Automated compliance documentation


- Every inward remittance auto-generates an e-FIRS, eliminating the manual FIRC chase and saving finance team hours at scale.


### Benefit 4 – Faster settlement compared to traditional SWIFT


- Gateway-routed card settlements typically arrive in T+2 to T+5 versus T+3 to T+7 for bank wires, improving working capital.


### Benefit 5 – Access to the growing global opportunity


- B2B cross-border payments alone are projected to grow from USD 31.6 trillion in 2024 to USD 50 trillion by 2032.


## Which Indian Businesses Need a Multi-Currency Payment Gateway? (Use Cases by Business Type)


Any Indian business that receives money from outside India or sells to customers who prefer to pay in a currency other than INR needs a multi-currency gateway. This includes SaaS companies, D2C exporters, IT agencies, freelancers, and educational institutions accepting overseas fees.


### SaaS companies and software exporters


- Bill global subscribers in USD, EUR, or GBP via recurring card mandates.
- GST zero-rating and automated FIRC are essential for every renewal.


### D2C exporters and e-commerce brands


- Show product prices in USD or AED to reduce checkout friction, especially during the Diwali rush when overseas Indian buyers spike.
- Retail cross-border flows are projected to grow from USD 44 trillion in 2023 to USD 65 trillion by 2030.


### IT agencies and consultants


- Invoice US, UK, and Australian clients in their local currency.
- Avoid flat SWIFT fees and 3 to 5% bank FX markups by collecting via gateway card payments.


### Freelancers (design, development, content)


- Accept card payments directly via Payment Links in the client’s currency. No personal SWIFT accounts needed.


### Educational institutions and EdTech platforms


- Accept overseas student fees and tuition in USD, GBP, AUD, with auto-generated FIRC for compliance.


### Global businesses entering India (Import Flow)


- Need UPI, RuPay, and Indian net banking integration, not just international card acceptance, and must comply with RBI’s data localisation and tokenisation mandates.


## How to Choose a Multi-Currency Payment Gateway in India – A 7-Point Checklist


Evaluate seven factors in order: RBI Payment Aggregator license status, supported currencies for your target markets, the true all-in fee, automatic e-FIRS generation, international card authentication (3DS 2.0), settlement timeline, and integration compatibility with your tech stack.


### Checkpoint 1 – RBI Payment Aggregator license


- Non-negotiable. Confirm the gateway holds a valid RBI PA license before any other evaluation.


### Checkpoint 2 – Currency coverage for your markets


- Map your top 5 customer countries to the gateway’s supported currency list.


### Checkpoint 3 – True all-in fee transparency


- Ask for MDR, FX markup, and cross-border assessment as separate line items, not a bundled “international rate.”


### Checkpoint 4 – Automatic e-FIRS/FIRC generation


- Confirm the gateway auto-generates e-FIRS at the transaction level and makes them downloadable in bulk.


### Checkpoint 5 – International card authentication (3DS 2.0)


- Without 3D Secure 2.0 support, a significant portion of international card transactions will be declined due to authentication failure, not fraud.


### Checkpoint 6 – Settlement timeline and working capital


- Compare T+2 versus T+5 versus T+7. For cash-flow-constrained businesses, faster settlement is valuable.


### Checkpoint 7 – Integration with your tech stack


- Plugins for Shopify, WooCommerce, Magento, and API support for custom builds. For SaaS: subscription billing with retry logic. For freelancers: Payment Links in foreign currencies.


> **PRO-TIP:** Negotiate FX markup rates alongside MDR when your international volume crosses Rs 10 lakh per month. Gateway FX markups for high-volume merchants can drop to 0.5% or below. Most merchants never ask.


## Razorpay and the Indian Exporter – Building a Complete International Payments Stack


India’s payment maturity is a present reality. UPI reached 24,161.69 crore transactions worth Rs 314.23 lakh crore in FY26. The businesses built on this foundation are now expanding globally.


Razorpay is India’s first full-stack financial solutions company, processing over $180 billion in TPV and powering 105 of India’s 119 unicorns.


What the complete Razorpay international stack looks like:


Product What it handles


International Payment Gateway Card-based export collection, 3% flat, 135 currencies, FIRC-compliant


Payment Links Foreign-currency collection for agencies and freelancers with no website


Subscriptions International card support with retry logic for SaaS billing


RazorpayX Payroll, vendor payments, and tax obligations


Agentic Payments via n8n Automated, AI-powered payment workflows


We educate first, then we build together. For deeper context, explore our[cross-border payments guide](https://razorpay.com/blog/cross-border-payments-guide) and the[foreign exchange market in India](https://razorpay.com/blog/foreign-exchange-market-india) explainer.


## Frequently Asked Questions


### What is the FEMA realisation period for service exports, and what happens if I miss it?


The window is date-dependent in 2026. Shipments dated before 1 October 2026 sit on a 9-month clock; those on or after move to a 15-month standard (18 months for INR-invoiced exports). Missing it requires an application to your Authorised Dealer bank and may attract RBI scrutiny.


### What is the difference between FIRC and e-FIRS?


FIRC confirms a foreign payment was received. e-FIRS is its digital equivalent, issued by banks and authorised payment aggregators. Modern gateways auto-generate e-FIRS at the transaction level, downloadable from the dashboard. Your CA accepts either for GST and audit.


### Can Indian businesses use UPI to accept payments from foreign customers?


UPI primarily powers the Import Flow, where a global business collects from Indian consumers. For the Export Flow, international cards remain the dominant method alongside supported alternatives.


### What documents do I need before international payments go live?


You typically need your business registration or GST certificate, PAN, an Import Export Code (IEC) for goods exporters, KYC documents, and a bank account in the business’s name. Requirements vary by gateway and business category.


### How does Dynamic Currency Conversion (DCC) work and should I offer it?


DCC lets a foreign buyer choose to pay in their home currency at checkout, at a rate that includes a DCC markup of 3 to 5%. It improves transparency but adds cost. Offer it as a choice, and always display the exchange rate applied.


### What is the true all-in cost per international transaction?


Add MDR (3 to 3.5%), the cross-border assessment (0.5 to 1%), and the FX markup (0.5 to 1.5%), then apply 18% GST on the FX markup component. On a USD 1,000 receipt, the combined cost typically lands around 4.75 to 5%.


### How is an international payment gateway different from a domestic one?


A domestic gateway processes INR transactions and may attempt international cards but does not handle multi-currency presentment, real-time conversion, FIRC generation, or FEMA reporting natively. An international payment gateway is purpose-built for cross-border flows.
