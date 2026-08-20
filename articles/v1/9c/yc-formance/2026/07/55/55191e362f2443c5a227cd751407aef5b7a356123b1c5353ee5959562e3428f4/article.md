---
schema_version: "1.0.0"
document_id: "55191e362f2443c5a227cd751407aef5b7a356123b1c5353ee5959562e3428f4"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/reconciliation-in-finance"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-24T08:07:59.167261+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:972fbc166b90dc021d2d60cf270dcedbc66e27a36374f1287482714247c0e68b"
---

# Reconciliation in Finance: From Spreadsheets to Programmable Policies

Reconciliation in finance is the control that keeps internal records aligned with banks, PSPs, custodians, and other external systems. High-volume financial operations teams need current cash visibility, audit-ready records, regulatory compliance, and the ability to explain exactly where money is at any point in time.


Take, for instance, when a $90,000 settlement discrepancy shows up in your ledger on a Tuesday. The PSP says the payout cleared, but your books say it's still pending. So you open a ticket to engineering, who checks webhook logs, then passes it to the PSP, who confirms the payout cleared on their side. A day passes before anyone lays the two records side by side. By then, the exception queue has three more like it.


For most high-volume teams, reconciliation runs on borrowed time and copied spreadsheets. The numbers don't match, nobody can say why, and the matching logic lives in application code that only one engineer understands. Once you're processing across multiple rails, fragile scripts and spreadsheet rules make reconciliation harder to explain and audit.


## What does reconciliation mean in finance?


