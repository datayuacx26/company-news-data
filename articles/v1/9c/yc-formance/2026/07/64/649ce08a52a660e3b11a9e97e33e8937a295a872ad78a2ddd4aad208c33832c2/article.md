---
schema_version: "1.0.0"
document_id: "649ce08a52a660e3b11a9e97e33e8937a295a872ad78a2ddd4aad208c33832c2"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/general-ledger-software-teams"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:5c3811dd809643530870cfa26c9cd78dac19af4c59fb188e1405263371fc3d32"
---

# General Ledger Definition for Software Teams

A hypothetical[$90,000 settlement](https://www.formance.com/blog/financial%20operations/account-reconciliation-patterns-for-high-volume-fintech) shows up in your ledger but never hits the bank. Finance opens a ticket; engineering checks the webhook logs, finds the payout marked as complete, and hands it to the PSP, which confirms the funds have cleared on its end.


A day passes before anyone lays the two records side by side and finds the duplicate posting behind it. By then, the auditor has questions, and the finance's general ledger no longer ties to cash.


The root cause of this failure lies upstream of the Enterprise Resource Planning (ERP) system, in how your software recorded the transaction in the first place. A product schema change or retry can create duplicate or split postings. A balance column updated in place with no trail creates another source of drift.


If you build payment systems, your architecture feeds the general ledger (GL). Reliable general ledger posting and reconciliation depend on the components your systems populate and the software properties that make the boundary trustworthy.


This article covers the general ledger components your software teams must populate correctly, the four software properties that ensure reliable posting, an example of a ledger-ready transaction, and a decision framework for building or buying a ledger.


## What is a general ledger?


A general ledger is the company-wide accounting record of every transaction, organized by the core financial statement elements, including assets, liabilities, equity, revenue, and expenses.


Every financial statement a company produces, the balance sheet and the income statement, is derived from it.


The general ledger usually resides within an ERP finance module, alongside accounts payable and accounts receivable. Finance and auditors use it to reconcile and close the books for reporting.


In this architecture, your engineering systems generate the transactional data that flows into the general ledger. The general ledger is the layer for credible financial reporting and period close, while your product systems remain the operational layer for availability and transaction context.


At the boundary, your systems translate the granular money movements your code records into general ledger-compatible journal entries finance can trust.


## Four components of a general ledger software that teams should know


Software teams integrating with a general ledger need to correctly populate four concepts: the chart of accounts, GL accounts, journal entries, and the general ledger, or GL codes.


1. **Chart of accounts**


The chart of accounts is the full list of accounts a company posts to, each with a GL code. In practice, a chart of accounts groups assets, liabilities, equity, revenue, and expenses into separate account families. Your systems must emit the correct GL code for each posting, or finance will map it by hand.


1. **GL accounts**


GL accounts record transactions for one accounting element. Finance will distinguish between balance sheet accounts, assets, liabilities, and equity, and income statement accounts, revenue and expenses, because those account families behave differently at period close.


1. **Journal entries**


Journal entries are the chronological record of each transaction, where debits must equal credits. One common implementation issue is using journal entries to force routine revenue or receivables into an ERP when the ERP expects those movements to originate from invoices, payments, or other transaction types. Bypassing expected transaction workflows can make subledger-to-GL reconciliation harder for finance teams.


1. **GL codes**


GL codes are the identifiers your systems attach to every posting so finance knows which account it lands in.


Your application subledgers, billing, payments, and revenue often aggregate detailed activity before posting to the general ledger. They post summarized entries through well-defined integrations.


For example, payables activity may aggregate into a single number posted to the accounts payable control account. A control account is a summary GL account, such as total AR or total AP, that finance compares against detailed records in your product-owned systems. When the control account and the related subsidiary ledger do not match, something in the integration broke.


All of this rests on double-entry. Every transaction sums to zero across its postings: total debits equal total credits. The trial balance checks that ∑debits = ∑credits across all accounts, which catches a class of integration errors. Some errors still produce a balanced but incorrect trial balance, including a missing transaction, a duplicate, or a posting to the wrong account. The trial balance is necessary because it cannot catch every integration error.


## What software teams must get right for general ledger integration


Four software properties decide whether your general ledger posting is reliable: immutability, idempotency, atomic postings, and deterministic balances. The reliability of your GL posting depends on how your product ledger records transactions, long before the data reaches the ERP.


1. **Immutability**


Immutability means a posting, once written, cannot be changed or deleted. Corrections happen through reversal entries. Overwrite a balance, and when it turns out wrong months later, you cannot reconstruct the sequence of events that produced it.


1. **Idempotency**


Idempotency means the same operation, run twice, moves money at most once. Distributed payment systems commonly face retries after client timeouts and duplicate broker deliveries.


The standard fix is to use a stable transaction ID as an idempotency key. If a single operation to move money is called multiple times, the system should move money at most once. Without it, the duplicate posting that breaks your bank reconciliation is inevitable.


1. **Atomic postings**


Atomic postings mean all entries in a transaction commit as a single unit, or none do. A multi-party split that touches every affected account either lands completely or fails. A partial write is a broken trial balance.


1. **Deterministic balances**


Deterministic balances are running totals of postings. A balance is the running total of the movements that touched the account. The moment you keep a balance column, you can write to it directly, and then you have created a second source of truth that will drift from the first.


### How they work together


Treat your product ledger as a clean, programmable source of truth that feeds the general ledger. Do not embed ad-hoc accounting logic in application tables.


A balance column updated within an order service has no intrinsic mechanism to enforce accounting correctness. Double-entry helps ensure that money is fully accounted for by recording equal and offsetting entries for every transaction. When you model fund flows as movements between accounts, a single system can unify data across product services and continuously monitor that balances remain in balance.


## A general-ledger-ready transaction


A general-ledger-ready transaction is one that finance can map to GL accounts without guessing. In a marketplace order, a buyer pays $1,000.00, the platform keeps a 10% commission and collects 8% sales tax, and the seller gets the rest.


### 1. The accounts


**Real-world party** **Ledger account** **Holds**


Carol (buyer) @buyers:carol:wallet the buyer's funds


Platform commission @platform:revenue:commission platform fee revenue


Sales tax owed @platform:tax:payable tax collected, owed to the authority


Dave (seller) @sellers:dave:payable what the platform owes the seller


### 2. How the money moves


Carol pays $1,000.00. 10% ($100.00) to platform commission, 8% ($80.00) to the tax liability account, and the remaining $820.00 to the seller.


### 3. How it connects with Numscript


This transaction is modeled in[Numscript](https://docs.formance.com/modules/numscript) , Formance's DSL for money movement:


// MARKETPLACE_SETTLEMENT


// Event: a $1000.00 order splits to seller, platform commission, and sales tax


```text
send   [USD/2 100000]   (
source   =   @buyers:carol:wallet
destination   = {
10  %   to   @platform:revenue:commission
8  %   to   @platform:tax:payable
remaining   to   @sellers:dave:payable
}
)
```


set_tx_meta("event_type", "marketplace_settlement")


set_tx_meta("order_id", "ord778")


### 4. After it posts


@buyers:carol:wallet −$1,000.00 · @platform:revenue:commission +$100.00 · @platform:tax:payable +$80.00 · @sellers:dave:payable +$820.00


Finance derives matching general ledger entries directly from this shape. The commission account maps to a revenue GL code. The tax account maps to a liability control account. The seller's payable maps to another liability. Cash settlement maps to an asset account when the funds clear the bank.


The postings already balance to zero, the event_type and order_id metadata give finance the audit reference, and nothing has to be reverse-engineered from application tables. The bridge between your code and the general ledger is concrete: clean postings in, clean journal entries out.


## Build an internal ledger or use a platform?


Engineering has two viable paths for product-ledger integration: build an internal ledger or adopt a programmable core ledger platform. Build when the ledger is a genuine differentiator or the scale is extreme. Adopt a platform when you need product velocity without having to own every ledger invariant.


**Signal** **Build** **Platform**


The ledger is a core product differentiator ✓


You need 100k+ TPS or an extreme custom scale ✓


You need product velocity without owning ledger invariants ✓


You have <18 months to ship a correct implementation ✓


You want future product lines to inherit correctness by default ✓


Your product ledger remains a clean source of truth, and the summarized journal entries continue to tie to cash because the underlying postings were correct by construction.


## The boundary that keeps the General ledger clean


General-ledger failures usually start upstream when software mutates a balance or writes a duplicate posting. An account model in finance cannot map to create the same problem. Fix that layer, and reconciliation stops being a monthly investigation.


The four properties that impact general ledger execution (immutability, idempotency, atomic posting and deterministic balances) are foundational. Enforce them in the storage layer, and every product line you add later inherits the same correctness guarantees without having to re-litigate them in application code.
