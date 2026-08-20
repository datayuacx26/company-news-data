---
schema_version: "1.0.0"
document_id: "5228e0174c3f57fce02351f214a69815a2bc4de6a9b7a39cafad18afb2525e10"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/gravity-mainnet-quicknode"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-25T20:23:53.195520+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:7ff736f577381b3e9ea4e84280fe79eb30f464e55fc1a01017ee3e99195d932a"
---

# Gravity Mainnet Is Now Available on Quicknode | Quicknode

#


Gravity Mainnet Is Now Available on Quicknode


Gravity Mainnet, a fully EVM-equivalent Layer 1 with sub-second finality and 12,000+ TPS, is now available on Quicknode. Learn about its native oracle, agent-focused design, and how it evolved from Gravity Alpha Mainnet.


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


July 21, 2026 — 3 min read


Gravity Mainnet is now available on[Quicknode](https://www.quicknode.com/chains/gravity) . Developers can connect to the network through Quicknode and start building on Gravity's high-performance, EVM-equivalent Layer 1 from day one. Here is a look at what Gravity Mainnet is and why it matters.


## What is Gravity Mainnet?


Gravity Mainnet is a fully EVM-equivalent Layer 1 that launched on June 4, 2026. It is the successor to Gravity Alpha Mainnet, which went live in August 2024 to bring Galxe onchain and processed hundreds of millions of transactions under real production load. Nearly two years of running that network informed the design of the new L1, from Grevm (Gravity's parallel EVM) to the Gravity SDK and upstream contributions to Reth.


The network delivers sub-second finality and 12,000+ transactions per second on modest validator hardware (8 vCPU / 16 GB RAM per node). Every layer of the stack is engineered for throughput:


-


**Pipelining:** 5 stages running concurrently across blocks


-


**Mempool:** Quorum Store streams batches in parallel with no leader bottleneck


-


**Consensus:** Pipelined AptosBFT with sub-second finality


-


**Execution:** Gravity Reth, a fork of Paradigm's Reth, powered by Grevm


-


**Storage:** Parallel merklization, multi-version Gravity Store, async I/O


Gravity Mainnet launched in permissioned mode by design. A small set of invited validators operates the network today, with Galxe as the first application migrating from Alpha Mainnet. Over time, Gravity will expand the validator set, onboard more applications, and move toward a fully permissionless public network. G is the network's native gas and staking token.


## A native oracle with consensus security


Most chains rely on third-party oracles and bridges to bring offchain state onchain, which stacks additional trust assumptions on top of the network's own. Gravity takes a different approach: the oracle is a first-class protocol primitive. The same validators that produce blocks attest to oracle messages, with the same security guarantees as the rest of the network.


At launch, the native Oracle powers the Ethereum-to-Gravity asset bridge. A user locks G on Ethereum, Gravity validators independently attest the deposit, and the oracle module mints native G directly on Gravity. No wrapped assets, no external oracle network, no separate bridge signer set. The roadmap extends the same validator-attestation pipeline to additional chains, price feeds, social attestations, and arbitrary offchain events.


## Built for the agent era


Gravity positions itself as an execution layer for AI agents. As agents begin acting on users' behalf across trading, payments, coordination, and content, they need a backend that can read and write state, execute rules, settle value, and stay synchronized with the external world. High-performance EVM execution, a native oracle, and protocol-level randomness, all in a single environment, make Gravity a natural fit for agent-driven applications. This direction will soon take shape in Gravity World, an upcoming dApp where AI agents can interact, build, compete, and participate in a shared agent economy, bringing more application activity and agent-driven experiences onto Gravity L1.


## Get started with Quicknode


Ready to build on Gravity?[Create a free Quicknode account](https://www.quicknode.com/signup) and connect to the network in minutes, or explore[Quicknode's Gravity RPC and data tools](https://www.quicknode.com/chains/gravity) to see the full stack.


### About Quicknode


Founded in 2017, Quicknode deploys institutional-grade blockchain infrastructure for developers and enterprises. With 99.99% uptime and support for 80+ chains, teams build and scale onchain applications without compromise.


[Start building today](https://www.quicknode.com/)
