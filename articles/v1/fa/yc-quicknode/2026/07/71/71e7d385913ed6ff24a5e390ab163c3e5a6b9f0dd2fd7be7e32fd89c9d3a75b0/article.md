---
schema_version: "1.0.0"
document_id: "71e7d385913ed6ff24a5e390ab163c3e5a6b9f0dd2fd7be7e32fd89c9d3a75b0"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/hyperliquid-developer-stack"
published_at: null
first_seen_at: "2026-07-25T20:23:53.195520+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:487352a4c9fc2a3d1c17a0877f763a2225ec7908a157df5283508cebccbbdc26"
---

# Hyperliquid Developer Stack: A Complete Guide (2026) | Quicknode

#


Hyperliquid Developer Stack: A Complete Guide (2026)


A developer guide to Hyperliquid: architecture, APIs, HyperCore data, market infrastructure, and what developers can build in 2026.


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


July 2, 2026 — 10 min read


Hyperliquid started as a perpetual futures exchange. Today, it is crypto's richest onchain order book, with native financial primitives beyond perps and an EVM module that gives developers access to all of it.


The numbers tell the story: the chain has generated over $1.25 billion in fees to date, serving more than 1.20M users whose combined transaction volume is $4.78 trillion.


Those numbers have attracted a lot of developers and teams. Today, over 100 protocols are live on HyperEVM. And beyond the numbers, it's easy to see why.


Most blockchains ask developers to bootstrap liquidity, users, and applications simultaneously. Hyperliquid arrives with active markets already running. This changes what developers can build and how quickly they can ship.


So what's the playing ground like for a developer? What are the moving parts? What use cases can be built on Hyperliquid? This guide answers each of those questions.


## Why Developers Will Keep Building on Hyperliquid


Perpetual futures remain Hyperliquid's largest application, but the ecosystem has expanded far beyond trading. Applications on Hyperliquid can access native liquidity, market data, oracle prices, and an EVM environment that shares the same state.


That creates business models that are difficult to replicate elsewhere.


1.


**Builder codes** let applications monetize order flow directly. Any application routing trades through Hyperliquid can attach a fee and collect it onchain. More than $65 million has already been paid to third-party developers.


2.


**HIP-3** introduced permissionless perpetual market creation.


3.


Prediction markets, lending protocols, analytics platforms, stablecoin projects, and trading infrastructure are now building on the same foundation.


**Did you know?** When SpaceX went public in June 2026, a third-party team had already deployed the SPCX perpetual on Hyperliquid. It did $1.34B in volume on IPO day.


So it is simple: developers are showing up because there is already money, liquidity, and distribution available. To understand what these developers are building, we need to look at how the stack is structured.


## How the Hyperliquid Developer Stack Works


At a high level, the stack consists of three layers. Hyperliquid runs two execution environments, HyperCore and HyperEVM, under one consensus layer, HyperBFT.


They share a global state. This single fact is what makes the composability described in the previous section possible.


### 1. HyperBFT


HyperBFT is Hyperliquid's consensus layer. Its job is straightforward: order transactions, reach consensus, and maintain the canonical state of the network. Most developers never interact with HyperBFT directly, but every application built on Hyperliquid ultimately relies on it.


### 2. HyperCore


HyperCore is the financial engine of the ecosystem. It is purpose-built market infrastructure designed for high-performance trading. Spot markets, perpetual futures, vaults, oracle infrastructure, and outcome contracts all live here. It is also where most of Hyperliquid's liquidity, trading activity, and market data originate.


### 3. HyperEVM


HyperEVM is Hyperliquid's smart contract environment. Developers can deploy Solidity applications using familiar Ethereum tooling while accessing assets and infrastructure already native to the Hyperliquid ecosystem.


The most important detail is that these layers share state: a HyperEVM application can read HyperCore oracle prices directly.


Already familiar with Hyperliquid's market position? Learn[why Hyperliquid is winning](https://www.quicknode.com/blog/why-hyperliquid-is-winning) .


Understanding the architecture is useful, but developers do not build against architecture diagrams. They build against APIs, SDKs, market data, and infrastructure endpoints. The next step is understanding how to connect to Hyperliquid and start interacting with the network.


## How to Connect to Hyperliquid


Every Hyperliquid application starts with the same requirement: access to market data, user activity, and network state. Whether you're building a trading terminal, analytics platform, vault, lending protocol, or agent framework, the first task is establishing reliable access to Hyperliquid.


### Accessing Hyperliquid APIs


Hyperliquid offers a number of public APIs for specific use cases.


Interface


Primary Use Case


Info Endpoints


Market data, positions, balances, metadata


Exchange API


Order placement and trading actions


WebSockets


Real-time updates and event streams


gRPC


High-throughput and latency-sensitive systems


