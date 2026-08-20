---
schema_version: "1.0.0"
document_id: "ce9e11e3228a4c9a6480c652f2d53ee0f2c2d73a092721ac6d3ef9881fc24dad"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/agentic-commerce"
published_at: "2025-09-18T00:00:00+00:00"
first_seen_at: "2026-07-21T23:41:43.316828+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:170de90d594c5b0c4673beea3d13489cd6ce9ce48442efba6ad65684b32ca513"
---

# Agentic commerce — the Increase perspective — Increase

Agents based on large language models are doing more and more for us. Empowering them to make payments is a live frontier.


Agentic commerce is often cited as the breakthrough use case for stablecoin payments. See[Why Agentic Commerce Needs Crypto to Scale](https://www.coinbase.com/developer-platform/discover/launches/agentic-commerce) from Coinbase. Stripe’s new[Tempo blockchain](https://www.paradigm.xyz/2025/09/tempo-payments-first-blockchain) is built with a similar motivation.


At Increase, we have a boring perspective: the existing rails are best.


## A consumer use case


I recently took a summer vacation to the Alps. ChatGPT did much of the heavy lifting in finding hikes, hotels, restaurants, and train tickets. With due affection for the rigor and thoroughness that Europeans bring to building web forms, I would have loved to have ChatGPT book everything for me. It can’t today, but soon it will. And when it does, I will want it to use my existing credit cards.


Partly this is for the rewards and benefits; the partner-specific benefits were one criterion I asked ChatGPT to consider in its research.


But beyond the rewards, the biggest reason is the protection of Visa’s dispute machinery. As agents become targets for scammers, I want the full dispute apparatus of the card networks behind me.


I expect product innovation from card issuers and acquirers here. If I were building this, I might spin up per-purchase virtual cards with a nice user-facing approval flow. Just as cards fight to be top of wallet, they’ll fight to be the card you give to ChatGPT.


## A commercial use case


Businesses of all sizes receive a large volume of invoices. Validating these is toilsome: Did we really authorize this? Is this invoice early? Is this the negotiated amount? Is someone impersonating our vendor?


This kind of checklist is reasonable for an agent to track down; but when it comes time to move money, they’ll likely use the same payment rails the operator would in the first place. Most of the time, that’s cards or bank transfers. That’s where payee preferences lie, and it’s where dispute protection lives.


By the way, this is a real use case today. Ramp is doing some amazing work around agent-powered bill pay. If this is a problem you have, we highly recommend signing up:[https://ramp.com/bill-pay](https://ramp.com/bill-pay) .


## A speculative use case


Cloudflare recently[announced](https://blog.cloudflare.com/introducing-pay-per-crawl/) a pay-per-crawl blocker:


> Imagine asking your favorite deep research program to help you synthesize the latest cancer research or a legal brief… and then giving that agent a budget to spend to acquire the best and most relevant content.


It’s early days, but intuitively this use case feels real. Today, valuable data is monetized in an all-or-none fashion designed for human readers: newspaper subscriptions, Bloomberg terminals, trade journals.


A potential challenge for existing rails here is cost. One could argue that stablecoins are a better fit for the market-clearing price of many of these transactions. But as with any low-dollar usage-based product, I suspect that these kinds of payments are likely to be settled in batches as part of broad commercial relationships.


## Wrapping up


Hypotheses should be falsifiable, so here’s ours: payments initiated by agents are going to skyrocket over the next two years, but the funding mix won’t look much different than it does today.


That agentic commerce *doesn’t* require reinventing money movement is a reason we expect it to take off. The existing rails work. What matters is giving agents programmable, low-level access to them and building the dispute protections and workflows that make them usable at scale.