In finance, reconciliation means checking your internal records against external systems to ensure balances and transaction-level money positions remain accurate. Reconciliation is the[control surface](https://www.formance.com/blog/financial-operations/account-reconciliation-patterns-for-high-volume-fintech) that prevents drift between what your records say and what your banks and providers hold.


Reconciliation includes matching, a step within the broader reconciliation control that pairs individual records, such as an invoice and a payment. It also includes exception investigation, corrections, review, and final balance certification.


Engineering teams need to treat reconciliation as a ledger issue. Reconciliation requires matching external events against an internal record that tracks every state transition.


A core ledger that gets this right enforces structural invariants: double-entry enforcement, immutability, idempotency, atomic multi-leg transactions, and balances derived directly from postings. When[those invariants hold](https://www.formance.com/blog/financial-operations/ensuring-ledger-integrity) , every posting has a clear source and destination, and balances can be recomputed at any time from the log with exact results.


## Why financial reconciliation is so important for operations, financial, and compliance


Reconciliation matters because it protects operational accuracy, satisfies regulators, and keeps treasury current on cash. Without it, operations and compliance teams only have an assumption about where their money is.


### Catching unauthorized transactions and fraud early


Reconciliation is often the first control where unauthorized transactions and fraudulent activity surface. When transaction records drift from external statements, finance teams lose the ability to distinguish a harmless timing gap from a real break, which is exactly the ambiguity fraudsters rely on to stay hidden. Automated reconciliation helps teams detect unauthorized, manipulated, or incorrect transactions earlier, before the issue compounds into a material loss.


A ledger recorded a deposit, but the bank statement shows a $20,000 discrepancy. Was it a commission bug, a timing issue, or a fraudulent adjustment slipped in between systems? Repeated discrepancies are a common fraud signal, and without[double-entry enforcement](https://www.formance.com/blog/engineering/double-entry-accounting-for-engineers-building-financial-products) and an immutable log, operations cannot distinguish them from legitimate differences. Teams need to prove that their platform posted the transaction to the correct accounts without errors or manipulation, and an immutable audit trail makes that proof possible.


### Meeting tightening reconciliation mandates


Regulators increasingly cite reconciliation as a primary control, and firms operating across the UK, EU, and US now face reconciliation obligations in every jurisdiction in which they operate.


In the UK, the[FCA's CASS 15 regime](https://www.fca.org.uk/firms/emi-payment-institutions-safeguarding-requirements) sets a daily cadence for internal and external reconciliation for payment and e-money institutions, with records of the time, actions taken, and outcomes retained for 5 years.


In the EU, PSD2 Article 10 requires payment institutions to safeguard user funds through segregation or insurance, and MiCA's safeguarding provisions require crypto-asset service providers and stablecoin issuers to segregate client assets and reserves from their own. Reconciliation is the control that proves both.


In the US, state money transmitter regimes require licensees to reconcile customer liabilities against permissible investments regularly, and federal rules for payment stablecoin issuers now name reconciliation explicitly as an examinable control.


A regulated firm running in all three markets has to satisfy all three regulatory bodies on the same set of books. Reconciliation is examinable evidence that meets compliance.


### Real-time cash visibility for treasury and liquidity management


For teams operating instant-payment or intraday settlement workflows, end-of-day cash checks expose visibility issues that treasury cannot afford.


When instant payments and intraday reporting change how treasury manages liquidity, a ledger that updates cash position only at month-end leaves teams guessing between closes. Worse, it falls on engineers who log into three bank portals, wait on batch files, and paste balances into a spreadsheet to answer a single funding question.


When reconciliation posts in real time, financial teams see the same cash position as the ledger. Funding calls, sweep decisions, and liquidity moves run against what is actually there, not what was there yesterday.


## The four types of financial reconciliation


There are four types of reconciliation: bank, transaction, intercompany, and balance sheet. Each pairs a different set of records against a different external source, runs on its own cadence, and breaks in its own way.


Whether the reconciliation is bank, transaction, intercompany, or balance sheet, any of these four types of reconciliation can break down easily when processes remain manual.


## Why manual reconciliation breaks down at scale


Manual reconciliation fails on two axes at once: spreadsheets that fail at scale and monthly cadence delays that become expensive. Both problems compound with volume, and neither is solvable by adding more analysts.


### Why spreadsheets fail at scale


Spreadsheets fail because they have little structural defense against the errors people make in them. At high volume, small mismatch rates across large monthly volumes compound into hard failures to trace once settlement windows close.


Large, interrelated reconciliation spreadsheets become slow and difficult to govern as data sources and formulas multiply. Every new payment rail, PSP, or jurisdiction[adds another data source](https://www.formance.com/blog/financial-operations/account-reconciliation-patterns-for-high-volume-fintech) with its own identifier scheme and settlement rules. Provider format changes break the scripts that parse them. Version control becomes fragile as anyone with access can edit the file, two copies circulate, and a formula gets overwritten in one place but not another.


Matching can force one-to-many and many-to-one relationships into one-to-one row logic, causing them to drop out of the match. The unmatched items pile into an exception queue that grows faster than any team can work it, and old breaks age past the point where the original context is still recoverable.


Month-end close slows when finance teams move files between portals, spreadsheets, and internal systems. Finance teams can spend so much time on payment reconciliation that cash management and other strategic work get pushed aside.


### The cost of monthly instead of daily reconciliation


Monthly reconciliation delays the detection of discrepancies, leading to extensive costs and wasted resources.


At high transaction volume, small manual error rates accumulate into a large exception backlog before anyone sees the pattern. Issues surface weeks after the fact, after settlement windows have closed, and manual journal entries end up papering over root causes instead of fixing them, so the same break resurfaces next cycle.


When matching runs daily or continuously, exceptions surface while the operational context still exists. Treating reconciliation as a month-end cleanup rather than as continuous infrastructure management poses a serious operational risk, especially for audit and regulatory reviews.


## Programmable reconciliation and how it changes processes


Programmable reconciliation encodes the rules that govern how money moves and how transactions are matched directly into your ledger, moving those rules out of spreadsheets and scattered application code.


### From Rules in Spreadsheets to Policies in Code


Moving reconciliation rules from spreadsheets into policy-as-code puts matching logic under version control, review, and test. The same engineering discipline already applies to production systems. Most financial infrastructure spreads[balance logic](https://www.formance.com/blog/financial-operations/ledger-balance-for-product-and-engineering) across application code, a PSP dashboard, and a month-end process that pulls in engineers and spreadsheets. The failure occurs when one customer's balance is recorded in multiple systems, with no single ledger explaining how it got there.


A[programmable ledger](https://www.formance.com/blog/engineering/what-is-programmable-money) keeps ledger rules, balances, and regulatory-grade traceability in the same place and enforces them on every write. With an open-source, programmable core ledger, Formance can unify fiat and digital assets.


When the matching rules reside in the same system as the postings, reconciliation ceases to be a distinct cleanup step. It becomes reconciliation-as-query, a deterministic read of the ledger state rather than a batch job that runs after the fact.


Policy-as-code means your reconciliation rules can be version-controlled, tested and remain auditable. The ledger model is open for inspection, enabling governance teams to map system behavior directly to risk policies.


### What a Reconciliation Policy Looks Like in Practice


A reconciliation policy starts with a named PSP boundary and clearing account: @counterparties:psps:provider:clearing.


The clearing account records money crossing the PSP boundary.


Fee accounts such as @counterparties:psps:provider:fees:markup, @counterparties:psps:provider:fees:scheme, and @counterparties:psps:provider:fees:interchange step out of provider fees.


At the same time, @merchants:acme:payable records what the platform owes the merchant. Any unexpected residual is a reconciliation break with a queryable balance.


Here is a $10,000.00 gross settlement carrying $300.00 in fees, posted as a single atomic multi-party transaction in[Numscript](https://www.formance.com/blog/engineering/numscript) , Formance's purpose-built language for describing financial transactions. The transaction yields a $9,700.00 net credit:


```text
// PSP_SETTLEMENT
// Event: PSP settlement posts $10000.00 gross; $300.00 in fees and $9700.00 to the merchant
send   [USD/2 1000000]   (
source   =   @counterparties:psps:provider:clearing   allowing   unbounded   overdraft
destination   = {
max   [USD/2 12000]   to   @counterparties:psps:provider:fees:markup
max   [USD/2 5000]   to   @counterparties:psps:provider:fees:scheme
max   [USD/2 13000]   to   @counterparties:psps:provider:fees:interchange
remaining   to   @merchants:acme:payable
}
)
set_tx_meta  (  "event_type"  ,   "psp_settlement"  )
set_tx_meta  (  "settlement_id"  ,   "stl001"  )


```


The fee decomposition and the net credit to the merchant occur as a single unit. Numscript describes the intent of the transaction: move this amount from the source to the destinations under these conditions.


Your engineers write Numscript so your finance team can read it. The ledger executes Numscript atomically and immutably, so every posting succeeds or fails together without race conditions or partial updates. The reconciliation check becomes a balance query against the ledger, not a spreadsheet exercise after the fact.


## The three-component architecture fintech teams use to run continuous reconciliation


Continuous reconciliation converges on a consistent three-component architecture: source-agnostic ingestion that applies across all sources, drift detection that compares internal ledger balances against external ones, and temporal reconciliation that accounts for the gap between when an event happened and when it settled.


### 1. Source-agnostic ingestion across banks, PSPs, and custodians


The ingestion layer normalizes data from all providers into a single model before matching. Per-provider adapters hold up as each provider gets its own adapter, schema changes stay localized to the affected adapter, and raw provider data is stored in its original format before any transformation, so you keep an immutable record for reprocessing.


[Formance Connectivity](https://www.formance.com/modules/connectivity) handles this ingestion layer, while ingesting transactions and account balances across bank accounts, payment rails, wallets, exchanges, and custodians.


Each provider maps into a single uniform data model, with pre-built connectors covering PSPs, banks, payment rails, and custodians; for anything else, the generic connector lets your team build a custom integration. A new provider becomes an adapter configuration.


### 2. Drift detection that catches discrepancies in real time


Drift detection compares internal ledger balances with external provider balances for each asset and flags discrepancies as they occur.


Reconciliation should run policy-based matching at configurable intervals and support matching cardinalities, such as one-to-one, one-to-many (a single bank credit covering a batched payout), many-to-one (multiple internal records mapping to one settlement entry), and cut-off time mismatches that are carried forward rather than becoming hard exceptions.


For on-chain balances, the drift pattern appears in four ways:


1. Poll the chain for the omnibus wallet's current balance.
2. Poll the internal ledger for the total in realized accounts.
3. Calculate the delta.
4. Create a synthetic transaction, a ledger entry for the drift itself.


The discrepancy becomes a posting in the ledger. If teams skip drift detection, bank, provider, and internal ledger records diverge long before the exception is visible in a month-end spreadsheet.


### 3. Temporal reconciliation and cut-off accuracy


Temporal reconciliation handles the gap between when an event happened and when it settled. Card payments are not instantaneous because authorization, capture, clearing, and settlement each run on their own clock. Ledgers that rely on a single timestamp create reconciliation discrepancies at month-end. Settlement across payment rails often takes days, which moves cut-off handling into ledger design.


[Bi-temporality](https://www.formance.com/blog/financial-operations/card-timeline-2) tracks both when an event was recorded and when it was effective. Run a cut-off as of a transaction's effective date rather than against the system clock, and in-flight settlements stop appearing as missing:


GET /api/ledger/v2/main/accounts/merchants:acme:receivable


?pit=2025-03-31T23:59:59Z


A point-in-time query reconstructs the exact balance state as of that effective moment. An account generating recurring adjusting entries has a process problem worth investigating, usually a timing difference, which a cut-off change would eliminate.


## Operations, finance, and compliance need reconciliation


Operations, finance, and compliance depend on a reconciliation record that reconstructs the time, actions, and outcome of every run.


The three teams share one dependency: a reconciliation record that reconstructs the time, the actions, and the outcome of every run.
