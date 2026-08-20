---
schema_version: "1.0.0"
document_id: "33c6a886c8de72a0867efdb5e53053bdb3f8a26d117271ae27c2b4d9a9f873ce"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/product/tokenization-of-money-infrastructure"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T08:07:59.167261+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:fc2b74be69b9cc9a8d2b4707e9b514adc3abc696927a9c5a7ef63cb776bb922d"
---

# Tokenization of Money: Infrastructure Guide for Builders

A payment processor sends a $50,000 USDC payout to an online marketplace seller to settle the day's card sales, and the transfer completes on-chain in four seconds. The seller immediately moves those tokens to an exchange to convert them into dollars. Still, matching the payout, the incoming USDC deposit, and the resulting sale takes three days across the payment provider, the custodian, the exchange, and the bank.


No one can tell regulators which batch of USDC arrived in the exchange wallet before the seller's sale went through, creating record-keeping and sanctions-compliance exposure.


Tokenized money infrastructure lets builders represent, move, and reconcile claims on money across programmable platforms and off-chain records. For engineering teams that own the infrastructure stack, the reconciliation between fast on-chain settlement and slower books-and-records, reserve, and compliance systems is what triggers failures.


Tokenized money is as much an off-chain problem as an on-chain one. The on-chain side is fast and well-documented, while the off-chain side breaks because it carries the customer liability. That off-chain layer is where an open-source programmable ledger operates, one that enforces double-entry at the storage layer, derives balances from an append-only log, and can apply cryptographic chaining across transactions.


## Tokenization defined: Claims on money in programmable form


Tokenization of money represents claims on money in digital form, enabling them to be transacted via smart contracts. It shifts money from a static record in a bank database to an active object that can carry rules, conditions, and settlement logic alongside its value.


It is often implemented on distributed ledger technology (DLT). However, the underlying technical substrate can vary across designs, ranging from public permissionless chains to private permissioned networks operated by banks or market infrastructures.


