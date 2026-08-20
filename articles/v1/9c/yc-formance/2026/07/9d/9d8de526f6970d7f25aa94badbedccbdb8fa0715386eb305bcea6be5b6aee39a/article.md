---
schema_version: "1.0.0"
document_id: "9d8de526f6970d7f25aa94badbedccbdb8fa0715386eb305bcea6be5b6aee39a"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/stablecoin-ledger-core-banking-system"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d2824d41bb525868fa442829810fdab6d469d7799859b7ae2d27d3f39fd616b6"
---

# How to ledger stablecoin flows on a regulated core banking system

When a $1M USDC mint hits your core banking system as a single fiat deposit, the chain records 1,000,000.000000 tokens and settles on a Sunday. By Monday, reserve and on-chain supply disagree by fractions of a cent per posting, and that fraction compounds within a reconciliation engine built for fiat-style decimal precision.


Legacy core banking systems assume things that stablecoins violate on every transaction, such as fixed precision, business-hours settlement, a single asset per ledger line, and reversible movements. The instinct is to rip out the core banking system and replace it, but that is expensive, slow, and unnecessary.


Run a programmable ledger alongside it instead. The core banking system stays the book of record for fiat, and the ledger handles every on-chain mint, transfer, burn, fee, and reserve movement with the precision, 24/7 timing, and multi-asset support the core banking system was never built for.


For regulated platforms handling customer funds, including stablecoin issuers and fintechs, this division turns stablecoin ledger flows from an audit liability into a reconciled, reportable record.


## Where legacy core banking systems fail on stablecoin flows


Legacy core banking systems fail on stablecoin flows in five predictable places: decimal precision, the on-chain/off-chain handoff, batch processing, gas tokens, and the omnibus account. Each one creates complex chains of messaging, clearing, and settlement, requiring repeated reconciliation.


### 1) Decimal precision mismatch produces systematic drift


Fiat-style decimal precision silently truncates stablecoin postings, and the rounding error compounds into material drift across the book.


Many legacy banking workflows were designed around fiat-style decimal precision, and stablecoin postings can carry more decimal places than those workflows were built to represent.


A reconciliation engine that can't represent full precision may truncate or round on every posting, allowing this drift to appear between the on-chain record and the internal books. The error is invisible per transaction and material in aggregate.


### 2) The on-chain/off-chain handoff has no native match


Stablecoin movements and their fiat counterparts settle in separate systems that don't automatically reconcile with each other. A single flow may involve multiple wallets, chains, network fees, and bridge calls, while a traditional accounting system is still trying to match a debit to a credit in one currency.


### 3) Batch processing collides with 24/7 finality


Overnight batches can't reconcile transactions that finalize instantly at a certain time on a certain day.


Legacy core banking systems rely on overnight processing, so when a stablecoin payment settles instantly, and the core, digital banking layer, and client accounts fall out of sync, balance visibility breaks. Most were not built for token-based settlement or on-chain reconciliation at all.


### 4) Gas tokens create orphan value


Network fees paid in ETH, SOL, or MATIC create orphan value. The ledger must record entries in assets that a fiat-oriented reconciliation model has no place for.


A traditional fee can often be represented as one line in the same currency, but in stablecoin operations, network fees introduce additional assets and ledger lines that a fiat-oriented model may not represent cleanly.


Any reconciliation model ported from fiat needs a way to represent gas-token balances before it works, and treasuries holding operational inventories in ETH, SOL, and MATIC need explicit representation for those assets in the ledger model.


### 5) The omnibus account concentrates the mismatch


