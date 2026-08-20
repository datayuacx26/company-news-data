---
schema_version: "1.0.0"
document_id: "ce24b99f30e38fefb8aa0b5cbfdc20a5e70eb80bea4ae544051ece407da634ea"
company_key: "yc-nucypher-aka-threshold-network"
company: "NuCypher (aka Threshold Network)"
source_id: "yc-nucypher-aka-threshold-network-news-import-0327ade41b3a"
canonical_url: "https://www.threshold.network/blog/verifiable-bitcoin-accounts"
published_at: "2026-04-23T00:00:00+00:00"
first_seen_at: "2026-07-25T17:17:15.313542+00:00"
fetched_at: "2026-07-28T22:15:40.956663+00:00"
content_hash: "sha256:0bee9d71000a1c3a7736446046dc9e8273e2632d0361d746ee830bffcf3cb970"
---

# Verifiable Bitcoin Accounts: Bitcoin for Institutions. Your Custody, Your Terms.

### Unlocking A Trillion Dollar Market


Bitcoin-backed lending reached $73.6 billion in 2025 and is projected to exceed $90 billion by the end of 2026. The yield on that lending comes primarily from stablecoin liquidity, a market that hit $308 billion in early 2026 and is on track to exceed $1 trillion. Institutional Bitcoin holders are sitting at the intersection of both trends.


‍
Ernst & Young, in a[January 2025 survey conducted with Coinbase](https://www.ey.com/content/dam/ey-unified-site/ey-com/en-us/insights/financial-services/documents/ey-growing-enthusiasm-propels-digital-assets-into-the-mainstream.pdf) , polled 352 institutional decision makers (each overseeing $1B+ in AUM) about their primary barriers to deeper crypto integration. The top three responses: *uncertain regulatory environment (52%), volatility (47%), and security of asset custody (33%)* . Among the subset that has no plans to engage with DeFi specifically, the barriers shift: *regulatory issues (57%), compliance risks (55%), and lack of internal knowledge (51%)* lead the list.


The constraint has never been in demand. It has been a deployment model that works within the custody arrangements, legal frameworks, and risk controls institutions already have in place. They are waiting for a deployment model that does not require them to abandon the custody relationships, legal frameworks, and risk controls they already have in place.


**Verifiable Bitcoin Accounts addresses this gap, unlocking a Trillion Dollar Market.**


### Introducing Verifiable Bitcoin Accounts


[Verifiable Bitcoin Accounts (VBA)](https://www.threshold.network/institutional) **** is **** a new framework for institutional Bitcoin deployment. Built on the same signer infrastructure that has operated with Bitcoin for 6 years, has processed over $5 billion in cumulative volume, and has sustained zero losses: ****[Threshold Network](https://www.threshold.network/) **.**


The premise is straightforward: allocators should be able to access onchain lending and yield markets without relinquishing custody of their Bitcoin to a new party. With VBA, they can.


BTC remains with the holder's existing custody arrangement. Capital is held in a segregated account, not pooled, and is verifiable through Bitcoin UTXO at all times. Spending conditions, multi-party controls, and recovery paths are written in Bitcoin Script and enforced on the Bitcoin Network. Every movement of capital executes a path that was agreed upon at setup and triggered by predefined conditions - *no party, not the custodian, not Threshold, not the depositor,* has unilateral access over deployed capital during the term of agreement.


### Your Custody, Your Terms


One of the most important design decisions behind **Verifiable Bitcoin Accounts (VBA)** is that it meets the current standard and process that institutions are accustomed to. There is no need for a new custodial counterparty. The custody relationship that the allocator already maintains governs every deployed position.


[Verifiable Bitcoin Accounts (VBA)](https://threshold.network/institutional) are also compatible with Qualified Custodians such as Anchorage and Fireblocks Trust, MPC-based custody networks, and self-custody setups. For institutions that have spent months or years establishing their custody arrangements, completing due diligence, satisfying compliance requirements, and negotiating insurance coverage, this matters. The cost of switching custodians is not just operational. It touches audit, insurance, regulatory reporting, and board-level risk approvals.


**VBA removes that entire category of friction. Your Bitcoin stays where it is. The deployment framework layers on top.**


Verifiable Bitcoin Accounts: Lifecycle by Threshold Network


### Built for How Onchain Lending Actually Works


Onchain Bitcoin lending and yield markets operate on a specific mechanical reality: collateral must be settled reliably when a position is liquidated. This is not a feature request. It is the baseline condition for any collateral type to be usable at scale.


**Verifiable Bitcoin Accounts are designed around this operational requirement.** Threshold signers independently monitor onchain activity across approved venues. When a position is liquidated, collateral migrates along a predefined settlement path encoded in Bitcoin Script, no external oracle, no off-chain dependency.


Verifiable Bitcoin Accounts: Onboarding Process


### Partially Signed Bitcoin Transaction (PSBT)


Every Verifiable Bitcoin Account is built on top of Bitcoin's native PSBT, the Partially Signed Bitcoin Transaction Standard (BIP-174). PSBTs allow a transaction to be constructed, inspected, and incrementally signed by multiple independent parties before it is broadcast. This is the coordination primitive that enables multi-party control *without requiring any party to hold a complete signing key or trust another participant to act honestly.*


The spending conditions governing each VBA are not enforced by application-layer software, a smart contract on another chain, or a policy engine maintained by Threshold. They are written directly in Bitcoin Script and enforced by Bitcoin's consensus mechanism.


### Verified, Not Just Trusted


Institutional adoption of Bitcoin in onchain markets does not scale on assurance alone. It scales on independent verification. Every condition governing a Verifiable Bitcoin Account can be audited by inspecting the Bitcoin Script attached to the relevant UTXOs. The monitoring logic is deterministic. The migration paths are predefined. The recovery mechanism is enforced by the Bitcoin consensus.


> "Institutions don't need additional layers of trust; they need systems where outcomes are defined, enforceable, and verifiable from the outset. By removing reliance on counterparties, we align Bitcoin onchain with the standards institutional capital actually requires." ***— MacLane Wilkison, Co-Founder of Threshold Network***


Verifiable Bitcoin Accounts establish a new standard for institutional Bitcoin deployment: every component of the architecture can be verified before a single satoshi is committed.