A token is something that can be legally and operationally exchanged on a programmable ledger ([like what Formance Ledger provides](https://www.formance.com/modules/ledger) ), meaning it must be recognized as a valid claim under applicable law and be transferable through the ledger's own mechanics, without requiring a separate settlement process to finalize ownership changes. Those benefits fall into three operational gains, each of which changes what the off-chain ledger must model.


## Three operational gains of money tokenization


Tokenized money matters to banks and fintechs because it delivers three concrete operational gains over legacy payment rails and banking: faster settlement with 24/7 operations, programmable payments and atomic settlements, and lower cross-border costs.


### 1. Settlement speed and 24/7 operation


Tokenized money settles faster than legacy payment rails because asset updates between ledger addresses can execute atomically. Many token arrangements operate 24/7, without the limited windows tied to business hours and cut-off times that constrain wholesale systems.


### 2. Programmable payments and atomic settlement


Programmable payments collapse messaging, reconciliation, and asset transfer into a single operation. At the same time, an atomic settlement links two assets so that the transfer of one occurs if and only if the other does as well. Smart contracts can bundle payments, swaps, and collateral adjustments into sequences that execute jointly or fail together.


### 3. Lower cross-border and remittance costs


Tokenized money can lower cross-border and remittance costs in expensive corridors by shortening the intermediary chain and enabling direct on-ledger settlement between counterparties. Remittance fees are reduced because each correspondent bank in a traditional payment chain adds a spread and a lifting fee, and replacing those money movements with a single on-ledger transfer collapses the markup into one transaction cost.


Faster settlement, programmability, and lower cross-border costs vary depending on the type of tokenized money in play, since the issuer, balance-sheet treatment, and settlement venue all shape what the off-chain ledger must model.


## The three types of tokenized money


Stablecoins, tokenized deposits, and CBDCs differ in their issuers, balance-sheet treatment, and settlement mechanics, and each combination changes what the off-chain ledger must model.


**Type** **Issuer** **Balance sheet** **Settlement venue** **Bearer instrument?**


**Stablecoin** NBFI or regulated bank Issuer liability, backed by reserves (cash, short-term Treasuries, reverse repos) Public permissionless ledger Yes


**Tokenized deposit** Commercial bank Stays on the bank's balance sheet; retains deposit-insurance treatment where applicable Private permissioned ledger; transactions authorized by the bank and settled off-chain No


**CBDC (retail/wholesale)** Central bank Direct claim on the central bank Central-bank-operated infrastructure No


As bearer instruments, stablecoins may trade away from par, violating the singleness of money, and holders may end up holding liabilities of multiple issuers. Tokenized deposits fall under[existing banking regulation](https://www.congress.gov/crs-product/IF12670) and are not the same as payment stablecoins. Wholesale CBDCs (wCBDCs) are restricted to financial institutions for interbank settlement, while retail CBDCs are available to the general public for everyday payments.


Of the three token types, stablecoins have the most involved off-chain lifecycle because issuance, transfer, and redemption all cross the boundary between a smart contract and a bank reserve account. The lifecycle below illustrates how tokens are executed and managed in practice.


## The stablecoin lifecycle: Mint, transfer, & redeem


A typical stablecoin lifecycle spans fiat funding, token creation, on-chain transfer, and redemption. Errors in the lifecycle most often occur at the handoff between on-chain events and the off-chain ledger.


### Minting fiat into a token out


Minting creates new tokens against incoming fiat. A verified customer sends fiat to the issuer's reserve bank account, and then the issuer credits reserves and calls mint on the smart contract to create tokens in the customer's wallet. Mint and burn permissions have to be tightly controlled.


[Paxos accidentally minted](https://www.formance.com/blog/financial-operations/300-trillion-minting-error) $300 trillion of PYUSD to one of its own wallets, then burned the same amount shortly after, raising the question of whether smart contracts should generate tokens without corresponding reserves at all.


### Transfer vs. settlement finality


On-chain transfers move value between addresses by updating balances, but a transaction is broadcast, not finalized, at the moment. Depending on the network, an operational transfer may sit in the mempool, be included in a block, and then wait several confirmations before it can be treated as settled. During that window, a reorg or replacement can still unwind it.


[Settlement or transaction finality](https://www.formance.com/blog/financial-operations/understanding-transaction-finality) is the legally defined moment at which a transfer becomes irrevocable and not susceptible to being unwound following insolvency. Operational transfer and final settlement may differ depending on the design, and the ledger must track the settlements between them by recording the transfer at initiation, updating its status as confirmations accrue, and releasing downstream postings only once finality is reached.


### Redemption token burned to fiat out


Redemption removes tokens from the circulating supply and returns fiat to the customer. The customer sends stablecoins to the issuer's redemption address, the redemption workflow burns them, and the issuer transfers equivalent fiat from reserves. Redemption terms typically specify one-for-one treatment against the reference currency and are subject to compliance review.


## How reserve backing, redemption rights, and custody rules are converging across jurisdictions


Tokenized money regulation is converging on full reserve backing, clear redemption rights, and well-defined custody controls across major jurisdictions.


Across regulatory bodies, laws focus on stablecoins, since tokenized deposits already fall under existing banking regulation, and CBDCs sit within central bank frameworks.


### GENIUS Act (US)


The GENIUS Act requires Permitted Payment Stablecoin Issuers to maintain reserves backing outstanding stablecoins at least[1-to-1 at all times](https://www.congress.gov/bill/119th-congress/senate-bill/1582/text) . Eligible reserve assets include:


- U.S. currency and demand deposits
- Short-maturity Treasuries and short-term repos
- Certain money market fund shares
- Other approved liquid federal government assets


Issuers must notify the Office of the Comptroller of the Currency (OCC) if reserves fall below[outstanding issuance on any day](https://www.occ.gov/news-issuances/news-releases/2026/nr-occ-2026-9a.pdf) , and produce monthly reserve reports with external accountant examination and CEO/CFO certification.


### MiCA (EU)


The EU Markets in Crypto-Assets Regulation (MiCA) covers two stablecoin types. Electronic Money Tokens (EMTs) reference one official currency with redemption at par. Asset-Referenced Tokens (ARTs) reference another value or right, or a combination including one or more official currencies.


EMTs require authorization as an Electronic Money Institution or credit institution, plus notification to the national competent authority before issuance. MiCA imposes[reserve, safeguarding, and redemption requirements](https://eur-lex.europa.eu/eli/reg/2023/1114/oj/eng) on EMTs and ARTs.


Supervisory implementation is underway. The European Securities and Markets Authority (ESMA) maintains an[interim MiCA register](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica) covering white papers, ART issuers, EMT issuers, authorized crypto-asset service providers (CASPs), and non-compliant entities.


### Reserve attestations and assurance levels


Auditors check whether reserve assets are sufficient against the outstanding token supply. A reserve attestation tests issuer assertions about backing assets and outstanding supply against the issuer's stated criteria. There are different types of assurance engagements, such as[examinations and reviews](https://www.sec.gov/files/ctf-written-input-aicpa-042125.pdf) , which affect the level of assurance provided.


Regulatory reporting is periodic, but token supply and reserve movements are continuous. Between reporting periods, the operational system still has to know whether backing assets, token supply, and customer liabilities remain aligned. Ongoing reconciliation and ledger tracking to keep those views aligned are the issuer's responsibility.


That compliance review is not left to issuer discretion, since reserve backing, redemption rights, and custody controls are now codified in regulation across the major jurisdictions.


## Why tokenized money is an off-chain problem as much as an on-chain one


Tokenized money is an off-chain problem because issuance has to coordinate three moving parts:


1. Smart contracts that control supply on-chain
2. Financial systems that hold reserves off-chain
3. Operational controls that connect the two.


Off-chain ledger-to-reserve reconciliation carries the main failure risk. A purpose-built off-chain ledger like Formance addresses that risk by exposing on-chain transaction hashes as queryable metadata against the same posting log used for fiat reserves.


No single place cleanly links an on-chain transaction hash, the internal customer balance, and the fiat legs at the bank. Adoption of DLT-based market infrastructure remains in its early stages, with many issuances still at the pilot or limited-production stage. Issuer liability to customers lives off-chain, while the chain handles settlement.


The FTX collapse in November 2022 is what happens when the internal ledger fails to reliably link customer claims to actual reserves.


There was a[failure of internal controls](https://www.fishmanhaygood.com/wp-content/uploads/2023/04/April-9-Debtor-Report-on-Failure-of-Internal-Controls-1.pdf) , including customer balances on the exchange not being backed by corresponding assets in custody and customer funds being commingled with the affiliated trading entity, Alameda Research.


With no reliable internal ledger linking customer claims to actual reserves, FTX filed for Chapter 11, and customers lost direct access to their balances pending years of bankruptcy proceedings.


## The off-chain infrastructure stack every builder needs


The off-chain stack connects reserves, customer sub-ledgers, fiat ramps, on-chain activity, and reconciliation into one controllable operating model.


### Reserve tracking and proof of reserves


Reserve tracking has to support 1:1 backing, segregated reserve accounts, daily reconciliation, and controls that align tokens outstanding, reserve statements, and treasury balances.


Proof of reserves is the public-facing counterpart: a signed attestation and, in some designs, an on-chain proof that reserve assets fully back the outstanding token supply at a given point in time. The off-chain ledger has to expose the same balances that feed those attestations, the published proof and the operational record can drift apart between reporting periods.


### User balance sub-ledgering in omnibus accounts


User balances are tracked at the sub-ledger layer because customer funds sit in pooled accounts. An[omnibus account](https://www.formance.com/glossary/omnibus-account) holds funds on behalf of many underlying owners, with ownership tracked at the books-and-records level; the on-chain equivalent is a single wallet address holding stablecoin balances for many customers.


In omnibus structures, a customer's legal claim runs against the product ledger entry, not the bank account balance, which makes the product ledger the operative record of customer ownership.


Four properties must hold for the subledger to work:


1. Double-entry invariants enforced in the storage layer
2. Append-only immutability
3. Write idempotency
4. Per-entity account isolation


Application-layer enforcement of these properties is insufficient, because bugs or code paths that are bypassed can violate invariants that only the storage layer can guarantee.


### On-ramp and off-ramp orchestration


On-ramp and off-ramp orchestration coordinates fiat rails, mint or redeem workflows, and wallet or ledger layers for on-chain transfers. On the on-ramp side, that means gating mint on confirmed fiat receipt and completed KYC checks, so tokens are not issued against funds that could still be reversed or blocked.


On the off-ramp side, an irreversible on-chain burn cannot complete without a[guaranteed fiat-side recovery path](https://www.formance.com/blog/industry-analysis/stablecoin-payments-architecture-for-regulated-fintechs) . The orchestration layer must run mint, burn, and settle sequences atomically with retry and fallback.


### Reconciliation between chain state and fiat accounts


Reconciliation for stablecoin operations compares on-chain activity, the internal ledger, and off-chain bank or custody records so that the three views stay aligned. A common failure is losing track of an in-flight transfer: record at initiation, never wait for settlement to write the row.


The system needs continuous comparison across these sources, ideally in near real time. A script that runs once per day is no longer sufficient.


Continuous reconciliation only works if fiat, stablecoins, and tokenized deposits share one posting model. Without it, the reconciliation logic is rebuilt for every currency added


## Multi-asset ledgering across fiat, stablecoins, and digital assets


A single system has to track fiat, stablecoins, and other asset types under the same posting model, or ledger logic gets re-implemented every time a currency is added.


Payment rails behave differently. For example, ACH transactions can be returned well after initiation, while stablecoin transfers are effectively final after network confirmation.


The bearer/non-bearer distinction becomes concrete in multi-asset ledgering. A stablecoin transfer transfers the issuer's liability without affecting the balance sheet. Tokenized deposits are typically associated with private permissioned ledgers, and transactions on a tokenized bank deposit are authorized by the bank and settled off-chain.


The account structure must represent stablecoins and tokenized deposits as distinct settlement semantics within the same posting primitive. Asset-agnostic postings, where fiat and digital assets share one account namespace, are the foundation a multi-rail state model needs.


## How to model a tokenized money issuance flow in a ledger


Modeling a token issuance requires two simultaneous postings: one for the issued token balance, and one for the incoming reserve or deposit. A $50,000 issuance expressed with[the Numscript language](https://www.formance.com/blog/engineering/numscript) starts with the account model:


- @users:alice:wallet: holds the balance of issued tokens.
- @external:wires:001: records fiat entering from outside the ledger.
- @platform:reserves:incoming:pending: holds the pending USD reserve entry until fiat clears.


@world issues the token to Alice's wallet, while Alice's fiat wire enters through the named external boundary and lands in pending reserve suspense.


```text
// TOKEN_ISSUE
// Event: issue 50,000 tokens and record Alice's fiat wire as pending reserves
send   [TOKEN/6 50000000000]   (
source   =   @world
destination   =   @users:alice:wallet
)
send   [USD/2 5000000]   (
source   =   @external:wires:001   allowing   unbounded   overdraft
destination   =   @platform:reserves:incoming:pending
)
set_tx_meta  (  "event_type"  ,   "token_issue"  )
set_tx_meta  (  "issue_id"  ,   "iss001"  )


```


The allowing unbounded overdraft clause lets a named external account post a debit without holding a prior balance, modeling the fact that the source of funds lives outside the ledger and is reconciled later against the bank statement.


If the issuance posts on-chain before the matching fiat deposit clears, the new tokens raise liabilities while the offsetting $50,000 sits in @platform:reserves:incoming:pending rather than confirmed reserves.


Once the bank confirms receipt, a second posting moves the $50,000 into confirmed reserves at @platform:banks:chase:reserves:


```text
// RESERVE_CONFIRM
// Event: fiat wire cleared at Chase; move $50,000 from suspense to confirmed reserves
send   [USD/2 5000000]   (
source   =   @platform:reserves:incoming:pending
destination   =   @platform:banks:chase:reserves
)
set_tx_meta  (  "event_type"  ,   "reserve_confirm"  )
set_tx_meta  (  "issue_id"  ,   "iss001"  )
set_tx_meta  (  "bank_ref"  ,   "chase_wire_abc123"  )


```


The shared issue_id ties the two postings together for reconciliation, and confirmed reserves stay accurate throughout.


Four constraints must hold continuously across the chart of accounts:


1. Reserve or deposit assets greater than or equal to tokens in circulation
2. Omnibus wallet balances equal the sum of customer sub-ledger balances and hold the operational balance
3. Total circulation is equal to the sum of per-chain sub-accounts
4. An issue-and-retire control account that resets to zero at the end of each cycle


If the control account does not return to zero, investigate before treating reserves and supply as reconciled. Postings through the control account can carry the on-chain transaction hash as queryable metadata.


A general-purpose database cannot hold tokenized money safely, which is why the flow above needs a purpose-built ledger.


## Why tokenized money needs a purpose-built ledger


Tokenized money needs a purpose-built ledger because general-purpose databases store balances as mutable numbers, allow double-posting on retries, and leave concurrent transactions to race each other into overdraft.


Overwriting a balance destroys audit traceability, and failed transactions, concurrency issues, or sync bugs produce[balance drift](https://www.formance.com/blog/financial-operations/ledger-balance-for-product-and-engineering) that is hard to trace and harder to correct. Transport-layer idempotency alone is insufficient because idempotency must be enforced at the level of[canonical financial mutation](https://www.irejournals.com/formatedpaper/1714977.pdf) , and double-entry invariants and append-only immutability belong in the ledger layer, where the corresponding failure modes are structurally impossible.


[Formance Ledger](https://www.formance.com/modules/ledger) enforces double-entry at the database level, rejects unbalanced postings, derives balances from an append-only log, and applies cryptographic chaining between transactions for tamper evidence. Reconciliation against bank, exchange, and custodian balances runs on top of the same posting log, so discrepancies surface against the authoritative record rather than a parallel system.


Tokenized money breaks when the off-chain ledger loses the link between chain activity and customer ownership. Formance captures that link and shifts reconciliation from batch, manual workflows toward automated, near-real-time monitoring. Clone[Formance on GitHub](https://github.com/formancehq/ledger) and start modeling your token flows today.
