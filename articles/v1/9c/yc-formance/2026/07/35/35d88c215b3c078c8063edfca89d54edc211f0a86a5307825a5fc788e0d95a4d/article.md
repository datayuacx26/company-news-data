---
schema_version: "1.0.0"
document_id: "35d88c215b3c078c8063edfca89d54edc211f0a86a5307825a5fc788e0d95a4d"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/programmable-finance-technical-guide"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:9f65924545eff20f3cd476e80a5f7ab88288ec3c442d563761c3d83127bd92b9"
---

# A technical guide on programmable finance

The stakes show up in an ordinary marketplace flow: a buyer pays $10,000 on your marketplace. By the time that payment lands, your system has already split it: $8,000 to the seller's available balance, $1,200 in platform fees to revenue, $500 into a chargeback reserve held through the applicable dispute window, and $300 in tax withholding routed to a liability account. There was no spreadsheet export, so no engineer could trace where the $300 went.


That split, the balance update, the payout to the seller's bank, and the reconciliation against the payment service provider (PSP) statement all run on a single stack: a[programmable ledger](https://www.formance.com/modules/ledger) that records what should be true, orchestration that applies the rules, connectivity that sends payment instructions to connected providers, and reconciliation that checks the result against external reality.


## What is programmable finance?


Programmable finance is the infrastructure layer that fintechs, platforms, and financial institutions build on, where code and rules govern how money and assets move. The ledger that holds balances and the surrounding orchestration enforce those rules directly during transaction execution. A debit fires, a fee splits, a payout triggers and a reserve releases. Each one happens because a rule said so.


Most teams start somewhere else. Funds are tracked in spreadsheets or read from a PSP's transaction log, while a scheduled job sums completed orders and triggers payouts. The approach works at low volume but breaks down when high-volume interactions arise.


