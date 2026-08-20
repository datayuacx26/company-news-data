---
schema_version: "1.0.0"
document_id: "759425cb22d128ca60d82e0ff8a24ef568d8686e9cc143f118a3c00cb61afd50"
company_key: "yc-formance"
company: "Formance"
source_id: "yc-formance-news-import-baf47baa49b1"
canonical_url: "https://www.formance.com/blog/industry-analysis/the-different-types-of-stablecoins"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-23T09:58:17.775073+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:017b6d0c9de0d35cb2f45256f28ca91637bfa1c6bbf5ebae01f9fdee6acf18e9"
---

# The Different Types of Stablecoins

Fiat-backed stablecoins only hold their dollar peg as long as the reserves behind them do. On Friday, March 10, 2023, Circle disclosed that[$3.3 billion of USDC's cash reserves](https://www.circle.com/pressroom/3-3-billion-of-usdc-reserve-risk-removed-dollar-de-peg-closes) were deposited at the failed Silicon Valley Bank.


Over the weekend, USDC traded as low as[$0.86](https://www.coindesk.com/business/2023/03/13/usdc-stablecoin-regains-dollar-peg-after-silicon-valley-bank-induced-chaos) , and roughly $3 billion of USDC was redeemed before the peg recovered on Monday. Any payment, payout, or reserve balance denominated in USDC that assumed a fixed $1 value was mispriced for about 72 hours, with no on-chain mechanism to pause or reverse settlements.


For any platform holding USDC as a settlement asset that weekend, outbound payouts either had to be re-priced manually or held until Monday, and reconciliation between on-chain token balances and off-chain reserve positions broke until the peg recovered. USDC's on-chain mechanics continued to function, but its off-chain bank reserves broke.


While this is a fiat-backed stablecoin situation, other stablecoin designs fail for different reasons. Three of the four are disqualified for payments. Only fiat-backed survives the test, and even they depend on whether the issuer can prove the reserves still match the float.


That makes fiat-backed the best choice for payments. But to use it, you have to do one thing the blockchain won't: check that the tokens you've issued still match the reserves backing them. That is why stablecoin payments need a ledger.


Next, we walk through how each of the four types fails and why only one works for payments.


## What is a Stablecoin?


A stablecoin is a digital asset designed to maintain a stable value relative to another asset, most commonly a fiat currency like the U.S. dollar. The total stablecoin market is roughly[$234 billion](https://home.treasury.gov/system/files/221/TBACCharge2Q22025.pdf) , with USD-denominated stablecoins accounting for over 99% of that total.


USDC is the canonical example: a token that trades at $1 can be sent across blockchains in seconds and is redeemable for one U.S. dollar held in cash or short-dated Treasuries. A merchant settling a cross-border invoice in USDC receives the same dollar value the sender posted, without waiting for correspondent banking rails to clear.


USDC is one of four backing models in production. The fiat-backed design it uses is the largest, and the one most regulators have built rules around, so it's the right place to start.


## Fiat-backed Stablecoins


Fiat-backed stablecoins are tokens backed 1:1 by off-chain reserves, including cash, short-term government securities, or insured bank deposits held by a centralized issuer.


Token holders can redeem directly with the issuer at par value, and the redemption obligation is what holds the peg. USDC and USDT are the two dominant examples. Fiat-backed stablecoins are the most widely used type for payments, where one token equals one dollar held in a bank account or a Treasury bill.


The[Federal Reserve](https://www.federalreserve.gov/econres/notes/feds-notes/payment-stablecoins-and-cross-border-payments-benefits-and-implications-for-monetary-policy-20260330.html) treats fiat-backed designs as the model most closely aligned with existing payment regulation, and reserve and reporting requirements are set forth in the[GENIUS Act](https://www.congress.gov/bill/119th-congress/senate-bill/1582/text) and the[MiCA framework](https://www.ecb.europa.eu/pub/pdf/scpops/ecb.op353~11120d3428.en.pdf) .


As Silicon Valley Bank's March 2023 failure showed, bank counterparty risk breaks the design. The token mechanism keeps working while the reserves backing it sit in accounts that can freeze, fail, or get clawed back. It’s imperative to audit which banks hold the cash portion of a stablecoin's reserves and to prefer issuers whose reserves are held at systemically important banks or in Treasury-only structures.


The fiat-backed model concentrates risk at the bank holding the reserves. The next stablecoin tries to remove that bank counterparty entirely by moving the collateral on-chain.


## Crypto-backed Stablecoins


A crypto-backed stablecoin is a token whose peg is backed by on-chain crypto collateral locked in smart contracts, rather than by off-chain bank reserves. Common collateral assets include ETH and wrapped BTC, locked in smart contracts at high overcollateralization ratios.


As collateral is volatile, crypto-backed stablecoins rely on substantial overcollateralization (DAI vaults, for example, typically require 150% or more) and automated liquidation that sells the collateral when its value falls too far.


DAI (now a legacy stablecoin that can be upgraded to USDS) is the primary example. The MakerDAO system lets users lock crypto assets in vaults and mint DAI against them, and it automatically liquidates the vault if the collateral value drops.


Smart contract bugs and oracle failures break these designs. Collateral positions are visible on-chain in real time, while off-chain reserve models rely on periodic reporting. Teams verify backing without waiting for an attestation cycle.


Crypto-backed designs trade bank risk for code and oracle risk, but the collateral is still a financial asset. For payments, the volatility is the problem. A token backed by assets that can crash overnight is not something you settle invoices in.


A third model swaps the collateral type entirely, anchoring the token to a physical commodity instead.


## Commodity-backed Stablecoins


Commodity-backed stablecoins hold reserves off-chain in physical assets, most commonly gold, which is held in custody. Each token is tied to a specific quantity of the underlying commodity.


PAXG and Tether Gold (XAUT) are two examples of gold-backed tokens. Their dollar price moves with the commodity's spot price, so they don't work for dollar payments, and their scale is much smaller than that of dollar stablecoins, which limits liquidity for high-volume operations.


Commodity-backed tokens at least have a physical reserve sitting behind them. The fourth category drops the reserve entirely and tries to hold the peg through code alone.


## Algorithmic Stablecoins


Algorithmic stablecoins are the only category without external reserves, as they use supply-adjustment algorithms to match supply and demand rather than collateral.


With no external reserve to absorb losses, they're the riskiest category, so the GENIUS Act excludes them from payment frameworks.


The clearest illustration of how an algorithmic design fails is the Terra/Luna collapse, which wiped out a top-ten stablecoin and its sister token in under a week.


TerraUSD (UST) maintained its peg through a mint-and-burn loop with its sister token LUNA. When UST traded below $1, holders could burn it to mint $1 worth of LUNA, in theory reducing supply. The backing was LUNA's market value, which was itself derived from demand for UST, so the reserve and the liability were the same closed system.


But on May 7, 2022, large holders began selling UST, and the peg broke. Traders burned UST to mint LUNA as designed, but the flood of new LUNA crashed its price, and LUNA's supply inflated[over 20,000-fold](https://www.federalreserve.gov/econres/feds/files/2023044pap.pdf) as the loop fed on itself. By May 13, UST was below $0.20, LUNA was worthless, and roughly[$40 billion](https://www.sec.gov/newsroom/press-releases/2024-73) in market value had been destroyed. No reserve audit would have helped because there was nothing external to audit.


The TerraUSD incident rules out algorithmic stablecoins for payments, as well as the crypto- and commodity-backed designs that preceded them.


## Which Stablecoin Matters Most for Payments?


Fiat-backed stablecoins are the practical default for payment and settlement.


**Type** **Backing model** **Peg mechanism** **Primary risk** **Payment fitness**


**Fiat-backed** Cash / T-Bills (off-chain) Direct issuer redemption at $1 Issuer insolvency, bank counterparty risk **Highest**


**Crypto-backed** On-chain crypto (overcollateralized) Automated liquidation Collateral crash, oracle failure, smart contract bugs Low


**Commodity-backed** Physical commodity (custodied) Tracks commodity spot price Not pegged to $1; low liquidity None


**Algorithmic** None Supply-adjustment algorithm Death spiral (self-reinforcing, irreversible) Disqualified


Under the GENIUS Act, only stablecoins issued by a Permitted Payment Stablecoin Issuer can be used as settlement assets in covered wholesale payment infrastructure.


Picking the right token is only half the work because your payment infrastructure also needs an internal ledger, such as[Formance Ledger](https://www.formance.com/modules/ledger) , to record issuance, redemptions, transfers, and reserve positions alongside the on-chain contract and your payment providers.


## How Ledgers Support Stablecoin-based Payments


A core ledger system lies beneath every stablecoin payment flow because on‑chain contracts only track token ownership. The ledger mirrors on‑chain token balances and off‑chain bank or custody accounts, records issuances, redemptions, internal transfers, and reserve positions, and gives finance and compliance teams evidence that liabilities (tokens in circulation) match assets (reserves held).


The GENIUS Act and MiCA both impose reserve and reporting obligations, including circulation tracking, that make ledger tracking an operational requirement. Formance Ledger is an open-source, real-time, programmable accounting database for financial transactions. It records fiat and digital assets using a double-entry model, so auditors can verify every mint, burn, transfer, and reserve movement, and compliance teams can query them.


To make the requirement concrete, here's what a daily reserve attestation actually looks like inside the ledger.


### A daily reserve attestation in practice


Suppose your platform has issued 10,000,000 USDC-denominated tokens across Ethereum and Solana. The internal ledger needs to prove, at the close of every business day, that reserves held match tokens outstanding.


On the liability side, the ledger records:


- @tokens:ethereum:issued: 7,200,000 USDC
- @tokens:solana:issued: 2,800,000 USDC
- **Total token liabilities:** 10,000,000 USDC


These two issuance accounts roll up to the total liability the reserves need to cover.


On the asset side, the ledger records:


- @reserves:treasury:t-bills: 8,000,000 USDC (short-dated Treasuries)
- @reserves:bank:primary: 2,000,000 USDC (cash at custodian bank)
- **Total reserve assets:** 10,000,000 USD


[Reconciliation](https://www.formance.com/modules/reconciliation) compares the liability and asset positions against external bank and custody data and reports zero drift. Now suppose a $50,000 mint posts on-chain before the matching fiat deposit clears. The ledger still balances because double-entry forces it to, so the new tokens raise liabilities to 10,050,000, and the offsetting $50,000 lands in a pending account rather than a confirmed reserve.


```text
send   [USDC 50000]   (
source   =   @world
destination   =   @tokens:ethereum:issued
)


send   [USD 50000]   (
source   =   @world
destination   =   @reserves:pending:incoming
)
```


- @reserves:pending:incoming: 50,000 USD


Confirmed reserves stay at 10,000,000 while total assets, confirmed plus pending, move to 10,050,000 and match liabilities. Reconciliation flags the pending leg as unsettled until the fiat clears and the $50,000 moves from @reserves:pending:incoming into @reserves:bank:primary. The ledger models the in-between state instead of pretending the mint is fully backed.


The result is a reserve attestation finance and compliance can sign at the close of any business day, including the ones where settlement runs late.


## Scaling Ledgers with Stablecoins


Ledger requirements grow quickly, and reconciliation load compounds as you add more stablecoins. Multi-chain supply, timing differences between on-chain mints and fiat settlement, and the lack of protocol-level reversals all compound the reconciliation workload.


In practice, choosing a token and choosing a ledger are tightly coupled decisions. For payments, fiat‑backed stablecoins are the most practical default, and Formance Ledger can act as the programmable core needed to keep their issuance, reserves, and corrections in sync at scale.


With a purpose‑built ledger under your stablecoin flows, you can add new tokens, chains, and reserve structures without re‑architecting every time markets or regulations change.


[Clone the Formance Ledger on GitHub](https://github.com/formancehq/ledger) , run it locally, and prototype your first stablecoin issuance and reserve‑attestation transaction.
