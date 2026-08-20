---
schema_version: "1.0.0"
document_id: "9f8789dd43929131ed603c559b13c2f6316af769101f412aa518c7a70d3e3cd5"
company_key: "yc-ledgerup"
company: "LedgerUp"
source_id: "yc-ledgerup-news-import-9e5c157fbb84"
canonical_url: "https://www.ledgerup.ai/resources/usage-based-pricing-examples"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-23T15:42:38.254221+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:6d72ff9d80af157cb380d7de1bc653f25a00d857906f130d34c850a162efda2a"
---

# Usage-based pricing examples for SaaS finance teams

Usage-based pricing charges customers based on what they actually consume: API calls, messages, transactions, credits, storage, seats above a baseline, or another measurable unit. The pricing idea is simple. The finance workflow behind it is not.


For SaaS teams, the useful question is not only "What should we charge for?" It is "Can we turn that usage into a clean invoice, a defensible revenue record, and a collectible payment without a spreadsheet scramble every month?"


The examples below show common usage-based pricing models and the billing controls each one needs before it is safe to scale.


## Quick comparison: 8 usage-based pricing examples


Example Customer pays for Common billing logic Finance control to solve


Pay-as-you-go API or event pricing Every billable call, message, record, job, or workflow run Quantity times a fixed unit rate Exclude failed events, retries, tests, and duplicates


Transaction or percentage-of-volume pricing A successful transaction, payment, payout, or processed dollar volume Fixed fee, percentage fee, or both Handle refunds, reversals, disputes, currency, and payout reconciliation


Tiered usage bands Usage in volume bands First block at one rate, next block at another rate Apply tiers consistently across contracts, periods, and amendments


Committed usage plus overage A committed minimum plus usage above the allowance Recurring commit, included usage, then overage rate Map usage to the signed contract and true up accurately


Prepaid credits or drawdown Credits consumed by actions, tasks, tokens, or capacity Credit balance decreases as usage happens; top-ups add balance Maintain a customer-visible credit ledger, expiration rules, and refund policy


Seat plus usage hybrid Base seats plus extra usage Subscription fee plus usage line for variable work Keep identity, product usage, and billing accounts synchronized


Outcome-based pricing A resolved ticket, recovered dollar, completed task, or other result Charge only when the outcome definition is met Prove attribution and define disputes, reopenings, and partial outcomes


Storage, compute, or capacity pricing Data stored, compute credits, processing time, or capacity consumed Quantity by storage, credit, runtime, capacity, or processing unit Define measurement windows, late usage, and customer-visible detail


These examples can be mixed. Many B2B SaaS contracts use a hybrid model: a base subscription or committed minimum for predictability, plus usage lines when customers grow.


## What makes a usage metric good enough to bill


A usage metric is ready for pricing only when it can survive billing, customer questions, and month-end close. A good metric is:


- **Measurable:** the source system can count it consistently.
- **Tied to value:** customers understand why the unit maps to value received.
- **Contract-ready:** the unit, exclusions, rates, caps, and period can be written into a signed agreement.
- **Auditable:** finance can trace the charge from source event to invoice line.
- **Explainable:** the customer can see the period, quantity, rate, calculation, and adjustment history.