But did you know that Hyperliquid's public RPC has been capped at 100 requests per minute since August 2025? Most applications use a combination of these interfaces and run out of capacity fairly quickly. That is the first problem to solve with Quicknode.


### Accessing the Quicknode Hyperliquid API


Quicknode's Hyperliquid endpoint serves both layers, HyperEVM and HyperCore, from a single URL. The routing is path-based.


1.


**For HyperEVM applications,** developers can use standard Ethereum JSON-RPC endpoints for wallet integrations, contract interactions, and transaction submission. Historical archive access, transaction tracing, and WebSocket subscriptions are available through Quicknode's archive infrastructure.


2.


**For HyperCore applications,** Quicknode exposes market-specific APIs covering order books, funding rates, positions, vaults, asset metadata, and other exchange-level data.


For teams focused on shipping applications, using Quicknode's API removes a significant amount of operational overhead.


Connecting to Hyperliquid lets developers access the network. The next key part is accessing the blockchain data and making it usable.


## How to Access Hyperliquid Data


Hyperliquid exposes significantly more than transactions and wallet balances. Developers can access order books, open interest, funding rates, liquidation events, oracle prices, vault performance, user positions, trade history, market metadata, and real-time exchange activity directly from HyperCore.


The exact interface depends on the application being built.


Data Type


Interface


Market data


Info Endpoints


User positions and balances


Info Endpoints


Order placement and execution


Exchange API


Real-time updates


WebSockets


High-throughput data ingestion


gRPC


### Accessing Hyperliquid Data with Quicknode


Quicknode offers a few distinct ways to access Hyperliquid data, each purpose-specific and performant.


1.


The Quicknode` /info` endpoint covers the full exchange surface with 52 total methods covering order books, user positions, open orders, funding rates, vault analytics, liquidatable accounts, TWAP history, oracle prices, asset metadata, and outcome market state.


2.


Quicknode offers event streams through two protocols, WebSocket and gRPC. Together these cover nine data streams including trades, order lifecycle events, book updates, and more.


