---
schema_version: "1.0.0"
document_id: "e59f81565bb88c1cb0c843f63072f81db2687c719b30ed636a1f65a0fff6a796"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/industry-analysis/asset-tokenization-explained"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T23:28:18.502414+00:00"
fetched_at: "2026-08-06T23:28:19.738129+00:00"
content_hash: "sha256:0836b848792ed40749f291774c0b3d7d55966280d10e5f79a372c7335066c0e4"
---

# Asset Tokenization Explained: RWAs, Custody, Tokens, and Ledgers

Asset tokenization converts claims on real-world assets into on-chain tokens, but those systems often break down at the off-chain layer. Treating the blockchain transaction log as the authoritative ownership record can work only as long as the custodian's books and the on-chain token supply remain aligned.


In hybrid environments where on-chain and off-chain processes interact, split records can create reconciliation requirements and control challenges. Every mint, transfer, coupon payment, and redemption has to match across the chain and the off-chain books, including the custodian's records. Miss a posting in any one of them, and the systems drift apart silently, because nothing in the token contract knows what the custodian actually holds.


FTX shows how off-platform custody failures can remain invisible to on-chain balance checks. The CFTC's[December 13, 2022, complaint](https://www.cftc.gov/PressRoom/PressReleases/8638-22) charges that Sam Bankman-Fried's conduct caused the loss of over $8 billion in FTX customer deposits. On-chain token balances accurately reflected customer positions throughout. The fiat that was supposed to back those positions had been routed off-platform entirely, so an audit of on-chain balances would not, by itself, have revealed the off-platform fiat shortfall.


Acknowledging the importance of tokenization structure matters. This guide explains how real-world assets, custody, tokens, and core ledgers fit together, and why reconciliation between them is the control point that keeps tokenized systems trustworthy.


### What is asset tokenization? Definition and core components


