---
schema_version: "1.0.0"
document_id: "fef4761eecb88e072b7bb28f142dffef934335f50822346e989fb5ceb71d105f"
company_key: "yc-ultra"
company: "Ultra"
source_id: "yc-ultra-rss-2323deaa6929"
canonical_url: "https://ultra.io/introducing-the-ultra-to-ethereum-bridge/"
published_at: "2026-01-30T11:39:39+00:00"
first_seen_at: "2026-07-27T12:33:47.447961+00:00"
fetched_at: "2026-07-28T22:22:54.577865+00:00"
content_hash: "sha256:426ae4b436f40bb981e9f8b04805384954db148366711991636a560ee21b8aa0"
---

# Introducing the Ultra ↔ Ethereum Bridge

Today Ultra releases a dedicated bridge interface that enables users to move assets between Ultra’s Layer-1 and Ethereum.


The bridge is designed as infrastructure to be simple, transparent, and centered on execution. Ultra provides and maintains an interface, while the underlying bridge protocol is operated by independent validators.


**How it works**


The Ultra Bridge enables cross-chain token transfers through smart contracts deployed on both Ultra and Ethereum. Users connect their wallet, select the asset and destination chain, approve the transaction, and track its progress directly in the interface.


Throughout the process, users retain full control of their assets and can monitor each step as it is confirmed on-chain.


**Phased support**


At launch, the bridge enables transfers between Ethereum and Ultra for **UOS only.**


After a brief monitoring window we will begin adding support for other ERC-20 tokens like USDT and WETH.


Support for additional Ultra-native tokens may be added as projects onboard.


Future phases will expand support to other EVM networks such as Base and BNB Chain, along with more automated token configuration and deeper integration with Ultra’s EVM environment.


**Fees and execution time**


Ultra does not charge a bridge fee.


Users only pay standard gas fees on the non-Ultra network, which are shown before confirming a transaction.


Typical execution times:


- Ethereum → Ultra: ~13 minutes (Ethereum finality dependent)
- Ultra → EVM: ~40 seconds to 1.5 minutes


There are no liquidity provider or validator fees.


**Security and risk considerations**


The bridge uses a multi-signature validator setup, requiring multiple independent approvals for administrative actions. Manual intervention may be required by the validator set in exceptional cases, allowing issues to be resolved without automated failures cascading.


As with any cross-chain system, bridging involves smart contract and network risk. The interface and protocol are provided as is, and users should assess their own risk tolerance before using it.


**Try it!**


Ultra continues to focus on building reliable, low-friction infrastructure.


The bridge is another crucial step in making Ultra interoperable and more connected to the wider ecosystem.


👉 Tutorial & documentation:[https://developers.ultra.io/tutorials/ultra-bridge](https://developers.ultra.io/tutorials/ultra-bridge) /


👉 Access the bridge:[https://bridge.ultra.io/](https://bridge.ultra.io/)


– The Ultra Team
