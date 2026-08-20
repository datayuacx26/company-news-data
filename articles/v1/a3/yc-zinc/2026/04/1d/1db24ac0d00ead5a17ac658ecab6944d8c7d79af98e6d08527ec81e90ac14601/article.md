---
schema_version: "1.0.0"
document_id: "1db24ac0d00ead5a17ac658ecab6944d8c7d79af98e6d08527ec81e90ac14601"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/mpp-vs-x402"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:cb8a620bdc73601cb7889fd657e47ed1bb8142489d77eda9718ab3c51ee2361b"
---

# MPP vs x402: Which Agent Payment Protocol to Use?

Two payment protocols shipped within months of each other, both reviving the same 30-year-old HTTP status code (` 402 Payment Required` ) to let AI agents pay programmatically. Both are open standards. Both are production-ready. Both are backed by Stripe in one way or another. And they solve overlapping but not identical problems.


This guide compares MPP (Machine Payments Protocol, Stripe + Tempo, March 2026) and x402 (Coinbase, V2 launched December 2025) on architecture, payment rails, cost, production readiness, and real use cases. At the end, we'll show which protocol to pick for which scenarios, and why services accepting agent payments often support both.


---


Zinc Agent


Give your AI agent commerce powers


Connect an agent to Zinc to search products, place orders, track shipments, and handle returns across top retailers.


