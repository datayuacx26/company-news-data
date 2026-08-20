---
schema_version: "1.0.0"
document_id: "fb193c98d6a94c3cc272c25624aa74fb90477e0d33b0953eb6e1fcd426be0975"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/ethereum-2-0-moves-closer-to-launch-with-quantstamp-audit-of-prysm"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8b0595bd4f6a04c82805677955c0801db4a0202428e11bdec9160abacdb9406c"
---

# Ethereum 2.0 Moves Closer to Launch with Quantstamp Audit of Prysm

Quantstamp has recently completed its audit of Ethereum 2.0 as implemented by Prysmatic Labs.


“Ethereum 2.0 is a major upgrade of Ethereum, with a large number of fundamental changes to the way it works,” noted Raul Jordan, Co-founder of Prysmatic Labs. “We chose Quantstamp as they’re one of the few firms capable of auditing a project of this scale. They’ve thoroughly analyzed everything we threw at them, communicating clearly with us the whole way. We are lucky to have chosen such a great partner.”


“Over 28 billion USD worth of Ether and other digital assets are potentially riding on the transition to proof-of-stake,” noted Richard Ma, CEO of Quantstamp. “The migration of Ether and the DeFi ecosystem to Ethereum 2.0 is a high stakes process. 10 engineers, including 7 PhDs, audited Prysm to facilitate a safe foundation and timely launch.”


Ethereum 2.0 transforms Ethereum from a Proof-of-Work blockchain to a Proof-of-Stake blockchain with a sharded architecture. This is designed to improve the incentive model of the platform while also increasing scalability. This radical change to the consensus mechanism and architecture of Ethereum is being rolled out over three phases.


### Prysm


Prysm currently implements Ethereum 2.0 Phase 0, the first phase of the Ethereum 2.0 rollout. Phase 0 focuses on the testing and launch of the Beacon Chain, the Proof-of-Stake blockchain that serves as Ethereum 2.0’s spine. The Beacon Chain is run by validator clients like Prysm which are responsible for executing the Proof-of-Stake protocol, overseeing validators and their stakes, and eventually validating shard chain data (coming in Phase 1).


Validators are the core consensus providers for Ethereum 2.0. Users who run Prysm and stake 32 Ether to help validate the Ethereum 2.0 blockchain earn rewards for doing so.


Quantstamp’s audit of Prysmatic Labs ETH 2.0 client involved ten engineers who examined the[entire codebase](https://github.com/prysmaticlabs/prysm) over the course of two months. They examined the beacon node logic, validator client, slasher logic, libp2p networking layer, gRPC API, client database, account management and key storage, client synchronization, and more.
Vulnerabilities found by Quantstamp engineers and addressed by Prysmatic Labs included:


- Granularity of timestamps
- Pseudo-random number generation
- Second pre-image attacks on Merkle trees


The full list of issues and their fixes can be seen on[Prysm’s issue tracking page](https://github.com/prysmaticlabs/prysm/issues/6327) .


Besides high and medium severity vulnerabilities, Quantstamp also provided low and informational severity guidance which improve code quality and readability. In general we found the code was well-written and documented.


**About Prysmatic Labs**


Prysmatic Labs currently builds technical infrastructure for the Ethereum blockchain. Our mission is to increase adoption through better tooling for users and developers of the Ethereum ecosystem through our expertise. Prysmatic is one of the several teams implementing Ethereum 2.0. Their client, Prysm, is written in the popular Go programming language.


**About Quantstamp**


Quantstamp is a leader in blockchain security, having performed over 140 audits and secured over $1 billion of value. Top crypto and enterprise companies including MakerDAO, Chainlink, eToro, and World Economic Forum choose Quantstamp to secure their blockchain applications.
