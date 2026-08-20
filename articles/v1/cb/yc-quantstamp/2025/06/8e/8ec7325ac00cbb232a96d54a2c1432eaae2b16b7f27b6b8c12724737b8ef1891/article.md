---
schema_version: "1.0.0"
document_id: "8ec7325ac00cbb232a96d54a2c1432eaae2b16b7f27b6b8c12724737b8ef1891"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/quantstamp-completes-audit-of-2nd-eth-2-0-implementation"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:574dab11215482ab4fe885f2f552b18f7b4d51ee6b64fad2bb4e16efa64c9f0e"
---

# Quantstamp Completes Audit of 2nd ETH 2.0 Implementation

### Quantstamp Completes Audit of 2nd ETH 2.0 Implementation


Quantstamp has now completed its audit of Teku, the Ethereum 2.0 client developed by ConsenSys. This marks Quantstamp’s 2nd audit of an Ethereum 2.0 implementation: the Prysm client by Prysmatic Labs[was audited by Quantstamp in July](https://www.coindesk.com/quantstamp-audit-greenlights-eth-2-0-client-prysm-for-launch) .


“Ethereum 2.0 is on track to deliver Phase 0 in the very near future,” says Quantstamp CEO Richard Ma. “Client teams have spent countless hours coordinating, testing, and working with auditing firms to ensure that the foundation of Ethereum 2.0 is ready for delivery.”


The audit included enforcing network consensus rules, maintaining consensus, and code that controls rewards and penalties for stakers. Quantstamp found the codebase to be of the highest quality and all findings were quickly resolved by the Teku development team. Quantstamp disclosed findings as they were discovered and the Teku team was highly collaborative throughout the process.


Resolved findings include:


- Unlimited inbound messaging queue which may leave individual nodes open to Denial of Service attacks
- Errors that may lead to clients falling out of sync
- Missing validation logic


*ETH 2.0 shards will eventually finalize data through the Beacon Chain. The Beacon Chain serves as the source of truth.*


### The Status of Ethereum 2.0


The deposit contract, the smart contract that accepts deposits from users intending to run Ethereum 2.0 validators, is expected to launch within weeks. Once deployed, users intending to validate transactions on the Beacon Chain will be able to submit their stakes.


The Beacon Chain was extensively tested by all client teams. Two testnets, Zinken and Spadina, were created to test the genesis process of the Beacon Chain. In addition to genesis specific testnets, numerous other multi-client testnets were operating since April in order to fix bugs that would otherwise emerge on mainnet. Multiple attack bounties were paid out and are still available to individuals that can successfully disrupt consensus on testnets.


The full transition to proof-of-stake is a multi-year process. When the Beacon Chain is launched, it will exist in parallel with the current Ethereum blockchain. The Beacon Chain will validate empty blocks and finalize empty epochs, and developers will observe how it operates under mainnet conditions. After the Beacon Chain is launched, development will focus on Phase 1 of Ethereum 2.0. In this phase, the base structure of the shard chains will be developed. It will not be until Phase 2 that shards store and validate Ethereum transactions.


The migration of Ether and the DeFi ecosystem to Ethereum 2.0 is a high stakes process. Over 28 billion USD worth of Ether and other digital assets depend on the successful transition to proof-of-stake. We’re thrilled to have worked with the Teku team and look forward to the future of Ethereum.
