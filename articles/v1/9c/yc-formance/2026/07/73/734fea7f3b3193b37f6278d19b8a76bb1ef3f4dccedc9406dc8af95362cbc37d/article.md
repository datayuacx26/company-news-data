---
schema_version: "1.0.0"
document_id: "734fea7f3b3193b37f6278d19b8a76bb1ef3f4dccedc9406dc8af95362cbc37d"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/embedded-finance/cross-border-payments-guide"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-24T08:07:59.167261+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:453baf704cdca6534ca583f1117d3ca44dd18da5d9100681a234ca6d668226e3"
---

# Cross-border Payments: A Technical Implementation Guide

Where is the money right now?


That is the only question that matters in a cross-border payment system. On 27 February 2025, the ECB's[TARGET Services outage](https://www.ecb.europa.eu/press/intro/publications/pdf/ecb.miptopical251107.en.pdf) showed how many teams couldn't respond when their T2 and T2S were unavailable for approximately 10 and 8 hours.


When T2 went down, interbank settlements stalled, and engineers were piecing the picture together from SWIFT messages, nostro statements, and PSP files arriving on different timelines and in different formats. The cross-border payment corridor was frozen for hours before anyone had the full picture.


The outage was difficult to contain because cross-border payments lack a single system of record. A payment passes through multiple intermediaries, foreign exchange (FX) legs, and message formats, each with its own clock and its own record. Engineering, finance, and compliance teams all depend on a single solution to track, move, and reconcile funds across jurisdictions.


A core ledger serves as the single solution for cross-border payments, acting as a system of record separate from the rails that move funds while supporting engineering, finance, and compliance teams.


## What are cross-border payments?


Cross-border payments are fund transfers in which the payer and payee are in different jurisdictions or countries and may involve different currencies.


The FX legs drive most of the engineering complexity because you are dealing with a patchwork of legal regimes, compliance obligations, capital controls, and supervisory authorities.


Three categories shape how you construct and execute cross-border payments:


