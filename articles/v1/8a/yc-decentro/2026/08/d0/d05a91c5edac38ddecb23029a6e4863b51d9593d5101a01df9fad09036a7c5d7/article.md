---
schema_version: "1.0.0"
document_id: "d05a91c5edac38ddecb23029a6e4863b51d9593d5101a01df9fad09036a7c5d7"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-e6182ea9811a"
canonical_url: "https://decentro.tech/blog/upi-mdr-recovery/"
published_at: "2026-08-13T09:13:57+00:00"
first_seen_at: "2026-08-13T09:46:00.329449+00:00"
fetched_at: "2026-08-13T09:46:02.050489+00:00"
content_hash: "sha256:ed83b03cc61c47844dd4edc73b58699689101f05238a50c919662260a6fbd7d0"
---

# UPI MDR Recovery: Can You Get the Fee Back on Refunds?

Table of Contents


**Key Takeaways**
UPI has carried 0% MDR for person-to-merchant payments since 2020, but that status is changing. Parliament passed the Taxation and Other Laws (Amendment) Bill, 2026 on August 10, 2026, enabling a threshold-based MDR on select large merchants.


Even under 0% MDR, payment gateways charge separate platform or processing fees, and these are never reversed on a refund, regardless of the reason.


A UPI refund and an MDR recovery are two different things. Refunds return money to the customer; recovery refers to a business trying to claw back the fee it paid, which isn’t possible on any successful transaction.


RBI mandates specific refund timelines (T+1, T+5) backed by automatic ₹100/day compensation when banks miss them.


GST at 18% applies to platform fees, not to MDR itself, since UPI’s MDR line is currently zero for most merchants.


Settlement mismatches between the refund date and the deduction date are the single biggest source of reconciliation confusion for finance teams running UPI at volume.


Every finance or payments team processing UPI at scale has asked the same question at some point: does the fee we paid on a transaction come back once we issue a refund? The short answer is no. UPI carries zero MDR by regulation, but the moment a business routes collections through a payment gateway, it’s paying platform and processing fees that behave differently from card MDR when a refund happens. This piece covers what UPI MDR recovery actually means for a business, how the refund timeline works, what GST does to the math, and how to build a reconciliation process that scales without adding manual headcount every quarter.


##


Why this matters right now