A tokenized asset is a digital token whose redeemable value is contractually backed by a specific off-chain asset held in custody. The Bank for International Settlements (BIS) defines tokenization as[recording claims](https://www.bis.org/publ/bisbull72.pdf) on real or financial assets onto a programmable platform. This contractual backing is what distinguishes tokenized assets from[natively digital assets](https://www.congress.gov/crs-product/IF12670) like Bitcoin and ether.


The token holder's claim rests on both legal rights and cryptographic control. RWA tokens generally represent a contractual claim to, or a fractional interest in, a real-world asset, and off-chain legal agreements define what the holder can enforce.


In regulated implementations, custody is maintained by a special-purpose vehicle or a qualified custodian before any on-chain representation is minted. A core ledger then records ownership, reconciliation, and regulatory reporting across these hybrid environments.


Every transfer, income distribution, and redemption requires a matched action across all layers. A transfer that updates the chain while leaving the master securityholder file unchanged can leave legal ownership ambiguous.


### Asset classes being tokenized: bonds, real estate, and carbon credits


Each tokenized asset class imposes a different ledger burden because the off-chain record must preserve the economic rights the token represents.


#### Bonds, treasuries, and money-market instruments


Tokenized treasuries and money-market funds create recurring reconciliation pressure among the fund administrator's records, the custodian's holdings, and the on-chain token supply.


BlackRock's[BUIDL fund](https://www.coindesk.com/markets/2025/06/18/blackrocks-29b-tokenized-treasury-fund-now-accepted-as-collateral-on-cryptocom-deribit) is one production example of a tokenized institutional liquidity fund. Frequent income distributions intensify that pressure when token balances are expected to reflect fund economics before the next settlement window. S&P Global reported that tokenized money market funds[surpassed $15 billion](https://thefintechtimes.com/sp-global-outlines-rating-framework-for-tokenised-money-market-funds/) in assets under management (AUM) globally in July 2026.


#### Real estate


Fractionalized real estate and private credit both force the core ledger to track partial positions, distribution waterfalls, and distressed states across many holders. Real estate programs can carry thousands of token holders in a single instrument, with distribution waterfalls and redemption queues that must reconcile against off-chain title records. Those holders often sit within a single[omnibus account](https://www.formance.com/glossary/omnibus-account) , which holds funds on behalf of many underlying owners.


RealT, which[raised roughly $93–100M](https://www.wxyz.com/news/crypto-real-estate-company-realt-collected-millions-from-investors-for-detroit-properties-it-doesnt-own) tokenizing Detroit properties, illustrates the scale at which a single property-tokenization program can require a ledger to map fractional token positions to off-chain property and title records. Distribution waterfalls create another off-chain issue with legal agreements that can define the economics, which a smart contract only enforces if those rules are explicitly programmed.


#### Carbon credits and semi-fungible instruments


Carbon credits require semi-fungible tracking, including project and vintage, issuing registry, methodology, and retirement status, all of which must remain distinguishable within the same token class.


Toucan illustrated the risk by bridging Verra credits into its fungible[Base Carbon Tonne pool](https://blog.toucan.earth/raising-standards-in-the-on-chain-carbon-market) . This design pooled credits from different projects into a single tradable token and ultimately absorbed many low-quality or retired credits. A plain fungible token contract cannot preserve credit-level distinctions once units are pooled as interchangeable balances, which is why the BioCarbon Tokenization guidelines have encouraged semi-fungible tokens for credit tokenization.


Your core ledger faces the same requirement off-chain: two credits of the same asset type with different vintages or retirement states that are not interchangeable units. This is why Formance ledgers model this class of instrument through Assets Coloring rather than a plain fungible balance.


### 3 steps in how the tokenization process works


The tokenization process works only when asset origination and custody setup, token issuance and on-chain minting, and lifecycle events are recorded as one connected control flow.


#### Step 1: Asset origination and custody setup


In regulated implementations, an off-chain legal structure comes before any mint transaction. The real-world asset is placed in a special-purpose vehicle or with a qualified custodian. The SPV structure provides bankruptcy remoteness, so the asset is claimable by token holders rather than the issuer's general creditors.


There are two ownership-bridge models: in Model A, on-chain transfers update the master securityholder file in real time; in Model B, the master file remains off-chain, and on-chain transfers instruct updates to it. Under either model, the legal link between the token and the asset exists before the first token is created.


#### Step 2: Token issuance and on-chain minting


Token issuance relies on a backing invariant because the issuer places the asset in custody, backing the tokens minted against it.


In the core ledger, a single atomic multi-posting records two facts at once: the custodian holds the bond, and the investor holds the matching allocation.


In the[Numscript language](https://www.formance.com/blog/engineering/numscript) , a purpose-built language from Formance for describing financial transactions, both postings live in a single transaction. Here is a $1,000,000 tokenized U.S. Treasury bond issuance:


```text
// TOKENIZED_TREASURY_ISSUE
// Event: record a custodied Treasury position and mint the matching token allocation
send   [UST/2 100000000]   (
source   =   @external:securities:depository   allowing   unbounded   overdraft
destination   =   @counterparties:custodians:bnyMellon
)
send   [TBOND/2 100000000]   (
source   =   @world
destination   =   @customers:001:portfolio
)
set_tx_meta  (  "event_type"  ,   "tokenized_treasury_issue"  )
set_tx_meta  (  "issuance_id"  ,   "iss001"  )


```


The first[ledger posting](https://www.formance.com/glossary/posting) records the immobilized bond position entering @counterparties:custodians:bnyMellon. The second credits the token allocation to @customers:001:portfolio.


Because both commits are one atomic transaction, no persisted ledger state can exist in which tokens are allocated without a recorded custodied-position entry, or in which a bond position exists with no recorded owner. The mint-equals-allocation invariant holds at the storage layer, with no application code checking it afterward.


#### Step 3: Lifecycle events: transfers, income, and redemption


Every lifecycle event requires a matched pair: an on-chain transaction and an off-chain core ledger posting.


For instruments with income or interest entitlements, a coupon payment arrives as cash from the paying agent, gets allocated pro rata across holder accounts in the ledger, and is mirrored by the on-chain distribution. A secondary transfer updates the on-chain registry and the ledger's ownership records. A redemption burns tokens on-chain while the custodian liquidates the asset, and the ledger debits the position and credits cash out.


An unmatched event in either direction is a reconciliation failure. Tokens burned without a ledger posting produce phantom ownership; a ledger posting without the chain event produces orphan tokens.


### The four invariants that the tokenization infrastructure must enforce off-chain


Tokenization infrastructure must enforce four off-chain guarantees: immutability, idempotency, bi-temporality, and multi-asset atomicity. Each one addresses a potential issue that the blockchain alone cannot cover in a way auditors can trust.


#### 1. Immutability


The transaction history is hash-chained and tamper-evident. Each transaction cryptographically references the previous one, so altering the history breaks every subsequent hash. Corrections are additive reversals, never edits, which preserves the chain of custody auditors verify.


#### 2. Idempotency


Every mint and redemption operation must be duplicate-safe, so a retry after a failed network call never results in double-crediting an investor. A mint retried after a timeout, without an idempotency key, is the same bug with a securities registry attached.


#### 3. Bi-temporality


The ledger must track both when an event was recorded and when it was effective. A dividend posted on March 15 with a March 1 record date needs both dates in the ledger, so auditors and regulators can reconstruct the exact ownership state at any historical point, not just the current balance.


#### 4. Multi-asset atomicity


A transaction that debits fiat from an investor's omnibus account and credits a tokenized bond position must complete both legs or roll back entirely, with no intermediate state persisted. Without atomicity, one leg cannot finalize without the other, and the failure mode is ledger desynchronization, or state drift.


At Formance, our position is that the off-chain control stack must provide these four guarantees, with the core ledger enforcing the ledger-level ones, because blockchains do not provide them in a form regulators can audit.


Chain finality is probabilistic on many networks, and blockchain reorganizations can create settlement-finality risk. In systems where personal data is kept off-chain for privacy reasons, auditors may need to reconcile on-chain transactions with off-chain records, regardless of the chain's guarantees.


### Reconciliation between the on-chain state and your core ledger


Tokenization reconciliation bottlenecks live in tokenization systems. Every on-chain event must produce a matching ledger posting, and any mismatch between chain state, custodian records, and core ledger balances is a direct regulatory risk.


The Financial Conduct Authority (FCA) now requires electronic money institutions (EMIs) and payment institutions to conduct[daily safeguarding reconciliations](http://www.fca.org.uk/publication/policy/ps25-12.pdf) ; the New York Department of Financial Services (NYDFS) requires stablecoin issuers to provide[monthly reserve reports](https://www.dfs.ny.gov/reports_and_publications/press_releases/pr202206081) reviewed by an independent public accountant.


Production-grade infrastructure runs reconciliation through these checkpoints:


1


**Token supply on-chain versus investor balances in the ledger:** Total minted tokens must equal the sum of allocated positions.


2


**Custodian asset holdings versus ledger asset positions:** What BNY Mellon or Fireblocks reports must match what your books say is immobilized.


3


**The on-chain holder registry versus investor ownership records in the core ledger:** Wallet addresses and legal owners must map one-to-one.


Teams without a dedicated reconciliation loop may discover discrepancies at month-end close or during a regulatory examination. By that point, the gap can compound across hundreds or thousands of lifecycle events.


The New York Attorney General found that starting no later than mid-2017,[Tether's claims](https://ag.ny.gov/press-release/2021/attorney-general-james-ends-virtual-currency-trading-platform-bitfinexs-illegal) that tethers were backed one-to-one by U.S. dollars were false because it did not maintain reserves at all times sufficient to back all tokens in circulation, a divergence the regulator later documented in settlement.


### What infrastructure teams need to build production-grade tokenization


Tokenization infrastructure must keep the blockchain in agreement with the custodian and a programmable core ledger, because most failures occur in the handoffs between those three layers. FTX, Tether, and the Toucan carbon-credit pools all share a pattern: the on-chain contracts executed while the off-chain layers beneath them broke down.


Reconciliation is one control among several. Building the full stack forces a build-versus-buy call, and that decision often centers on the core ledger layer.[Ensuring ledger integrity](https://www.formance.com/blog/financial-operations/ensuring-ledger-integrity) from scratch can become a long-running project that diverts engineering time from investor-facing product.


Because Formance is built around an open-source core ledger, engineering teams can inspect and self-host the foundation they use for fiat and digital assets, with regulatory-grade traceability that auditors can verify. Teams model tokenized asset flows in Numscript and semi-fungible instruments through Assets Coloring, instead of rebuilding immutability, bi-temporality, multi-asset support, and atomic multi-posting from scratch.
