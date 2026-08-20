---
schema_version: "1.0.0"
document_id: "cd2c04a70badf81d495841c62a15a88c6e8cdf796c5676c33cdaf90b16317fd8"
company_key: "yc-nucypher-aka-threshold-network"
company: "NuCypher (aka Threshold Network)"
source_id: "yc-nucypher-aka-threshold-network-news-import-0327ade41b3a"
canonical_url: "https://www.threshold.network/blog/abra-shifts-from-wbtc-to-tbtc-for-bitcoin-backed-lending-platform"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-25T17:17:15.313542+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:11384f4374dc4800405aa678f282552429b54f5172252cd25170bb82a65c2b3b"
---

# Abra Shifts from WBTC to tBTC for Its Bitcoin-Backed Lending Platform

[Abra](https://www.abra.com/?utm_source=threshold&utm_medium=referral&utm_campaign=tBTCannouncement) , a digital asset wealth and treasury management platform, **shifts to tBTC** as the main onchain Bitcoin collateral for lending. Abra provides high-net-worth individuals and institutions with a competitive edge in trading, investing, and collateralized borrowing, while maintaining full control through segregated account infrastructure. The transition is consistent with a broader re-evaluation of the design of wrapped Bitcoin among platforms with fiduciary obligations to their clients.


### A Shift in DeFi


The maturation of DeFi has brought with it a more rigorous view of tokenized asset security and infrastructure. For platforms underwriting collateralized credit, the choice of Bitcoin wrapper is embedded in every pricing assumption, every risk review, and every audit. The common questions that usually arise during these assessments are:


1. Does the wrapper's security model minimize risk?
2. What does the cost stack look like across mint, redemption, and routing?
3. How quickly can the wrapper move at scale, and with what operational lift?
4. How visible is the underlying collateral to a risk committee or external auditor?


Across those four axes,[tBTC was the clear answer.](https://app.threshold.network/)


### Why Underwriters are now Looking at tBTC


Since January 2024, WBTC has seen a steady 20-30% decline in its BTC holdings, while[tBTC has grown organically by 281%](https://www.threshold.network/blog/threshold-q1-2026-benchmark-report) without any major corporate entity driving it. By the end of 2025, that organic pull had carried tBTC to $800M in DeFi TVL across 26 integrations, roughly $1M of liquidity per integration, the broadest and deepest footprint of any decentralized Bitcoin wrapper. So the question worth asking is, *what are these integrators responding to?* The list usually consists of the following:


Liquidity Comparison Across Bitcoin Wrappers | Threshold Network


1. **The growing importance of trust assumptions.** With most wrappers, you're trusting a custodian to hold the underlying Bitcoin. tBTC replaces that assumption with mathematical probability through threshold cryptography, where a randomly selected set of signers holds the BTC via a cryptographic node system, so no single party can access those funds. Redemption back to native Bitcoin is also permissionless: no centralized authority gating issuance, no off-chain handoffs to coordinate.
2. **Lower cost across the lifecycle.** Mint and redemption are action-based at 20 basis points, with rebates and waivers available to T-staking holders that can partially or fully offset those fees. There is no idle-holding charge. For a lending platform processing client BTC at scale, the embedded cost of moving collateral is materially lower than the legacy wrapper stack.
3. **Operationally faster.** Minting and redemption run at chain speed instead of custodian speed, and the Threshold Unified Bitcoin Router shortens the path from client BTC to deployable tBTC. For a lending platform moving collateral at scale, that's the difference between an integration and an ongoing operational relationship.
4. **Fuller visibility for risk and audit.** Because every locked UTXO, mint, and redemption is visible on both Bitcoin L1 and Ethereum, risk committees and auditors can verify the backing in real time rather than waiting on periodic attestations. For the people whose job is to sign off on collateral, this adds an extra layer of convenience when verifying asset allocations.


### The Perfect Match: Abra x tBTC


To start, Abra is a platform that lets users borrow against their Bitcoin and Ethereum without selling their core assets, with up to 50% LTV, no minimums, no credit checks, no monthly payments, and interest accruing on the principal and due only at repayment, typically at 4–8% APR. The loan stays open as long as the LTV holds. It's a lending business, which means clients hand over collateral and trust Abra to hold it safely until they repay.


Which is exactly why the wrapper underneath the Bitcoin isn't a back-office detail for Abra; it's an extension of the promise Abra makes to its users. If the collateral asset depends on an upstream custodian, then *Abra's trust guarantee is only as* *strong as the wrapper's custodian,* no matter how sound Abra's own operations are. A lending platform built on trust can't quietly outsource that trust to a third party its clients never agreed to.


**That's what makes tBTC the natural fit.** The four properties a fiduciary lender actually needs from its collateral are addressed by tBTC: security, cost, operational tempo, and verifiability. And for the user, none of this adds friction. The borrowing experience is unchanged:


tBTC-Collateralized Lending Flow on Abra | Threshold Network


Same terms still apply to Abra's existing offers on BTC and ETH: USD or USDC, open term, no monthly payments, no credit checks, up to 50% LTV. The only thing that's changed is that the trust Abra extends to its clients is now backed by something its clients can verify for themselves.


### The Growing Pattern


Abra isn't an outlier in this regard; it's one data point in a migration that's becoming hard to miss. Around 2025, Aave's tBTC supply climbed from roughly 700 BTC to 1,800 BTC. In March 2026, **tBTC surpassed 50,000 BTC in cumulative processed volume (about $5.2 billion)** and hit an all-time high TVL of 6,620 BTC during a stretch when DeFi liquidity was *contracting* .


That last detail is the tell: platforms don't add exposure into a tightening market for ideological reasons. They do it for fiduciary ones. Trust-minimized Bitcoin collateral has simply become the easier position to defend inside a risk committee. Which reframes the question for everyone still running core financial infrastructure on custodian-wrapped BTC:


> It's no longer a question of whether the trust model is acceptable today. It's whether that position survives the next risk review to ***manage the next trillion-dollar market, and tBTC is proving to be the strongest contender in the field.***


##### About Abra


[‍ Abra](https://www.abra.com/?utm_source=threshold&utm_medium=referral&utm_campaign=tBTCannouncement) is a leading crypto-native products and wealth management platform, offering institutions and individual investors a suite of digital asset services including custody, trading, yield, lending, and advisory services through its SEC-registered investment advisor. Abra was Founded in 2014 with headquarters in Palo Alto, California. ****
