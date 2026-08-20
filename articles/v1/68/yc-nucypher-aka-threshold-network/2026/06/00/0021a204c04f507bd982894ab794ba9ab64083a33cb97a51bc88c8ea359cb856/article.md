---
schema_version: "1.0.0"
document_id: "0021a204c04f507bd982894ab794ba9ab64083a33cb97a51bc88c8ea359cb856"
company_key: "yc-nucypher-aka-threshold-network"
company: "NuCypher (aka Threshold Network)"
source_id: "yc-nucypher-aka-threshold-network-news-import-0327ade41b3a"
canonical_url: "https://www.threshold.network/blog/case-study-on-btcfis-capital-structure-wrapped-bitcoin-vs-bitcoin-layer-2s"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T17:17:15.313542+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:f0f1cc0ad5da3b3cdd81688e8600dea8dad58d1de615364a7134b6d512b5b8b1"
---

# Case Study on BTCFi's Capital Structure: Bitcoin Layer 2 vs Wrapped Bitcoin

There are two ways to put Bitcoin to work in decentralized finance: native execution environments built on the Bitcoin base layer and[tokenized representations of Bitcoin](https://www.threshold.network/blog/what-is-a-wrapped-bitcoin-how-threshold-network-brings-bitcoin-onchain) that run onchain and application infrastructures. Understanding the structural drivers behind that outcome, rather than treating it as a temporary market mispricing, is the primary purpose of this analysis.


### Market Composition


#### Tokenized Bitcoin and Bitcoin L2s


**Wrapped Bitcoin** *(like cbBTC, wBTC, tBTC...)* **holds $12.87B in TVL, against $223.5M across all Bitcoin L2 DeFi combined** . Market cap counts every coin in circulation, whether or not it's deployed. So we're measuring Bitcoin L2s by their best number and Wrapped Bitcoin still leads by a wide margin. On a like-for-like basis, the gap in usable liquidity is still relatively wide.


The tokenized (or wrapped) Bitcoin market is composed of WBTC which holds $7.16B in market cap, representing approximately 116,045 BTC in custodied reserves, operating under an institutional co-custody structure established in 2019. cbBTC has $5.40B since its 2024 launch, held in Coinbase institutional custody with Chainlink Proof of Reserve attestation.


**However, tBTC the only decentralized tokenized Bitcoin onchain, showcases promising momentum with a 281% increase in TVL in 2025.** The key architectural distinction lies in the custody model: tBTC uses a[threshold ECDSA scheme across rotating node operators set](https://docs.threshold.network/tbtc-v2) , where minting and redemption are executed permissionless on-chain without reliance on any single entity to authorize movement of underlying Bitcoin. This structural difference, is becoming increasingly important for custody conscious users.


Bitcoin Layer2 or BRC-20 Side Chain TVL | Image by Threshold Network


### Structural Drivers of Wrapped Bitcoin


The margin by which wrapped Bitcoin has outperformed Bitcoin-native execution environments is not primarily explained by first-mover positioning or distribution advantages. It reflects two compounding structural properties that emerged from Ethereum's application layer maturity.


**Integration cost asymmetry.** WBTC launched as a standard ERC-20 in January 2019. Every lending protocol and DEX that was subsequently deployed on Ethereum natively supported it, not through bilateral integration agreements, but because ERC-20 conformance carries zero marginal integration cost. When cbBTC launched years later, Aave v3 and Uniswap v3 were available on day one by the same mechanism. A Bitcoin L2 has to build every protocol relationship from the ground up. Rootstock offers a useful test case: live since 2018, it has had more runway than most, yet its protocol depth still trails that of a single Ethereum rollup such as Base or Arbitrum: both of which launched years later. The pattern points to the role of the ERC-20 standard, whose network effects appear to compound over time in ways that protocol-by-protocol bootstrapping has struggled to match.


**Liquidity depth as collateral credibility.** Risk frameworks at lending protocols and institutional desks are calibrated against position liquidity at depth. Aave's WBTC market supports nine- figure collateral positions; the entire Stacks DeFi ecosystem is smaller than many individual Aave pools. Capital requiring Bitcoin-denominated collateral or BTC-scale position execution has no functionally equivalent venue on any Bitcoin-native L2. The feedback mechanism is self-reinforcing: liquidity depth attracts further capital deployment, each new integration widens the gap, and the marginal incentive to deploy on a Bitcoin L2 declines relative to the existing wrapped BTC market.


Key Timelines for Wrapped Bitcoin and Bitcoin Layer 2 | Image by Threshold Network


### The Custody Gradient


The events of August 2024 provided a direct data point on how protocol risk frameworks respond to structural changes in wrapped Bitcoin custody arrangements. Following a restructuring of **WBTC** 's co-custody model, MakerDAO moved to restrict the use of WBTC collateral and initiated a phased reduction of its WBTC exposure. The episode established that custodial governance changes are evaluated and acted upon by downstream protocols at speed. WBTC's $7.16B market position did not prevent institutional reallocation when the counterparty profile was perceived to have shifted.


**cbBTC** sits in the regulated custodial tier, backed by Coinbase's institutional custody infrastructure, and currently benefits from its issuer's regulatory standing and operational continuity. That standing is a meaningful risk differentiator relative to WBTC's current co-custody structure, but it remains a property of the issuer rather than the token's architecture.


**tBTC** occupies a structurally sound position. Its market share is marginal relative to the custodial incumbents; however, its growth is mostly organic, with no large entities inflating its TVL. The result is a base of real users and supporters evident by a notably stable TVL that suggests genuine demand for its decentralized nature. The growth in demand since 2024 *(of 281% increase in TVL)* mirrors the broader rise in interest in a trust-minimized, decentralized Bitcoin wrapper.


Structurally, tBTC is the only protocol in the tokenized BTC market that is the most decentralized, and in which no single entity controls the underlying Bitcoin. As the custodial tier of this market continues to consolidate...


> tBTC's structural guarantees position it as the natural destination for capital that prioritizes trust-minimized BTC at scale.


Ultimately, Bitcoin’s continued demand from users and institutions reinforces its position as the **most secure and trusted digital asset** . Amid the market cycles, DeFi restructurings, and downturns that continually shakes the sector, Bitcoin stands apart as the preferred digital asset, anchored by its pristine nature, benchmark durability, scarcity, and the long-term conviction it sustains.
