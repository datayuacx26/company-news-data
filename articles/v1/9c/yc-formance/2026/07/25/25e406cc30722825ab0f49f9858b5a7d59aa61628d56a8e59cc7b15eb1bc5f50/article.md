---
schema_version: "1.0.0"
document_id: "25e406cc30722825ab0f49f9858b5a7d59aa61628d56a8e59cc7b15eb1bc5f50"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/product/on-chain-off-chain-reconciliation-at-scale"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-24T08:07:59.167261+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:7de6049ad06493f99ee687f30812458469d94dfd4501ba4690eb2649a6cb210c"
---

# On-chain/off-chain reconciliation across chains

Three scenarios break on-chain/off-chain reconciliation in production, and they rarely happen on their own. A deposit confirms three blocks late, while an omnibus sweep posts as two unexplained on-chain credits. A cross-chain transfer moves USDC to Optimism, while the payout obligation falls on Base. Each one is a data mismatch, but bundled together, they are an architectural problem.


At low volume, reconciliation is a comparison of wallet balances against ledger totals. At higher volume, confirmation timing makes balances stale before the comparison runs, internal transfer double-counting inflates external flow, and cross-chain position drift leaves obligations underfunded.


The engineering and financial teams that hit this on-chain/off-chain reconciliation wall are trying to reconcile wallet, custodian, exchange, and ledger records with different identifiers, different timing models, and no shared definition of the source of truth.


## Where on-chain/off-chain reconciliation breaks under higher volume


Four failures show up at volume: confirmations that land after the ledger write, internal transfers that read as external credits, new chains that break position queries, and negative drift that signals a payout you are about to miss.


### 1. Chain confirmations arrive after the internal record was written


Confirmation timing breaks reconciliation when an internal ledger write lands before the chain confirms the deposit. Deposits become operationally safe through multiple stages, such as detection, block inclusion, confirmation, and settlement, which can all happen at different moments. If an internal ledger write is allowed before finality, it exposes withdrawals.


