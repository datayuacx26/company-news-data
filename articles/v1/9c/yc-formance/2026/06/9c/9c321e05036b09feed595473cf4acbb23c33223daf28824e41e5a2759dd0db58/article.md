---
schema_version: "1.0.0"
document_id: "9c321e05036b09feed595473cf4acbb23c33223daf28824e41e5a2759dd0db58"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/financial-operations/how-to-invest-in-stablecoins-for-money-movement"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:2bc2263957a49e2cf0b75a05607e86d9767551b0fd043ce8aab8ccd9c877983b"
---

# How to Invest in Stablecoins for Money Movement

Consider this scenario: your treasury team sends $50,000 in USDC to a supplier in Europe on Monday morning. The on-chain leg settles in 90 seconds. The ACH transfer that funds the stablecoin position doesn't move for 48 hours. For two business days, your ledger holds a settled obligation against an unsettled funding source, and your treasury, reconciliation, and compliance systems all need to know it. This is the three-clock problem: your money now runs on three clocks (the chain, the bank, and your books) that never agree on the same source.


This is the cost of stablecoin adoption. Suppliers want to be paid faster and expect 24/7 settlement, while finance leaders keep asking why cross-border stablecoin payments still lose margin to intermediary banks.


Among 350 corporates and financial institutions surveyed by[EY-Parthenon](https://www.ey.com/content/dam/ey-unified-site/ey-com/en-us/insights/financial-services/documents/cs-eyp-stablecoin-survey.pdf) , 52% cited reduced transaction costs as a leading driver of stablecoin adoption, 45% cited faster cross-border payments, and 54% of non-users expect to adopt within 6 to 12 months. Adoption is showing up in volume too: stablecoin settlement activity has reached a[$7B annualized run rate](https://www.theblock.co/post/399405/visa-stablecoin-settlement-hits-7-billion-run-rate-pilot-expands-nine-blockchains) .


The speed and availability of stablecoins come with operational and compliance risks that don't exist on traditional rails. Responsible business investment in stablecoins means evaluating the token, choosing a custody model, and rebuilding the ledger and reconciliation architecture.


## What Is Stablecoin and How Does It Work?


A stablecoin is a digital dollar; a token on a blockchain designed to always equal $1.


The most common kind for business payments is fiat-backed, meaning every token in circulation is matched by a dollar of cash or short-term treasuries held by the issuer. Deposit a dollar to mint a token; return a token to get a dollar back.


Because these tokens live on a blockchain, they move between wallets in minutes rather than business days, with settlement finality set by the chain rather than a bank cut-off. The peg holds because redemption at par is credible: traders buy it back to $1, but only as long as they trust the reserves are actually there.


The stablecoin structure is straightforward in theory. The long-term question for businesses is whether a given stablecoin can honor redemptions under stress.


## Why Do Businesses Invest in Stablecoins?


Businesses invest in stablecoins to move money across borders faster and at lower cost.


Among current stablecoin users in the[EY-Parthenon survey](https://www.ey.com/content/dam/ey-unified-site/ey-com/en-us/insights/financial-services/documents/cs-eyp-stablecoin-survey.pdf) , the top use cases were paying suppliers cross-border (62%), accepting business payments cross-border (53%), and liquidity, treasury, or cash management cross-border (44%). These are primarily business-to-business (B2B) companies in fintech or financial services, typically with more than $10B in annual revenue.


Stablecoins can function as a cash-like operational asset in digital environments, carrying risks distinct from bank deposits, including limited protection for certain crypto-assets, issuer counterparty exposure, and smart contract attack surfaces.


They also carry different capabilities, such as atomic settlement, continuous availability, and programmable movement, which are not available on traditional rails. Whether a given token can actually deliver on those capabilities depends on evaluating five factors.


## What Should You Evaluate Before Investing in Stablecoins?


Reserve quality and transparency, redemption mechanics, regulatory posture, and counterparty concentration are the five factors that determine whether engineering and treasury teams should invest in stablecoins.


1. **Reserve quality**


The composition of an issuer's reserves determines whether redemptions hold at par under stress. Higher-quality reserves such as short-duration Treasuries, repo backed by Treasuries, and bank deposits behave differently from commercial paper or longer-duration assets when forced selling occurs.


They require a published reserve composition and update frequency before allocating treasury exposure.


1. **Reserve transparency**


Major stablecoin issuers publish periodic reserve disclosures from independent accounting firms, and the quality, scope, and frequency of those reports vary significantly by issuer.


Some disclosures are point-in-time confirmations of reserve composition rather than full financial statement audits, so teams should review what is actually being examined before relying on it as assurance.


For a stronger benchmark, finance and risk teams can use the American Institute of Certified Public Accountants’ (AICPA)[Stablecoin Reporting Criteria](https://www.aicpa-cima.com/resources/download/stablecoin-reporting-criteria) to determine what robust reporting on outstanding tokens, backing assets, and asset sufficiency should cover.


1. **Redemption mechanics**


Direct issuer redemption channels matter more than exchange liquidity, because secondary market liquidity fails during the exact stress moments when redemptions are most needed.


A direct issuer channel typically provides 1:1 USD redemption for institutional clients, subject to applicable conditions, including Anti-Money Laundering/Know Your Customer (AML/KYC) and sanctions restrictions.


1. **Regulatory posture**


A stablecoin's standing under the[Markets in Crypto-Assets regulation](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica) (MiCA), the[GENIUS Act](https://www.congress.gov/bill/119th-congress/senate-bill/394/text) , and US state regimes affects which counterparties can hold or transact in it. Issuers with clear and broad regulatory authorization are generally easier to integrate into a regulated business, because their tokens can be held and used by a wider range of institutions.


Issuers without authorization in a given jurisdiction may face restrictions on who can hold, distribute, or transact in their token, so verify current regulatory standing before allocating exposure in any market where you operate.


1. **Counterparty concentration**


Reserves held at a small number of banks create concentrated bank-failure risk. A stablecoin's price can detach from its target value when a custodian bank becomes impaired, and the market loses confidence in the issuer's ability to redeem at par, as past bank-related stress episodes have shown. They require a public custodian breakdown with concentration data before allocating exposure.


Issuer bank reliability is only one side of the equation; the other is the security of your own token custody.


## How Do Businesses Hold and Manage Stablecoins?


Businesses hold stablecoins through one of three custody models, each with different control and liability characteristics.


1. **Third-party qualified custody**


A regulated qualified custodian holds client assets in accordance with applicable banking or trust-company custody obligations. Digital assets held in custody are not guaranteed and are not subject to[Federal Deposit Insurance Corporation](https://www.fdic.gov/) (FDIC) or[Securities Investor Protection Corporation](https://www.sipc.org/for-investors/what-sipc-protects) (SIPC) protections, so you bear full counterparty risk tied to the custodian's operational resilience.


1. **Self-custody via an institutional platform**


Your institution controls private keys using vendor-provided multi-party computation (MPC) or hardware security module (HSM) infrastructure. Full legal, regulatory, and operational risk stays with you.


1. **Regulated exchange / prime brokerage**


Integrated custody, trading, and prime brokerage services are used together. Custody runs through a trust company regulated by the New York Department of Financial Services (NYDFS). Derivatives clear through CFTC-regulated entities or venues. The model is operationally simpler, though your custody and trading counterparty are the same entity, concentrating risk.


No custody choice is risk-free, and the exposures it creates sit alongside several other risks of investing in stablecoins.


## What Are the Risks of Investing in Stablecoins?


Stablecoin risks fall into five categories: depegging, platform insolvency, smart contract vulnerabilities, systemic contagion, and regulatory shifts. Each carries distinct requirements.


1. **Depegging**


Stablecoin prices are usually very close to their target value, but stress episodes can produce much larger deviations. Some stablecoin designs have failed entirely under stress, while fiat-backed stablecoins have temporarily detached from par before recovering. The magnitude of a depeg depends on the token's design, the quality of the reserves, and market confidence.


1. **Platform insolvency**


Stablecoins held on a centralized exchange may amount to unsecured credit exposure to that exchange, depending on the exchange's custody and account terms and the treatment of customer assets in insolvency. Multiple centralized crypto platforms have failed in recent years, and customer recoveries in those proceedings have been partial and slow.


1. **Smart contract vulnerabilities**


Onchain protocols that route, lend, or convert stablecoins can be exposed to bugs, governance exploits, and cross-chain bridge failures. Smart contract attack surfaces have no analog in traditional payment rails and require dedicated audit and monitoring practices.


1. **Systemic contagion**


Greater interconnectedness between crypto and the traditional financial system could amplify the transmission of shocks and[spillovers](https://www.federalreserve.gov/econres/notes/feds-notes/in-the-shadow-of-bank-run-lessons-from-the-silicon-valley-bank-failure-and-its-impact-on-stablecoins-20251217.html) . Stress in a bank holding stablecoin reserves can further propagate to the stablecoin itself and, from there, into on-chain protocols that hold the same stablecoin as collateral.


1. **Regulatory shifts**


US stablecoin regulation is still evolving. The GENIUS Act was enacted in 2025, but its framework does not become operative immediately in all respects, and key implementation details are still being defined through rulemaking by agencies including the OCC and FDIC.


MiCA's e-money token regime imposes its own requirements on issuers operating in the EU, and firms should consult counsel on how those rules interact with PSD2 and other local payment obligations.


Most of these risks are absorbed or amplified by the systems that move the money, which is where the operational picture starts to shift.


## How Do Stablecoins Change Your Money Movement Architecture?


Stablecoins change three things in your money movement architecture: how you reconcile across settlement cadences, how you track on-chain transaction state, and where compliance work lives. Your ledger sits at the center of all three.


The first change is cadence. Stablecoins can settle in seconds and operate continuously, while your Automated Clearing House (ACH) leg does not initiate until Monday.


When a customer sends you $50,000 in USDC on Monday, the fiat leg is pending until Wednesday, and the intermediate state must be accurately represented to customers, your treasury, and your compliance systems.


The three-clock problem is what makes that Monday-to-Wednesday situation so hard to manage; you have three onchain systems operating on different timelines. Blockchains settle in minutes and never close, banks settle in batches over one to three days with hard cut-off times, and your[general ledger](https://www.formance.com/glossary/general-ledger) runs on accounting periods.


When these three systems are architecturally incompatible, your ledger must bridge them:


1. **Rail-agnostic unified ledger**


Every transaction, whether it settles over ACH, wire, Real-Time Payments (RTP), FedNow, or stablecoin rails, posts to the same core ledger. Maintaining separate ledger systems for each rail creates fragmentation: every new rail adds another system to reconcile, and every downstream workflow becomes harder.


1. **Onchain transaction lifecycle of Intent, Execute, Reconcile**


Track the desired state, the chain submission (which may fail or be reorganized), and confirmed chain events independently. A chain reorganization can cause a confirmed transaction to become unconfirmed without any explicit notification, a failure mode with no direct analog in traditional payment rails.


1. **Compliance ownership shift**


On traditional rails, the responsibility for compliance is partially shared with rail operators. On stablecoin rails, the platform owns the full stack, including KYC/AML, sanctions screening, wallet risk monitoring, and[Travel Rule](https://www.formance.com/blog/financial-operations/when-the-travel-rule-meets-stablecoins) . Compliance checks must be integrated into the transaction execution path rather than applied retrospectively to logs.


Each pattern shifts work that used to live with banks, processors, or batch ETL jobs into your real-time stack.


## How Should You Design a Ledger for Stablecoins?


Designing a ledger for stablecoin and fiat activity comes down to four invariants and one architectural separation: keep your product ledger distinct from your general ledger, and enforce double-entry per currency, integer arithmetic, immutability, and idempotency at the ledger layer.


The separation matters because the two ledgers operate at different cadences. A product ledger records stablecoin balances, transfers, conversions, and redemptions, along with all other money movement activity, in real time, and then feeds the general ledger for period-based accounting and reporting. Most traditional GL systems are not built for continuous, real-time posting at the cadence stablecoin activity produces.


Four invariants your ledger must enforce for multi-asset correctness:


1. [Double-entry](https://www.formance.com/blog/engineering/defining-double-entry) **per currency:** Debits must equal credits per currency rather than in aggregate, so a USD debit cannot be balanced by a USDC credit. Without per-currency balancing, a system handling fiat and stablecoins can silently create or destroy value at currency boundaries.
2. **Integer arithmetic in atomic units:** Store amounts as integers in the smallest unit, using cents for USD and six-decimal atomic units for USDC. Never represent monetary amounts as floating-point at any layer.
3. **Immutability with revert semantics:** Ledger rows are never updated after posting. A chain reorganization revert creates a new offsetting posting rather than mutating the original.
4. **Idempotency keyed to blockchain state:** Blockchain event listeners can deliver the same event multiple times, so the[idempotency](https://docs.formance.com/modules/ledger/working-with-the-ledger/idempotency) key for any blockchain-triggered posting must be derived from the transaction hash and the log index and enforced by a unique constraint in the database layer.


Together, these invariants shape how even simple flows, like a USDC→USD conversion, are represented in the ledger. In practice, a USDC→USD conversion appears as a single transaction with two balanced legs — one per asset — and the conversion price captured as metadata:


```text
send   [USDC/6 50000000]   (
source   =   @users:1234:wallet
destination   =   @treasury:conversion:usdc
)
send   [USD/2 5000]   (
source   =   @treasury:conversion:usd
destination   =   @users:1234:bank
)
```


The first posting moves $50 USDC from the user's wallet to a treasury conversion account. The second posting moves $50 from the treasury USD pool into the user's bank account.


Each leg balances against its own asset — USDC against USDC, USD against USD — and the conversion itself is an atomic boundary rather than an aggregate net-zero across currencies. If either leg fails, the transaction is rejected, and neither posting is applied.


Once those invariants are in place, the remaining work is operational: continuous reconciliation between onchain balances, custodians, banks, and internal systems.


## How Do You Build a Stablecoin Program?


Investing in stablecoins means assessing the token evaluation, custody model, and ledger design. These three foundations must be decided before volume goes live, and the regulatory compliance deadline also serves as a systems delivery deadline.


Engineering, finance, and compliance need to be in the same room from the start, with the GENIUS Act framing US operations and MiCA framing EU operations.


The deciding question is whether your current stack can satisfy the four ledger invariants and the cadence patterns outlined above, or whether you need infrastructure built to support them.
