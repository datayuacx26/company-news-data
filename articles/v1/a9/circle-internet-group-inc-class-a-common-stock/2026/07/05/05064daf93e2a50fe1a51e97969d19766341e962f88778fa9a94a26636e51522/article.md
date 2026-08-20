---
schema_version: "1.0.0"
document_id: "05064daf93e2a50fe1a51e97969d19766341e962f88778fa9a94a26636e51522"
company_key: "circle-internet-group-inc-class-a-common-stock"
company: "Circle Internet Group Inc."
source_id: "circle-internet-group-inc-class-a-common-stock-news-import-6b20a10366df"
canonical_url: "https://www.circle.com/blog/relay-scales-onchain-payments-with-less-idle-capital-on-circle-gateway"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:44.770994+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:e9c053715eff0b05bda23ee6f20ca04b710f6ef5ff43a76b211e83e9d8e7719b"
---

# Relay Scales Onchain Payments with Circle Gateway | Circle

Crosschain payments only feel instant if the dollars are already where the payment needs to land. In the past, to settle the moment a user taps “pay,” networks have had to hold liquidity on the destination chain in advance. Spread that across dozens of chains and the economics get hard: capital sits pre-positioned everywhere, and the lowest-traffic chains are where parking that capital makes the least sense.


[Relay](https://www.relay.link/) is crosschain payments infrastructure that lets any business accept payment from any chain, in any token, with one integration. Since launching in 2024, it has scaled to more than $40 billion in crosschain volume. Over the same period, Relay supported 186 million transactions across 85 blockchains with a 99.9% success rate. Its customers — from payment processors and wallets to exchanges, neobanks, and commerce platforms — integrate Relay once and let their own users pay across chains.


Previously, to keep crosschain settlement nearly instant, Relay maintained[USDC](https://www.circle.com/usdc) inventory chain by chain — an approach that carried two costs. To cap risk, Relay deliberately held only a limited amount of USDC on any given chain, which meant a large order could exceed that inventory and have to wait. And on lower-demand chains, where locking up liquidity made little sense, a payment could wait while Relay rebalanced funds.


As payment volume grew, those two costs grew with it.


## Settling from one balance instead of many with Circle Gateway


Relay rebuilt that liquidity model with[Circle Gateway](https://www.circle.com/gateway) . Gateway gives Relay a single[unified USDC balance](https://www.circle.com/blog/circle-gateway-redefining-crosschain-ux) it can spend across any supported chain. Instead of fragmenting reserves across every network, Relay now holds a portion of its USDC in Gateway as one balance, spendable across the 12 Gateway-supported chains it routes through. As of June 2026,[Gateway is supported](https://developers.circle.com/gateway/references/supported-blockchains) on Arbitrum, Avalanche, Base, Ethereum, HyperEVM, Optimism, Polygon PoS, Sei, Solana, Sonic, Unichain, and World Chain.


When a destination chain’s inventory runs low after a high-volume period, Relay draws from that shared balance and replenishes the chain in under 500 milliseconds, so a payment that would previously have waited for a rebalance settles right away.


“Before Gateway, our USDC was only as useful as the chain it happened to be sitting on,” said Julien Genestoux, Relay’s Head of Engineering. “Now it's one balance that backs payments wherever they need to settle — so big orders and thin chains stopped being a bottleneck, and we do it with less capital sitting idle.”


By routing payments through Gateway, Relay can:


- Hold USDC as one[unified, chain-abstracted balance](https://www.circle.com/blog/coming-soon-circle-gateway-and-the-future-of-chain-abstracted-usdc) instead of fragmenting reserves across every network
- Replenish a destination chain on demand in under 500 milliseconds
- Support larger payments that once exceeded the inventory Relay would keep on any single chain


Relay’s routing decides, payment by payment, whether to settle from per-chain inventory or the Gateway pool, and that logic stays internal. The design is non-custodial: USDC in transit is secured by a user signature and a Gateway attestation, and Relay does not hold user funds.


For Relay’s customers, the integration cost is effectively zero. They never touch Gateway directly, inheriting its large-payment capacity, multichain USDC coverage, and sub-500-millisecond replenishment through that single integration.


## More volume backed by less idle capital


For Relay, the headline of its Gateway integration is increased capital efficiency. Because USDC no longer has to be pre-committed chain by chain, liquidity that once sat parked across dozens of chains now works as one balance that moves to wherever a payment needs to settle, letting Relay back more volume with the same or less working capital.


From March 2026 through June 2026, approximately $410 million in crosschain Relay user volume settled via Gateway. Notably,[everyday settlement](https://www.circle.com/blog/settlement-vs-clearing-the-hidden-plumbing-of-payments) on well-funded routes remained at a median of under three seconds. What changed is that a class of orders that used to stall now clears immediately.


For the businesses building on Relay, the effect is increased reliability. Large payments settle, routes recover faster after volume spikes, and performance holds steady across chains, all of it invisible to the end user who simply sees the payment go through.


## A liquidity layer that scales with the network


Gateway today spans[12 of the more than 85 chains](https://developers.circle.com/gateway/references/supported-blockchains) Relay supports, leaving room to route more volume through the unified balance as coverage expands. The direction is straightforward: the more of the network that can draw on one shared USDC reserve, the less working capital has to sit pre-positioned to keep payments instant.


Relay’s Gateway integration shows that for any payments platform settling across many chains, treating crosschain USDC as a single balance — rather than dozens of parked reserves — is a repeatable way to grow volume without growing the capital tied up to support it.


‍
[Get started with Circle Gateway](https://developers.circle.com/gateway)


Reference to any specific company, product, service, or website of any third party does not constitute an implied or express endorsement, recommendation, favoring or validation by Circle. The content presented is intended for informational purposes only. Reliance upon any content or information presented is at the sole discretion of the audience; Circle shall not be liable for any damage or loss relating to the use of or reliance upon any such content or information presented. The views and opinions expressed herein do not necessarily state or reflect those of Circle.


Circle Technology Services, LLC ("CTS") is a software provider and does not provide regulated financial or advisory services. You are solely responsible for services you provide to users, including obtaining any necessary licenses or approvals and otherwise complying with applicable laws. For additional details, please see the Circle Developer terms of service, available at console.circle.com/legal/developer-terms.


USDC is issued by regulated affiliates of Circle. See[Circle’s list of regulatory authorizations](https://circle.com/legal/licenses) .


‍
