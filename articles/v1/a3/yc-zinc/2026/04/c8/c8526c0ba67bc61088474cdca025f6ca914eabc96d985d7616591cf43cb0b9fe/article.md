---
schema_version: "1.0.0"
document_id: "c8526c0ba67bc61088474cdca025f6ca914eabc96d985d7616591cf43cb0b9fe"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/http-402-agentic-commerce"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:ea0ac1190b033867e0ed0022def11667beb3bd9736aa7742b1d20c587d31967e"
---

# What Is HTTP 402 Payment Required? A Complete Guide

The HTTP specification has reserved the status code` 402 Payment Required` since 1997. For almost thirty years it was the internet's most famous never-used feature - a placeholder for a feature the early web assumed would arrive any day and then never bothered to define.


In late 2025, two serious implementations finally landed. Coinbase launched[x402](https://www.x402.org/) in October 2025 with V2 shipping in December. Stripe and Tempo launched[MPP](https://www.zinc.com/blog/what-is-mpp) in March 2026. Both protocols revive HTTP 402 as the primary mechanism for AI agents to pay for resources over HTTP.


This guide walks through the history of HTTP 402, why it sat unused for three decades, how the current revival works at the protocol level, and why agentic commerce is the use case that finally gave it a job.


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


## What HTTP 402 means (and has always meant)


The HTTP specification (RFC 7231, updating the earlier RFC 2616 and originally appearing in RFC 2068) includes the following definition:


**402 Payment Required** This code is reserved for future use.


That's it. That's the whole spec entry, for 30 years.


The intent was clear. Some resources cost money. When an agent or user requests one without payment, the server should respond with a structured refusal that tells the client how to pay. The specifics of "how to pay" were left deliberately open - the web didn't have a payment primitive to standardize against.


Other status codes tried to fill the gap incidentally.` 401 Unauthorized` handled authentication.` 403 Forbidden` handled authorization.` 429 Too Many Requests` handled rate limits. But none of them said "you need to pay for this." That job stayed vacant.


---


## Why HTTP 402 sat unused for 30 years


Three reasons:


**1. No universal payment layer.** The early web didn't have a standard way for a browser to initiate a payment in response to a server hint. HTTPS wasn't ubiquitous, wallets weren't a thing, and credit card flows required full HTML forms. Even if a server returned 402, the client had nothing to do with it.


**2. Subscription economics took over.** Instead of per-request payments, the web settled on ad-supported free content and monthly subscriptions for paid content. A single-page checkout per month scales better than a browser popup per request.


**3. Human latency made it pointless.** Even if a 402 response told a browser "pay $0.10 for this article," the human behind the browser couldn't realistically do that hundreds of times a day. Transaction UI, cognitive load, and psychological friction killed the idea every time someone tried it.


Each attempt at internet micropayments over the years (FirstVirtual, CyberCash, Millicent, Flooz, Bitcoin micropayments via Lightning, Brave's BAT) ran into the same issue: humans don't want to approve per-page charges. Any solution that required human approval per request couldn't scale.


AI agents remove that bottleneck entirely.


---


## Why agents change the equation


An AI agent doesn't have the psychological friction of a human browser. It can evaluate a 402 response, authorize the payment programmatically based on a pre-set budget, and retry the request in milliseconds. Hundreds of tiny payments per second are fine. Thousands are fine.


The human's role changes from "approve every charge" to "set the spending rules." Once an agent has a wallet and a budget, 402-based payments stop being a UX problem and start being a plain HTTP round trip.


That shift is why 402 suddenly works. The technology has been ready for decades. The user model finally caught up.


---


## The current 402 revival


Two protocols, both reviving HTTP 402, both open, both production-ready.


### x402


Launched by Coinbase in October 2025 (V2 December 2025), governed under the x402 Foundation alongside Cloudflare as of September 2025.


**The flow:**


1. Agent requests a paid resource
2. Server responds with 402 including payment instructions (network, asset, amount, destination address)
3. Agent signs a stablecoin payment to the destination
4. Agent retries the request with the payment signature in an` X-PAYMENT` header
5. Server verifies on-chain and delivers the resource


**Example 402 response:**


```text
HTTP/1.1     402     Payment Required
Content-Type  :     application/json


{
"accepts"  :     [
{
"scheme"  :     "exact"  ,
"network"  :     "base"  ,
"maxAmountRequired"  :     "10000"  ,
"asset"  :     "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913"  ,
"payTo"  :     "0xa0b8...ef8B"  ,
"resource"  :     "https://api.example.com/weather"  ,
"mimeType"  :     "application/json"
}
]
}
```


**Example retry with payment:**


```text
GET     /weather     HTTP/1.1
Host  :     api.example.com
X-PAYMENT  :     <base64-encoded-signed-payment>


```


Server-side, an entire x402 integration can be one line of middleware:


```text
app  .  use  (
paymentMiddleware  (  {
'GET /weather'  :     {
accepts  :     [  {     network  :     'base'  ,     asset  :     'USDC'  ,     amount  :     '0.01'     }  ]  ,
description  :     'Weather data'  ,
}  ,
}  )
)  ;
```


Public usage is tracked by the[x402scan](https://www.x402scan.com/) explorer: roughly a few million transactions and ~$1M in volume over a recent 30-day window, at an average around $0.30 per call - volatile, and down from a late-2025 peak. Adopters include Stripe, AWS, Messari, Alchemy, Nansen, Vercel, and Cloudflare.


### MPP (Machine Payments Protocol)


Launched by Stripe and Tempo on March 18, 2026.


**The flow:**


1. Agent requests a paid resource
2. Server responds with 402 including multiple accepted payment methods (stablecoin, fiat, card, Bitcoin Lightning)
3. Agent selects a method, authorizes payment through the appropriate rail
4. Agent retries with a payment credential in the` Payment-Authorization` header
5. Server verifies and delivers the resource


**Example 402 response:**


```text
HTTP/1.1     402     Payment Required
Content-Type  :     application/json
WWW-Authenticate  :     tempo realm="zinc" amount="25.99" currency="usd"
WWW-Authenticate  :     stripe realm="zinc" amount="25.99" currency="usd"


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


MPP extends the core 402 pattern with a **session** primitive: an agent can pre-authorize a spending cap once and stream many micropayments within that cap without an on-chain transaction per request. Critical for high-frequency workloads.


**Launch partners** : Visa (card extension), Lightspark (Bitcoin Lightning extension), Anthropic, OpenAI, Shopify, Mastercard, 100+ services including[Zinc](https://www.zinc.com/) for programmatic retail orders.


For a detailed walkthrough of both, read[MPP vs x402: Which Agent Payment Protocol to Use?](https://www.zinc.com/blog/mpp-vs-x402) .


---


## How 402 responses actually look in HTTP


The response structure varies by protocol but follows a consistent pattern:


**Shared headers:**


```text
HTTP/1.1     402     Payment Required
Content-Type  :     application/json
```


**x402 uses a body-heavy structure:**


```text
{
"accepts"  :     [
{
"network"  :     "base"  ,
"asset"  :     "USDC"  ,
"amount"  :     "0.01"  ,
"payTo"  :     "0x..."  ,
"resource"  :     "/path"  ,
"mimeType"  :     "application/json"
}
]
}
```


**MPP uses WWW-Authenticate headers plus body:**


```text
WWW-Authenticate  :     tempo realm="..." amount="..." currency="..."
WWW-Authenticate  :     stripe realm="..." amount="..." currency="..."
```


Both follow RFC 7235's framework for authentication-challenge responses, adapted for payments. The` WWW-Authenticate` header is a natural fit: it's already reserved for telling clients how to satisfy a server's auth requirements. Payment is a kind of auth.


---


## What HTTP 402 is good for


Once agents can respond to 402 programmatically, whole categories of commerce open up:


### API monetization


Per-request billing without account creation. An agent queries a weather API, pays $0.001, gets the response. No sign-up flow, no API key provisioning, no billing page. The API provider's Stripe or on-chain wallet fills up with micropayments.


### Compute and data feeds


LLM inference, image generation, database queries, real-time data streams. Usage-based pricing becomes usage-based payment. No billing cycles, no overage surprises. Agents pay as they go.


### Dynamic service discovery


An agent encounters a service mid-task it's never seen before. Instead of signing up for an account (impossible for the agent, requires a human), it handles the 402 response and pays. The service gets new customers without needing to onboard them.


### Physical retail (via execution layers)


[Zinc](https://www.zinc.com/) exposes 402 endpoints (both x402 and MPP) for retail orders. An agent can post an order request without authentication, get 402 back with the order total, pay inline, and Zinc places the real order at Amazon, Walmart, Target, or 50+ other retailers. This is the most ambitious current 402 use case - physical goods commerce via a status code that was dormant for three decades. For the end-to-end build, see our[AI shopping agent tutorial](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) .


### Agent-to-agent payments


When Agent A asks Agent B to do something, B can charge for the work. 402 is the interface. A pays, B executes. Multi-agent systems become commercially self-sustaining instead of requiring upstream funding.


---


## What HTTP 402 is not good for


The current revival has limits worth noting:


**Human-present commerce.** 402 works because agents can authorize payments programmatically. Humans still can't realistically approve a 402 response on every page load. Traditional checkout pages and[ACP](https://www.zinc.com/blog/what-is-acp) (Agentic Commerce Protocol) remain better fits for user-facing checkout.


**High-value single transactions.** Buying a car with a 402 response is technically possible but operationally unwise. For large transactions, full checkout flows with explicit user confirmation, tax handling, and receipt generation are more appropriate.


**Regulated products.** Prescription drugs, firearms, alcohol, age-gated content. 402 doesn't handle KYC, age verification, or regulatory compliance. Those use cases need traditional flows layered on top.


**Traditional card networks alone.** 402 pairs well with stablecoin settlement (x402) and multi-rail (MPP). For pure card-based transactions, you're really just using 402 as a transport for PSP credentials. Works, but doesn't give you much beyond what direct PSP integrations already offer.


---


## The protocol layer stack


HTTP 402 is the transport mechanism. It's useful to name the different jobs a full system has to do:


- **Authorization** ([AP2](https://www.zinc.com/blog/what-is-ap2) ): Prove the user authorized this transaction
- **Checkout** ([ACP](https://www.zinc.com/blog/what-is-acp) ): Merchant-facing four-endpoint protocol for cart management
- **Settlement** ([x402, MPP](https://www.zinc.com/blog/mpp-vs-x402) ): HTTP 402-based payment negotiation and execution
- **Execution** ([Zinc](https://www.zinc.com/) , merchant-native APIs): Actually place the order and fulfill it at the retailer


These are jobs, not products you wire up one-to-one - and a single service can span several of them.[Zinc](https://www.zinc.com/) is the clearest example: it's not a downstream "execution layer" you bolt on after settlement. It's an abstraction that accepts the 402 payment itself (both x402 and MPP) *and* does everything downstream - retailer checkout, bot mitigation, account management, fulfillment - at retailers that expose none of these protocols. An agent doesn't assemble a settlement layer and then a separate execution layer; it pays Zinc's 402 endpoint and Zinc handles the rest.


HTTP 402 sits at the settlement layer, as the transport for protocols like x402 and MPP. It doesn't replace the jobs above or below it; it makes payment negotiation work cleanly so services like Zinc can build on it.


---


## Implementing 402 on your service


If you're building a service that AI agents should be able to pay:


**1. Decide which protocols to accept.** x402 is simpler if you only want crypto.[MPP](https://www.zinc.com/blog/what-is-mpp) adds fiat and enterprise controls. Supporting both maximizes your agent population.


**2. Pick a middleware.** x402 has an Express/Next.js package (` x402-express` ). Stripe has MPP support through its SDK. Both let you expose 402 responses in a few lines.


**3. Decide on pricing.** 402 works best for granular, per-request pricing (sub-cent to low-dollar). If your service is higher-value, consider whether 402 is the right shape.


**4. Handle session-based pricing (MPP).** If your service benefits from streaming micropayments (continuous data, long-running compute), implement MPP sessions rather than per-request 402.


**5. Test with real agents.** The[agent.zinc.com](https://agent.zinc.com/) playground is a live 402-paid endpoint (x402 and MPP) you can test against as a reference.


---


## Implementing 402 on your client (agent side)


If you're building an agent that needs to pay services:


**1. Support both x402 and MPP.** The code paths are similar, but detecting and handling both covers more services. See the[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) for when to use which.


**2. Pick a wallet stack.** x402 needs a wallet that can sign stablecoin transactions (MetaMask, Coinbase Wallet, embedded wallets via Privy or Thirdweb). MPP needs either a Tempo wallet for crypto or a Stripe SPT for fiat.


**3. Budget and guardrails.** Every agent should have a spending cap. If you're running multiple agents or acting on a user's behalf, enforce per-session and per-day limits in your code, not just at the payment rail.


**4. Handle failures gracefully.** Payment can fail (insufficient funds, network issues, service errors). Your retry and escalation logic should be clean.


**5. Log every payment.** For audit, debugging, and user trust. For stronger audit trails, layer[AP2 Mandates](https://www.zinc.com/blog/what-is-ap2) on top so every transaction carries a verifiable record of user authorization.


---


## Bottom line


HTTP 402 was a thirty-year placeholder waiting for the right user. Humans never became that user. AI agents are.


x402 (Coinbase, December 2025) and[MPP](https://www.zinc.com/blog/what-is-mpp) (Stripe + Tempo, March 2026) are the two serious revivals, and both are production-ready, processing real payments across live services every month. For the first time since the HTTP specification was written, the code 402 means something operationally, not just "reserved for future use."


If you're building agent infrastructure, supporting 402-based payments (via x402, MPP, or both) future-proofs your service for the next wave of machine commerce. If you're building an agent, handling 402 responses cleanly unlocks access to services you haven't explicitly integrated with.


For physical goods specifically,[Zinc](https://www.zinc.com/) exposes 402 endpoints (x402 and MPP) that let agents place real orders at Amazon, Walmart, Target, and 50+ retailers through a single 402-based handshake. See[Zinc MPP Integration: Agents Buy Online, No Setup](https://www.zinc.com/blog/mpp) and try the[agent.zinc.com playground](https://agent.zinc.com/) to watch a real order via HTTP 402.


For the full context on how 402 fits in the agentic commerce stack, read[Agentic Commerce in 2026: The Complete Developer Guide](https://www.zinc.com/blog/agentic-commerce) . For the MPP-specific walkthrough, read[What Is MPP?](https://www.zinc.com/blog/what-is-mpp) . For the x402 comparison, read[MPP vs x402](https://www.zinc.com/blog/mpp-vs-x402) . To build an agent from scratch using all of this, follow our[AI shopping agent tutorial](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) or the[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) reference app.


---


## Frequently Asked Questions


### What does HTTP 402 Payment Required mean?


HTTP 402 Payment Required is a status code reserved in the HTTP specification since 1997 indicating that a client needs to pay before the server will deliver the requested resource. It was unused for nearly 30 years and has been revived in 2025-2026 by protocols like x402 (Coinbase) and[MPP](https://www.zinc.com/blog/what-is-mpp) (Stripe + Tempo) to enable AI agents to pay for services programmatically.


### Why was HTTP 402 unused for 30 years?


Three reasons: the early web had no standard payment layer, subscription economics won over per-request payments, and human psychological friction made per-page payment flows unworkable. AI agents eliminate the human-approval bottleneck, which is why 402 is finally seeing real use.


### Which protocols use HTTP 402 today?


Two main protocols: x402 (Coinbase, launched October 2025 with V2 in December 2025) for stablecoin-only payments, and[MPP](https://www.zinc.com/blog/what-is-mpp) (Stripe + Tempo, launched March 18, 2026) for multi-rail payments including stablecoins, fiat, cards, and Bitcoin Lightning. Both are open standards.


### What's the difference between x402 and MPP?


x402 is stablecoin-only with zero protocol fees. MPP is multi-rail (stablecoin + fiat + cards + Lightning) with Stripe's enterprise financial infrastructure (tax, fraud, refunds). Most serious agent platforms support both. See our full[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) .


### Can regular websites use HTTP 402?


Yes, technically. Any server can return 402 responses. The question is whether clients (browsers, agents) can handle them. Traditional browsers don't. AI agents using x402 or MPP libraries can. For public-facing websites with human users, 402 isn't practical today. For agent-to-service traffic, it works.


### Is HTTP 402 only for cryptocurrency payments?


No. x402 is stablecoin-focused, but MPP supports fiat via Stripe Shared Payment Tokens, cards via Visa's extension, and Bitcoin via Lightspark's Lightning extension alongside stablecoins. A service accepting MPP can receive payments across multiple rails.


### How do I implement HTTP 402 on my service?


Pick a middleware. x402 has an Express package (` x402-express` ) that lets you enable 402 responses in one line. Stripe has SDK support for MPP. Both let you expose paid endpoints that accept agent payments with minimal integration work.


### What happens after the agent pays a 402 response?


The agent retries the original request with a payment credential in the header (` X-PAYMENT` for x402,` Payment-Authorization` for MPP). The server verifies the payment (on-chain for x402, via the appropriate rail for MPP), and if valid, delivers the resource. The entire flow is one extra round trip on top of the original request.


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