[Gate.io's 2023 policy update](https://www.gate.com/announcements/article/37593/gate.io-adjusted-transaction-confirmation-for-layer-2-deposits) is a documented example of what happens when confirmation timing is handled incorrectly. The exchange tightened its L2 deposit crediting rules after finding that crediting at L2 sequencer confirmation, rather than at L1 finality, created exposure on zkSync Era, Base, and Arbitrum.


Under the old policy, a deposit confirmed by the sequencer but not yet finalized on L1 could, in principle, be reorganized away; some L2 transactions now take up to 24 hours to be credited under the new finality-anchored policy. The architecture fix is to create a corresponding internal posting at each stage (sequencer confirmation, L1 batch posting, L1 finality), so no withdrawal path opens before the state is irreversible.


Create a corresponding internal posting for deposit detection, confirmation, and settlement-broadcast progress, so that each state change has its own record.


### 2. Internal wallet transfers appear as unexplained credits


Internal wallet transfers are misclassified as external flows when the reconciliation system has no way to mark wallet-to-wallet movements as internal.


When you sweep funds from hot to cold, or move balances between[omnibus wallets](https://www.formance.com/glossary/omnibus-account) , each leg lands on-chain as a raw transaction. A reconciliation system that lacks an internal-transfer classification layer mistakes wallet-to-wallet movement for external flow.


Collapse wallet-to-wallet moves under common control, or the ledger turns internal movement into misleading external activity. The cross-chain variant has a similar shape, with a bridge movement that appears as an outflow on one chain and an inflow on another. Unless the outflow and inflow records are linked as a single business movement, the ledger double-counts the movement.


### 3. New chain or custodian breaks cross-chain position queries


Adding a chain or custodian breaks any position query that lives outside the reconciliation system, because each new source brings its own timing model and identifier scheme.


Source and destination chains may have different block times, finality windows, and indexer lag, so one logical transfer may produce two on-chain events that become final at different times.


Cross-chain transfers can rely on bridge messages, venue ledgers, or custodian records, making it difficult to maintain a single authoritative trace. When position queries run outside the reconciliation system, in a treasury dashboard or a one-off script, every new chain widens reporting gaps.


### 4. Negative drift signals payout risk before it becomes a settlement failure


Negative drift occurs when a transaction is entered into the system, but the corresponding funds do not arrive. This points to a settlement failure or a missing inflow.


A treasury operating across many chains may see USDC accumulate on Optimism due to a busy settlement run. At the same time, Base-side obligations, such as vendor payments or payroll disbursement, are scheduled for the same afternoon. Negative drift on the destination chain is a payout risk you can still act on if the system surfaces it before the deadline.


## What the reconciliation architecture needs to handle under higher volume


Reconciliation architecture requires independent cut-off timestamps, internal transfer classification at write time, cross-chain position queries, and direction-based drift classification to address high-volume transactions.


**Requirement** **What it handles** **Why a balance check fails without it**


Independent cut-off timestamps per source Different block times, finality windows, and indexer lag across chains A calendar close is too blunt for digital-asset systems; you need a source-specific cutoff, such as a block height per chain


Internal transfer classification at write time Hot-to-cold sweeps, omnibus moves, and bridge legs that look external Raw chain ingestion records both legs as external flows and double-counts


Cross-chain position query inside the system True position across wallets, exchanges, and custodians Position queries that live in dashboards drift as soon as a new chain is added


Drift is classified by direction Negative drift (missing inflow, settlement risk) vs. positive drift (unrecognized inflow, possible shadow liability) An aggregate mismatch count identifies a mismatch without showing whether you are about to miss a payout


Independent cut-off timestamps per source are implemented through block-height anchoring, which, instead of comparing a live wallet balance with a ledger total calculated on a different clock, has the system read each chain at a selected block height and compare it to the ledger balance calculated at the corresponding point in time.


For cross-chain transfers, treat each movement as two independent single-chain reconciliations linked by a correlation ID.


## Reconciliation patterns that digital-asset engineering teams run under higher volume


Digital-asset engineering teams employ various reconciliation patterns to manage[high-volume transactions](https://www.formance.com/blog/financial%20operations/account-reconciliation-patterns-for-high-volume-fintech) , including balance, transaction-level, and proof-of-reserves reconciliation. Each of these methods is crucial for upholding financial integrity and ensuring regulatory compliance.


### Balance reconciliation that matches figures


Balance reconciliation answers one question: Do the ending balances match? It typically runs daily, weekly, or monthly, comparing aggregate wallet or account balances between the internal ledger and on-chain state.


Balance reconciliation is a standard periodic close check and the foundation for attestation production. It also shows only a net position, including timing, fee splits, and per-payment attribution, which remain unresolved.


### Transaction-level reconciliation


Does every individual transaction match across sources? Transaction-level reconciliation matches by transaction hash, amount, and timestamp, runs continuously or on a per-settlement-cycle basis, and is the primary tool for detecting timing gaps and failed transactions.


It also catches the late-confirming deposit before it becomes a discrepancy that the finance team finds three months later.


### Proof-of-reserves reconciliation


Proof-of-reserves reconciliation compares custodied assets with the liabilities owed.


For exchanges, one implementation hashes user balances into a Merkle tree, publishes it, and lets an independent reviewer assess both wallet ownership and whether the published tree includes the relevant balances.


For fiat-backed stablecoin issuers, the pattern shifts toward accounting attestations and on-chain wallet disclosures.


However, proof-of-reserves is a snapshot view of assets relative to reported balances, leaving open the question of whether every liability has been captured.


Balance, transaction-level, and proof-of-reserves reconciliation all depend on how you map internal pools to external state. At Formance, we built an open-source,[programmable core ledger](https://www.formance.com/modules/ledger) that unifies fiat and digital assets with a regulatory-grade audit trail. Coupled with[Formance Reconciliation](https://www.formance.com/modules/reconciliation) , you can map internal account pools to external payment pools with independent cut-off timestamps per source, which is the structure proof-of-reserves work demands.


## What makes on-chain/off-chain reconciliation audit-ready


Audit-readiness depends on meeting regulatory requirements, while enforcing four structural properties, each enforced at write time: atomic internal transfers, queryable on-chain metadata, block-height-anchored reports, and traceable drift logs.


### Regulatory Requirements for on-chain/off-chain reconciliation


Regulatory requirements from the following bodies are needed:


- GENIUS Act (§4(a)(1), monthly reserve composition requirement; §4(a)(3)(A), monthly examination by a registered public accounting firm; §4(a)(3)(B), CEO and CFO certification of accuracy to the primary regulator)
- MiCA Article 22 (quarterly reporting obligations for ART issuers, including reconciliation of transaction data received from crypto-asset service providers and submission to competent authorities),
- [IOSCO recommendations](https://www.iosco.org/library/pubdocs/pdf/IOSCOPD747.pdf) for Crypto and Digital Asset Markets (2023, Recommendation 15) now require these properties on an ongoing basis, not just at quarter-end.


### Atomic internal transfers


A matching debit and credit stop a custody sweep from appearing as external drift. Take a hot-to-cold sweep of $250,000 in USDC. In the Formance Ledger, the platform's hot and cold wallets are multi-asset accounts: @platform:treasury:hot and @platform:treasury:cold. Posted as a single atomic transaction that touches two internal accounts, the sweep remains classified as an internal movement.


It is as described in[Formance’s Numscript DSL language](https://www.formance.com/blog/engineering/numscript) , and is readable by both engineers and finance teams:


```text
// INTERNAL_TREASURY_SWEEP
// Event: move 250,000 USDC from the platform hot wallet to the platform cold wallet
send   [USDC/6 250000000000]   (
source   =   @platform:treasury:hot
destination   =   @platform:treasury:cold
)
set_tx_meta  (  "event_type"  ,   "internal_treasury_sweep"  )
set_tx_meta  (  "sweep_id"  ,   "swp001"  )


```


Both the debit and credit legs move the same value between two accounts under your control. Because the debit and credit post atomically, the reconciliation engine reads the transaction as a controlled internal movement tied to both accounts.


### Queryable on-chain metadata


On-chain metadata, including transaction hash, network fee, and sub-status, should be stamped on every internal posting as queryable fields so the reconciliation system can match ledger entries to chain records.


A reconciliation system should treat raw blockchain records as inputs that require an accounting context; a single transaction can have multiple accounting impacts, depending on the protocol. Stamping each posting with transaction hash, network fee, and sub-status makes absence meaningful.


In a[digital-asset custody connector](https://docs.formance.com/modules/connectivity/connectors/exchange/fireblocks) , the absence of tx_hash indicates an off-chain transfer. The Ledger's multi-segment account namespace makes those fields queryable, so a read against users: returns every position across every provider without a migration. Asset-level tagging keeps positions of the same asset across different chains distinguishable in the same ledger.


### Block-height-anchored reports


Reconciliation reports anchored to a specific block height let an examiner compare internal balances and the on-chain state at the same point in time.


A practical architecture combines two modes:


1. Real-time reconciliation continuously compares transfers against the on-chain state after settlement, catching webhook failures quickly.
2. Batch point-in-time reconciliation compares full balances at fixed cutoffs, such as block heights, for close and attestation production.


Each reconciliation run and its drift are surfaced so an operator can review pass, fail, and drift states without writing a query. The Ledger's bi-temporality, which tracks both when events were recorded and when they were effective, supports precise cut-off reconciliations regardless of machine time.


### Traceable drift logs


Drift logs should include the policy ID, internal account query, external pool, and both the recorded and effective timestamps so that any examiner can reconstruct the run.


A drift log becomes useful as a compliance artifact when every run is traceable, regardless of outcome: pass, fail, drift, or custodian API down. The trace, with both recorded and effective timestamps, is what an examiner reads.


## The architecture decision for reconciling on-chain and off-chain transactions


Teams either make the reconciliation architecture decision once or carry the maintenance burden indefinitely. Teams that patch reconciliation with per-chain ingestion scripts, manual address tagging, and end-of-day batch checks end up maintaining infrastructure instead of shipping product. Engineering drag accumulates as they trace unmatched transactions across disparate sources.


One architecture that holds has four parts: a single[Formance Connectivity layer](https://www.formance.com/modules/connectivity) that ingests data from every provider into a single data model; policy-based matching with independent cut-off timestamps per source; internal transfers classified at write time so they remain classified as internal movement; and point-in-time drift reports that serve as the regulatory-grade audit trail.


With the four-part architecture in place, adding a chain becomes an adapter config rather than a reconciliation rewrite. Formance delivers this as infrastructure, with[pre-built connectors](https://www.formance.com/connectors) for major custodians and exchanges, including Fireblocks, Kraken, Coinbase, and Binance.