[Try Zinc Agent](https://agent.zinc.com/)[Read the docs](https://www.zinc.com/docs)


Amazon order


In transit


POST


/orders


Product


AirPods Pro


Tracking


webhook sent


Returns


label ready


## MPP vs x402 at a glance


**MPP** **x402**


**Created by** Stripe + Tempo Coinbase (x402 Foundation with Cloudflare)


**Launched** March 18, 2026 V2 December 2025


**Payment rails** Stablecoin (Tempo), fiat (Stripe SPTs), cards (Visa), Bitcoin (Lightning) Stablecoins only (USDC primary)


**Networks** Tempo blockchain Base, Ethereum, Polygon, Solana, Avalanche, Sui


**Protocol fees** None native; Stripe fees apply on fiat Zero (only on-chain gas)


**Key primitive** Sessions (streaming micropayments) Single-request HTTP 402 with signed payment


**Best for** Physical goods + services + API payments, multi-rail, enterprise controls Pure crypto-native API micropayments


**Enterprise features** Tax, fraud, refunds via Stripe Minimal (crypto-native)


**Production adoption** 100+ integrations at launch (Visa, Mastercard, OpenAI, Anthropic, Shopify, Zinc) Live; ~3.7M tx / ~$1.1M in a recent 30-day window (x402scan), volatile


---


## The shared foundation: HTTP 402


Both protocols work the same way at the transport layer.


An agent requests a resource. The server responds with` HTTP 402 Payment Required` and payment instructions (amount, currency, destination). The agent signs and attaches a payment, retries the request, and gets the resource.


[The HTTP status code 402](https://www.zinc.com/blog/http-402-agentic-commerce) has been reserved in the HTTP spec since 1997 and was never widely implemented. x402 and MPP are the two serious attempts at turning it into a real protocol, driven by a new category of user (AI agents) that doesn't work with traditional checkout forms.


The difference is what happens after the 402 response:


**x402** : Agent signs an on-chain payment, attaches it to the retry request, server verifies on-chain and delivers.


**MPP** : Agent selects a payment method (stablecoin, fiat via SPT, card, or Lightning), handles it through the appropriate rail (Tempo for stablecoin, Stripe for fiat/card, Lightspark for BTC), and resubmits with a credential.


Same HTTP shape. Different payment infrastructure underneath.


---


## MPP in detail


MPP is Stripe's bet that agents will pay across multiple rails, not just crypto. It was designed with Tempo, an EVM-compatible blockchain that Stripe built specifically for payment primitives. Tempo has no native gas token - fees are paid in the stablecoin being transacted, through an integrated AMM.


**What MPP is built for:**


- Agents that pay for a mix of APIs, services, and physical goods
- Integration with existing Stripe accounts (payments flow into your normal balance)
- Enterprise financial infrastructure (tax calculation, fraud protection, refunds, accounting)
- Sessions model: pre-authorize a spending limit once, stream many micropayments
- Multi-rail from day one (stablecoin + fiat + cards + BTC Lightning via extensions)


**Example 402 response with MPP:**


```text
HTTP/1.1     402     Payment Required
Content-Type  :     application/json
WWW-Authenticate  :     tempo realm="zinc" charge="ch_..." amount="25.99" currency="usd"
WWW-Authenticate  :     stripe realm="zinc" charge="ch_..." amount="25.99" currency="usd"


{
"error"  :     {
"code"  :     "payment_method_required"  ,
"details"  :     {
"methods"  :     [
{     "method"  :     "tempo"  ,     "amount"  :     "25.99"  ,     "currency"  :     "usd"     }  ,
{     "method"  :     "stripe"  ,     "amount"  :     "25.99"  ,     "currency"  :     "usd"     }
]
}
}
}
```


Services can advertise multiple rails. Agents pick what they can pay with.


**Production use cases:**


- [Zinc](https://www.zinc.com/) : programmatic orders at Amazon, Walmart, Target, 50+ retailers
- Browserbase: agents spin up headless browsers and pay per session
- Parallel Web Systems: per-API-call web access
- Prospect Butcher Co.: sandwich orders for pickup or delivery in NYC
- PostalForm: agents pay to print and mail physical letters


For a full walkthrough, read[What Is MPP? A Developer's Guide to Machine Payments](https://www.zinc.com/blog/what-is-mpp) .


---


## x402 in detail


x402 is Coinbase's bet that stablecoin payments are sufficient for the machine economy, and that the HTTP-native experience beats traditional PSP infrastructure for agents.


**What x402 is built for:**


- Fully crypto-native agents that already hold wallets
- Ultra-low-value transactions where Stripe's 2.9% + 30¢ is too high
- Services that don't need tax, fraud, or refund infrastructure
- Maximum neutrality (x402 Foundation is co-governed with Cloudflare, not controlled by a single vendor)
- One-line integration for servers that accept it


**Example x402 middleware (Node.js):**


```text
import     {   paymentMiddleware   }     from     'x402-express'  ;


app  .  use  (
paymentMiddleware  (  {
'GET /weather'  :     {
accepts  :     [
{     network  :     'base'  ,     asset  :     'USDC'  ,     amount  :     '0.01'     }  ,
{     network  :     'solana'  ,     asset  :     'USDC'  ,     amount  :     '0.01'     }  ,
]  ,
description  :     'Real-time weather data'  ,
}  ,
}  )
)  ;
```


A service adds one middleware line, specifies accepted networks and assets, and it now accepts agent payments. No account creation, no API key issuance, no merchant registration.


**Production usage:** x402 is live and processing real volume, but far less than the early hype implied, and it's volatile. The public[x402scan](https://www.x402scan.com/) explorer shows roughly 3.7M transactions and ~$1.1M settled over a recent 30-day window, at an average around $0.30 per call - down from a late-2025 peak (roughly $5M in monthly volume) as early adopter campaigns tapered off. Treat any single number as a snapshot: x402 activity is workload-driven and swings 30-50% month to month depending on which agent services are active.


**x402 adopters** : Stripe, AWS, Messari, Alchemy, Nansen, Vercel, Cloudflare, World. Stripe specifically integrated x402 for USDC on Base in February 2026, then launched MPP alongside it in March.


---


## Where they overlap


Both protocols handle the core case of "agent calls an API, pays, gets response" equally well. If all you need is:


- An agent that pays for API calls
- Payments in USDC
- No enterprise financial controls
- Low-value per-request pricing


Either works. x402 might be simpler (single rail, single protocol); MPP gives you a migration path to fiat later.


---


## Where they diverge


### Payment rails


**x402** : Stablecoins only. If an agent doesn't hold USDC (or equivalent), it can't pay. Great for crypto-native use cases, but limits the user population.


**MPP** : Multi-rail from day one. An agent with a Stripe Shared Payment Token can pay MPP services in fiat. An agent holding USDC on Tempo can pay the same services in stablecoin. Cards via Visa's extension. Bitcoin via Lightspark. More rails, more agents can use it.


For services like Zinc that need to bill across both crypto-native agents and fiat-funded corporate buyers, the multi-rail capability is decisive.


### Enterprise financial controls


**x402** : Minimal. You get on-chain settlement. Tax, fraud prevention, refunds, accounting integrations are all on you. For most crypto-native builders, this is fine.


**MPP** : Inherits Stripe's full stack. Payments appear in the Stripe Dashboard. Tax calculated automatically. Fraud scoring runs. Refunds go through the same flow as any Stripe charge. Accounting reconciliation works out of the box.


If you're running a business that needs tax reporting and refund flows, MPP removes weeks of integration work.


### Cost structure


**x402** : Zero protocol fees. Pay only on-chain gas (fractions of a cent on L2s like Base). Cheapest option for crypto-native micropayments.


**MPP** : Zero protocol fees on-chain for Tempo stablecoin settlement. Standard Stripe processing fees apply to fiat/card SPTs (2.9% + 30¢ typically). Lightspark and Visa have their own fee structures.


For a pennies-per-request API, x402 is materially cheaper. For physical goods and services where the fee is a small percentage of the transaction, MPP's Stripe integration value outweighs the fee difference.


### Sessions model


**x402** : One HTTP round trip, one on-chain payment. Fine for infrequent requests. Expensive for high-frequency streaming workloads (each payment is a blockchain transaction, gas costs add up).


**MPP** : Sessions. Pre-authorize a spending cap once, stream many micropayments within that cap without per-request on-chain overhead. Critical for agents that make thousands of tiny calls (LLM inference, streaming data, continuous compute).


If your service bills per-token or per-millisecond, MPP's session model is a significant advantage.


### Neutrality and governance


**x402** : Coinbase-created, now co-governed with Cloudflare under the x402 Foundation. Broadly adopted (Stripe, AWS, Vercel, Cloudflare, Alchemy). Neutral in theory and practice.


**MPP** : Stripe + Tempo. Tempo is a Stripe-adjacent chain. Neutrality is weaker - you're accepting Stripe's infrastructure as a dependency.


For builders who distrust Stripe lock-in, x402 is the more neutral choice. For builders already on Stripe, MPP's integration is a feature, not a bug.


---


## When to use which


### Use x402 if:


- Your agents pay for APIs, compute, or data feeds
- You want zero protocol fees and crypto-native settlement
- You're building in the Coinbase/Base/Ethereum ecosystem
- You don't need fiat, card, or Lightning support
- Simplicity and neutrality matter more than enterprise features


### Use MPP if:


- Your agents pay for a mix of APIs, services, and physical goods
- You need fiat, cards, or Bitcoin support alongside stablecoins
- You're already on Stripe and want a unified dashboard
- You need enterprise financial controls (tax, fraud, refunds)
- You have high-frequency streaming workloads that benefit from sessions


### Use both if:


- You're an infrastructure provider and want to serve all agent types
- You're a service accepting payments (supporting both maximizes reachable agents)
- You're building a shopping agent that needs maximum flexibility


Stripe supports both MPP and x402 in their own payment infrastructure. Most serious agent platforms support both on the accepting side. For client-side agents, you can often pick one based on your wallet and integration preferences.


---


## What neither protocol solves


Both protocols are about paying for something. Neither handles what happens after the payment, especially when that something is a real-world physical good.


**Neither MPP nor x402 can place an order at Amazon, Walmart, Target, or any major retailer on its own.** They handle the payment handshake between the agent and a service. If the service happens to be a checkout flow at a retailer, great - but you still need someone to run the checkout.


That's why execution-layer infrastructure like[Zinc](https://www.zinc.com/) exists. Zinc accepts both MPP and x402 for retail orders and translates the payment into real orders placed at 50+ retailers. The agent sends the payment request; Zinc handles the retailer-specific checkout, bot protection, account management, and shipping - none of which either protocol touches.


---


## Bottom line


x402 and MPP are competitors. They share HTTP 402 as the foundation and target the same core job - an agent paying a service over HTTP - so for most decisions you're choosing between them, not stacking them.


x402 is the right choice for pure crypto-native, zero-fee, low-friction API micropayments. MPP is the right choice for multi-rail, enterprise-friendly, session-based commerce that includes physical goods and services.


The "support both" advice only applies to one side of the market: **services accepting payments** , where advertising both rails in your 402 response maximizes the agents that can pay you. Stripe does this;[Zinc](https://www.zinc.com/) accepts both MPP and x402 for retail orders. If you're building the agent (the client side), you generally pick one, based on the rails your wallet and users actually support.


To see an agent place a real retail order, try[agent.zinc.com](https://agent.zinc.com/) . To learn more about MPP specifically, read[What Is MPP? A Developer's Guide to Machine Payments](https://www.zinc.com/blog/what-is-mpp) or[Zinc MPP Integration](https://www.zinc.com/blog/mpp) for the product walkthrough. For the broader context on where both protocols fit in the agentic commerce stack, read[Agentic Commerce in 2026: The Complete Developer Guide](https://www.zinc.com/blog/agentic-commerce) . To build something that uses these protocols end-to-end, follow our[AI shopping agent tutorial](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) .


---


## Frequently Asked Questions


### Are MPP and x402 competing protocols?


Yes. They're two overlapping standards for the same job - an agent paying a service over[HTTP 402](https://www.zinc.com/blog/http-402-agentic-commerce) - launched within months of each other. x402 is stablecoin-only and fully crypto-native; MPP is multi-rail (stablecoin + fiat + cards + Lightning) with Stripe's enterprise financial infrastructure. They compete for adoption, though a service can hedge by accepting both at its 402 boundary.


### Which protocol is cheaper?


x402 is cheaper for pure crypto micropayments - zero protocol fees, only on-chain gas (fractions of a cent on L2s). MPP charges no protocol fees for Tempo stablecoin settlement but adds Stripe's standard processing fees (2.9% + 30¢) on fiat/card payments.


### Does Stripe support both protocols?


Yes. Stripe integrated x402 for USDC on Base in February 2026, then launched MPP with Tempo in March 2026. Stripe's stated position is that both protocols serve different use cases and developers should support both.


### Can I use MPP and x402 on the same service?


Yes. A service can advertise both payment methods in its 402 response and let the agent pick. This maximizes the agent population that can successfully pay.


### Which protocol do AI agents actually use today?


x402 has more live transaction volume right now due to its earlier launch and crypto-native adoption - the public[x402scan](https://www.x402scan.com/) explorer tracks it in the low millions of transactions per month (a few million dollars settled), though volume is volatile and off its late-2025 peak. MPP launched later with 100+ integrations including major enterprises (Visa, Mastercard, Anthropic, OpenAI, Shopify, Zinc).


### Which protocol should I support if I'm a service accepting agent payments?


Both, if your resources allow. If you can only support one: x402 for pure API and crypto-native services; MPP for services that need fiat, enterprise controls, or physical goods commerce.


### Do I need either protocol to build an AI shopping agent?


Not strictly. You can use direct API keys and pre-configured billing. But if you want your agent to transact dynamically with services it discovers at runtime (without pre-provisioning), MPP or x402 is how that works. For physical goods specifically, pair either protocol with an execution layer like[Zinc](https://www.zinc.com/) . Our[AI shopping agent tutorial](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) walks through the full pattern.


Zinc Agent


Give your AI agent commerce powers


Amazon order


In transit


POST


/orders


Product


AirPods Pro


Tracking


webhook sent


Returns


label ready