That is why simple units tend to work well. Twilio's[SMS pricing](https://www.twilio.com/en-us/sms/pricing/us) is based on message details such as destination, message type, and carrier. Stripe's[standard card pricing](https://stripe.com/pricing) is tied to successful transactions. Snowflake describes[consumption-based pricing](https://www.snowflake.com/en/data-cloud/pricing-options/) with credit consumption and monthly storage charges.


Each one uses a metric the customer can connect to activity. The hard part is making sure the same metric also works inside contracts, invoices, revenue schedules, and reconciliation.


## Book a LedgerUp Demo


Stop chasing invoices manually. LedgerUp’s AI agent Ari automates collections, reduces DSO, and recovers revenue on autopilot.


[Book a LedgerUp Demo](https://www.ledgerup.ai/book-a-demo)


## 1. Pay-as-you-go API or event pricing


In a pure pay-as-you-go model, the customer pays for each billable event. That event might be an API call, SMS, webhook, AI run, document processed, record enriched, or background job completed.


**LedgerUp Insight:** The workflow described above is one that LedgerUp automates end-to-end. Teams using LedgerUp typically cut manual effort by 80% and reduce errors across their billing pipeline.


Example invoice logic:


Field Example


Meter Successful production API calls


Billing period July 2026


Quantity 240,000 billable calls


Unit rate $0.002 per call


Amount $480


This model is clean when the event is easy to define. It gets messy when product data includes failed calls, retries, sandbox traffic, test accounts, or duplicate imports. Finance needs rules for what counts before the first invoice goes out.


The control questions are practical:


- Which system is the source of truth for usage?
- What counts as a successful billable event?
- How are retries and duplicates removed?
- How are late-arriving events handled after the billing cutoff?
- Can the customer see enough detail to verify the charge?


LedgerUp's[API usage billing guide](https://www.ledgerup.ai/resources/api-usage-billing) goes deeper on this workflow. The important pattern is that raw events need account, contract, billing-period, billable-status, idempotency, and source context before they are finance-ready.


## 2. Transaction or percentage-of-volume pricing


Transaction pricing charges when money, orders, payouts, or other business events move through the product. Payment processors are the familiar example. Stripe publishes[standard online card pricing](https://stripe.com/pricing) of 2.9% plus $0.30 per successful domestic card transaction.


For SaaS companies, a transaction model can also apply to invoices processed, payments reconciled, claims completed, orders routed, or marketplace GMV handled.


Example invoice logic:


Field Example


Meter Successful payments processed


Billing period July 2026


Volume $500,000


Rate 0.8% of processed volume


Amount $4,000


This model aligns vendor revenue with customer activity, but it creates finance edge cases. Refunds, chargebacks, failed transactions, reversals, multi-currency fees, minimums, and payout timing all affect the final amount. If the pricing event is close to cash movement, reconciliation matters as much as rating.


## 3. Tiered usage bands


Tiered usage pricing charges different rates as usage moves through volume bands. The first block may cost more, while later blocks are cheaper because the customer has scaled.


Example invoice logic:


Usage band Units Rate Amount


Included usage 0 to 100,000 Included $0


Tier 1 overage 100,001 to 250,000 $0.020 $3,000


Tier 2 overage 250,001 to 500,000 $0.015 $1,500


Tiered pricing is useful when customer value and vendor cost both change with volume. It also introduces rating complexity. Finance has to know whether tiers reset monthly, accumulate annually, apply per workspace, apply per parent account, or apply only after a committed allowance is used.


The contract should answer those questions explicitly. Otherwise, the same usage file can produce different invoice amounts depending on who interprets the deal.


## 4. Committed usage plus overage


Committed usage gives the customer a predictable baseline and gives the vendor a revenue floor. The customer commits to a monthly, quarterly, or annual amount that includes a usage allowance. Usage above the allowance is billed as overage.


Example invoice logic:


Field Example


Monthly platform commitment $12,000


Included usage 500,000 successful records


Actual usage 640,000 successful records


Billable overage 140,000 records


Overage rate $0.018 per record


Overage amount $2,520


Total invoice $14,520


This is one of the most common enterprise SaaS patterns because it balances predictability and expansion. It also depends heavily on contract-aware billing. Finance needs to know the commitment amount, included usage, measurement period, overage rate, discount terms, ramp schedule, renewal changes, and whether unused allowance rolls over.


If those terms live only in a PDF or someone's notes, usage billing becomes manual contract interpretation every month.


## 5. Prepaid credits or drawdown


Credit-based pricing lets customers buy or receive a pool of credits that are consumed by usage. AI products often use credits because raw costs can vary by task, model, token count, or workflow complexity.


Example invoice logic:


Field Example


Starting credit balance 50,000 credits


Credits consumed 37,250 credits


Ending balance 12,750 credits


Top-up rule Auto-purchase 25,000 credits when balance falls below 10,000


Credits can make usage easier to package, but they can make billing harder to explain if customers do not know what consumes credits. Finance and RevOps need a ledger that shows purchases, included credits, consumption, expiration, refunds, adjustments, and top-ups.


Credit models also need clear contract language. Do credits expire? Are they refundable? Can they roll over? Do different actions burn different numbers of credits? What happens when a customer disputes a task that consumed credits?


## 6. Seat plus usage hybrid


A seat plus usage model charges a predictable subscription for access, then adds usage fees for variable work. The seat fee covers the team using the product. The usage line captures the cost or value that changes with activity.


Example invoice logic:


Field Example


Base seats 25 seats at $80 per month


Base subscription $2,000


Included usage 10,000 documents processed


Actual usage 18,500 documents processed


Overage 8,500 documents at $0.12


Total invoice $3,020


This model is useful when seats alone do not reflect value. A 10-person team processing 500,000 documents may create more cost and value than a 50-person team that barely uses the product.


The finance risk is identity and account mapping. Which users count as seats: invited, active, deactivated, admin-only, temporary, or external users? Which workspace owns the usage? Does usage roll up to the parent account or bill separately by subsidiary? These rules need to be consistent across CRM, product, billing, and contracts.


## 7. Outcome-based pricing


Outcome-based pricing charges for a result instead of raw activity. A company might charge per resolved support ticket, recovered dollar, approved claim, completed booking, or verified lead.


Example invoice logic:


Field Example


Outcome AI-resolved support tickets


Eligible outcomes 1,850 tickets


Exclusions Reopened within 72 hours, escalated to human, spam, test tickets


Rate $1.25 per eligible outcome


Amount $2,312.50


Outcome pricing is attractive because it points directly at value. It is also the hardest model to govern. The contract has to define the outcome, attribution, exclusion window, partial success, reopenings, disputes, and audit evidence.


If the customer and vendor cannot agree on what counts as a successful outcome, the model will create billing disputes instead of trust.


## 8. Storage, compute, or capacity pricing


Storage, compute, and capacity pricing charges for the infrastructure or processing resources a customer consumes. The customer might pay for data stored, compute credits used, processing minutes, bandwidth, model runs, indexed records, or another capacity unit.


Example invoice logic:


Field Example


Meter Average storage used during the month


Billing period July 2026


Quantity 12 TB average storage


Unit rate $24 per TB


Amount $288


This model works when consumption maps naturally to infrastructure cost or customer value. It gets harder when the buyer cannot predict usage, when one workflow creates several billable dimensions, or when the invoice combines usage from multiple systems.


Finance needs rules for the measurement window, rounding, late-arriving usage, bundled capacity, free allowances, customer-level rollups, and customer-visible detail. Snowflake's[pricing page](https://www.snowflake.com/en/data-cloud/pricing-options/) is a familiar example of this pattern because it separates consumption-style credit usage from monthly storage charges.


## Control pattern: usage caps and approval-based overages


A usage cap is not a pricing model by itself. It is a control pattern that makes variable pricing safer. The customer gets an allowance, a warning threshold, a cap, or an approval step before a large overage is billed.


Example workflow:


1. Customer has 100,000 included records per month.
2. Usage alert fires at 80% of allowance.
3. Approval is required if projected overage exceeds $2,000.
4. Finance previews the overage before the invoice is sent.
5. Customer success is notified if usage changed because of a new launch or abnormal behavior.


This is useful for enterprise customers that want usage flexibility without surprise bills. It is also useful internally because it catches the overages that can turn into credits, write-offs, disputes, and tense renewal conversations. Treat caps and approvals as guardrails around the pricing model, not as the model itself.


## The finance workflow behind every usage-based example


Every usage-based pricing example eventually has to move through the same chain:


1. **Capture usage.** Product, data, billing, or third-party systems record the event.
2. **Normalize the record.** The event gets customer, account, contract, meter, period, billable-status, and source context.
3. **Apply contract terms.** Included usage, tiers, credits, caps, minimums, ramps, discounts, and overage rates change the amount owed.
4. **Preview exceptions.** Spikes, late events, manual adjustments, high-value accounts, and custom approvals are reviewed before invoicing.
5. **Invoice with detail.** The customer sees the unit, quantity, period, rate, and calculation.
6. **Recognize revenue.** Accounting handles usage timing, variable consideration, unbilled usage, corrections, and close cutoff rules.
7. **Collect and reconcile.** Payments, short-pays, disputes, credits, and cash application tie back to the customer and invoice.


This is where many usage programs break.[Zuora reported](https://www.zuora.com/guides/usage-based-pricing/) that 71% of SaaS finance leaders had breakdowns or major challenges when trying to support usage-based models, and its finance guidance points to the same problem areas: visibility, complex commercial models, disconnected systems, revenue recognition, unbilled usage, and reconciliation.


The lesson for finance teams is direct: do not launch a usage model with only a meter and a rate card. Launch it with the controls that turn usage into a customer-facing invoice and a clean close process.


For the revenue side, LedgerUp's[usage-based revenue recognition guide](https://www.ledgerup.ai/resources/usage-based-revenue-recognition) covers how usage timing, contract terms, and billing timing affect revenue processes.


## How to choose the right usage-based pricing model


Start with the unit of value, then work backward into billing operations.


- If customers choose discrete actions, pay-as-you-go event pricing can work.
- If the product moves money, transaction or percentage pricing may align naturally with value.
- If usage grows predictably with customer size, tiered bands or volume discounts can reward scale.
- If enterprise buyers need budget certainty, use committed usage plus overage.
- If raw usage is hard to explain, credits can create a cleaner customer-facing unit.
- If access matters but activity varies, use a seat plus usage hybrid.
- If value is defined by a result, outcome pricing can work only when attribution is clean.
- If customers consume data, compute, bandwidth, or capacity, use storage, compute, or capacity pricing with clear measurement windows.


Whatever model you choose, add caps, alerts, and approval-based overages when customers fear surprise bills.


Then pressure-test the model with finance questions:


- Can the contract define the unit without ambiguity?
- Can the source system produce the usage file every period?
- Can the billing system apply the signed terms without manual math?
- Can customers understand the invoice?
- Can accounting support the revenue treatment?
- Can collections answer questions without chasing product or engineering?
- Can finance reconcile usage, invoice, payment, and accounting records after the fact?


If the answer is no, the pricing model may still be valid. It just is not operationally ready.


## Where LedgerUp fits


LedgerUp is not the meter that counts every product event. It is the contract-aware finance workflow around the meter.


That distinction matters. Product systems may know that a customer used 640,000 records. Finance still needs to know which contract applies, how many records were included, whether the overage needs approval, what invoice line should be created, how revenue should be handled, what the customer can see, and whether cash later reconciles to the right account.


LedgerUp's[contract-to-cash workflow](https://www.ledgerup.ai/product/automate-contract-to-cash) is built for that post-signature handoff. Ari can read contract and customer context, turn billing events into actions, route exceptions for approval, and keep invoices, collections, and reconciliation connected. For teams already dealing with API usage, the[API usage billing guide](https://www.ledgerup.ai/resources/api-usage-billing) shows the raw-event-to-invoice version of the same problem.


Usage-based pricing can be a strong growth model. It only stays strong when the finance workflow is as clear as the pricing page.


## FAQ


### What are examples of usage-based pricing?


Common usage-based pricing examples include pay-as-you-go API calls, per-message pricing, transaction-based pricing, tiered usage bands, committed usage with overages, prepaid credits, seat plus usage hybrids, outcome-based pricing, and storage or capacity pricing. Controls such as caps, alerts, and approval-based overages can make those models safer, but they are guardrails rather than standalone pricing models.


### Is usage-based pricing the same as metered billing?


They are often used interchangeably, but they are not always the same in practice. Usage-based pricing is the broader pricing model where what the customer pays changes with consumption.[Metered billing](https://www.ledgerup.ai/resources/metered-billing) is the operating method for measuring a unit and turning it into a billable charge.


### Can SaaS companies bill customers based on usage?


Yes. SaaS companies can bill based on usage when they have a clear billable metric, a reliable usage source, contract terms that define the unit and rate, invoice detail customers can verify, and finance controls for approvals, revenue recognition, collections, and reconciliation.


### How is usage-based pricing different from subscription pricing?


Subscription pricing charges a recurring amount for access during a period. Usage-based pricing changes the amount based on consumption during the period. Many SaaS companies combine them with a base subscription or committed minimum plus usage-based overages.


## Book a LedgerUp Demo


See how LedgerUp connects your CRM, billing, and ERP systems to eliminate manual work and accelerate revenue.


[Get Started with LedgerUp](https://www.ledgerup.ai/book-a-demo)
