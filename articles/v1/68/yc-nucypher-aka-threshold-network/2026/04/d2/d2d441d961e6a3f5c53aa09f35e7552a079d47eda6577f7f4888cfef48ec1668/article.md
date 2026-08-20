---
schema_version: "1.0.0"
document_id: "d2d441d961e6a3f5c53aa09f35e7552a079d47eda6577f7f4888cfef48ec1668"
company_key: "yc-nucypher-aka-threshold-network"
company: "NuCypher (aka Threshold Network)"
source_id: "yc-nucypher-aka-threshold-network-news-import-0327ade41b3a"
canonical_url: "https://www.threshold.network/blog/tbtc-mint-fee-reinstatement"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-25T17:17:15.313542+00:00"
fetched_at: "2026-07-28T22:15:53.113233+00:00"
content_hash: "sha256:a39e0b82cbd2f4105136a84dd25371f03a557d240469c937c249f36c0157e839"
---

# tBTC Mint Fee Reinstatement Goes Live April 15, 2026

### Background


Threshold DAO has adjusted tBTC bridge fees through multiple governance decisions as the protocol matured.


- Launch (tBTC v2): 20 bps mint fee and 20 bps redemption fee.
- [TIP-054](https://threshold.network/) : Mint fee reduced to 10 bps; redemption fee unchanged.
- [TIP-065](https://threshold.network/) : Mint and redemption fees reduced to 0%, with the explicit goals of accelerating adoption, growing TVL, and supporting integration across DeFi.
- [TIP-075](https://threshold.network/) : Partial end to the fee holiday: 20 bps redemption fee restored; 0% mint fee retained.
- [TIP-093](https://threshold.network/) : Rejected proposed fee increases and introduced a mandatory 30-day delay on future tBTC fee changes.


The fee holiday achieved its objectives: higher tBTC usage, broader integrations, and improved liquidity. Fee waivers for T stakers have supported the peg and addressed the persistent discount often associated with a 20 bps redemption fee for non-stakers. Because those waivers also apply to mint fees, the DAO sees limited risk that restoring a 20 bps mint fee will materially harm the peg. As the protocol continues to mature, moving from a zero mint fee to a sustainable, governance-aligned structure is appropriate.


tBTC Mint Fee notice, activated April 15, 2025


### Proposal Details (TIP 109)


[TIP 109](https://forum.threshold.network/t/tip-109-reintroduce-a-tbtc-mint-fee/1171) , as approved by the DAO, does the following:


- **Mint fee** The tBTC mint fee is set to 20 bps (0.20%) in line with governance approval. The redemption fee is unchanged, consistent with TIP-075.
- **Committee discretion**
The Threshold Committee may lower the mint fee if needed. Increases still require standard governance.
- **Clarity for the market**
Reinstating the mint fee in one step gives integrators and liquidity providers a clear, predictable fee regime aligned with historically approved parameters.
- **Operational flexibility**
Committee authority to reduce the mint fee allows a fast correction if 20 bps creates unintended effects or disproportionate pressure on bridge volume, while increases remain governance-gated.


### Rationale


The fee holiday (TIP-065, with partial extension via TIP-075) supported growth during a critical expansion phase. Restoring the mint fee supports long-term sustainability by contributing to operations, maintenance, and development. A single, well-communicated change reduces prolonged uncertainty.


Reinstating the mint fee also aligns with Threshold’s incentive design. With staking-based rebates live (including related work such as TIP-106), participants who commit to the network can offset or reduce the effective cost of minting. Staker fee waivers help avoid peg pressure from bridge fees, consistent with experience since T staking gained traction in early 2026.


### Effective Date and Impact


Following approval of TIP 109, the 20 bps tBTC mint fee is scheduled to reactivate on April 15, 2026, subject to the conditions below.


#### Rebates and Integrations


Some tBTC-based projects and integrations must route rebates to mints or redemptions, depending on the product structure. That is most relevant where gas, settlement, or other protocol-level mechanics determine how fees are paid. Thesis, among others, requested this flexibility for Mezo and similar uses.


End users of the public dapp do not have control to toggle rebate allocation between mint and redeem; that capability is available only to qualified integrations built on tBTC.


#### What dApp users will see


Explorer and My Activity will continue to show rebates earned for staking T. Because rebates can apply to both mint and redeem fees, they will appear on both transaction types, in the same spirit as redemption fee rebates have been displayed to date.


Threshold tBTC mint fee reinstatement Summary
