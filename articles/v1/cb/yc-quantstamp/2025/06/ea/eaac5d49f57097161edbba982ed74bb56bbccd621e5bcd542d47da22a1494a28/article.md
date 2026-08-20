---
schema_version: "1.0.0"
document_id: "eaac5d49f57097161edbba982ed74bb56bbccd621e5bcd542d47da22a1494a28"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/meet-the-l2s-in-the-race-to-scale-ethereum"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:b5c80f1553948a1ff17dc39e54e63e07d2020288f294482895a7838c0e95c5dd"
---

# Meet the L2s in the Race to Scale Ethereum

### Why Ethereum Scaling is Needed


Since its launch in 2015, Ethereum has gained significant traction. However, scalability has been a consistent issue and a problem that must be tackled for the network to reach its full potential. Ethereum has a big vision: to be scalable, secure, and decentralized—but meeting all three requirements is known as the scalability trilemma.


*The scalability trilemma, first referenced by*[Vitalik Buterin](https://vitalik.ca/) *.*


While Ethereum is considered very secure, this security comes at a cost: the main reason behind Ethereum’s scalability issues is that each node in the network has to process each transaction. And, as the number of people using Ethereum has grown, its capacity has been tested.


Some rapidly growing sectors, such as NFTs or gaming, have been held back due to costs and transaction times. Many users have been priced out of using decentralized exchanges or yield farms due to high gas fees. And while scaling solutions such as sharding will help when Ethereum moves from Proof of Work (PoW) to Proof of Stake (PoS), this is a complex upgrade that may take a while to happen. Enter Layer 2 scaling solutions.


### Scaling Solutions


Scaling solutions can be divided into two categories: on-chain and off-chain scaling. Sharding is an on-chain scaling solution, meaning it involves changes to the protocol layer itself. On the other hand, L2s are scaling solutions that handle transactions off-chain without losing the security and decentralization of the mainnet. And, with the demand for faster and cheaper transactions growing exponentially, the Layer 2 landscape is definitely heating up.


Rollups perform transaction execution outside of Layer 1, then post data to Layer 1 where consensus is reached. Optimistic rollups, used by Optimism and Arbitrum, and ZK Rollups (dYdX, DeversiFi, ImmutableX) have different security models, but both aim to reduce transaction costs and increase throughput.


### Optimistic Rollups


Optimistic Rollups don't do any computation by default, but assume transactions are valid and rely on fraud proofs to dispute any fraudulent transactions. Unfortunately, this comes with the tradeoff of longer wait times during the period where a transaction could potentially be challenged.


In July, Arbitrum made headlines when[Reddit announced plans](https://www.theblockcrypto.com/post/112250/reddit-is-scaling-its-two-ethereum-based-tokens-using-layer-2-solution-arbitrum) to scale its two community tokens using Arbitrum One by Offchain Labs. Since the launch of their mainnet in August, Arbitrum has gained significant traction, recently rising to dominate the Layer 2 landscape at more than[60% market share](https://l2beat.com/) . Only 42 days after the launch of Arbitrum One, OffChain Labs then announced Arbitrum Nitro, the next iteration of Arbitrum which promised faster speeds and cheaper transactions. This upgrade could bolster even more growth.


The idea of a “Layer 2 Summer” seemed likely in July this year with Uniswap V3’s deployment onto Optimism. Optimism took a different approach by initially whitelisting a few projects including Uniswap and Synthetix, a decentralized synthetic asset issuance protocol. While Uniswap V3 was highly anticipated and Synthetix has gained a substantial user base, Optimism hasn’t been able to capture even close to the same market share as Arbitrum.


Like Arbitrum, Optimism also uses Optimistic Rollups; however, they differ in respect to fraud-proof logic. While Optimism limits smart contract sizes to what can be re-executed on Ethereum, Arbitrum resolves disputes interactively and minimizes the amount of on-chain computation necessary to resolve them. One of the biggest drawbacks of Optimistic Rollups is the period during which a transaction can be challenged. For the user, this translates into a 7-day wait when bridging tokens back to the mainnet. However, this issue is being mitigated through numerous bridging solutions that let users transfer funds back and forth almost immediately.


Recently, the Optimism team made[the bold claim](https://medium.com/ethereum-optimism/the-future-of-optimistic-ethereum-7f22d987331) that Ethereum’s native protocols will be able to launch on Optimism with “a single click” by the end of October. If any existing Ethereum mainnet protocol could decide to seamlessly deploy to Optimism, this could make them a formidable contender in the race for market share.


### ZK Rollups & Validium


Another type of scaling solution is Zero-Knowledge (ZK) Rollups, which improve upon some of the pain points that exist with Optimistic Rollups. ZK Rollups work by bundling transactions off-chain and generating a cryptographic proof, referred to as a SNARK (succinct non-interactive argument of knowledge). Since only the proof is required—rather than all the transaction data—validating a block is faster and less expensive. The biggest benefit of ZK Rollups is that they allow for much faster withdrawals than Optimistic Rollups— since the validity proofs are already verified, no challenge period is required.


StarkEx is already being used to help scale transactions on several Ethereum dApps, including[dYdX](https://dydx.exchange/) ,[DeversiFi](https://dydx.exchange/) , and[ImmutableX](https://www.deversifi.com/) . StarkEx can be deployed in either ZK-Rollup mode (publishing data on-chain) or Validium mode (verifying data-availability with a sidechain).


dYdX is a margin, derivatives, and futures trading protocol on Ethereum that has seen its trading volume skyrocket over the past few months,[making headlines](https://finance.yahoo.com/news/ethereum-based-leverage-trading-protocol-162512422.html) in October for surpassing both Uniswap and Coinbase in terms of trading volume. While the protocol’s leveraged trading undoubtedly inflates these numbers, dYdX has still grown its daily exchange trade volume an impressive 20,000% since March.


While dYdX’s StarkEx implementation is a ZK Rollup, DeversiFi and Immutable X use Validium, a scaling solution that also uses zero-knowledge proofs, but unlike ZK Rollups, data availability is kept off-chain.


TikTok Top Moments,[powered by Immutable X](https://finance.yahoo.com/news/ethereum-based-leverage-trading-protocol-162512422.html) , is turning some of TikTok’s most popular moments into NFTs. Already, celebrities such as Curtis Roach, Grimes, and Lil Nas X have drawn a lot of attention to Immutable X and its potential to power NFT projects while leveraging Ethereum’s security and avoiding gas fees. StarkNet Alpha is set to go live in November, which will introduce an exciting new feature: composability.


Another player in the scaling landscape is Polygon. Polygon is not a Layer 2, but a suite of Ethereum scaling solutions—however, the team has alluded to future plans and recently[acquired Hermez Network](https://newsroom.tiktok.com/en-us/tiktok-launches-first-creator-led-nft-collection-powered-by-immutable-x) , a ZK-Rollups-based Ethereum Layer 2 scaling solution. Polygon Hermez will add to their existing products including[Polygon Avail](https://www.coindesk.com/business/2021/08/13/polygon-merges-with-hermez-network-in-250m-deal/) and[Polygon SDK](https://polygon.technology/polygon-sdk/) . With NFTs and gaming fueling growth, Polygon has seen a substantial rise in its user base over the past few months, so it will be interesting to see the impact their upcoming developments will have on their role in the Layer 2 landscape.


### Sidechains


While Layer 2 solutions like Arbitrum have dominated the landscape in terms of TVL, other scaling solutions are also making a name for themselves. Sidechains are independent, EVM-compatible blockchains that run in parallel to the mainnet. These are compatible with Ethereum via two-way bridges, and run under their own consensus rules and block parameters.


While sidechains don’t inherit the security of the mainnet like true Layer 2 solutions, they certainly add value to the ecosystem. At the end of June, Axie Infinity[launched Ronin](https://www.coindesk.com/business/2021/08/13/polygon-merges-with-hermez-network-in-250m-deal/) , an Ethereum-linked sidechain made specifically for the game. Complete with the Ronin wallet, described as a “boarding pass to a new digital nation,” players experience fast and seamless transactions that weren’t possible on Ethereum’s mainnet.


### The Competition Heats Up


While there will certainly be multiple solutions that emerge in the journey to scaling, it will be interesting to see which of these solutions garner the most traction and market share. However, improving network capacity in terms of speed and throughput is crucial to broader adoption, and all of these solutions—whether Optimistic Rollups, ZK Rollups, or even sidechains—bode well for Ethereum’s growth.


While other Layer 1s have certainly attracted new DeFi projects and users through incentive programs and liquidity mining rewards, the rollout of numerous bridges and increased composability among these Layer 2 solutions is creating a more connected, cohesive ecosystem.[According to Messari](https://medium.com/axie-infinity/introducing-ronin-axie-infinitys-ethereum-sidechain-8745e31eaef1) , Ethereum’s dominance among Layer 1s dropped about 20% over the past six months, holding in July at about 77% of the $110B TVL. With close to $4B TVL in these scaling solutions, a flourishing Layer 2 landscape may not only help Ethereum retain its market share, but also propel its growth significantly over the coming months.