The[omnibus account](https://www.formance.com/glossary/omnibus-account) concentrates the mismatch because a pool holding funds for many owners can't be sub-ledgered in real time. Any stablecoin inflow that doesn't immediately reconcile to a named sub-account sits unattributed in the pool.


In stablecoin operations, an unattributed inflow means you cannot prove which customer's funds sit in the pool when a regulator asks.


## A ledger is the bridge between the core banking system and Stablecoin


A programmable ledger addresses the failure modes caused by legacy core banking and stablecoin flows by running a real-time, multi-asset, append-only book next to the existing core.


The core banking system remains the regulated book of record, and the ledger captures the flows that your banking system can't natively see.


### How a programmable ledger runs alongside the core banking system


A programmable ledger runs alongside the core banking system as a modern, real-time ledger that handles new digital flows without ripping out the book of record. It applies the separate component pattern to banking, handling real-time payments and new digital flows while supporting a controlled transition rather than being a wholesale replacement.


Operational independence gives the programmable ledger its advantage in stablecoin flows. Running on independent infrastructure, the ledger handles 24/7 liquidity and real-time updates, unconstrained by the legacy core's batch processing or maintenance windows.[When a stablecoin settles](https://www.formance.com/blog/industry-analysis/the-different-types-of-stablecoins) on a specific date and time, the programmable ledger posts it immediately.


At Formance, we've built an open-source,[programmable ledger](https://www.formance.com/modules/ledger) that unifies fiat and digital assets with regulatory-grade traceability, deployed precisely as this kind of control. One posting model spans every rail your banking system can't natively see, while your existing core stays the regulated book of record.


### Dividing responsibilities between the core banking system and programmable ledger


The core banking system owns regulated operations, legal account processing, and its role as the book of record. The programmable ledger owns product-level state, including real-time postings, digital reconciliation, authorization holds, reversals, and cross-currency posting at speed.


**Responsibility** **Core Banking System** **Programmable Ledger**


Immutable transaction log Operational records may be updated Append-only by design


Cross-provider balance reconciliation Does not cover Owns


Regulatory-grade traceability across rails Does not cover Owns


Multi-asset support (fiat and digital assets) Typically fiat only Asset-agnostic


The programmable models money movement as balanced postings across an asset-agnostic account namespace.[Numscript](https://www.formance.com/blog/engineering/numscript) , Formance's purpose-built language for money, describes the intent of each movement in terms that both engineers and finance teams can read. A two-phase hold against a customer's omnibus position looks like this:


```text
// TWO_PHASE_HOLD
// Event: move part of a customer's balance from available to pending
send   [USD/2 400000]   (
source   =   @users:1234:available
destination   =   @users:1234:pending
)
set_tx_meta  (  "event_type"  ,   "two_phase_hold"  )
set_tx_meta  (  "hold_id"  ,   "hld001"  )


```


The same posting shape works whether the asset is USD/2, USDC/6, or a custom token. The asset column carries the type, so a single double-entry model records fiat and digital assets in one ledger without a separate system per asset class.


Hierarchical account paths such as @users:carlos:bitso:custody let you query @users:carlos: to see[all providers](https://www.formance.com/blog/financial-operations/the-stablecoin-sandwich) , or drill into a single provider leg for reconciliation.


### Double-entry, immutability, and idempotency keep everything in sync


Three invariants keep the two systems in sync, and the programmable ledger’s storage layer enforces all three:


### ### 1) Double-entry, enforced at storage


Every transaction sums to zero. Balances are materialized views over immutable postings; the postings are the primary source of truth. The invariant (∑debits = ∑credits) must hold at the database level.


### 2) Immutability through compensating postings


You never update or delete a posting. A correcting entry is appended. Append-only correction matters acutely for stablecoins, because base-layer finality has no native reversal, and any corrective action must be a subsequent transaction. Compensating-entry error handling is a fundamental departure from how most banking middleware handles disputes through rollbacks.


### 3) Idempotency at the boundary


Idempotency is the one correctness property that double-entry does not give you. A duplicate posting can still balance internally, which means the trial balance may tie to zero while cash is overstated. You enforce idempotency with a unique transaction reference so a retried webhook or redelivered message can't book the same transfer twice. On irreversible on-chain rails, a phantom double-posting means the money has already moved.


## Ledgering a stablecoin transaction step by step


A full stablecoin lifecycle ledgers in four stages: a fiat deposit triggers an on-chain mint, an on-chain transfer posts to the ledger, a redemption burns tokens and releases reserves, and the ledger pushes the reconciled posting back to the core.


Each stage maps to distinct postings across the on-chain record and the off-chain reserve ledger.


### Step 1: Fiat deposit triggers an on-chain mint


A fiat deposit triggers an on-chain mint when the issuer confirms funded reserves and calls the mint function, though reserve funding, issuance, and the mint event are not always a single transaction. Smart contracts control supply on-chain, while financial systems hold reserves off-chain, and operational controls connect the two.


The ledger records two coordinated facts:


- On-chain, the mint event increases circulating supply, typically in line with the corresponding fiat funding amount.
- Off-chain, the reserve ledger records the corresponding fiat deposit.


The issuer verifies that fiat funds have been received and settled before the mint function is called on-chain. In your programmable ledger, the mint posting and the reserve posting commit together or not at all.


### Step 2: On-chain transfer posts to the programmable ledger


Once a transaction confirms under network consensus, balances are reassigned directly on-chain, and the ledger posts the corresponding internal movement.


On some programmable ledgers, execution, clearing, and settlement collapse into a single atomic state transition, and on-chain finality is determined by technical confirmation rather than institutional agreement. The ledger treats the on-chain cryptographic receipt, with its timestamp, as the primary source of truth.


When funds are split across a banking rail and a blockchain simultaneously, the ledger attributes the inflow to a named sub-account immediately, so no value sits orphaned in the omnibus pool waiting for an overnight batch to claim it.


### Step 3: Redemption burns tokens and releases reserves


A typical redemption can be modeled in four ledgered steps:


1. The customer sends tokens to the issuer for redemption
2. The smart contract executes the burn, removing tokens and decreasing the total supply
3. The issuer transfers equivalent fiat from reserves to the customer's bank account
4. The issuer's reserve balance decreases proportionally, and the proportional decrease maintains the 1:1 ratio.


Weak ledger designs fail fastest at redemption. After money leaves your system, external providers introduce timing gaps between initiation and confirmation, and weak idempotency can turn retries into duplicate movements.


Because an on-chain burn is irreversible, the burn must not complete without a defined fiat-side recovery path. The irreversibility makes the case for an atomic orchestration layer: running burn-and-settle as a coordinated workflow with retry and fallback so the irreversible step is never stranded.


A redemption flow expresses that contract as named, ordered stages. The example uses[Formance Flows](https://www.formance.com/modules/flows) ' send and wait_event stages to hold the reserve, await on-chain burn confirmation, then release fiat to the customer:


REDEEM_AND_SETTLE


Hold reserve, await on-chain burn, then release fiat to the customer


name: redeem_and_settle


stages:


- send:


source:


account: "@reserves:usd:operational"


destination:


account: "@reserves:usd:pending:redemption_001"


amount:


amount: 4000


asset: USD/2


- wait_event:


event: chain.burn.confirmed


params:


burn_id: "redemption_001"


- send:


source:


account: "@reserves:usd:pending:redemption_001"


destination:


account: "@payouts:bank:user_1234"


amount:


amount: 4000


asset: USD/2


The workflow makes the contract auditable. The reserve holds posts to @reserves:usd:pending:redemption_001 before the burn, so an in-flight redemption is visible on the ledger at every stage. Idempotency on mutating operations means a retried call resolves to the same posting rather than a duplicate.


If the on-chain burn event never arrives, the held reserve stays in pending and operations can act on that recorded state rather than re-deriving it from logs. Because it does not hold or move funds itself, the fiat leg routes through a connected payment provider, and the workflow records the posting once the provider confirms settlement.


### Step 4: The programmable ledger pushes the reconciled posting to the core banking system


The ledger pushes the settled, reconciled posting back to the banking system through the core's existing integration pathways, so reporting and compliance workflows stay intact. The core banking system remains the book of record for[regulated fiat operations](https://www.formance.com/blog/industry-analysis/stablecoin-payments-architecture-for-regulated-fintechs) and receives a clean, attributed entry instead of a raw on-chain event it has no schema for. Stablecoin data flows back through the same pathway your core banking system already uses, so you don't re-architect downstream reporting.


## Regulatory requirements GENIUS and MiCA place on the ledger


GENIUS and MiCA both require the ledger to prove 1:1 reserve backing, segregate reserve assets, and produce recurring attestation-ready reports. Your data model has to support those obligations from day one. The two regimes diverge on cadence and specifics, but the underlying postings must satisfy both.


### GENIUS Act reserve tracking and monthly attestation (US)


The GENIUS Act requires Permitted Payment Stablecoin Issuers to maintain[1:1 identifiable reserves](https://www.congress.gov/119/plaws/publ27/PLAW-119publ27.pdf) and publish monthly reserve composition subject to CEO/CFO certification and independent examination.


Eligible reserves are an exhaustive list, including cash, insured deposits, Treasury bills with[93 days or less](https://www.congress.gov/bill/119th-congress/senate-bill/1582/text) remaining maturity, overnight repos, qualifying money market funds, and central bank reserves. Reserves must be segregated, not commingled, and not rehypothecated except in narrowly defined cases.


Reporting cadence drives the ledger design. Issuers must publish monthly reserve composition, and implementing materials must describe disclosures such as average tenor and custody location for each asset category. It requires that the previous month-end report be examined through a monthly examination by a registered public accounting firm, and requires the CEO and CFO to certify the report's accuracy monthly.


### MiCA EMT reporting and audit trail requirements (EU)


MiCA imposes quarterly reporting and continuous audit-trail obligations on EMT issuers, backed by strict reserve segregation and custody rules. Reserve assets must be kept separate from the issuer's own assets, with custody arrangements required for asset-referenced tokens.


EBA technical standards set deposit floors (at least 30% at credit institutions for non-significant issuers, 60% for significant ones) and liquidity requirements based on[short maturity buckets](https://www.eba.europa.eu/sites/default/files/2024-06/580db2f3-8370-4927-baa3-0f995722b417/Final%20report_draft%20RTS%20further%20specifying%20the%20liquidity%20requirements%20Article%2036%204.pdf) .


MiCA diverges from GENIUS on reporting cadence. There is a[quarterly reporting regime](https://link.springer.com/chapter/10.1007/978-3-031-74889-9_9) for asset-referenced tokens (ARTs) above[an issue value](https://www.eba.europa.eu/sites/default/files/2024-06/a7bb4083-f886-4a44-8b03-3810155248a7/Final%20Report%20on%20draft%20ITS%20on%20reporting%20on%20asset-referenced%20tokens%20under%20MiCAR.pdf) of EUR 100m, and this regime is extended to certain e-money tokens (EMTs) via other MiCAR provisions. Required reporting covers holders, issued value, reserve size, and daily transaction metrics used as a means of exchange within a single currency area. Templates require reserve composition by asset type, counterparty, issuer, and a maturity ladder.


For teams operating across both jurisdictions, a reserve ledger design has to satisfy differing reporting and attestation obligations across the two jurisdictions from the same underlying postings.[Ledger bi-temporality](https://www.formance.com/blog/financial-operations/card-timeline-2) , tracking both when an event was recorded and when it was effective, is what lets you produce a correct point-in-time snapshot for either cadence without rewriting history.


### How the programmable ledger generates proof of reserves (POR)


The programmable ledger generates proof of reserves by snapshotting both sides of its ledger, including outstanding tokens as liabilities, verified against on-chain totals, and reserve account balances as assets. Build PoR into the ledger's data model rather than reconstructing it after the fact.


A programmable ledger that hash-chains every transaction through cryptographic links produces the tamper-evident history that the monthly attestation depends on. Any retroactive edit is detectable on read.


For a stablecoin issuer, the mechanism that makes recurring reserve reports possible is an immutable record where outstanding supply and reserve composition can be snapshotted at any point in time and verified independently.


## Failure modes that persist without a programmable ledger


Without a unified, append-only, double-entry programmable ledger feeding the regulated core banking system, every failure mode in this article stays open.


Drift compounds across postings with the omnibus account, orphaning inflows that never get attributed. An irreversible burn strands its fiat counterpart when a retry fails. When a regulator or an auditor asks you to prove 1:1 backing on a specific date, you're reconstructing it from spreadsheets and block explorers instead of querying a point-in-time view.


The difference between a clean audit and a reconstruction exercise is whether double-entry, immutability, and idempotency are enforced at the storage layer or bolted on in application code that one engineer understands.


A programmable ledger gives you a single posting model for fiat and digital assets. Hash-chained immutability supports proof of reserves, and atomic orchestration keeps an irreversible on-chain step from stranding its fiat counterpart.


Clone the[Formance Ledger repo on GitHub](https://github.com/formancehq/ledger) and spin it up alongside your core banking system.
