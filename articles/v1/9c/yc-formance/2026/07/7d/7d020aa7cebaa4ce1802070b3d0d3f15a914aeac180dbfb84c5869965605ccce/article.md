---
schema_version: "1.0.0"
document_id: "7d020aa7cebaa4ce1802070b3d0d3f15a914aeac180dbfb84c5869965605ccce"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/embedded-finance/embedded-finance-architecture-primer"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:8d9e42f2e96bdc5f4a7c6ccbc7b7bc9d7214c07e2bb8572b108f5bc2e9c1ecf3"
---

# Embedded finance architecture: a technical primer

You shipped embedded payments first. A checkout flow with a PSP integration and a webhook handler that updates an orders table when a charge settles worked while the product stayed narrow.


Then the product wanted stored balances, so you added a wallet, reading the user's available funds from a column you increment on deposit and decrement on spend. Then came payouts to sellers, then a small lending feature. Each one leaned on a different provider API as its source of truth, and each provider defined balance state and settlement timing differently.


Now reconciliation runs nightly across three providers and two bank accounts, and two of the numbers don't agree. Nobody can tell you why, because the logic that decides what a user "has" is spread across four services and one provider's dashboard.


**Embedded finance gets hard when multiple products, providers, balances, and controls all have to be resolved through a single internal architecture, and provider APIs cannot be that architecture.** The[RushCard outage](https://files.consumerfinance.gov/f/documents/201702_cfpb_UniRush-Mastercard-consent-order.pdf) is the canonical lesson: in October 2015, UniRush migrated RushCard to a new back-end processor, erroneously double-posted over 10,000 ACH deposits, used later deposits to offset negative balances caused by its own processing errors, and left tens of thousands of cardholders unable to access their funds.


The CFPB ordered about $10 million in restitution to affected customers and a $3 million civil penalty, for a total of $13 million. The operational lesson for embedded finance is that when processor data goes wrong, the platform still needs an authoritative internal record of customer balances to fall back on.


## What is embedded finance?


Embedded finance is financial services built directly into non-financial products and workflows. These services appear as native product features instead of redirects to a separate financial institution. Payments, accounts, cards, lending, and insurance live inside the product experience your team already owns.


Architecturally, separate the embedded product experience from the infrastructure that powers it. Banking partners, payment rails, BaaS APIs, and ledger systems sit underneath the feature the user sees. The[European Banking Association](https://www.abe-eba.eu/wp-content/uploads/2025/08/navigating-the-path-to-embedded-finance-eba-2024-1.pdf) describes three roles: a regulated provider that supplies the license and balance sheet, an enabler that connects the licensed entity to the platform, and a distributor that holds the customer relationship.


You are usually the distributor. That means you own the customer's expectation that their balance is correct, even when the funds and the records sit at a partner you don't control.


## How does embedded finance work?


Every embedded finance transaction runs a similar loop: a user acts inside your product, your application logic decides what should happen, a request goes out to external providers, and your system records the result internally and checks it against the provider over time.


Four roles participate in the loop: the provider, the enabler, the distributor, and the end consumer. In product terms, the platform surfaces the feature and maintains the customer relationship; the end user transacts; the regulated partner provides the license and balance sheet; and the underlying providers process the money movement.


Sequencing determines whether your internal record can explain provider activity. Get the sequencing wrong, and your internal record becomes a guess about what a provider did.


### Step 1: Capture the financial action inside the product


The product captures the user action that triggers a payment, a balance change, a credit decision, or an account event. A tap on "Pay" or a withdrawal request might be enough; a loan acceptance might do the same. At this point, you have only intent. Record that intent durably and attach a stable identifier to it before anything leaves your system, because that identifier is what makes the next steps safe to retry.


### Step 2: Send the request to external providers and partners


The application sends requests via APIs to banks, PSPs, issuers, lenders, or other financial partners.


These calls can be asynchronous: payment transactions may acknowledge a request before the final downstream state is known and then deliver a later state via provider events or webhooks. Treating authorization and settlement as a single synchronous call can contribute to duplicate charges, duplicate refunds, or missed dispute states. Because provider events can arrive late or out of order, your system has to process them idempotently and by financial state rather than raw arrival order.


### Step 3: Record the financial event internally


The platform needs its own internal record of balances, liabilities, fees, and state changes. Provider APIs are[independent data silos](https://www.formance.com/blog/financial-operations/what-is-a-subledger) . Each operates with its own formats, timelines, and definitions of "pending," and nothing forces them to agree. When a dispute arises, the question is which record is right across your dashboard, the processor's log, and the bank's core. Without an internal source of truth, disputes turn into manual exception handling.


Record the transaction before the external call resolves, as a balanced double-entry posting. Corrections use reversal postings, not edits. Each entry is hash-chained to the previous one, so the log is tamper-evident and replayable end-to-end, giving you a record that survives even when a provider's books don't.


### Step 4: Verify that internal and external states match


The platform verifies that its internal records align with provider balances, settlements, and transaction states over time. This is reconciliation, and it works best continuously rather than once a day. A[stablecoin payout](https://www.formance.com/blog/industry-analysis/stablecoin-payments-architecture-for-regulated-fintechs) can clear on-chain in a few seconds, while reconciliation may still take days across a PSP, a custodian, an exchange, and a bank. Each holds its own ledger and identifiers. Reconciling continuously turns mismatches into visible drift instead of surprise write-downs.


## What are the architectural layers of embedded finance?


A durable embedded finance system is divided into six layers, each with a specific role. Collapsing them (especially folding the financial record into application code or provider responses) is what produces the reconciliation debt that eventually blocks scaling.


### 1. Customer experience layer


Users interact with financial features in the customer experience layer, such as checkout, wallet screens, and loan application forms. It shapes the visible workflow and the timing of what users see. It holds no financial truth. A balance rendered here is a reading of a state that lives elsewhere, and treating the UI's number as authoritative is the first step toward conflating balance.


### 2. Application logic layer


Application logic applies eligibility, pricing, routing, and workflow rules. It decides whether a user qualifies for credit, which rail to use, what fee applies, and what happens next. It coordinates product behavior, but it must not become the source of balanced truth. The moment a running balance lives as a mutable field that application code increments, you lose the ability to explain how the balance got there.


### 3. Financial system of record layer


A ledger or[financial subledger](https://www.formance.com/glossary/sub-ledger) tracks balances, liabilities, fees, and transaction states as the authoritative financial system of record. For most embedded finance use cases, its job is to track liabilities: what the platform owes each user, merchant, partner, and internal pool.


Storage-layer invariants are stronger than application-layer validation because application code is exposed to bugs and race conditions. The[zero-sum constraint](https://www.formance.com/blog/financial-operations/ledger-balance-for-product-and-engineering) is strongest when enforced as low in the stack as possible. Picture two requests hitting a wallet at once. Both read $100, both see $50 available, both subtract $50. The wallet lands at $50 when it should be $0. Production wallets have encountered this exact failure when users repay multiple debts simultaneously.


The same internal posting model should handle a user balance, a platform fee, and a provider clearing account in one atomic transaction. Account paths use a multi-segment namespace (@drivers:042, @platform:fees), so you can query and aggregate at any prefix (every driver, every fee bucket, every clearing account) without restructuring the schema.


In Numscript, a purpose-built language for describing financial transactions, a single $5.99 ride payment moves funds from a payment clearing account into a per-ride escrow and then splits the result:


```text
send   [USD/2 599]   (
source   =   @payments:001
destination   =   @rides:0234
)
send   [USD/2 599]   (
source   =   @rides:0234
destination   = {
85  %   to   @drivers:042
remaining   to   {
10  %   to   @charity
remaining   to   @platform:fees
}
}
)
```


The split produces $5.09 to the driver, $0.09 to charity, and $0.81 to platform fees (509 + 9 + 81 = 599 minor units, the full $5.99), recorded as separate postings that either all commit or none do. The clearing account, the user-facing balance, and the fee all live in the same[double-entry model](https://www.formance.com/blog/engineering/defining-double-entry) . There is no second place where a balance can quietly diverge.


### 4. Provider integration layer


Provider adapters connect banks, PSPs, issuers, KYC vendors, and other external systems to the platform. Their job is to isolate provider-specific APIs and formats from the rest of the architecture.


This is a translation boundary applied to money: each provider's events get translated into one shared internal data model before they reach the ledger, so an ACH batch file and a card settlement file become the same kind of posting. The product code never sees the raw rail format. Adding a rail changes an adapter, not your checkout flow.


### 5. Orchestration layer


Orchestration manages routing, retries, state transitions, and multi-step flows across providers. Real embedded finance flows run in sequence: funds may be held while KYC is completed before a payout is triggered. Each leg runs on an independent timeline and reaches its own state before the next advances.


This layer coordinates those decisions across products and external systems with native retry and fallback, so that an irreversible action (an on-chain burn, a confirmed payout) cannot be completed without a guaranteed recovery path.


### 6. Reconciliation and controls layer


Reconciliation and controls verify internal records against external balances, settlements, and reports. They are detective controls that surface errors after they occur and also form the foundation of your audit.


Keep initiation and verification independent where possible: the service that initiates a payment should not also be the only service that verifies it, or the same error can recur in both and produce silent exceptions. As the system grows, this layer is what supports monitoring, audits, and operational trust.


## What types of embedded finance products exist?


Six product categories appear in embedded finance, each adding its own complexity to the financial system of record. Payments are often first; later products tend to raise the demands on balances, state, and reconciliation.


### 1. Embedded payments


With[embedded payments](https://www.formance.com/blog/engineering/money-movement-architecture-for-multi-rail-fintechs) , users pay or get paid inside the product workflow. This covers acceptance, marketplace splits, payouts, and settlement across ACH, RTP, wire, and card networks. It is often the first embedded feature a platform launches because it maps cleanly to an existing checkout. Every new rail it adds inherits that rail's settlement model, error codes, and reconciliation format, each of which can break reconciliation logic written for the previous rail.


### 2. Embedded wallets and stored balances


Wallet features hold user balances, credits, or spendable value inside the product. They introduce persistent stored value, which typically requires tracking at least three balance views: ledger balance, available balance, and pending or held balance. Reading one while acting on another is balance conflation, a common source of production bugs in regulated entities running stored-value products. Wallets that support authorizations use two-phase transfers: a hold that drops available balance and raises pending, then a settlement that clears it. Every authorization needs idempotency, and the available balance should come directly from ledger reads.


Formance offers a Wallets module for quick-start patterns that match this shape, but most teams build wallet behavior directly on the Ledger Account object, which gives more flexibility for custom balance semantics.


### 3. Embedded card issuing


Card issuing gives platforms branded or virtual cards for users. It adds real-time authorization decisioning and a multi-phase settlement cycle on top of basic payments. The authorization entry and the later settlement entry face different timing, consistency, and throughput pressures, yet must reconcile against each other. Legacy designs that treat card issuing as a standalone product can become harder to operate when a platform combines it with lending or wallets and funds flow across product types.


### 4. Embedded lending


Lending inside the product flow offers credit without sending users to an external lender. It adds underwriting, servicing, and repayment state to the architecture. Eligibility and credit decisions may need to synchronize real-time identity verification with core systems, while servicing can introduce schedule management, accruals, statements, reason-coded entries, and controlled reversal and refund workflows. A modular model can support installment loans, lines of credit, and BNPL (Buy Now, Pay Later) within a single system.


### 5. Embedded accounts and treasury products


Account-like products let platforms offer experiences for holding, receiving, and moving funds. They introduce FBO structures and virtual account management. Without clear virtual-account mapping, reconciliation may fall back to manually matching batch settlement files days later. With them, an authorization checks a specific virtual bucket rather than a general pool, and the transaction is audit-ready by default. These products raise the bar on reliable balance logic and provider coordination, especially inside[omnibus accounts](https://www.formance.com/glossary/omnibus-account) (single omnibus or FBO accounts holding funds on behalf of many owners), where fund attribution must be provable on demand.


### 6. Embedded insurance


Insurance products offer protection at the point of need within another workflow, such as coverage offered at checkout. Their operating model differs from payments and balances: a single customer action can trigger underwriting engines, policy lifecycle systems, and third-party claims processors. They belong in the category because they are financial services surfaced natively inside a non-financial product, and they carry the same demand for accurate state and traceability across external systems.


## How should teams design embedded finance architecture from the start?


Design for an internal financial system of record from day one. Balances, holds, fees, payouts, reversals, and liabilities must be modeled consistently across all products and providers, because the alternative is each feature trusting a different provider's definition of the truth. The same architectural risk arises whenever account ownership, external funds, and internal records no longer align cleanly.


This is the approach Doctolib took when building their Financial OS. They chose[Formance Ledger](https://github.com/formancehq/ledger) specifically because the open-source model let them retain control and ownership over their financial infrastructure, which they consider non-negotiable at their scale. Formance Ledger is an MIT-licensed, programmable core ledger that unifies fiat and digital assets with regulatory-grade traceability. It deploys as a sidecar ledger alongside existing core banking systems rather than replacing them. The ledger holds the internal system of record, and Connectivity, Flows, and Reconciliation sit around it. The provider logic remains at the boundary rather than being embedded in the product code.


When a rail's behavior is buried in your checkout flow, every provider change becomes a product change. When it lives in an adapter behind a normalization layer, adding a provider changes only the adapter.


## How can infrastructure support the architecture of embedded finance?


Hold the system of record in a[core ledger](https://www.formance.com/blog/financial-operations/what-is-a-ledger) and keep provider integrations, orchestration, and reconciliation as separate architectural concerns. These are different problems with different failure modes, and collapsing them is how external APIs quietly become your source of financial truth.


[Formance Ledger](https://www.formance.com/modules/ledger) is the double-entry system of record and enforces balance constraints and immutability at the storage layer, with a hash-chained log that makes every posting tamper-evident.[Connectivity](https://www.formance.com/modules/connectivity) normalizes PSPs, cross-border providers, banks (direct API and open banking), digital asset custodians, digital asset exchanges, and direct on-chain rails into one data model, with a generic connector for anything not natively supported.


[Flows](https://www.formance.com/modules/flows) orchestrates multi-step sequences with native retries and fallbacks, and coordinates atomic operations across internal ledgers, banking rails, and digital asset connectors during multi-hop transfers, while[reconciliation](https://www.formance.com/modules/reconciliation) verifies internal balances against external pools with drift detection and bi-temporal cut-offs, separating record-time (when the entry hit the ledger) from effective-time (when the event was true in the world). Because the logic lives in[Numscript](https://www.formance.com/blog/engineering/numscript) , both engineering and finance can read the same definition of how money moves.


Formance does not hold or move funds itself. All money movement runs through connected payment providers (PSPs, banks, exchanges, or custodians), and Formance triggers programmatic payouts and transfers through these connectors while keeping the authoritative record on your side.


You can add products and providers without turning a provider API into the place where balances are decided.


The RushCard outage points to a persistent architectural risk: treating an external party's records as the effective source of truth, with no authoritative internal ledger to fall back on when those records go wrong. A programmable core ledger at the center gives you one record that survives provider migrations, partner failures, and audits. That is what you get to build on instead of maintaining a reconciliation spreadsheet with one tab per provider.


Clone the[Formance Ledger on GitHub](https://github.com/formancehq/ledger) (MIT license), run it locally, and post your first multi-destination transaction modeling user balances and platform fees against provider clearing in a single atomic posting.
