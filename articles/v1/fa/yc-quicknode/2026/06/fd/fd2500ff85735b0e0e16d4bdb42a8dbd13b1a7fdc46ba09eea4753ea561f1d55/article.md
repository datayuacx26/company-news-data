---
schema_version: "1.0.0"
document_id: "fd2500ff85735b0e0e16d4bdb42a8dbd13b1a7fdc46ba09eea4753ea561f1d55"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/pay3-real-time-payment-detection-streams"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-25T20:23:53.195520+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:8dc611d03ab34af98b2fe75bec925748be08d02e2dcebf295ad801d393f3ad6d"
---

# How pay3 Built Real-Time Payment Detection on Streams | Quicknode

#


From wallet addresses to payment links: how pay3 built real-time multi-chain detection on Quicknode Streams


pay3 replaced the wallet address with a single link, pay3.so/@username. Here's how it built real-time payment detection across Ethereum, BNB Smart Chain, Solana, and Tron on Quicknode Streams, with no indexer and no per-chain polling.


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


June 15, 2026 — 5 min read


For a stablecoin payment to feel like a real payment, the receiver needs to know the instant the money lands. Not in a minute. Now.


That is the whole point of pay3. A user shares one link, pay3.so/@username. The sender visits it, picks a token, and sends. The receiver sees a confirmed payment within seconds. No manual checking, no "did it go through?" Telegram messages, no refreshing a block explorer.


Before pay3, receiving crypto meant sharing a long wallet address, a string of 42 characters that changes per chain, breaks the moment one digit is wrong, and tells the sender nothing about whether they are even on the right network. pay3 replaces all of that with one readable link. But making that link actually work, detecting payments in real-time across every chain a sender might use, is the infrastructure problem.


Pulling that off is straightforward when you only care about one chain. pay3 cares about four: Ethereum, BNB Smart Chain, Solana, and Tron. That is where stablecoin users actually live. Tron for retail USDT transfers, Solana and BNB Smart Chain for newer and mobile-first users, and Ethereum for global and higher-value transfers. Four chains, all monitored in real-time, for every pay3 user's wallet addresses. That is the problem pay3 solved with Quicknode Streams.


"In payments, 'eventually' is the same as 'failed.' The moment someone sends you $500 in USDT, they expect you to know. Not in a minute, now."


Varun


Founder & CEO, pay3


## The challenge: a watchlist that grows with every user


When a user signs up for pay3, they add wallet addresses across multiple networks: EVM chains, Solana, and Tron. From that moment, pay3 has to know the instant anything lands at any of those addresses, on any of those chains.


Multiplied across pay3's user base, that is a watchlist that grows every time someone joins, and a monitoring problem that gets harder, not easier, with each new chain.


The obvious starting point was polling: periodically querying each chain for new transactions to monitored addresses. It burned RPC requests constantly, introduced unpredictable latency between a payment landing and the user notification, and across four chains became both expensive and unreliable. For a product where the entire UX promise is instant confirmation, a polling loop was not a foundation pay3 could build on.


## How Streams works for pay3


Streams is Quicknode's real-time data pipeline. You tell it which addresses, contracts, or events you care about, it watches the relevant chains, and it pushes you a webhook the moment something matches.


For pay3, the pattern is:


1.


A user signs up and adds their wallet addresses across EVM chains, Solana, and Tron.


2.


pay3 registers those addresses with Quicknode Streams, on every chain it supports.


3.


Streams continuously watches Ethereum, BNB Smart Chain, Solana, and Tron for any incoming transaction to any of those addresses.


4.


The moment a payment hits, Streams fires a webhook to pay3's backend.


5.


pay3's backend confirms the transaction, updates the payment status, and pushes a real-time notification to the receiver.


The receiver sees a payment confirmation within seconds of the transaction landing onchain. That is pay3's entire UX promise, a single readable link that replaces every wallet address on every chain, and the pipeline delivers it.


## Why this matters: four chains, including Solana and Tron


Solana and Tron are not nice-to-haves for pay3. They are load-bearing. Tron carries the largest share of pay3's retail USDT transfers, and ignoring it would mean ignoring where most of pay3's users actually send money. pay3 needed all four chains from day one, not EVM-first with the others bolted on later.


Streams supports all four pay3 chains in the same product, with the same registration model. Adding a chain to pay3's coverage does not mean adopting a new tool or rebuilding a pipeline. Ethereum went live first. Adding BNB Smart Chain, Solana, and Tron followed the same registration model, with no new tooling and no separate pipeline. Adding a chain is a config change, not a rebuild.


## The result


-


**Detection latency.** Streams fires a webhook within ~2 seconds of a payment landing onchain.


-


**Scale.** pay3 monitors 7,000+ wallet addresses across 4 chains, with $500K+ in stablecoin volume processed to date.


-


**Engineering footprint.** No in-house indexer, no per-chain polling jobs to maintain. A small team building a consumer payments product stays focused on the product instead of babysitting blockchain infrastructure.


-


**Coverage.** EVM, Solana, and Tron, handled in one product.


## What's next: merchants


pay3 recently started rolling out merchant-side integrations. The interesting part for this story: the same Streams pipeline that handles consumer payments also underpins the merchant side. The detection layer is the same. What pay3 does with the webhook downstream is what changes.


pay3 is beginning to work with businesses that want to accept stablecoins from customers without paying a percentage cut on every transaction. The zero-fee model matters as much on the merchant side as the consumer side. The webhook is identical to the consumer pipeline. More details as the merchant rollout develops.


Our users share one link, pay3.so/@username, and their sender just pays. No addresses, no chain confusion, no 'did it go through?' messages. They just see 'payment received.' Quicknode Streams is what makes that real across four chains simultaneously, and it's the same infrastructure we're now extending to merchants without rebuilding a thing.


Varun


Founder & CEO, pay3


## About pay3


pay3 makes sending stablecoins as simple as sharing a link. Built by Todayq, an India-based team that has been shipping products for the crypto community since 2017. pay3 was founded by Varun, who dropped out of school at 16 to build in crypto. The same frustration with broken wallet UX that drove him then is what pay3 is solving now.


## Build real-time, multi-chain products on Quicknode


Quicknode is the complete blockchain development platform. Streams pushes real-time onchain data to your backend across 80+ chains from a single integration, so your team ships product instead of maintaining monitoring infrastructure.


****[Start building for free](https://dashboard.quicknode.com/?prompt=signup) **·**[Contact sales](https://www.quicknode.com/contact-us)


### About Quicknode


Founded in 2017, Quicknode deploys institutional-grade blockchain infrastructure for developers and enterprises. With 99.99% uptime and support for 80+ chains, teams build and scale onchain applications without compromise.


[Start building today](https://www.quicknode.com/)