1. **Wholesale** payments move between financial institutions and are generally high-value; in some operational monitoring frameworks, wholesale payments are defined as USD $100,000 or more. The IMF puts the wholesale cross-border payments[market at about $145 trillion](https://www.imf.org/-/media/files/publications/ftn063/2025/english/ftnea2025002.pdf) .
2. **Retail** payments are low-value end-user transfers (P2P, P2B, or B2B) across jurisdictions. They are lower-value and higher-volume, making retail payments relatively expensive and slow.
3. **Remittances** are a retail sub-segment, usually regular low-value transfers to emerging markets.


Settlement occurs through several CPMI payment models, including correspondent banking (respondent and correspondent banks adjusting nostro/vostro balances via SWIFT), interlinking separate payment systems across jurisdictions, and newer peer-to-peer rails, including digital assets.


## The four steps to build a cross-border payment system


Building a cross-border payment system requires you to map your use cases and corridors, design a cross-border ledger, implement APIs, routing, FX, and reconciliation flows, and embed compliance, fraud controls, and observability into the system.


The order matters. Map corridors first so provider-specific assumptions don't leak into your core. Build the ledger next to anchor the system of record before any rail-specific code is written. Then add normalization, routing, FX, and reconciliation around it to move the money and keep the books clean. Update the flow with compliance and observability to ensure every payment is processed correctly.


## Step 1: Map your cross-border payment use cases and corridors


A corridor is an origin-destination pair, so map only the cross-border payment corridors you actually serve. Each one carries its own rails, partners, settlement windows, and failure modes. For example, USD-to-MXN is a different engineering problem than EUR-to-INR. If you treat cross-border payment corridors generically, provider-specific logic ends up in the product code.


For each corridor, document the rails available to you and their issues:


- **SWIFT correspondent banking** reaches 200+ countries and 11,000+ banks, and about 80% of the total journey time happens *after* the message leaves the SWIFT network. The post-SWIFT leg is your dominant production failure mode.
- **Local instant schemes** settle in seconds or near real-time within their domestic boundary. Reaching local schemes across borders requires a local partner or an interlinking arrangement.
- **Card push rails** can be an option for selected payout corridors, with performance depending on corridor, issuer, and receiving institution.
- [Stablecoin rails](https://www.formance.com/blog/industry-analysis/the-different-types-of-stablecoins) can be an option for selected corridors, with performance depending on network, corridor, and settlement model, and with the caveat that stablecoin payments do not achieve the same finality as central bank reserve settlement.


Build a corridor-to-rail matrix, so you know which rails you can depend on for each corridor and where you need fallback routes. It also tells you, before you write any code, how many distinct message formats and settlement timelines your ledger will have to absorb.


## Step 2: Design a cross-border ledger and money movement core


The ledger collects[user balances](https://www.formance.com/blog/financial-operations/ledger-balance-for-product-and-engineering) , settlement accounts, FX legs, and fees, while the rails move the funds.


Separating ledger and rails lets you swap a payout partner in one corridor without rewriting product logic in another, and it keeps rail-specific behavior at the edge instead of spreading through product code.


Define payment orders and ledger entries as shared primitives, with accounts governed by the same model, and use immutable posted orders with double-entry auditability and zero-sum error detection so the platform can scale to new payment types and geographies without re-architecture.


Two invariants make the ledger trustworthy, and both have to be enforced at the database or API boundary:


### Double-entry at write time


Every financial event creates at least two postings that sum to zero, and no code path can bypass this rule. Enforcing double-entry only in application code leaves too much room for silent failures.


### Immutability through reversal postings


Posted entries stay fixed, so a mistake is fixed with a reversal posting. The append-only log makes retroactive changes detectable; altering a past entry requires rewriting everything downstream of that entry.


Build balances as materialized views over the immutable postings, and treat postings as the primary source of truth. Intermediate clearing accounts should normally return to zero; a non-zero balance is an unresolved state you need to investigate.


A programmable ledger, paired with rail connectivity and orchestration, is the system of record for cross-border money movement. Formance is an[open-source, programmable core ledger](https://www.formance.com/modules/ledger) that unifies fiat and digital assets with regulatory-grade traceability, so the same posting model holds whether a payment settles over SWIFT, a local scheme, or an on-chain transfer.


## Step 3: Implement APIs, routing, FX, and reconciliation for cross-border payments


Implement normalization, routing, FX, and reconciliation around the ledger.


### Normalize external formats before routing


[ISO 20022 MX](https://www.swift.com/standards/iso-20022/iso-20022-standards) became the exclusive standard for cross-border payment instructions on SWIFT on 22 November 2025. Legacy MT instructions and statements, therefore, need to be translated into the relevant MX or CAMT family, and each translation should be explicit in your internal model. Aligning your field names, identifiers, and reconciliation logic with ISO 20022 primitives reduces downstream translation costs.


On-chain transfers require chain-specific normalization. This includes transaction records, event logs, fees, and confirmation states. These elements do not map cleanly to a payment record, so you need to parse them into internal postings and account for failed, replaced, or reorganized transaction states at the edge.


Make every posting idempotent by keying it to the source event ID, so that a retried or replayed message commits exactly once rather than double-posting the same payment.


### Route each payment


A routing layer should select a partner based on payment attributes and corridor objectives, such as straight-through processing rate and cost. Add validation before committing so obvious downstream rejection conditions are caught earlier.


Operational inputs worth evaluating include corridor-level rejection rates, bank cut-off times, liquidity, FX spread changes, and beneficiary bank settlement reliability. Within a corridor, the engine can choose among available local schemes instead of treating all payouts as the same route.


### Book FX as linked legs


An FX conversion routes through a conversion account and an FX counterparty, with the spread posted to revenue as its own leg. Modeling FX as explicit postings keeps the accounting trail defensible at audit.


### Reconcile against every source


A payment can settle on an external rail before your books are clean, while reconciliation is the control process that ties bank, PSP, FX, and ledger records back to the same event.


For FX, reconcile across bank/trade data, ledger entries, and booked rates, then verify that the booked rate reflects the agreed trade rate. Expect timing gaps because nostro banks may not notify you until later on the settlement day or the next working day, and correspondent statements often arrive as delayed messages. Set tolerance windows based on your own observed payout-clearance distribution rather than on a guess.


You need compliance, fraud controls, and observability to prove it moved correctly.


## Step 4: Embed compliance, fraud controls, and observability into cross-border flows


Compliance, fraud controls, and observability sit at specific checkpoints inside the flow of funds, and every money movement in a correspondent chain carries its own obligation.


### Compliance at three points


Place compliance screening at three points:


1. **Onboarding:** This starting point involves identifying and verifying the customer's and beneficial owner's identities, understanding the purpose and nature of the business relationship, and conducting ongoing due diligence. Sanctions screening is addressed under separate FATF recommendations.
2. **Payment initiation and pre-settlement:** The[Travel Rule](https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/Best-Practices-Travel-Rule-Supervision.pdf) (FATF Recommendation 16, revised June 2025) requires that originator and beneficiary information accompany the transfer and be submitted concurrently.
3. **Per-transaction:** In correspondent chains, bank AML/CFT checks happen at each institution before a payment can be completed. Speed does not remove the need for sanctions and financial crime controls before funds are credited.


Reflect licensing and data obligations in where systems run and how data is stored. Retention requirements vary across corridors. Records should be kept for 10 years from the date of the transaction, and a single corridor can require data held across multiple jurisdictions with different retention clocks.


### Fraud controls to check legitimate transactions


Fraud controls run alongside AML and sanctions screening, not as a replacement for them. While compliance asks whether a party is permitted to transact, fraud controls ask whether the transaction itself appears legitimate.


Score payments at initiation using velocity, device, and behavioral signals, and pull in beneficiary risk signals from corridor history, prior chargebacks, and recalls.


Decisions should be explicit postings or metadata on the payment order so the reason a transaction was held, stepped up, or released is auditable later. Feed confirmed fraud back into the routing layer so that repeat patterns, mule accounts, and high-risk beneficiaries get blocked earlier on the next attempt, rather than being caught again downstream.


### Observability for complete monitoring


For observability, track cross-border payment states, corridor-level latency, failure patterns, and reconciliation breaks in production. The[Financial Stability Board (FSB) cross-border payments program](https://www.fsb.org/uploads/P171122.pdf) will require every PSP to surface expected delivery time, tracking status, and terms of service by the end of 2027.


Because most of the journey time occurs at the final stage, you must instrument the full payment path, including network transit and domestic delivery. Monitoring should alert on known thresholds, and observability should preserve identifiers, timestamps and partner responses. Preserve the ledger context needed to debug new failure modes, such as latency spikes during cross-border settlement or race conditions in orchestration.


Treat long-lived clearing balances as an operational signal that a persistent balance points to a clearing issue you have not resolved.


## A proactive cross-border payment


The example below shows Steps 2 and 3 working together on a single remittance: Carlos in Houston sends money to Rosa in Guadalajara, funded by ACH, bridged from USD to USDC, converted to MXN, and paid out locally. The accounts model that holds what is at each stage.


Real-world party Ledger account Holds


Carlos's funded balance` @users:carlos:wallet` Carlos's available USD funds


USDC on-ramp counterparty` @counterparties:onramp:provider_a` running balance against the USD/USDC on-ramp


Carlos's USDC custody` @users:carlos:provider_a:custody` USDC held before FX conversion


FX counterparty` @counterparties:fxProviders:provider_a` running balance against the FX provider


The conversion` @exchanges:conv:RMT001` transient funds mid-conversion


Carlos's payout balance` @users:carlos:local_bank:payout` MXN earmarked for Rosa's payout


Platform FX revenue` @platform:revenue:fx` the spread you keep


First, cash enters via the ACH bank rail into Carlos's USD wallet. Next, the platform converts his USD balance one-for-one into USDC via an on-ramp counterparty, resulting in 500 USDC held in the external provider's custody.


Then Carlos converts that 500 USDC to MXN: the 500 USDC move to the FX provider, the provider delivers 8,600 MXN into the conversion account at the wholesale execution rate of 17.20, the platform pays Rosa 8,400 MXN at the customer rate of 16.80, it quoted her, and books the 200 MXN difference as spread revenue.


The three transactions below are written in[Numscript](https://www.formance.com/numscript) , Formance's domain-specific language for expressing money movements as atomic, double-entry postings on the ledger.


```text
// WALLET_TOPUP
// Event: Carlos funds his wallet; cash enters via the ACH bank rail
send   [USD/2 50000]   (
source   =   @platform:banks:us_bank:main   allowing   unbounded   overdraft
destination   =   @users:carlos:wallet
)
set_tx_meta  (  "event_type"  ,   "wallet_topup"  )
set_tx_meta  (  "remittance_id"  ,   "RMT001"  )
```


```text
// USD_TO_USDC
// Event: Carlos's USD wallet balance is bridged 1:1 into USDC in external provider custody
send   [USD/2 50000]   (
source   =   @users:carlos:wallet
destination   =   @counterparties:onramp:provider_a
)


send   [USDC/6 500000000]   (
source   =   @counterparties:onramp:provider_a   allowing   unbounded   overdraft
destination   =   @users:carlos:provider_a:custody
)


set_tx_meta  (  "event_type"  ,   "usd_to_usdc"  )
set_tx_meta  (  "remittance_id"  ,   "RMT001"  )
set_tx_meta  (  "bridge_rate"  ,   "1.00"  )
```


```text
// FX_CONVERT
// Event: Carlos converts 500 USDC to MXN. The FX provider executes at 17.20 (wholesale);
// the platform quotes Rosa 16.80 (customer rate) and keeps the 0.40 MXN/USDC markup as revenue (200 MXN total).
send   [USDC/6 500000000]   (
source   =   @users:carlos:provider_a:custody
destination   =   @counterparties:fxProviders:provider_a
)


send   [MXN/2 860000]   (
source   =   @counterparties:fxProviders:provider_a   allowing   unbounded   overdraft
destination   =   @exchanges:conv:RMT001
)


send   [MXN/2 840000]   (
source   =   @exchanges:conv:RMT001
destination   =   @users:carlos:local_bank:payout
)


send   [MXN/2 20000]   (
source   =   @exchanges:conv:RMT001
destination   =   @platform:revenue:fx
)


set_tx_meta  (  "event_type"  ,   "fx_convert"  )
set_tx_meta  (  "remittance_id"  ,   "RMT001"  )
set_tx_meta  (  "customer_rate"  ,   "16.80"  )
set_tx_meta  (  "execution_rate"  ,   "17.20"  )
```


Each conversion records as one atomic entry: the ledger commits all legs or none. The USD-to-USDC bridge posts as its own transaction so the asset change is explicit and auditable, and the FX spread posts to revenue as its own leg (difference between the 17.20 wholesale rate the platform executes at and the 16.80 customer rate quoted to Rosa) with both rates riding along as metadata.


When the three-way reconciliation runs the next morning, the on-ramp bridge, the execution rate booked, the customer rate quoted, the spread kept, and the payout earmarked are already on the ledger, traceable to one remittance ID, enough to prove the FX gain from the remittance ID and ledger metadata alone.


## Where is the money in your cross-border payments system?


Can you confirm right now, without opening a PSP report, where each multi-currency, multi-rail balance is located? If the answer is no, you are one rail outage away from ECB's TARGET Services outage; reconstructing the picture from SWIFT messages and nostro statements while the corridor stayed frozen for hours.


The hard part is not building a ledger, an integration layer, an orchestration engine, and a reconciliation process in isolation. It is wiring these modules together so a cross-border payment commits atomically across all four and still ties back to one event at audit. That integration is a multi-year program in-house.


Formance ships it as a single stack, so your engineers write the flow once in Numscript, and the same auditable record applies whether funds settle via SWIFT, SEPA Instant, or on-chain.
