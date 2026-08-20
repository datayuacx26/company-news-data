---
schema_version: "1.0.0"
document_id: "bdde3ef2f39a7f0a0120803d989709616dc733975d1ab7c67e45e055476addb9"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-to-secure-solanas-core-infrastructure"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:3a2a0f9dd6e6f6a78908e234599f683ae4c71e0c22d94ba2fdd52883187a4879"
---

# Quantstamp to Secure Solana’s Core Infrastructure

Quantstamp recently began a security engagement with Solana, a scalable Layer 1 blockchain that can support up to 50k transactions per second and 400ms block times, without shards or roll-ups. As a first step, Quantstamp will review the on-chain portion of the Stake Pools, a liquid staking solution that aims to further decentralize the network by spreading staked SOL across an increased number of validators.


Solana is relatively new, having only launched its beta mainnet in March 2020. However, aiming to solve the frequently-referenced trilemma of achieving decentralization, security, and scalability, Solana has caught the attention of both users and developers. Solana uses Rust, rather than Solidity, to write smart contracts executed on the blockchain’s Low-Level Virtual Machine (LVM). Unlike Ethereum smart contracts, Solana smart contracts are stateless or read-only.


Solana also has a completely different system architecture. Ultimately, its impressive speed comes from the fact that transactions are verifiable without all the nodes agreeing at the same time: one of Solana’s key innovations is Proof of History (PoH). In most cases, blockchains require validation nodes to communicate with each other to determine block order. PoH uses a recursive verifiable delay function to hash incoming events. Validation nodes use information encoded in the ledger itself to determine whether a transaction is valid or invalid without having to wait for other validation nodes. Other features include Tower BFT, a PoH-optimized version of Practical Byzantine Fault Tolerance (PBFT), and Gulf Stream, a transaction-forwarding protocol without a MemPool. However, PoH is the core innovation that differentiates Solana from other traditional blockchains.


Lacking the composability of Ethereum's "money legos," DeFi’s biggest value proposition, could be seen as a serious drawback of the Solana ecosystem. However, its open infrastructure could also make it easier for some developers to navigate without the added layer of complication due to sharding and scalability solutions. What’s more, there are already plans in development for an EVM compiler to convert Solidity to Rust, which would unlock the potential for Solidity smart contracts to be coded on Solana.


‍


### **Staking in Solana**


SOL token holders can earn rewards by staking tokens to validators that process transactions and run the network.


Previously, when staking SOL, users did not receive a token (representing their staked SOL) that they could use elsewhere in DeFi. Recently, liquid staking protocol Lido Finance[expanded to Solana](https://blog.lido.fi/introducing-liquid-staking-on-solana/) and announced the launch of SOL staking. The stSOL token represents fractional ownership in the staking pool and allows users to earn rewards and contribute to the chain’s security while still maintaining liquidity. These stSOL tokens can even be used elsewhere—for example, to earn additional yield, or as collateral in lending protocols—without losing out on staking rewards. With Lido Finance currently staking more than 1% of all ETH, it will be interesting to see how much traction the protocol can gain on Solana.


The Stake Pool Program was enabled via an on-chain governance process and aims to offer users the same flexibility unlocked by Lido Finance. Before this program, SOL holders delegated their stake directly to validators. In an effort to increase the censorship resistance of Solana, the Stake Pool Program allows SOL holders to delegate their stake to a pool manager who is in charge of deciding how to delegate SOL over multiple validators. This allows SOL holders to stake and earn rewards without having to manage their stake.


‍


### **A Growing Ecosystem**


The Solana ecosystem has seen many other projects emerge, most notably[Serum](https://projectserum.com/#/) , a non-custodial DEX running via an on-chain central limit order book (CLOB), and[Metaplex](https://www.metaplex.com/) , an NFT platform that promises independent creators the ability to self-host their own NFT storefront.[Solstarter](https://projectserum.com/#/) , described as the first IDO platform for Solana, lets projects raise liquidity and launch tokens more fairly. Over the past year, there has been a significant uptick in investment and projects moving over to the network. As of August 2021, there were[over 400 projects in the ecosystem](https://solana.com/news/solana-ecosystem-news) , ranging from decentralized exchanges and lending protocols to digital games and NFT platforms.


Currently, the TVL of Solana's DeFi network is[just over $8B](https://defillama.com/chain/Solana) . While this may be a modest number compared to Ethereum, the promise of faster transaction speeds and lower costs could certainly help fuel future growth. Considering the explosion in the popularity of digital art, Solana may become increasingly attractive to investors and users within the NFT space. And, new developments such as[Wormhole](https://defillama.com/chain/Solana) —a bridge connecting Solana with other ecosystems such as Ethereum and Terra—may unleash even more potential for the blockchain’s emerging DeFi landscape.


Quantstamp is pleased to secure the assets in your digital nation and is proud to audit innovative Layer 1 blockchains like Solana.
