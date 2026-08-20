---
schema_version: "1.0.0"
document_id: "21aa523812f462b967bc3fe74cbe5037d828f63b4d9ab2b44c8bd921d88453d0"
company_key: "yc-karbon-card"
company: "Karbon Card"
source_id: "yc-karbon-card-news-import-50a5f6bde3bc"
canonical_url: "https://www.karboncard.com/blog/swift-card-paypal-fees"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-22T01:15:46.648745+00:00"
fetched_at: "2026-07-28T22:20:47.930048+00:00"
content_hash: "sha256:44972e24412b79f809ea01f1d96b5ba3978dc8048a9db4b3ebeed27d4dc6f67c"
---

# SWIFT vs Card vs PayPal: Vendor Payment Fees Showdown

## Key takeaways


- For under ₹15,000 and urgent needs, use an international card or PayPal, for ₹15,000 to ₹75,000 it depends on vendor preference and fee split, for over ₹75,000 or exact-credit needs, choose SWIFT with OUR.
- FX markup and intermediary fees drive the real cost, not just visible bank charges, get a[true cost comparison](https://www.lightspark.com/knowledge/swift-vs-paypal) before sending.
- For audits and ironclad proof, send via SWIFT and keep the[MT103 receipt](https://www.karboncard.com/blog/what-is-a-swift-mt103-document) as settlement evidence.
- Cards and PayPal offer buyer protection and reversals, SWIFT offers finality, traceability, and compliance documentation.
- Always confirm which methods your vendor accepts before you pay, it prevents delays and surprise fees.


## Quick decision rules for SWIFT, card, and PayPal


- **Under ₹15,000, urgent:** Card or PayPal, weigh forex markup against convenience.
- **₹15,000 to ₹75,000:** Depends on vendor preference and SWIFT fee split (SHA, OUR, BEN).
- **Over ₹75,000, or exact amount required:** SWIFT with OUR.
- **Need buyer protection or easy reversals:** Card or PayPal.
- **Need traceability or audit proof:** SWIFT with an[MT103 receipt](https://www.karboncard.com/blog/what-is-a-swift-mt103-document) .
- **First step:** Confirm the vendor’s accepted methods, then choose the cheapest reliable path.


> *Getting this choice wrong can easily cost 3% to 5% of your invoice, plus delays. Choose the method based on ticket size, urgency, and documentation needs, not habit.*


## Complete fees comparison for international vendor payments


To understand what you will really pay, you must consider every fee component, not just what your bank or app shows on screen. For a deeper overview, see this[true cost of each method](https://www.lightspark.com/knowledge/swift-vs-paypal) explainer.


### SWIFT bank transfer fees


- **Sender bank outward remittance fee:** About ₹500 to ₹2,000 flat, plus 18% GST.
- **Intermediary or correspondent bank fees:** Unpredictable, often ₹500 to ₹5,000, depends on route and country.
- **Beneficiary bank fees:** Deducted by recipient’s bank, amount varies.
- **FX markup:** Typically 1.5% to 3.5% over mid-market, timing spreads add uncertainty.
- **Net effect:** Best for invoices above ₹75,000, use OUR to ensure exact credit to the vendor.


### International card payment fees


- **Bank forex markup:** Usually 1.5% to 3.5% over the network rate.
- **International transaction fee:** Often 1% plus 18% GST from the issuer.
- **Dynamic Currency Conversion trap:** Always pay in the vendor’s local currency, skip DCC at checkout.
- **Cash advance risk:** Some issuers treat certain payments as cash advances, interest can spike.
- **Net effect:** Great for small, recurring spends, roughly ₹5,000 to ₹25,000, quick and dispute friendly.


### PayPal fees and hidden costs


- **Sender side:** Often “no visible fee,” but a weaker exchange rate bakes in about 2% to 3% hidden cost.
- **Vendor side:** Recipient typically pays 3% to 5% cross-border fee, they may ask you to[gross up](https://www.skydo.com/blog/paypal-in-india-charges-review-alternatives) the invoice.
- **Double conversion risk:** INR to USD, then USD to vendor’s currency, markup at each hop.
- **Speed and reversibility:** Near instant in-network, but account holds and disputes are common, vendors dislike the uncertainty.


## Speed, reliability, and reversibility compared


- **SWIFT:** 1 to 3 business days, can stretch to 5 around holidays, final and hard to reverse, strong traceability via MT103 and bank references.
- **Card:** Instant authorization, settlement in 3 to 5 days, reversible via chargeback up to ~120 days, reasonable traceability via transaction ID and issuer support.
- **PayPal:** Near instant to PayPal balance, bank withdrawal takes ~2 to 3 days, disputes often favor the buyer, traceable via PayPal transaction ID.


*Pro tip:* For[time sensitive payments](https://wise.com/in/blog/international-payment-methods) , add a 1 to 2 business day buffer, especially near Indian or destination bank holidays.


## Compliance and documentation for Indian freelancers


- **Keep core records:** Vendor invoice, contract or PO, and complete bank coordinates such as IBAN, SWIFT, or BIC.
- **Purpose codes:** Select the correct RBI purpose, for example Professional Services, Software License, or Design.
- **15CA and 15CB forms:** Some payments require these, confirm with your CA or bank, here is a helpful primer on[receiving and sending cross-border payments in India](https://razorpay.com/blog/receive-international-payments-in-india/) .
- **Record FX rates and fees:** Save screenshots of mid-market rates at initiation, reconcile debits to your books, explain variances.
- **Match receipts:** Confirm the vendor’s net credit, differences often indicate intermediary deductions.
- **Timing buffer:** Initiate 3 to 5 days before the due date to avoid late fees and tense follow-ups.


## Understanding SWIFT fee instructions: SHA, OUR, BEN


Who pays intermediary and beneficiary bank fees is defined by the SWIFT charge setting, often called the[fee instruction](https://www.karboncard.com/blog/our-ben-sha-charges) . Your choice directly impacts what the vendor receives.


### SHA (Shared fees)


- **You pay:** Your bank’s outward fee.
- **Vendor pays:** Their bank and any intermediary deductions.
- **Best for:** When both sides agree to split costs.
- **Risk:** Intermediary banks may shave off unpredictable amounts, vendor receives less than expected.


### OUR (You pay all fees)


- **You pay:** Your bank fee, all intermediary, and beneficiary bank fees.
- **Vendor receives:** Full invoice amount, or very close to it.
- **Best for:** Exact-credit invoices, tight vendor margins, or goodwill building.
- **Cost:** About 1% to 3% higher than SHA overall, but precision prevents disputes.


### BEN (Beneficiary pays all)


- **You pay:** Minimal fees beyond your bank’s outward charge.
- **Vendor pays:** Everything else, usually disliked and often rejected.
- **Use case:** Avoid unless explicitly agreed in writing, can sour relationships.


*How to specify:* In your bank’s SWIFT form, pick “OUR” in the Charges field for most vendor payments, state “SWIFT transfer OUR, fees borne by payer” on the invoice, and confirm by email.


## Step by step framework: how to choose the best method


### Step one: Confirm methods accepted


Ask the vendor upfront whether they accept SWIFT, card, or PayPal. This single question can remove options and save you hours.


### Step two: Classify the payment


- **Amount:** ₹5,000, ₹50,000, or ₹200,000?
- **Urgency:** Due in 3 days or 2 weeks?
- **Nature:** Recurring SaaS tends to favor card, one off projects above ₹75,000 tend to favor SWIFT with OUR.


### Step three: Estimate total cost


- **SWIFT (SHA):** ₹500 to ₹1,500 plus 18% GST, 2% to 3.5% FX markup, intermediary fees unpredictable.
- **SWIFT (OUR):** ₹2,000 to ₹5,000 plus 18% GST, 2% to 3.5% FX markup, higher but ensures full credit.
- **Card:** 1.5% to 3.5% FX markup plus about 1% issuer fee, decline DCC.
- **PayPal:** Hidden 2% to 3% FX cost to you, plus 3% to 5% taken from the vendor, potential double conversion.


*Quick math:* Invoice × (1 + FX markup + other %) = expected INR debit.


### Step four: If SWIFT, agree fee split upfront


Email the vendor that you will send via OUR, confirm IBAN, BIC, bank name, and the exact amount they will receive.


### Step five: Protections vs finality


Pick card or PayPal if you need a safety net, pick SWIFT for finality and compliance trail.


### Step six: Get live quotes


Ask your bank or issuer for the day’s actual FX rate, and check PayPal’s rate if relevant. Small shifts add up on large invoices.


### Step seven: Execute and document


- Reference the invoice or PO in the payment purpose field.
- Share the[MT103 receipt](https://www.karboncard.com/blog/what-is-a-swift-mt103-document) or transaction proof immediately.
- Record the INR debit, applied FX, and fees in your books, confirm receipt within 24 hours.


## Real world examples: Indian freelancer vendor payments


### Example one: Pay $120 monthly SaaS


**Scenario:** Recurring design or dev tools, small ticket, vendor indifferent to method.
**Analysis:** SWIFT is wasteful on fees for this size, PayPal’s rate is weaker, card is simplest and predictable.
**Best method:** International card, total cost typically 1.5% to 2.5%.
*Pro tip:* Enable autopay in the tool, always pay in the tool’s billing currency, not INR.


### Example two: Pay $2,500 to a UK designer, exact credit needed


**Scenario:** Vendor needs full £1,850 to clear costs.
**Analysis:** SHA may shortpay due to intermediary fees, card can worry vendors because of reversals.
**Best method:** SWIFT with OUR, final, documented, and precise. Share the MT103 once sent.


### Example three: Pay €600 one off to an EU stock site


**Scenario:** One time license purchase, multiple methods likely accepted.
**Analysis:** Card is fast and cheaper than SWIFT for this amount, PayPal depends on acceptance and rate that day.
**Best method:** Card, roughly ₹600 to ₹800 in total cost, instant access.
*Alternative:* If card is unavailable, SWIFT with SHA can work if the vendor is okay with net credit variance.


## Common pitfalls and pro tips


- **Avoid DCC:** Always pay in the vendor’s currency, DCC commonly adds 2% to 4% extra cost.
- **Complete bank details:** Typos in IBAN or BIC cause returns and delays.
- **Agree fee splits in writing:** Add a line in your contract clarifying SHA, OUR, or BEN.
- **No Friends and Family for business:** PayPal F&F removes protection and violates terms, use Goods and Services.
- **Mind holidays:** Initiate earlier around Indian and destination holidays, or you risk missing deadlines.
- **Archive the mid-market rate:** Screenshot it at initiation to reconcile FX variances later.


> *Documentation discipline today is audit relief tomorrow. Save your quotes, receipts, and MT103s the moment you pay.*


## Tools to simplify international vendor payments from India


[Karbon Business](https://karboncard.com/) helps Indian freelancers and solo founders manage international payments with virtual USD, GBP, EUR, and CAD accounts, flat 1% fees, and zero FX markup at mid-market rates. It supports quick INR settlement in 24 to 48 hours, plus automatic e-FIRA generation for compliance.


Other options include Wise Business, Payoneer, PayPal, RazorpayX International, and WorldFirst. Compare based on your currencies, invoice sizes, and compliance needs.


## Frequently asked questions: SWIFT vs card vs PayPal


### Cheapest way to pay international vendors from India for small amounts?


For under ₹25,000, an international card usually costs 1.5% to 2.5% all in, and is the simplest. PayPal looks easy but the exchange rate markup and recipient fees make it pricier. For frequent small payouts, consider a platform like[Karbon Business](https://karboncard.com/) to consolidate tracking and keep FX predictable.


### Should I use SHA, OUR, or BEN on SWIFT for agency or designer invoices?


Use OUR for most vendor payments, your vendor receives the exact amount, which avoids disputes. SHA is okay if both sides agree to split costs, BEN is rarely acceptable. If you promise “exact credit,” choose OUR and share the MT103 after sending.


### How long do SWIFT transfers from Indian banks take, and how can I trace them?


Most clear in 1 to 3 business days, though 5 days is possible around holidays or with multiple intermediaries. Trace using the MT103 reference your bank gives you, forward it to your vendor so their bank can locate the funds quickly.


### Is PayPal safer than card for disputes and chargebacks?


Cards offer strong chargeback rights for up to ~120 days, which is buyer friendly. PayPal disputes can favor buyers too, but outcomes are less predictable, and vendors dislike holds. For final, audit ready payments, SWIFT is better, for dispute leverage, card or PayPal works.


### What is the best method for large invoices where the vendor needs exact credit?


SWIFT with OUR. Inform the vendor of the target currency amount, ask them to confirm IBAN and BIC, then send and share the MT103. Platforms like[Karbon Business](https://karboncard.com/) can help keep the FX rate transparent at mid-market, which reduces reconciliation headaches.


### How can I reduce hidden FX markup when paying overseas vendors?


Always pay in the vendor’s local currency to avoid DCC, request live FX quotes from your bank or card issuer, and compare with mid-market references. Consider solutions that use mid-market with transparent fees, for example[Karbon Business](https://karboncard.com/) which advertises zero FX markup plus a flat fee.


### Do I need 15CA or 15CB for paying international contractors or SaaS?


Requirements vary by nature of payment and thresholds. Your bank or CA will advise if 15CA or 15CB is needed for your transaction category. Keep invoices, contracts, and purpose codes ready, and plan a buffer of 1 to 2 extra days in case documentation is requested.


### How do I ensure my vendor is not shortpaid by intermediary bank fees?


Specify OUR on the SWIFT transfer so you bear all fees and the vendor receives full value. Confirm the exact net credit by email before sending, and ask the vendor to verify receipt. This practice builds trust, reduces back and forth, and avoids top up payments.


### For recurring monthly tools, should I use card, SWIFT, or PayPal?


Use card for recurring SaaS, it is convenient, predictable, and easy to dispute if needed. SWIFT fixed fees make little sense for small recurring charges, PayPal can be costlier due to FX and the vendor’s receiving fees. A payment platform that supports virtual cards and unified tracking, like[Karbon Business](https://karboncard.com/) , can simplify management.


### How do I document international payments for GST and audits?


Save the vendor invoice, contract or PO, purpose code, the bank advice or MT103, and screenshots of the FX rate at the time of payment. Reconcile the INR debit to your expense ledger. If you use a platform such as[Karbon Business](https://karboncard.com/) , download statements and e-FIRA where available for a clean paper trail.


### What if my vendor only accepts PayPal but the fee is too high?


Ask if they can accept card via an invoice link, or a SWIFT transfer with OUR. If PayPal is non-negotiable, request their net amount after PayPal fees, and agree whether you or they will bear the difference. Clarify everything in writing to prevent misunderstandings.


### Is there a single platform that can help me both receive client money and pay vendors internationally?


Yes, look for platforms that provide multi currency accounts, transparent FX, and fast INR settlement. For example,[Karbon Business](https://karboncard.com/) offers virtual USD, GBP, EUR, and CAD accounts, flat 1% fees, and mid-market FX with zero markup, which suits freelancers who both receive client funds and pay overseas vendors.
