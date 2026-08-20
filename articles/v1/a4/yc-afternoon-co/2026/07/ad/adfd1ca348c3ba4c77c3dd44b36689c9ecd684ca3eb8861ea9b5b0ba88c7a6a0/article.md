---
schema_version: "1.0.0"
document_id: "adfd1ca348c3ba4c77c3dd44b36689c9ecd684ca3eb8861ea9b5b0ba88c7a6a0"
company_key: "yc-afternoon-co"
company: "Afternoon.co"
source_id: "yc-afternoon-co-news-import-d204747fe0e0"
canonical_url: "https://www.afternoon.co/blog/accounts-receivable-SaaS"
published_at: null
first_seen_at: "2026-07-21T20:47:37.461730+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:76ff744fd18331563635cc17b475e2e031142017183a22bd600d33a6ee42185e"
---

# AR for SaaS: How Recurring Revenue Changes Your Receivables Process In 2026

Accounts receivable (AR) sounds simple: you invoice a customer and wait to get paid.


In SaaS, recurring revenue changes the mechanics. Subscriptions renew, customers upgrade mid-cycle, usage fluctuates, credits get applied, and payments often arrive in bulk (one wire paying multiple invoices). If your AR process is built for one-time invoices, it will break as soon as you add net terms and recurring billing.


This guide explains what’s different about SaaS receivables and how to build an AR workflow that stays clean at month-end.


Last verified: Jan 2026


---


## What AR Means in SaaS


AR is the amount customers owe you for goods or services you have already billed. Operationally, AR is a system of records that answers:


- which invoices are open
- which invoices are past due
- which customers have credits
- which payments are unapplied or partially applied
- which balances are disputed


If you invoice on net terms, you have AR even if you are “just using Stripe.”