Supporting your financial product is a layer your team must build: internal ledgering, reconciliation, state management,[omnibus account](https://www.formance.com/glossary/omnibus-account) attribution, and multi-provider coordination.


## How a programmable finance stack works end-to-end in four steps


A programmable finance stack has four layers, and they always run in the same order. External events enter first, followed by business rules. Then, the ledger posts with strong guarantees, before finally the payment instructions go out, and reconciliation checks the result against reality. Each stage hands off to the next with a clean boundary. Adding a payment rail changes only the adapter.


### Step 1 – Ingest and normalize external financial events


Events arrive from PSPs, banks, custodians, and wallets in formats that have little in common. A card charge and a bank settlement file may use different schemas and identifiers; custodian transfers and cross-border payouts add their own provider vocabulary. The ingestion layer abstracts all providers into a single, uniform data model so that a single event can fan out to the correct internal postings.


A normalization schema does the flattening. Provider events get mapped to a small set of standard types:


- PAY-IN for funds arriving at an internal account
- PAYOUT for funds leaving for an external destination
- TRANSFER for movement between two internal accounts at the same provider
- OTHER for catch-all, so downstream workflows can treat similar events consistently across providers


Duplicate and invalid events get rejected here. Add idempotency keys to every payment operation, so that when a rail fails and retries, repeated attempts resolve to a single intended transaction rather than accumulating as separate charges.


### Step 2 – Apply business rules and routing logic


Rules for fee splits, reserves, revenue sharing, and chargebacks live in workflows or scripts that engineering and finance can inspect. Workflows are typically defined in YAML using stages such as send, wait_event, and delay, with a full run history for each instance.


Multi-party distribution (platform fees, creator splits, and tax withholding) executes atomically at the moment of the transaction. In a[domain-specific language for money](https://www.formance.com/blog/engineering/numscript) , the $10,000 marketplace split from the opening reads directly:


```text
send   [USD/2 10000]   (
source   =   @world
destination   = {
80  %   to   @sellers:acme:available
12  %   to   @platform:revenue:fees
5  %    to   @reserves:chargebacks:acme
3  %    to   @liabilities:tax:withholding
}
)
```


One atomic transaction with four destination accounts. The $8,000, $1,200, $500, and $300 commitments are either made together or not at all. Your engineers write it so your finance team can read it.


Routing chooses the rail. Dynamic routing decides which provider to use for each transaction based on the destination bank's capabilities, the amount, the time of day, the corridor, the cost, and historical success rates.


The routing layer stays separate from core product logic, so adding a rail is a small, contained change. When a rail fails, the system retries with the same idempotency key, then falls back to the next available option before surfacing a failure. Every attempt, with its status and metadata, gets recorded.


### Step 3 – Post to the ledger with strong guarantees


The ledger writes all sides of a transaction together or not at all. The Numscript split above uses @world, @sellers:acme:available, @platform:revenue:fees, @reserves:chargebacks:acme, and @liabilities:tax:withholding in a single atomic multi-leg posting.


Either every posting commits, or none do. For a batch payout, a failure in any one posting rolls back the entire transaction, while the ledger remains consistent.


Two properties make those balances auditable over time. First, each transaction produces a hash from its data combined with the previous transaction's hash, linking the chain so that tampering with any entry breaks the linkage. Second, a[bi-temporal ledger](https://www.formance.com/blog/financial-operations/card-timeline-2) tracks both when an event was recorded and when it was effective.


### Step 4 – Send payment instructions and reconcile with external reality


The connectivity layer sends payment instructions to providers and feeds responses back in. The stack itself does not hold or move funds. For providers without a pre-built[connector](https://www.formance.com/connectors) , a generic connector framework gives customers a contract to implement against, typically in two to four weeks: the framework handles the contract, and the customer implements the translation layer to the provider.


[Reconciliation](https://www.formance.com/modules/reconciliation) then compares ledger balances against provider statements and flags mismatches. The ledger tracks what *should* be true while the provider tracks *what* is true, so you can review the statements for drift. Continuous reconciliation, which means matching your internal ledger to processor and bank statements, turns mismatches into visible drift rather than surprise write-downs at month-end:


## Five architectural principles that secure programmable finance


Five invariants keep the stack safe because they are structural constraints, and the order of the four layers depends on each one holding.


### 1. Event-driven design


Events trigger state changes and workflows. A PSP webhook or payout “confirmed” event can drive the next step. The orchestration layer responds to events and coordinates multi-step operations, including creating ledger transactions in response to connectivity events. A centralized controller beats choreography here, because it maintains an authoritative record of every step.


### 2. Idempotency on every write


Without idempotency, a network timeout retry processes the same business event twice, turning a timeout into a double-charge. For stronger guarantees, ensure idempotency records are stored durably with the operation rather than relying solely on a cache check.


### 3. Atomic multi-step transactions


Each step is committed with a matching reversal, so any partial failure rolls back cleanly across inventory, processor, fulfillment, and ledger. The burn cannot complete without a guaranteed fiat-side recovery path.


### 4. Immutable, append-only records


Corrections use offsetting reversal postings. Once data is mutated, the audit trail is gone, and forensic reconstruction becomes much harder. Combined with granular role-based access control and strong authentication (OAuth, JSON Web Token (JWT), OpenID Connect (OIDC)), this produces an audit-ready base.


### 5. Embedded policy controls


Circuit breakers, limits, rate caps, and approvals are implemented as policies within the execution layer. Regulatory limits are encoded into transaction execution, while sanctions screening and spending restrictions can also run there rather than as separate post-processing steps. An autonomous payouts agent that assembles supplier transfers can be gated to require approval from a finance lead before any funds move.


A programmable ledger plus orchestration, connectivity, and reconciliation runs as a sidecar alongside existing cores. The ledger owns posting truth, the core owns product state, and reconciliation runs between them.


## Four programmable finance use cases and 1 failure


The same four-layer stack and five invariants underwrite marketplaces, card issuing, lending, revenue sharing, treasury automation, and audit programmable finance use cases. They would also have caught the $894 million wire Citibank sent to Revlon's lenders by mistake.


### Marketplace payouts


Per-seller balances live in a multi-segment namespace (sellers:acme:pending, sellers:acme:available, sellers:acme:payouts). Refunds and chargebacks become reversal postings against the same paths.


### Card issuing


A two-phase transfer: the first posting places a hold (@cardholders:42:available → @cardholders:42:holds:auth_id); the second captures it to the merchant or releases it back. The balance check and the deduction must be one operation, or two authorizations pass when one should.


### Lending disbursements


A payment posts to deposits:{depositID}:pending, the workflow waits for a payout.confirmed event, then releases funds to deposits:{depositID}:disbursed.


### Revenue sharing


Revenue sharing breaks when the fee split and the balance movement are separate operations. If the platform recognizes its fee first and the underlying transfer fails, the ledger show revenue that was never earned. If the transfer settles and the fee recognition fails, the platform is short with no clean audit trail explaining why.


### Treasury and audit


Automated workflows handle sweeps and intra-group settlements. When compliance logic runs at execution time, append-only logs answer questions such as: “ *What was this account's balance on March 14 at 3:42 PM?”*


### The Citibank case


On August 11, 2020, Citibank tried to wire interim interest on Revlon's loan and instead sent[nearly $900 million](https://www.reuters.com/article/business/citigroup-cannot-recoup-revlon-payouts-after-nearly-900-million-gaffe-us-jud-idUSKBN2AG1TG) , essentially the full principal, using its own funds. The[court opinion](https://www.nysd.uscourts.gov/sites/default/files/2021-02/20cv6539%20Citibank%20Opinion.pdf) records that an operator failed to set the FRONT and FUND fields to the internal wash account in Oracle Flexcube, and the transaction still passed manual review.


A programmable finance stack would have encoded the principal-stays-internal rule as a structural constraint, including a posting whose source is an internal Citi account (@citi:wash:revlon_principal), validated before execution, that is atomic and impossible to bypass.[Double-entry enforcement](https://www.formance.com/blog/engineering/defining-double-entry) , plus continuous reconciliation, would have surfaced the $894 million drift within minutes.


## Support your programmable finance with a unified stack


In a programmable finance stack, every split, reserve, and reversal is a structural posting that an auditor can verify, rather than a tangle of if-statements nobody wants to touch.


Formance provides that stack with a Ledger with strong guarantees, Flows that apply rules, Connectivity that normalizes every provider, and Reconciliation that catches drift the moment it appears. The core Ledger is MIT-licensed and open source, so your team can read the code, run it locally, and own the deployment. Connectors remain swappable, so adding a rail or switching a provider changes the adapter while the core logic stays stable.


Clone the MIT-licensed[Formance Ledger on GitHub](https://github.com/formancehq/ledger) , run it locally, and post your first multi-leg marketplace split in Numscript.
