---
schema_version: "1.0.0"
document_id: "1c2383474d6d56b7ae04ccbfd751c21d323e2a4e35b9b12b9d51300e1402e7ab"
company_key: "yc-argument-computer-corporation"
company: "Argument Computer Corporation"
source_id: "yc-argument-computer-corporation-news-import-6d03b0bcc333"
canonical_url: "https://argument.xyz/blog/aptos-eth/"
published_at: null
first_seen_at: "2026-07-21T07:37:57.416159+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:97f2855555f2dca4e22b75807cbff2df224a2ad9ff30054aff0afe2d2d89f9b2"
---

# Unveiling Our ZK-Light Clients for Ethereum and Aptos

Today, we’re excited to open-source[two ZK-light client implementations](https://github.com/argumentcomputer/zk-light-clients/) :


- [Aptos light client](https://github.com/argumentcomputer/zk-light-clients/tree/dev/aptos) : a ZK-proof of the Aptos light client protocol and a Solidity verifier.
- [Ethereum light client](https://github.com/argumentcomputer/zk-light-clients/tree/dev/ethereum) : a ZK-proof of the Deneb sync protocol and a Move verifier.


Together, these components form a complete bi-directional ZK-bridge between Aptos and Ethereum.


## Introducing our Open-Source ZK-Light Clients for Ethereum and Aptos


[Light clients](https://a16zcrypto.com/posts/article/an-introduction-to-light-clients/) are sub-protocols that verify the inclusion of data in a blockchain without trust assumptions and without requiring the verifier to maintain a copy of the blockchain. They are essential for the security of wallets and bridges.


By creating a zero-knowledge proof (ZK-proof) of the light client sub-protocol, one can prove the result of this verification succinctly, enabling efficient on-chain verification.


Our released software uses our fork of the SP1 prover,[Sphinx](https://github.com/argumentcomputer/sphinx) , which includes precompiles specifically developed for these ZK-light clients, as detailed in our[previous blog post](https://argument.xyz/blog/sphinx-oss) . Each repository also contains servers and clients for managing the proving and orchestration of proving requests for the light client, demonstrating the full operation needed in such a bridge.


## Performance


Our ZK-light client implementations are performance-oriented, focusing on minimizing the worst-case proving latency of the bridge. This worst-case latency arises in situations that vary slightly between Aptos and Ethereum. Detailed benchmarking documentation, including worst-case description, and our hardware specifications (CPU-only), can be found for[Aptos](https://github.com/argumentcomputer/zk-light-clients/blob/dev/aptos/docs/src/benchmark/overview.md) and[Ethereum](https://github.com/argumentcomputer/zk-light-clients/blob/dev/ethereum/docs/src/benchmark/overview.md) .


The proofs that land on the *home* chain (the verifying chain) are Plonk proofs using BN254, deduced by recursion from the original STARK proof.


The worst-case end-to-end proving latency (for an on-chain verifiable SNARK proof) is:


- **Aptos light client:** 11min 35s, for a total of 10,866,949 VM cycles.
- **Ethereum light client:** 11min 40s, for a total of 10,430,605 VM cycles.


We are pleased with these initial results and will continue to refine these light clients, expecting performance improvements in the future.


## Partnerships in the ZK Ecosystem


It has been a pleasure to work with the[Wormhole Foundation](https://wormhole.foundation/blog/wormhole-foundation-awards-contributor-grant-to-lurk-lab-to-bring-trustless-transfers-to-wormhole-with-zk-proofs) and[Kadena](https://www.kadena.io/blog/kadena-announces-partnership-with-lurk-lab-to-build-zk-bridge) on these light clients. We are immensely grateful for their continued support.


We have also enjoyed excellent interactions with the Aptos Core developers, who have been responsive and supportive. Notably, we have a[pending issue to allow the Aptos Public Full Node to serve signature data](https://github.com/aptos-labs/aptos-core/issues/13596) , which includes working code. If you are part of the Aptos community and have a business-critical need for trustless communication with the Aptos network, your comments on this issue would be invaluable in prioritizing this work. Please feel free to reach out to us as well!


We have worked on a Plonk verifier for Move compatible with the proofs produced by the gnark go library as the final step of a Sphinx or SP1 proof. If you are part of the[gnark](https://github.com/Consensys/gnark) or[SP1](https://github.com/succinctlabs/sp1) projects and think this could have a long-term home in your project, we’d be glad to make a contribution, please contact us!


Finally, we want to highlight the quality of the[Succinct SP1 ZKVM](https://github.com/succinctlabs/sp1) , which has been the foundation of our Sphinx fork. Without it, none of this would have been possible. By open-sourcing this work, we aim to expand the roster of open-source ZK-light clients, building on the foundation that Succinct started with their[Tendermint light client](https://github.com/succinctlabs/sp1-tendermint-example) .