The IRS emphasizes keeping supporting documents and records for business transactions so you can prepare financial statements and tax returns. Invoices, payment records, and customer contracts are part of that record trail.[Recordkeeping overview](https://www.irs.gov/businesses/small-businesses-self-employed/recordkeeping) .


---


## Why Recurring Revenue Changes AR


### AR becomes a rolling balance, not a one-time collection


In many SaaS businesses, AR is not “a single invoice that gets paid.” It’s an ongoing relationship:


- monthly invoices stacking over time
- annual renewals
- add-ons and seat expansions
- credits for downtime or plan changes
- late payments that overlap with new invoices


That creates a new requirement: you need customer-level balances, not just invoice PDFs.


### Timing differences get more common


Recurring billing creates more timing differences between:


- invoices issued
- cash received
- service delivered


This is why SaaS teams often track both:


- AR (what is invoiced and unpaid)
- deferred revenue (cash collected for service not yet delivered)


Revenue recognition rules in U.S. GAAP are governed by ASC 606, which provides the framework for recognizing revenue when control of promised goods/services transfers to a customer.[FASB Topic 606 update (ASU 2016-10) PDF](https://storage.fasb.org/ASU%202016-10.pdf) .


You don’t need a complex revenue recognition engine to run AR, but you do need to keep AR (invoicing and collections) separate from deferred revenue (timing of revenue).


---


## SaaS Billing Models and Their AR Implications


### Monthly subscription, paid by card


This is the simplest SaaS AR setup.


- many payments settle automatically
- dunning is often automated (card retries)
- AR is low because invoices are quickly paid


Even here, you still need:


- refunds and chargebacks tracking
- clean reconciliation of processor payouts to invoices


### Annual prepay with invoices (common in B2B)


Annual prepay introduces:


- larger invoice amounts
- approval workflows (AP, PO numbers, vendor onboarding)
- net terms (Net 30/45/60)


AR risk increases because:


- invoices can be delayed by procurement steps
- payments arrive by ACH/wire outside your billing tool
- customers may request invoice changes after issuance


### Hybrid subscription + usage (most common at scale)


Usage adds variability:


- invoice amounts vary month to month
- customers dispute usage calculations
- credits and adjustments are more frequent


If your invoice does not include a service period and a clear usage period, you create avoidable dispute risk.


### Multiple channels and payment rails


As soon as you accept payments outside your primary billing tool:


- ACH or wire initiated outside the platform
- check payments
- marketplace or reseller collections


…you need a place to record and apply payments consistently to open invoices. Otherwise AR aging becomes unreliable.


---


## What a SaaS AR Process Should Include


A scalable SaaS AR process is not “send invoices.” It is a set of controls and routines.


### 1) Standard invoicing rules


Define:


- invoice timing (first day of period, in arrears, upon milestone)
- service period on every subscription line
- usage period on usage line items
- a consistent invoice numbering scheme


These rules reduce disputes and make payments easier to apply.


### 2) Payment application rules


SaaS payments often come as:


- one payment covering multiple invoices
- partial payments
- payments that include deductions for credits


Define a default policy, such as:


- apply payments to the oldest open invoice first unless remittance says otherwise


Track unapplied cash as its own item until it is applied.


### 3) Credits, refunds, and adjustments policy


Credits are normal in SaaS. The problem is when they are handled ad hoc.


Define:


- when you issue a credit memo vs reducing a future invoice
- how credits apply (oldest invoice, specific invoice, next invoice)
- how refunds are approved and recorded


This keeps customer balances stable and avoids “negative AR” confusion.


### 4) Dunning and collections cadence


Collections is a routine, not a scramble.


A simple cadence:


- reminder before due date (for invoice customers)
- follow-up on due date
- 7 days past due
- 14 days past due escalation
- 30 days past due escalation and potential service hold per contract


For B2B, dunning also includes non-payment blockers:


- missing PO number
- vendor onboarding incomplete
- invoice sent to wrong AP contact


---


## How Recurring Revenue Changes AR Metrics


### AR aging becomes a forecasting input


Once invoices recur, AR aging helps predict cash collection:


- current
- 1–30 days past due
- 31–60
- 61–90
- 90+


If you cannot generate an AR aging you trust, your cash forecast will be guesswork.


### DSO matters more


Days Sales Outstanding (DSO) becomes meaningful as you add net terms and enterprise customers. It’s not an investor KPI in the earliest stage, but it becomes a real operational metric once invoice-based revenue is material.


### Renewal season creates AR spikes


Annual renewal cycles can create concentrated AR exposure if:


- invoices go out at the same time
- large customers pay on slow payment runs
- procurement delays push payments past due


Planning for renewal AR is part of SaaS cash management.


---


## Month-End Close: What Changes in SaaS AR


SaaS AR affects close because you need cutoffs and reconciliations.


A practical close checklist for AR:


1. confirm invoices issued in the period are recorded to AR
2. match payments received to invoices and record unapplied cash
3. reconcile payment processor payouts through a consistent clearing approach (if applicable)
4. review credit memos and adjustments issued in the period
5. generate AR aging and flag past-due balances and disputes
6. confirm AR aligns with billing reports (invoice totals, credits, cash received)


The goal is that billing data and the ledger tell the same story.


---


## Common SaaS AR Failure Modes


### Treating AR as a “billing tool” problem


Billing tools create invoices. AR is the system that tracks balances, applies payments, and manages collections.


If invoicing and payment application are inconsistent, your AR will be wrong even with great software.


### Not separating credits from cash


Credits reduce what a customer owes, but they are not cash. If your system treats credits as cash, AR balances will drift.


### Letting usage disputes sit in open AR


Usage disputes should be tracked explicitly (disputed status, dispute amount, owner). Otherwise, aging becomes meaningless.


### No single source of truth for customer balances


If customer balances live in:


- Stripe
- spreadsheets
- email threads
- the accounting system


…you will always have “which number is right” problems.


---


## When It’s Time to Formalize AR


Formal AR becomes worth it when:


- invoice-paying customers are material (not just one-off)
- you offer net terms
- you have credits/adjustments often
- you accept payments outside a single processor
- you spend significant time reconciling payouts and invoices
- you are preparing for audit, diligence, or enterprise scale


If you are in between, a lightweight AR layer (standard invoices + payment application + aging + weekly collections routine) is usually enough.


---


## Where Afternoon Fits


SaaS AR works best when invoicing, customer balances, credits, payment application, and the general ledger stay connected. Afternoon offers an AR product designed for recurring revenue workflows, so invoice activity ties cleanly into customer balances and month-end close.


---


## Summary


Topic Details


Why SaaS AR is different Recurring billing creates renewals, plan changes, credits, usage variability, and bulk payments


What you must track open invoices, applied/unapplied cash, credits, disputes, aging, and collections status


What changes at scale net terms, procurement steps, multi-rail payments, usage disputes, renewal AR spikes


Close implications invoice cutoffs, payment application, clearing reconciliation, credit memo review, AR aging


When to formalize invoice-based revenue becomes material, reconciliation is painful, or diligence/audit is coming


---


## Official References


- [IRS recordkeeping overview](https://www.irs.gov/businesses/small-businesses-self-employed/recordkeeping)
- [FASB Topic 606 update (ASU 2016-10) PDF](https://storage.fasb.org/ASU%202016-10.pdf)
