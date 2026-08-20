---
schema_version: "1.0.0"
document_id: "0c13b58f6894c51c7f4d6c0c4959690b4c1a7f5db42021d5e03658f45ba43b0e"
company_key: "circle-internet-group-inc-class-a-common-stock"
company: "Circle Internet Group Inc."
source_id: "circle-internet-group-inc-class-a-common-stock-news-import-6b20a10366df"
canonical_url: "https://www.circle.com/blog/amp-rethinking-block-building-with-multi-proposer-consensus"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:44.770994+00:00"
fetched_at: "2026-07-28T22:07:11.290939+00:00"
content_hash: "sha256:fbef0c97b14d1a530cdbbb6451fc9d9f3edac75b89a3356ddaed8434c4b7a2ba"
---

# AMP: Multi-Proposer Block Building on Arc | Circle

Many blockchains rely on a single *block assembler* , a validator or builder, to determine which transactions enter a block and in what order. This concentration creates an architectural problem for any blockchain that aims to settle financial activity.


The block assembler has two powers: selecting which transactions to include and their order. Together, these enable maximal extractable value (MEV). Traditional markets treat this kind of discretion as a market-structure problem and regulate it accordingly. As an example, the Bank for International Settlements (BIS) has[described](https://www.bis.org/publ/bisbull58.pdf) transaction reordering at the discretion of validators as “activities that would be illegal in traditional markets.” It is no surprise, then, that as interest in crypto grows, the block assembler’s powers are drawing more scrutiny.


The[Arc Multi-Proposer Protocol](https://arxiv.org/pdf/2605.23677) (AMP) introduces a new role — the proposer — that participates in block construction alongside validators, enabling protocol-level mechanisms for transaction inclusion and order. While other solutions exist for MEV mitigation and Arc itself will ship with private-mempool protection from day one, AMP goes further, opening block construction to applications and letting them capture the value of the order flow they originate as proposers.


‍


## An expansion path for Arc


Arc, a[layer-1 blockchain](https://www.arc.io/) designed by Circle, is built on Malachite, a Tendermint-based consensus engine that provides deterministic, sub-second finality. Because replacing a blockchain's consensus layer is a major undertaking, AMP is designed to avoid touching it altogether. AMP composes entirely on top of Malachite, changing only how transactions enter blocks and their order. Should AMP run on Arc, the integration would not require modifying the consensus layer itself.


‍


## How AMP works


Instead of routing transaction flow through a single block assembler via mempool, AMP introduces dedicated proposers between users and validators. Proposers collect user transactions, bundle them together into payloads, and broadcast those payloads to validators. Validators attest to the payloads they have received by sending a short identifier for each payload to other validators, and then run consensus to decide which payloads belong in the next block.


A secondary benefit of AMP’s design is that it separates transaction dissemination, handled by proposers, from transaction ordering, handled by validators. Proposers are responsible for disseminating payloads to validators, while validators agree on short attestations to the received payloads, rather than on the transactions themselves. As a result, throughput scales with how quickly proposers can disseminate payloads in parallel, rather than with how quickly a single block assembler can publish a full block.


‍


## What AMP enables


AMP provides two protocol-level features that conventional block construction does not.


**Bounded inclusion.** Once a payload has been attested by a super-majority of validators, it *must* appear in the next block — the block assembler has no choice. This shifts block building away from selection over the assembler's private set of pending transactions to forced inclusion of payloads known to sufficient validators. For a proposer, this means that the transactions included in their payloads will be in a finalized block within a defined time.


**Fixed ordering.** In the included set of payloads, transaction order is determined by a fixed rule rather than chosen by the block assembler. Descending priority fee is one natural choice; it replaces discretionary reordering with an open, market-based auction. For a proposer, this means validators cannot move transactions around to extract value from their flow.


Together, these capabilities turn building a block from a discretionary act into a constrained one.


‍


## Operating a proposer


We are exploring how AMP can open block construction to participants outside of validators. Joining our permissioned proposer set may be appealing, for example, to an entity that has application specific order flows that are of value as AMP allows for:


- **Direct path for transaction flow.** A proposer submits its users' transactions to validators, without going through any intermediaries.
- **Inclusion certainty.** Payloads broadcast by a proposer land in the next block once enough validators attest to them.
- **Application-aligned order flow value** Fixed ordering over included payloads removes the assembler's ability to reorder transactions for extraction. The value associated with a proposer's order flow instead accrues, transparently and by protocol rule, to the application that originated it.
‍


In practice, validators can operate and serve as proposers, capturing the same benefits described above alongside their consensus role. In this way, a validator making payloads (even without being a block assembler) can force the other validators to include their transactions.


‍


## Conclusion


Blockchains that aim to support regulated financial activity need to do more than finalize blocks quickly. They need predictable settlement, fair transaction handling, and fewer opportunities for manipulation from concentrated control over transaction inclusion and ordering.


AMP is a step toward that. It introduces the proposer role and creates two protocol features — bounded inclusion and fixed ordering — that take the most consequential parts of block construction out of any single participant’s hands. For more details on AMP, see the[full paper](https://arxiv.org/pdf/2605.23677) and if you are interested in being a design partner for AMP or running a proposer, please[reach out](https://www.circle.com/circle-research#research-subscribe) .


‍


*AMP is a research protocol in exploratory development. Its adoption on Arc is not guaranteed and remains subject to further design, analysis, and validation.*


*Arc testnet is offered by Circle Technology Services, LLC ("CTS"). CTS is a software provider and does not provide regulated financial or advisory services. You are solely responsible for services you provide to users, including obtaining any necessary licenses or approvals and otherwise complying with applicable laws.*


*Arc has not been reviewed or approved by the New York State Department of Financial Services.*


*The product features described in these materials are for informational purposes only. All product features may be modified, delayed, or cancelled without prior notice, at any time and at the sole discretion of Circle Technology Services, LLC. Nothing herein constitutes a commitment, warranty, guarantee or investment advice.*