**Bonus:**[gRPC setup guides and working examples](https://www.quicknode.com/docs/hyperliquid/grpc-api) .


3.


Quicknode Filters work across gRPC, JSON-RPC, and WebSocket protocols, allowing developers to filter data by coin, side, user address, and event type.


4.


Quicknode's[SQL Explorer](https://www.quicknode.com/sql-explorer) gives direct SQL access to fully indexed HyperCore data — over 385 billion rows across 37+ tables covering trades, fills, orders, funding rates, liquidations, clearinghouse states (positions and margin), oracle prices, perpetual and spot market snapshots, staking, ledger updates, bridge events, builder activity, vault equities, and pre-aggregated daily and hourly metrics. Query it in the dashboard editor or via REST, with 40+ pre-built queries and no indexing pipeline to build or maintain.


The more interesting opportunity is what developers can build once they have direct access to Hyperliquid's markets, liquidity, and financial infrastructure.


## What to Build on Hyperliquid


Hyperliquid's shared state collapses what would typically require multiple protocols into a single build surface. The application categories below reflect what's already being built and what the infrastructure now makes possible. So what can actually be built?


### 1. Trading Applications


Trading applications are the most mature category today.


-


Custom frontends


-


Portfolio dashboards


-


Broker experiences


-


Wallet-native trading interfaces


Developers can build these and more while routing activity directly through Hyperliquid. Quicknode's HyperCore APIs expose order books, positions, fills, open orders, and account data through the` /info` endpoint, making it possible to build trading experiences without operating exchange infrastructure yourself.


**Bonus:**[Building a real-time portfolio tracker using HyperCore data](https://www.quicknode.com/guides/hyperliquid/build-portfolio-tracker-using-hypercore-data) .


### 2. Market Intelligence Products


HyperCore gives developers access to several data points: funding rates, liquidations, vault performance, trade activity, and real-time market updates. This data can power research terminals, monitoring platforms, alerting systems, and institutional analytics products.


Applications requiring live updates can consume WebSocket streams, while larger systems can ingest data through Quicknode's gRPC infrastructure and filters.


**Try it:**[building a real-time Hyperliquid Whale Alert Bot](https://www.quicknode.com/guides/hyperliquid/real-time-hyperliquid-whale-alert-bot) .


### 3. New Markets


HyperCore allows developers to create markets rather than simply consume them. HIP-3 introduced permissionless perpetual market deployment, so teams can launch new perpetual markets without waiting for Hyperliquid itself to list them. Outcome contracts extend the same model into prediction markets.


Outside these three, developers can always build around existing liquidity instead of having to bootstrap. This cuts the time and effort of building any application, letting teams focus on distribution, user experience, and new market designs.


Now we know how the Hyperliquid developer stack works, how to connect to Hyperliquid, how to access data, and what to build on it.


**Did you know?** Everything above is accessible from a single Quicknode endpoint.


## Getting Started Building on Hyperliquid With Quicknode


Building on Hyperliquid has never been easier, and Quicknode makes the developer experience seamless. Quicknode already supports the network with a full suite of tools, APIs, and guides that developers can access today with a single endpoint.


Quicknode's HyperCore integration, currently in public beta, promises infrastructure superpowers like:


-


Streams support for HyperCore datasets, enabling filtered, push-based delivery of Hyperliquid data


-


gRPC APIs designed for high-volume, low-latency data pipelines


-


JSON-RPC and WebSocket methods for compatibility with existing workflows


View current pricing:[quicknode.com/api-credits/hyperliquid](https://www.quicknode.com/api-credits/hyperliquid) .


**More Quicknode resources for Hyperliquid builders:**


1.


[Hyperliquid API Overview | Quicknode Docs](https://www.quicknode.com/docs/hyperliquid/api-overview)


2.


[Build a Real-Time Hyperliquid Whale Alert Bot](https://www.quicknode.com/guides/hyperliquid/real-time-hyperliquid-whale-alert-bot)


3.


[Read HyperCore Oracle Prices in HyperEVM](https://www.quicknode.com/guides/hyperliquid/read-hypercore-oracle-prices-in-hyperevm)


4.


[Real-time risk monitoring and liquidation analytics API for HyperLend Protocol on Hyperliquid](https://marketplace.quicknode.com/add-on/hyperlend-risk-api)


5.


[Comparison of the top 6 Hyperliquid RPC providers for HyperCore and HyperEVM](https://www.quicknode.com/blog/best-hyperliquid-rpc-providers-2026-full-comparison)


### SDKs & Tools


-


[Hyperliquid Python SDK](https://github.com/quiknode-labs/hyperliquid-sdk/tree/main/python) — also on[PyPI](https://pypi.org/project/hyperliquid-sdk/)


-


[Hyperliquid API Examples](https://github.com/quiknode-labs/hyperliquid-api-examples) — clone-and-run examples in Python, TypeScript, Rust, and Go


-


[Hyperliquid SDK Examples](https://github.com/quiknode-labs/hyperliquid-examples) — advanced SDK usage patterns


-


[HyperCore CLI](https://github.com/quiknode-labs/hypercore-cli) — stream and backfill HyperCore data from the terminal


-


[hyperliquidapi.com](https://hyperliquidapi.com/) — zero-custody Hyperliquid trading API


There is also a marketplace add-on that Quicknode offers for Hyperliquid builders: a real-time risk monitoring and liquidation analytics API for HyperLend Protocol on Hyperliquid.


Together, these resources give builders everything from RPC access to real-time event handling and offchain logic, making Quicknode the fastest path to launching apps, markets, and analytics on Hyperliquid.


## Frequently Asked Questions


### What is the Hyperliquid developer stack?


The Hyperliquid developer stack consists of three layers sharing one unified state: HyperBFT consensus, HyperCore (the native trading engine), and HyperEVM (an Ethereum-compatible smart contract layer).


### What is the difference between HyperCore and HyperEVM?


HyperCore provides native financial infrastructure such as order books and perpetual markets. HyperEVM provides smart contract functionality. Applications often use HyperEVM for logic while sourcing liquidity and market data from HyperCore.


### Is HyperEVM compatible with Ethereum tooling?


Yes. HyperEVM supports Foundry, Hardhat, Ethers.js, Viem, and Web3.js without modification.


### What are Hyperliquid builder codes?


Builder codes are an onchain fee attribution mechanism. Any application routing orders through Hyperliquid can attach and collect a fee of up to 10 bps on perps and 1% on spot, per order.


### What is HIP-3 on Hyperliquid?


HIP-3 allows developers to deploy permissionless perpetual markets on HyperCore. Deployers stake 500,000 HYPE, configure an oracle, and launch a perp market with its own order book and collateral.


### What is HIP-4 on Hyperliquid?


HIP-4 introduced fully collateralized outcome contracts to HyperCore in May 2026. It supports binary and multi-price markets settling in USDH, running on the same order book infrastructure as perps and spot markets.


### How do I get a Hyperliquid RPC endpoint?


Create a Quicknode account, select Hyperliquid from the endpoint dashboard, and copy the generated URL. The endpoint serves both HyperEVM and HyperCore from a single connection.


### What is the difference between /evm and /nanoreth on Quicknode?


` /evm` provides standard Ethereum JSON-RPC access to recent blocks only, with no WebSocket support.` /nanoreth` provides full historical archive access, debug and trace methods, and WebSocket support.


### About Quicknode


Founded in 2017, Quicknode deploys institutional-grade blockchain infrastructure for developers and enterprises. With 99.99% uptime and support for 80+ chains, teams build and scale onchain applications without compromise.


[Start building today](https://www.quicknode.com/)
