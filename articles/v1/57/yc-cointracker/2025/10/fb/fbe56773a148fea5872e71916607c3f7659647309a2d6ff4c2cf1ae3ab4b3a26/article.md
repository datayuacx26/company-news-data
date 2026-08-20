---
schema_version: "1.0.0"
document_id: "fbe56773a148fea5872e71916607c3f7659647309a2d6ff4c2cf1ae3ab4b3a26"
company_key: "yc-cointracker"
company: "CoinTracker"
source_id: "yc-cointracker-news-import-8eddc0d2ffc8"
canonical_url: "https://www.cointracker.com/blog/how-we-made-adding-evm-chains-25x-faster"
published_at: "2025-10-14T00:00:00+00:00"
first_seen_at: "2026-07-24T23:51:12.353224+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:0be2c0caebf388ba56f57359c9ae008c95597d797a7e02110fb74835d5de6e8b"
---

# How we made adding EVM chains 25x faster

CoinTracker’s mission is to[enable everyone to use crypto with peace of mind](https://www.cointracker.io/about?ref=blog.ghost.cointracker.io#:~:text=Our%20mission%20is%20to%20enable,of%20every%20decision%20we%20make.) . In practice, this means handling the tax and tracking side of crypto seamlessly while users focus on their activities across wallets, exchanges, and blockchains.


To deliver on this mission, CoinTracker must support a wide variety of integrations: exchanges, blockchains, CSV uploads, and anything else that represents a user’s crypto activity. This leads to a classic fan-out problem. New blockchains and exchanges appear constantly. On top of that, new DeFi protocols, transaction types, assets, and crypto concepts emerge every year.


One of the most common requests we hear from customers is to support “chain X” or “exchange Y.” Meeting these requests requires a substantial amount of work.


Adding a new chain requires understanding how it works, classifying on-chain activity, and ensuring chain specific assets are properly supported.


We think about integrations across four dimensions:


- **Breadth** : ingesting basic transactions and assets for new blockchains or exchanges
- **Depth** : supporting advanced features and protocols beyond the basics
- **Quality** : ensuring correctness in balances, classifications, and data transformations
- **Scalability** : reliably handling transaction volume at scale


On top of this, integrations need continuous maintenance since blockchains and exchanges evolve and new issues inevitably surface.


## Background on EVM chains


EVM chains are blockchains that use the Ethereum Virtual Machine to process transactions and execute smart contracts. Because the EVM specifies a standardized execution environment, a smart contract or dapp built for one EVM chain will usually run on another with little or no change. Developers can deploy the same Solidity contract on Ethereum, Arbitrum, Optimism, Polygon, Avalanche C-Chain, and others without rewriting the logic. This interoperability is possible because the EVM is deterministic: given the same bytecode and input, it will always produce the same output,` f(X) → Y` . That determinism is what ensures all nodes across the network reach consensus on blockchain state. Most EVM chains got their start by forking Ethereum’s open-source software and then tweaking a few details, such as how they confirm transactions, how fast blocks are created, or how fees are handled.


## Adding new EVM chains should be easy right?


In the early days, CoinTracker started simple, with support for Ethereum. Things worked smoothly at first. As the ecosystem grew, we added bits of custom code to handle new protocols. Those quick fixes worked well enough at the time.


Then came new EVM chains.


When Binance Smart Chain and Polygon appeared, the fastest path seemed to be copying our Ethereum integration and tweaking it. It felt efficient in the moment, but it scattered multiple versions of the same logic across the codebase and quietly built up tech debt.


In theory, adding a new EVM chain *should* have been straightforward. After all, they all use the same external provider, with identical APIs, response shapes, and transformations into our internal transaction models.


⁉️ One integration ended up at **+165 / -3 lines changed and 25 files modified** . ⁉️


Engineers had to update dozens of different areas across the codebase, spanning core data models, external integrations, configuration layers, supporting assets, and test coverage.


Because even one missed update could break the integration, only a handful of engineers knew the full set of changes required. This knowledge silo made the process slow, error-prone, and hard to scale. For users, the result was long delays in being able to add their requested integration as a new chain request could take weeks or even months before it reached production.


## The turning point


Since the differences among EVM chains are minimal, the scattered logic created unnecessary overhead and complexity. It would make sense to use a **centralized, config-driven approach** that all components could reference. With this config, only one file would need to be updated and the rest of the components would extract the necessary data automatically.


## Refactoring the right way


We built this config from the ground up. Each new field in the config required its own PR, since we had to carefully extract values and rewrite logic from existing files and migrate them into the config. Some migrations were straightforward, others needed special handling, and a few exposed underlying tech debt.


Luckily, we were able to rely on our existing unit tests to ensure that functionality didn’t break during refactoring. These tests validated API responses, error handling, and data transformations, which were consistent across all chains. As part of the cleanup, we consolidated duplicate tests into a single, streamlined test file and moved any chain-specific logic into separate one-off files. Being able to leverage our existing test suite was a huge win, giving us confidence throughout the migration.


After several refactor PRs were merged, the speed of adding new EVM chains increased dramatically.


Immediately, we were able to onboard and thoroughly test five chains in quick succession.


At that point, the most time-consuming part of chain addition was no longer writing code but testing the chain locally.


To make things even more efficient, we split up the work. Since no coding knowledge was required, we created a structured template where anyone could enter the config values for each chain which we then transformed into code. This opened the door for contributions from the entire team, not just engineers.


Before → **After**


- 26 files → **2 files**
- 165 lines → **56 lines**


## Unexpected benefits


We also saw some unexpected benefits from moving to a config-driven approach. Along the way, we were able to retire a significant amount of tech debt. Components that no longer needed updates were removed, and others were rewritten to be more efficient.


Our testing practices improved as well. Instead of duplicating test cases across chains, we began using[VCR](https://github.com/kevin1024/vcrpy?ref=blog.ghost.cointracker.io) , a tool that records real API responses once and then replays them for future test runs. This gave us a consistent and reliable set of fixtures without needing to hit third-party providers every time. It also made generating chain-specific test fixtures straightforward, which in turn increased our test coverage and confidence.


Finally, the project sparked deeper conversations about the next level of refactoring. It opened the door for broader architectural discussions about how best to evolve our ingestion and transformation layers to be more scalable and resilient


## Takeaways


The old way of adding EVM chains wasn’t inherently complex, but it was messy. Each new chain required updates across dozens of components, which made the process slow, error-prone, and restricted to a handful of engineers. By reducing this mental overhead with a config-driven system, we turned a scattered process into one that anyone, developer, contractor, or PM, can safely contribute to.


0:00 /0:01 1×


One key lesson for us is that small architectural changes can have an outsized product impact **.** The config-driven approach didn’t add new features, but it unlocked speed, reliability, and flexibility that directly benefit users. Refactors like this may seem invisible at first, but they pay dividends by accelerating the product roadmap.


The benefits also extend to the team. With a single source of truth, onboarding new engineers is easier, silos are eliminated, and even non-engineers can participate in expanding chain support. By reducing cognitive load, we made chain additions 25x faster, going from one per week to five in a single day, and also made the development process more collaborative.


We’ve already applied this same approach to other “family” chains like Polkadot, and we see opportunities to extend it to exchanges, wallets, and beyond. Looking ahead, a dream scenario would be for providers themselves to unify their APIs and deliver a truly standard data surface across blockchains. Until then, our config-driven approach brings us as close as possible to that future.


Ultimately, the biggest winners are our users. Faster integrations mean users can track their assets across more chains without waiting months for support, and they get peace of mind with the accuracy, reliability, and speed that CoinTracker is committed to delivering.


We’d love to hear from you![What chain should we add next?](https://cointracker.canny.io/product-feedback?ref=blog.ghost.cointracker.io)
