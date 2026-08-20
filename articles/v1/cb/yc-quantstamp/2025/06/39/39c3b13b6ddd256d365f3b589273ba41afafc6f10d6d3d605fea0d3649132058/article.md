---
schema_version: "1.0.0"
document_id: "39c3b13b6ddd256d365f3b589273ba41afafc6f10d6d3d605fea0d3649132058"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-audits-layer-1-blockchains"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:39815a5cc8c11574a16c51d3a9ab8a98e8558a4286e66db3ecd1a36a430b9621"
---

# Quantstamp Audits Layer 1 Blockchains

Quantstamp has secured over 5 billion USD in digital assets and provided security services for over 130 organizations including startups, foundations, and enterprises. Apart from securing the applications that run on blockchain platforms, we also offer security services for base layer protocols. Our experience with base layer protocols includes ETH2, Avalanche, and Cardano. For ETH2,[we audited the Prysm client by Prysmatic Labs](https://www.google.com/url?q=https://quantstamp.com/blog/ethereum-2-0-moves-closer-to-launch-with-quantstamp-audit-of-prysm&sa=D&ust=1598992855867000&usg=AOvVaw0c4xR6gXN8OGdYHmzUhP9d) and we are currently auditing the Teku client by ConsenSys.


In this post, we describe what goes into a Layer 1 audit and highlight some of the unique mechanisms we have worked with.


*ETH2 uses proof-of-stake as its consensus mechanism. ETH2 eventually aims to utilize proof-of-stake to validate data across 64 shards.*


### The Consensus Layer


Quantstamp searches for bugs that may prevent Layer 1 networks from reaching consensus. For a network to be in consensus, nodes of a specific network need to be in agreement about the latest state of that network. For a distributed network to be successful, consensus disruptions must be rare because they can make the network unusable for a time.


ETH2, Cardano, and Avalanche each have a unique protocol for producing consensus. ETH2 and Cardano both use proof-of-stake (PoS): however, ETH2 has a PoS model that incentivizes good behavior through slashing, while Cardano uses a delegated proof-of-stake model without slashing. Cardano’s consensus model is referred to as “delegated proof-of-stake” because users delegate their right to validate transactions to a stake pool operator in exchange for a portion of that pool’s rewards.


*Avalanche includes a directed acyclic graph (DAG) component. DAG nodes have a unique internal mechanism for determining which transactions will ultimately be included in the DAG.*[image source](https://www.google.com/url?q=https://files.avalabs.org/papers/consensus.pdf&sa=D&ust=1598992855869000&usg=AOvVaw3d8Krur59jqBp-eJyj0XF-)


‍


Quantstamp looks for vulnerabilities that interfere with consensus and leave networks susceptible to attacks including, but not limited to:


- Unwarranted slashing
- Blockchain state corruption
- Eclipse attacks
- Denial of service attacks
- Network partition
- Channel eavesdropping
- Socket stress attacks
- Disk space exhaustion attacks
- Connection reset attacks
- Issues with file permissions
- Cheating the consensus due to logical errors


*Not all distributed networks are blockchains; some are directed acyclic graphs (DAGs).* ‍


### The Ledgers


The Layer 1 protocols we have secured do not only differ in how they achieve consensus, they also differ in how they store their data. Avalanche’s ledger is actually not a blockchain but a directed acyclic graph. Cardano and ETH2 use blockchains. Quantstamp audited ETH2’s Beacon Chain, the blockchain at the heart of ETH2’s future sharded ledger system. Quantstamp ensures that the data stored in these ledgers is immutable, honest, and free of vulnerabilities.


‍


*Quantstamp audits wallets to secure user funds.*


### User-Facing Applications


Organizations seeking a Layer 1 audit also need security for the user-facing applications that help non-technical users interact with the blockchain. For Cardano, Quantstamp also audited the Daedalus wallet in order to secure user private keys and funds.


‍


‍
