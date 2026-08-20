---
schema_version: "1.0.0"
document_id: "0789d500d247b5dc6258d2e1a1410b5d53a06cb70ef57db25ecd3fff862c5fe2"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/the-merge-results-and-the-future-of-ethereum"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:da3e22b1bbe39aad05e5d1aa227d38d83f1b5583dcb0a59cae1cfacc696e51fd"
---

# The Merge Results And The Future of Ethereum

Proof-of-stake is finally here!


Ethereum is now secured using proof-of-stake (PoS) and has deprecated proof-of-work (PoW). The value of all ETH, ERC20 tokens, and other digital assets secured by Ethereum PoS is over $300 billion USD.


## Why Proof-of-Stake?


Before Ethereum went live in July 2015, Ethereum developers were[already considering transitioning to PoS](https://blog.ethereum.org/2014/07/05/stake) . One reason for transitioning to PoS was to drastically reduce the electricity required to secure the network. The Ethereum Foundation estimates that PoS will reduce Ethereum’s electricity consumption by ~99.95%.


Another reason for switching to PoS was to change the reward / punishment system for participants responsible for securing the network. In PoW, if a miner controls over 51% of the network, they can censor transactions and erase recent transactions. In Bitcoin, if a miner is conducting a 51% attack, there is no “protocol level” punishment for their behavior. The primary cost of a failed 51% attack is the electricity used by the attacker’s mining equipment. After the failed attack, the miner can continue mining on Bitcoin uninterrupted.


In PoS, these punishment and reward systems are more direct and handled at the protocol level. If a block proposer doesn’t propose a block or a validator didn’t sign a block because they were offline, some of their staked ETH is deleted. If a block proposer or validator intentionally tries to disrupt the network, the amount of staked ETH deleted is much greater.


The first significant milestone in the road leading to PoS[was the launch of the Beacon Chain in December 2020](https://quantstamp.com/blog/eth-2-0-beacon-chain-launch-results-phase-1-and-beyond) .


‍


## The Heart of PoS Ethereum


The Beacon Chain is the heart of proof-of-stake Ethereum: it randomly selects validators to propose and vote on blocks, issues punishments and rewards to validators, finalizes transactions, and more. Although the Ethereum state (aka all account balances, DeFi, NFTs, and Ethereum as we know it) was managed by PoW up until today’s merge, the Beacon Chain operated in parallel with PoW Ethereum since its launch in 2020.


Before the Beacon Chain was launched, Quantstamp audited two Beacon Chain clients,[Prysm](https://certificate.quantstamp.com/full/prysm) and[Teku](https://certificate.quantstamp.com/full/teku) . At launch, 877,249 ETH (~500 million USD) was staked on the Beacon Chain. Now that the merge has taken place, the over $300 billion in digital assets managed by Ethereum benefit from the security services provided by Quantstamp.


‍


## Block Production in PoS vs PoW


In PoW, the tempo and time in which the blockchain grows is managed by the difficulty rating which influences the probability a miner discovers the “golden nonce,” which makes a miner eligible to add a block to the blockchain. In Ethereum’s now deprecated PoW, a block was found roughly every 14 seconds.


In PoS, the time and tempo of the blockchain is determined by epochs and slots. Every epoch is exactly 6.4 minutes long and divided into 32 slots. At the beginning of each epoch, validators are randomly assigned roles which include proposing blocks or voting on the validity of those blocks. Each epoch, every validator is assigned a single role: a block proposer, a beacon committee member, or participating in a sync committee (we won't get into that here).


Validators assigned to propose blocks are assigned to a specific slot. Let’s say a validator was assigned to propose a block in Slot 28 of Epoch 2. Once they submit the block, validators assigned to a beacon committee vote on the validity of that block. Validators are only assigned to a single beacon committee each epoch and every beacon committee is assigned to vote on the validity of a single slot. At the end of Epoch 2, validator roles are shuffled.


Ethereum PoS also introduces a protocol level definition of finalization. Epochs (and the blocks and transactions within them) are finalized after ⅔ of all validators agree on 2 future checkpoints. A checkpoint is the first block in an epoch. In our example above, we mentioned Epoch 2. Epoch 2 will not be finalized until ⅔ of all validators agree on the first block of Epoch 3 and the first block of Epoch 4.


> The result would be that rollups can get up to 100x cheaper fees, without having to wait on the full version of[#danksharding](https://twitter.com/hashtag/danksharding?src=hash&ref_src=twsrc%5Etfw) . It is designed to be forward-compatible with[#danksharding](https://twitter.com/hashtag/danksharding?src=hash&ref_src=twsrc%5Etfw) however, meaning the advantages compound...
>
>
> — Optimism (✨🔴_🔴✨) (@optimismFND)[March 4, 2022](https://twitter.com/optimismFND/status/1499830652249821184?ref_src=twsrc%5Etfw)


‍


## Next Steps for Scaling


Ethereum is scaling in order to be able to handle all of the world's economic activity and is likely to be used to record lots of other stuff as well. We are not talking about the $100s of billions of assets it tracks today, we are talking about tracking value in the[$100s of trillions](https://www.visualcapitalist.com/all-of-the-worlds-money-and-markets-in-one-visualization-2020/) .


In order to achieve this grand scale, Ethereum’s scaling roadmap is “[rollup centric](https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698) ”: this means Ethereum’s final form is still expected to have multiple shards, but these shards are being optimized for L2 transactions. Users are expected to spend most of their time on L2 where their transactions are fast and cheap, while rollups post data on Ethereum shards to secure these transactions.


While sharding is still years away (Ethereum Foundation projects that shards will ship around 2023, but this won’t be the first time Ethereum postponed a development deadline),[proto-dank sharding](https://eips.ethereum.org/EIPS/eip-4844) is a near-term scaling upgrade expected to take place “shortly” after the merge. Proto-danksharding introduces the "blob" transaction type which Optimism, a leading rollup, anticipates will make fees 100 times cheaper for rollups without sacrificing decentralization.


‍


## ETH Net Issuance Approaches Zero


Before the merge, approximately 14,600 ETH was minted per day. ~13,000 ETH was issued as PoW rewards and 1,600 ETH was issued via staking rewards on the Beacon Chain. Now that PoW is obsolete, only staking rewards remain. This means that issuance dropped by nearly 90% overnight.


In order to consider the full implications of net ETH issuance, we must also consider Ethereum’s fee burning mechanism. Over the last month, approximately 1,200 ETH was burned (aka deleted from the supply). This means the removal of PoW brings ETH net issuance much closer to 0.


‍


## When Can Stakers Withdraw ETH?


Validators are expected to be able to withdraw their staked ETH from the Beacon Chain in Ethereum’s next upgrade, the Shanghai upgrade. Currently over 13.5 million ETH is staked on the Beacon Chain. This is over 10% of all ETH in existence.


Some fear that once this upgrade is enabled, mass withdrawals of ETH will take place which would flood the market and tank the price, but this fear is unwarranted. Ethereum limits the amount of stakers that can exit at a time. Only 6 validators can withdraw their stake at each epoch, and, if too many validators withdraw their ETH jeopardizing the security of the network, the rate of withdrawal continues to drop. ETH rewards for active validators also rise when other validators exit with their stake.


‍


A diagram made by Vitalik depicting areas of research and planned Ethereum upgrades.


## Ethereum’s Final Form


Ethereum improvements are far from over. In a recent presentation, Ethereum founder Vitalik Buterin claimed that Ethereum will be 55% complete after the merge. So what will Ethereum feel like for users after the next 45% is complete?


Vitalik predicts that with rollups, Ethereum will handle 100,000 transactions per second with fees between $0.002 and $0.05 cents (for reference,[VISA currently supports 76,000 tps](https://usa.visa.com/about-visa/visanet.html) ). Currently, MetaMask and many DeFi applications rely on intermediaries like Infura to give users the Ethereum state and relay their transactions. Researchers are working on reducing the hardware requirements of light clients so that these applications can directly connect to Ethereum instead of depending on intermediaries.


Apart from reducing the hardware requirements of light clients, developers are working on reducing the requirements for validators and nodes in general in order to further decentralize the baselayer. There may be a future where someone can reliably run a validator from their cell phone.


‍


## Optimism in a Tumultuous Time


Vitalik recently stated that Ethereum is scaling so that even people living in countries like Zambia, where the median income is $4 a day, can afford to participate in the network. Ethereum is developing so that people from all over the world can one day participate in an open, fair, and transparent network and utilize applications that have not even been invented yet. Although speculation on Ethereum based assets is rampant, Ethereum aims to be a platform that provides a new landscape of opportunities for the disenfranchised where anyone can leverage their own creativity to escape legacy systems that fail to meet their needs.


At Quantstamp, we are proud to have participated in this optimistic future since 2017. In our early days, we worked with great projects like PoolTogether, BuyCoins, and Ramp and more recently, provided security services to projects like Arbitrum, Lido, and Ethereum PoS clients. We do not know what web3 will look like in 20 years, but we will be there every step of the way to secure a better future.
