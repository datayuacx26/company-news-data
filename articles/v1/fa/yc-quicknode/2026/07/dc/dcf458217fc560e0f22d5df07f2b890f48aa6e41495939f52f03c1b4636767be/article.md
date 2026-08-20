---
schema_version: "1.0.0"
document_id: "dcf458217fc560e0f22d5df07f2b890f48aa6e41495939f52f03c1b4636767be"
company_key: "yc-quicknode"
company: "Quicknode"
source_id: "yc-quicknode-news-import-d0b2f3935987"
canonical_url: "https://www.quicknode.com/blog/introducing-the-quicknode-earn-api-automated-usdc-yield-in-your-app"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-31T17:05:20.319078+00:00"
fetched_at: "2026-07-31T17:05:21.899177+00:00"
content_hash: "sha256:614df1b6a342b5873a673fe57b48ac24d66047c523d3b8bd5fb54cef6b7c3f15"
---

# Earn API: Automated USDC Yield in Your App | Quicknode

#


Introducing the Quicknode Earn API: Automated USDC Yield in Your App


Create, activate, modify, monitor, and close Earn strategies programmatically. Non-custodial, live on 7 chains.


[Quicknode](https://www.quicknode.com/blog/author/quicknode)


July 29, 2026 — 4 min read


Adding USDC yield to a product usually means vault due diligence, rebalancing logic, cross-chain bridging, and months of work before the first deposit. Today we're changing that. The **Earn API** lets your code do everything the Earn app does, over HTTP.


## What is the Earn API?


Every five minutes, Quicknode Earn ranks every eligible Morpho vault across 7 chains and moves USDC when a better rate clears the strategy's rules, without ever taking custody. The Earn API is that engine, exposed as a public API: the whole strategy lifecycle in the order you'd actually call it, while Quicknode keeps running the ranking, the rebalancing, and the cross-chain moves. There's nothing new for your team to operate.


## Why the Earn API is the easy way to ship yield


-


**One integration, the whole lifecycle** **** Browse and rank the approved vaults, record the one-time terms agreement, sign any missing approvals from templates the API returns, then create the strategy with your rules. Activate it with one ready-to-sign deposit transaction covering every chain in the strategy, cross-chain transfers included, and poll the intent until it clears.


-


**Auth for wallets, not accounts** **** Reads are public, no signature required. Writes carry a per-action Sign-In-With-Ethereum proof, and the wallet that signs is the owner — the recovered address, not a field in the request body. No user model to map, no session to hold, and it works with whatever wallet layer you already shipped: EOAs, Safes, or smart account wallets.


-


**Non-custodial by architecture** **** Vault shares mint to the wallet that signs, never to Quicknode. Onchain, the rebalancer can only move funds between approved vaults or back to the owner's wallet, and the owner can always exit directly on Morpho without permission from Quicknode. If a cross-chain deposit ever stalls, an emergency-claim endpoint mints the USDC straight back to the owner.


-


**Your rules, our infrastructure** **** Set the rate improvement a move must clear, the TVL and liquidity floors a vault must pass, how many vaults to spread across, and which to exclude outright. Cross-chain moves can clear a separate, higher bar. Pause, resume, and tweak settings anytime.


-


**A gas-cost fee, never a cut of your yield**
The API costs nothing extra. Fees are tied to the gas cost of each rebalance, and a principal-erosion guard blocks any rebalance that would cut into original capital. The[fee documentation](https://earn.quicknode.com/docs/how-it-works/fees-and-gas) has the detail.


## What can you build with the Earn API?


-


**Wallet & mobile apps:** A savings feature in something people already use. Behind account abstraction, a strategy looks like a plain balance that grows.


-


**Payments & treasury products:** Idle balances earning between settlements instead of sitting still.


-


**Agents:** Everything is in the spec, including the signing. An agent with a wallet and the reference can run the whole lifecycle.


-


**Your own capital:** Scripts and internal tooling, no interface at all.


## Ready to get started?


Everything the app does, your code can do.[Read the API reference](https://earn.quicknode.com/api) and create your first strategy today.


Browse the[live vaults](https://earn.quicknode.com/vaults) , try it in[the app](https://earn.quicknode.com/) , or read the[Earn launch post](https://www.quicknode.com/blog/introducing-quicknode-earn-morpho-yield-on-autopilot) for how the underlying product works.


---


***Disclaimer.*** *Quicknode Earn is non-custodial: you keep custody of your assets at all times, and they are never transferred to Quicknode. Yield rates are variable and set by onchain market conditions, with no guaranteed return. Deposits carry smart-contract and DeFi risk, including bugs or exploits that could cause partial or total loss, and the underlying Morpho vaults operate independently and may be subject to governance changes, liquidity crises, or insolvency. Nothing here is investment, financial, tax, or legal advice, and the Earn Services are provided "as is" without warranties. You are solely responsible for evaluating the risks and for reviewing every transaction before you sign it. See the full Terms.*


### About Quicknode


Founded in 2017, Quicknode deploys institutional-grade blockchain infrastructure for developers and enterprises. With 99.99% uptime and support for 80+ chains, teams build and scale onchain applications without compromise.


[Start building today](https://www.quicknode.com/)