UPI isn’t a side rail anymore; it’s the backbone of digital payments in India. In July 2026, NPCI recorded[23.66 billion transactions](https://entrackr.com/news/upi-hits-highest-ever-monthly-volume-with-2366-bn-transactions-in-july-12217613) worth ₹29.88 trillion processed on UPI, its highest-ever monthly volume, or roughly 763 million transactions a day, moving close to ₹96,383 crore daily. Year on year, volume is up 22 percent and value is up 19 per cent, and most of that growth isn’t coming from festive spikes.


For any business processing meaningful UPI volume, be it a marketplace, a D2C brand, a lending platform, or an enterprise handling thousands of daily collections, that scale is both an opportunity and an operational load. More UPI volume means more refunds and more line items in settlement reports that don’t immediately reconcile. If finance and payments ops teams are still reconciling refunds manually, or growth teams assumed “0% MDR” meant zero cost anywhere in the stack, this is worth circulating internally.


##


What is UPI MDR actually?


MDR, or Merchant Discount Rate, is the fee a merchant pays every time a customer completes a payment. It compensates the acquiring bank, the network, and the payment gateway for processing, routing, and settling that transaction.


On UPI specifically, the government mandated 0% MDR for person-to-merchant transactions to keep the rail free and encourage adoption. Nobody is charging interchange on a UPI collection.


But here’s where finance teams often get tripped up: zero MDR does not mean zero cost. When a business routes UPI collections through a payment gateway or aggregator like Decentro, it’s still paying for the infrastructure that makes the collection happen, things like payment routing, fraud checks, reconciliation tooling, and settlement. These typically show up as platform or processing fees, separate from the MDR line, and they behave completely differently when a refund happens.


***Quick example:** a retail business processes a ₹1,500 order through UPI. There’s no MDR charged on it, because it’s UPI. But the payment gateway still charges a small platform fee for facilitating that collection. If the order is cancelled and the business refunds the full ₹1,500, the customer gets ₹1,500 back. The platform fee on the original transaction, however, isn’t reversed. That’s the “MDR recovery” question in a nutshell, and on UPI, it’s really a platform fee recovery question, multiplied across every refund a business processes at volume.*


###


MDR by payment method: How UPI compares


**Payment Method** **Typical MDR** **Who Pays** **Notes**


UPI (P2P) 0%, free by mandate N/A Not expected to change under the August 2026 amendment


UPI (P2M, standard merchants) 0% currently N/A Government has stated small merchants will stay free


UPI (P2M, large merchants) Under discussion, reportedly ~0.04% Merchant Applies only above a turnover or per-transaction threshold, not yet notified


Debit card Up to 0.9% Merchant RBI-capped, varies by transaction band


Credit card 1% to 3% Merchant Varies by card network and card type


Net banking 0.5% to 1.5% (approx.) Merchant Varies by bank and gateway


Wallets 0.5% to 2% (approx.) Merchant Varies by wallet provider and gateway agreement


The gap between UPI’s near-zero cost structure and card MDR is exactly why the proposed large-merchant UPI MDR, even at a reported 0.04%, is still described as “nominal” and “far lower than debit or credit card MDRs” by the Finance Ministry. It’s not remotely close to parity with cards, even if it goes ahead as proposed.


##


The August 2026 amendment: what’s actually changing


This is the single most important update for any business tracking UPI cost structures right now, so it deserves its own section rather than a footnote.


On August 10, 2026,[Parliament passed the Taxation and Other Laws](https://www.ndtv.com/india-news/rajya-sabha-clears-taxation-and-other-laws-amendment-bill-2026-government-rules-out-upi-charges-11891090/amp/1) (Amendment) Bill, 2026, after the Rajya Sabha returned it to the Lok Sabha for discussion. The bill amends Section 10A of the Payment and Settlement Systems Act, 2007, the specific legal provision that has, until now, barred banks and payment system providers from charging users any fee on notified electronic payment modes, including UPI.


Here’s what the amendment does and doesn’t do:


- **It removes the legal prohibition on charging MDR** , giving the government the power to notify which digital payment modes may attract a fee going forward.
- **It is explicitly an enabling provision** , not an MDR notification. Finance Minister Nirmala Sitharaman told Parliament directly that the amendment “does not impose any tax or transaction charge on UPI users.”
- **The actual rate and eligibility rules haven’t been finalised.** Once the amendment takes effect, the UPI and Services Steering Committee, headed by NPCI, will decide whether an MDR is introduced, and if so, at what rate and threshold.
- **Consumers and small merchants are explicitly protected.** The government has repeatedly confirmed that all peer-to-peer UPI transfers stay free, and the vast majority of merchant transactions will remain unaffected.
- **Only large merchants are in scope for any future charge.** Reports around the proposal point to a threshold based on annual turnover, with one figure circulating at businesses above roughly ₹4 crore in annual turnover, alongside discussion of a per-transaction threshold around ₹2,000. Neither figure is confirmed policy yet.
- **Budget support continues in parallel.** The 2026-27 Budget has earmarked ₹2,000 crore for incentives on RuPay debit cards and low-value UPI merchant transactions, which is the government’s way of keeping the subsidy alive for the segment it wants to keep free.


**A disclaimer worth stating plainly:** as of this writing, ***no MDR rate or eligibility rule has been notified for UPI*** . Every mechanic described in this article, including the refund flow, the fee recognition logic, and the reconciliation guidance, assumes the current 0% MDR structure for standard merchant collections. If and when a threshold-based MDR for large merchants is notified, the same principles will extend directly, since the core question this article answers isn’t really about the MDR rate at all. It’s about whether any fee, MDR or platform charge, comes back once a transaction is refunded. That answer doesn’t change based on what the rate is.


##


What does “MDR recovery charge” mean on UPI?


This phrase shows up often enough in merchant settlement statements and support tickets that it’s worth defining precisely, because it’s frequently misunderstood.


An “MDR recovery charge” isn’t a standard regulatory term, and on UPI specifically, it’s something of a misnomer. What it usually refers to in practice is one of two things:


1. **A gateway’s internal line item for the platform or processing fee** , sometimes labelled loosely as an “MDR recovery” charge in a settlement report even though it isn’t technically MDR at all, since UPI’s actual MDR is zero for most merchants today. This labelling inconsistency is common across providers and is one of the more frequent sources of confusion finance teams raise with their payment partners.
2. **A pass-through charge some gateways apply when a transaction routes through a card network layered on top of UPI** , such as RuPay credit card on UPI, where card-network MDR genuinely does apply and gets passed to the merchant. In these cases, the “recovery” language is more accurate, since the gateway is recovering a real network cost from the merchant.


For a business trying to reconcile a settlement report, the practical takeaway is to check what the underlying transaction type actually was. If it’s a standard UPI collection from a bank account, any “MDR recovery” line is very likely a platform or processing fee mislabelled, and it will not be waived on a refund regardless of what it’s called. If the transaction involved a credit line on UPI, there may be a genuine card-network MDR component, and it’s worth confirming with the payment partner exactly which cost is being passed through before assuming it’s negotiable.


Either way, the term describes a cost recovery mechanism for the payment provider, not a mechanism for the merchant to recover anything. That distinction is the one worth getting right internally before finance teams spend time chasing a refund path that doesn’t exist.


###


Is there anything to “recover” on a UPI refund?


This is the part every business finance and payments team should internalise early: **no payment aggregator in India reverses the platform fee or processing cost on a refunded UPI transaction, regardless of the reason.**


It doesn’t matter if the customer cancelled before dispatch, returned an item within policy, or the business issued the refund as goodwill. In every case, the service the gateway rendered, routing the payment, running fraud checks, confirming the debit, and settling funds, was already delivered the moment the transaction succeeded. A refund is a new, separate financial event. It doesn’t undo work already done on the first one.


One exception worth knowing: if a transaction fails outright and never completes, there’s usually no fee charged in the first place. That’s different from a successful transaction refunded later. Businesses often conflate the two internally, and clearing that up early saves finance teams from chasing a recovery that was never coming.


##


**The UPI MDR** recovery **flow**


Since the fee itself never comes back, it helps to walk through exactly where it sits at each stage of a transaction’s life. This is the flow finance and payments ops teams should map internally, both to close books accurately and to know precisely which step to point to when a stakeholder asks where the platform fee went.


1. **Transaction succeeds.** The customer completes payment via UPI. MDR on this leg is zero by mandate, but the payment gateway’s platform fee is triggered the moment the transaction is marked successful, not when it’s later settled.
2. **Fee is recognised.** The platform fee, along with 18% GST on it, is logged against that transaction ID at the point of success. This is the moment the fee becomes a sunk cost from the business’s perspective.
3. **Refund is initiated.** The business triggers a full or partial refund against the same transaction reference, either through the dashboard or via API.
4. **Refund is processed and settled to the customer.** The gateway routes the reversal through the UPI rail, and the customer’s bank credits the funds back, typically within 2 to 3 business days.
5. **Fee remains on the ledger.** The platform fee and its GST component stay recognised as an expense against that transaction. There is no reversal entry to offset it, because the underlying service was already delivered.
6. **Refund debit appears in settlement.** The refunded amount is deducted from the business’s payout, usually landing in a settlement cycle 5 to 7 days after the refund was initiated, separate from the cycle the original transaction was settled in.
7. **Reconciliation closes the loop.** Finance ties the original transaction, its platform fee, the refund debit, and the GST entries back together under one transaction ID, confirming that the fee was never reversed and accounting for it as a genuine cost of the refunded sale.


Mapping the flow this way makes one thing clear: there is no separate “recovery” step to chase, because the fee was never designed to be refundable in the first place. The actual work for a business isn’t pursuing recovery; it’s making sure every fee, refund, and GST entry is correctly tied to its originating transaction so the true cost of the sale is visible in the books, rather than showing up as an unexplained variance weeks later.


##


The UPI refund timeline, step by step


Refunds and fee recovery are separate conversations, but business teams usually ask about both at once, so here’s how the actual refund flow works on UPI from a customer’s perspective.


1. **Refund initiated.** A refund is triggered from the dashboard or via API, full or partial, against the original transaction reference.
2. **Gateway processes the request.** The aggregator forwards the refund instruction through the UPI rail back to the customer’s issuing bank.
3. **Funds move back to the customer.** UPI and wallet refunds typically complete within 2 to 3 business days.
4. **Failed transactions are treated differently.** If the original transaction failed rather than succeeded, NPCI mandates auto-reversal within roughly one hour.
5. **Settlement adjustment.** The refunded amount is deducted from the business’s next settlement cycle, not necessarily the one the original transaction fell in, which is where most reconciliation headaches start.


###


Refund timelines by payment method


**Payment Method** **Typical Refund Time** **Failed Transaction Auto-Reversal**


UPI 2 to 3 business days Approximately 1 hour


Debit card 5 to 7 business days 1 to 2 business days


Credit card 5 to 10 business days 1 to 2 business days


Net banking 3 to 7 business days 1 to 2 business days


Wallets 2 to 3 business days Nearly instant to 1 business day


UPI is consistently among the fastest refund rails in this comparison, which is part of why it’s become the default expectation for online customers. That speed is also exactly why settlement mismatches feel more jarring on UPI than on cards. A refund that reaches the customer within three days but only debits the business’s settlement a week later creates a gap that looks inconsistent even though it’s simply how the two clocks are structured.


###


Why refunds mess with your settlement reports


Refund deductions typically appear in settlement files between **T+5 and T+7** from the day they were initiated. If a customer paid on the 28th of a month and the refund is processed on the 3rd of the next month, that deduction lands in a different settlement cycle than the original credit, creating a mismatch that looks like a discrepancy but is actually just timing.


This is precisely why automated reconciliation matters more than it seems at first glance. Manual matching of settlement reports against order IDs is known to accurately capture roughly half of all transactions. Automated systems that tie every refund back to its original transaction ID push that accuracy well above 85 per cent. For a business processing high UPI volume, that gap is the difference between finance closing books in a day and spending a week chasing variances across business units.


##


What about chargebacks?


Refunds and chargebacks often get lumped together, but they’re not the same animal.


A refund is something the **business** initiates. A chargeback is something the **customer’s bank** initiates on their behalf, usually because of a fraud claim, non-delivery, or a billing dispute. Chargebacks take far longer to resolve, typically 30 to 120 days, and carry direct dispute fees on top of the refunded amount, generally in the ₹200 to ₹600 range per incident.


The practical impact for a business at scale: a chargeback usually costs three to four times the original transaction value once dispute fees, lost goods, and staff time spent gathering evidence are added up. A clean, fast refund policy is almost always cheaper than letting disputes escalate. If a refund process feels slow or opaque, customers are more likely to go straight to their bank, and at enterprise volumes this compounds quickly across a chargeback ratio that gateways and networks actively monitor.


A few practices that meaningfully cut chargeback risk:


- Keep billing descriptors recognisable, matching the brand name
- Publish a clear, visible refund policy at checkout, not buried in a terms page
- Respond to refund requests fast, before customers feel the need to escalate
- Retain transaction and delivery evidence for at least 120 days, the standard dispute window


##


RBI Compliance and Regulatory Framework 2026


Everything covered so far, the refund flow, the settlement lag, the chargeback exposure, sits inside a regulatory framework that RBI enforces fairly strictly on the customer-facing side. Enterprise compliance and legal teams should have this framework documented internally, both to set accurate customer expectations and to know when a bank, not the business, is the one on the hook for a delay.


###


Mandatory Refund Timelines


RBI has set specific timelines that govern how quickly a customer must get their money back once a refund or reversal is triggered. These timelines apply to the customer-facing leg of the transaction and sit separately from anything a business owes its payment aggregator.


- **T+1 for UPI peer-to-peer transfers.** A failed transfer must auto-reverse to the customer within one business day.
- **T+5 for merchant payment reversals.** Where a customer’s account is debited but the merchant never receives confirmation, the reversal must reach the customer within five business days of initiation.
- **Roughly one hour for failed UPI transactions** , per NPCI’s operational guidelines. This is distinct from the formal T+1 regulatory mandate and is best understood as the typical operational outcome rather than a hard legal ceiling.
- **5 to 10 business days for card refunds** , depending on the issuing bank’s own processing window, which still has to sit within RBI’s broader compliance limits.


For a business running UPI collections at enterprise scale, these timelines matter operationally even though they don’t touch MDR recovery. Customer support and grievance teams need to know exactly which timeline applies to which failure mode, so they can set correct expectations instead of guessing or over-promising.


###


Automatic Compensation Rules


RBI’s framework doesn’t stop at setting deadlines, it backs them with automatic penalties on the banking side.


- **₹100 per day in compensation** accrues automatically when a bank breaches the mandated refund or reversal timeline, whether that’s T+1 for peer-to-peer transfers or T+5 for merchant reversals.
- **No formal complaint is required** to trigger this. The penalty clock starts the moment the timeline is breached, regardless of whether the customer has raised a ticket.
- **RBI Ombudsman escalation** is available after 30 days if the delay remains unresolved, giving customers and businesses a formal channel beyond the bank’s own grievance desk.


None of this creates any entitlement to MDR or platform fee recovery for the business. It’s purely a consumer protection mechanism governing how fast money moves back to the customer, and how banks are held accountable when it doesn’t.


But for enterprise teams managing customer experience at volume, understanding this framework is what separates a proactive compliance posture from a reactive one: knowing the **T+1 and T+5** clocks in advance means support teams can flag genuine bank-side delays early, direct customers to the right escalation path, and avoid unnecessary chargebacks that stem purely from confusion about who’s responsible for a delay.


##


GST implications on UPI MDR and platform fees


Since UPI carries 0% MDR, there’s technically no MDR line to apply GST on for the collection itself. But most gateways still levy platform or processing fees for infrastructure and reconciliation, and GST at 18% applies to that fee. For GST-registered businesses, this is typically eligible for input tax credit, so it’s not a dead cost, but it needs to be tracked correctly, ideally at a transaction-line level rather than aggregated at month-end.


The practical formula for the true UPI transaction cost:


**Net settlement = Gross transaction value − platform/processing fee − GST on that fee − refunds − chargebacks**


Businesses often build pricing and margin models assuming UPI is “free” because MDR is zero. It isn’t. Once platform fees, GST, and the operational cost of reconciling refunds are factored in, UPI collections have a real, calculable cost per transaction, and getting this right matters more as volume scales.


##


Building a reconciliation process that actually holds up


Here’s what tends to work for finance and payments ops teams that have been burned by manual reconciliation once and don’t want to repeat it at scale:


- **Tag every refund to its original transaction ID** , not just the order ID, so cross-period deductions are traceable automatically across business units.
- **Review settlement reports daily** , not monthly. A discrepancy caught within a few days is a five-minute fix; caught at month-end, it’s a multi-day investigation.
- **Consolidate across payment methods and providers** if the business runs more than one gateway, since providers format settlement data differently and manual consolidation introduces errors.
- **Retain records for at least 120 days** to cover the standard dispute window, including delivery confirmation, order timestamps, and refund acknowledgements.


None of this requires scaling headcount for its own sake. It requires collection infrastructure that delivers clean, structured settlement and refund data from the start, so reconciliation becomes a report finance teams pull, not a spreadsheet they rebuild every cycle.


##


Where Decentro fits into this


This is exactly the gap Decentro’s UPI collections stack is built to close for enterprises and B2B platforms that need payment infrastructure to be reliable and legible at scale.


As a licensed Payment Aggregator and[Payment Gateway](https://decentro.tech/products/upi-payment-gateway) (PA-PG), Decentro lets businesses collect via UPI payment links, dynamic QR codes, and collect requests, all through a single set of APIs, without an extra layer of third-party aggregation sitting between the business and the settlement bank. That means cleaner cost structures and full visibility on every transaction from collection to settlement, which matters directly to finance and treasury teams responsible for closing books accurately across high transaction volumes.


On refunds specifically, Decentro’s collections API supports direct refund issuance against any[payment link](https://decentro.tech/resources/upi-apis) , QR, or collect request, tied to the original transaction reference. Every refund is traceable back to its source transaction from day one, instead of becoming a mystery line item several settlement cycles later. Combined with real-time transaction status checks and webhook-based callbacks, enterprise finance and ops teams get refund and settlement visibility without building custom reconciliation logic in-house, fewer manual matches, faster book closures, and bandwidth freed up to focus on core business priorities.


##


Conclusion


UPI’s 0% MDR is a genuine cost advantage, but it isn’t a free pass on transaction costs altogether. Platform fees, GST, and the operational cost of reconciling refunds are all real, and no aggregator in India reverses those fees once a transaction is refunded, regardless of the reason. The businesses that get ahead of this build these costs into their financial planning early, choose collection infrastructure that makes refunds traceable by default, and treat daily reconciliation as an operational discipline rather than a month-end scramble. As UPI volumes keep climbing past record after record, that discipline only becomes more valuable, especially for enterprises running payments at scale.


[Let’s Connect](https://decentro.tech/signup?)


---


# Frequently Answered Questions


**Does UPI have any MDR at all?**


No. UPI carries 0% MDR by government mandate for person-to-merchant transactions. However, payment gateways and aggregators still charge separate platform or processing fees for infrastructure, routing, and settlement, and these are not affected by the zero-MDR rule.


**If UPI has no MDR, why do I see deductions in my settlement report?**


Those deductions are typically platform fees charged by your payment gateway for facilitating the collection, plus GST on that fee. They’re separate from MDR and apply regardless of payment method.


**Can I recover platform fees on a refunded UPI transaction?**


No. Once a UPI transaction is successfully processed, the platform fee charged on it is not reversed even if you later refund the customer in full. This applies across every payment aggregator operating in India.


**How long does a UPI refund take to reach the customer?**


Typically 2 to 3 business days for a merchant-initiated refund. Failed transactions, where the payment never actually completed, are auto-reversed much faster, usually within about an hour, under NPCI’s guidelines.


**Why does a refund I issued this month affect next month’s settlement?**


Refund deductions usually appear in settlement reports 5 to 7 days after you initiate them, which often pushes them into the following settlement cycle. This is a timing issue, not an error, and it’s the most common source of reconciliation confusion for growing businesses.


**Is GST charged on UPI platform fees, and can I claim it back?**


Yes, GST at 18% applies to the platform fee charged on UPI collections. If your business is GST-registered, this is generally eligible for input tax credit, so it’s worth tracking separately in your books rather than writing off as a pure cost.


**What happens if a bank delays a refund beyond RBI’s mandated timeline?**


Banks must pay automatic compensation of ₹100 per day for every day the delay continues beyond the mandated T+1 or T+5 timeline. This kicks in without a formal complaint from the customer. If the delay remains unresolved after 30 days, the case can be escalated to the RBI Ombudsman.
