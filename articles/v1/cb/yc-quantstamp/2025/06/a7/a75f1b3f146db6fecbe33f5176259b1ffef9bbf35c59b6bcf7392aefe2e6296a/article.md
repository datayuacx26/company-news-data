---
schema_version: "1.0.0"
document_id: "a75f1b3f146db6fecbe33f5176259b1ffef9bbf35c59b6bcf7392aefe2e6296a"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/ethereum-gas-fees-rising-but-l2-solutions-are-coming"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:caa5dcfa2ca6d1b661fa2551e8dd370773b8d4f671ba1b069bb08e66e750bb5f"
---

# Ethereum Gas Fees Rising, But L2 Solutions Are Coming

Ethereum gas fees are steadily rising. As fees continue to break all-time highs, non-whale DeFi users are slowly priced-out of DeFi because transaction fees are eliminating their profits. In addition, many non-DeFi applications such as gaming applications are becoming unusable on Ethereum Layer 1.


‍


*The Ethereum gas price recently experience high volatility.*


As the demand and popularity of Ethereum increases, transaction fees are likely to continue to rise. To address this, many projects including OMG Network, SKALE Network, and IDEX are producing scaling solutions that drastically reduce the cost and increase the speed of transactions for users while keeping users in control of their assets.


### Tether on OMG Network


Tether (USDT) is the stablecoin with the most liquidity in the entire blockchain ecosystem. Tether has a 15 billion USD market cap and trading volumes that regularly surpass both Bitcoin and Ethereum. Historically, it is the highest consumer of gas compared to any other smart contract on Ethereum ([Uniswap recently surpassed Tether](https://ethgasstation.info/) ). On August 19th, Bitfinex, the issuer of Tether, integrated the OMG Network in order to enhance the user experience of Tether.


*Tether has the 3rd highest market capitalization out of all cryptocurrencies. Image source:*[coinpaprika](https://www.google.com/url?q=https://coinpaprika.com/coin/usdt-tether/&sa=D&ust=1601996728889000&usg=AOvVaw3aIfom7SZEyfu50viAMHgW)


In order for an exchange or user to access their Tether on the OMG Network, they must first deposit Tether into a smart contract on Ethereum Layer 1. Once Tether is deposited, users are eligible to transfer their Tether on OMG Network. The Layer 2 OMG Network is composed of its own blockchain with blocks produced by a single operator. After every block is added to the OMG Network chain, the operator submits the merkle root of the transactions in each OMG block on Ethereum Layer 1.


A “watcher” is used to keep the single block producer accountable. The watcher views all data produced by the single block producer and, in the event that the block producer submits invalid information or attempts to cheat the network, the watcher uses the merkle roots submitted to Ethereum Layer 1 to cryptographically determine if invalid activity took place. If invalid behavior is detected, the watcher notifies users, who have time to challenge invalid behavior and then safely withdraw their funds


Bitfinex will initially use OMG Network to facilitate user transfers of Tether between large exchanges. OMG Network is powered by an implementation of Plasma called MoreVP,[which was audited by Quantstamp](https://www.google.com/url?q=https://quantstamp.com/blog/omisego-advances-plasma-with-its-latest-implementation&sa=D&ust=1601996728891000&usg=AOvVaw2xVQPNoI5joK6rLvJmkAks) .


‍


‍


‍


### Enabling New Use Cases with SKALE Network


SKALE Network intends to enable new decentralized use cases by not only reducing transaction costs, but also by allowing dApp developers to deploy their own customizable SKALE chains that cater to their applications' unique needs. SKALE Network refers to these Ethereum-as-a-service Layer 2 blockchains as *elastic sidechains* .


Each elastic sidechain is dedicated to a single decentralized application. dApp developers have a lot of flexibility as to how their sidechain functions. For instance, since there is a monthly flat fee paid to SKALE Network Operators (similar to Ethereum miners), they can dramatically decrease transaction fees for their users by taking thousands of transactions off-chain and settling on mainnet Ethereum only when necessary. Also, these elastic sidechains are not subject to Ethereum’s storage and computation limitations. If dApp operators need more storage or computational power to run their application, they can pay for what they need.


*Node operators can run multiple chains. Each chain is dedicated to a single dApp. Image source:*[SKALE Network](https://www.google.com/url?q=https://skale.network/whitepaper&sa=D&ust=1601996728893000&usg=AOvVaw0OwFZJBDW9VO_KB2OmAsL-)


SKALE Network is ideal for potential use cases that are too costly to run solely on Ethereum mainnet. For instance, playing a blockchain-based trading card game directly on Ethereum is undesirable for users because they need to make 10s or 100s of transactions each game session, which can quickly get expensive. On the SKALE Network, users can play their trading card game without having to worry about expensive transactions, while also maintaining sovereignty over their in-game assets. Users will only have to make Ethereum transactions upon entering or exiting the elastic sidechain.


Quantstamp audited components of the SKALE Manager smart contract system which impacts SKALE Network nodes, validators, and elastic sidechains. Quantstamp also audited the SKALE Allocator contracts which manage the vesting structure of SKALE tokens for investors.


‍


‍


‍


### IDEX and Optimized Optimistic Rollups


IDEX is a decentralized exchange that plans to incorporate Optimized Optimistic Rollups, a Layer 2 solution, later this year to enhance user experience. Since IDEX’s inception in Oct 2017, IDEX users cumulatively paid over 7 million USD in ether for gas fees. In order to reduce gas fees for users and speed up transaction times, IDEX intends to execute trades and maintain user balances on Layer 2.


*Storing merkle roots of Layer 2 blocks allows observers to mathematically prove if malicious or invalid behavior took place. Image source:*[IDEX](https://www.google.com/url?q=https://blog.idex.io/all-posts/o2-rollup-overview&sa=D&ust=1601996728894000&usg=AOvVaw2VcOupvTqy35deAs62Dpac)


Optimized Optimistic Rollups works very similar to how OMG Network works. All Layer 2 blocks are produced by IDEX, a single block producer, and merkle roots of transactions in these blocks are stored in Layer 1 smart contracts in order to keep the single block producer accountable.


Validator nodes in IDEX’s Optimized Optimistic Rollups have the same function as watchers in OMG Network: they monitor all of the block producer’s activity in order to detect invalid or malicious behavior. One key difference is that IDEX validators have the ability to prevent invalid data from being accepted by the Layer 1 smart contracts. This means that users don't have to take action in the event that an invalid Layer 2 block is published. Any individual validator that can cryptographically prove fraud is able to prevent the invalid block from finalizing, in which case the state will automatically revert to that of the previous valid block.


Quantstamp recently audited the on-chain components of the IDEX 2.0 upgrade.


‍


### The Future of Scaling and Ethereum


Many view high fees as a detriment to Ethereum, however, high fees are also an indicator of Ethereum’s success. High fees are also driving Layer 2 development: many leading DeFi projects are already brainstorming ways in which they can drive and connect their liquidity over a Layer 2 solution.


ETH 2.0 will also put downward pressure on fees and, in combination with Layer 2 scaling solutions, will make room for a level of financial activity that is magnitudes greater than what currently exists. Ethereum is preparing to absorb as much worldwide economic activity as possible and development continues at an impressive pace.
