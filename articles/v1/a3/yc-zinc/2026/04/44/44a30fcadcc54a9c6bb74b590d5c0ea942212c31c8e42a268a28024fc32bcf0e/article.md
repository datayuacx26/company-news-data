---
schema_version: "1.0.0"
document_id: "44a30fcadcc54a9c6bb74b590d5c0ea942212c31c8e42a268a28024fc32bcf0e"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/what-is-mpp"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T09:14:52.140847+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:da7af5333532c15884db1ff07240cf76ed67fb303ae56575c0ec88fba12f4637"
---

# What Is MPP? A Developer's Guide to Machine Payments

Every payment system built today was designed around the assumption that a human is present to sign in, click a button, and approve a charge. AI agents don't work that way. They make hundreds of small decisions per session, and none of them survive a checkout form.


The Machine Payments Protocol (MPP) is an open payment standard for this new class of user. Co-developed by[Stripe](https://stripe.com/) and[Tempo](https://tempo.xyz/) , it lets agents pay for APIs, data, compute, physical goods, and anything else addressable over HTTP, without an account, an API key, or a pre-loaded credit card. It launched March 18, 2026 with 100+ integrated services at launch, including Visa, Lightspark, Anthropic, OpenAI, Shopify, and[Zinc](https://www.zinc.com/) .


This guide walks through what MPP actually is, how it works at the protocol level, how it differs from x402, ACP, and AP2, and shows working code from Zinc's production MPP integration for agent-driven retail orders.


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


## What is MPP, in one paragraph


MPP is an open HTTP-based protocol for AI agents to pay services programmatically. An agent makes a request, the service responds with HTTP` 402 Payment Required` and a list of accepted payment methods, the agent authorizes payment (stablecoins on Tempo, fiat via Stripe Shared Payment Tokens, cards via Visa, or Bitcoin via Lightning), and the service delivers the resource. No accounts. No API keys. No pre-configured billing. Settlement happens in seconds.


You can read the full specification at[mpp.dev](https://mpp.dev/) and Stripe's implementation docs at[docs.stripe.com/payments/machine](https://docs.stripe.com/payments/machine) .


---


## The problem MPP solves


AI agents hit a wall when they need to pay for anything.


Consider what an agent actually has to do today to use a paid API:


1. Find the service
2. Identify the pricing page
3. Figure out which plan fits
4. Create an account
5. Verify an email
6. Enter payment details
7. Get approved
8. Generate an API key
9. Manage the key securely
10. Finally make the request


Every step requires a human. Every step assumes accounts are cheap and API keys are managed carefully. Neither is true when you have thousands of agents that each need to make one request to a different service.


Existing payment rails weren't designed for this. Credit card networks assume a cardholder present. SaaS billing assumes a tenant relationship. Even crypto payments (before MPP and x402) typically required a pre-funded wallet and service-specific integration.


MPP collapses the entire flow into a single HTTP round trip. The agent sends a request, gets a 402 back, pays inline, and the service responds with the resource. No ceremony.


---


## How MPP works


The core idea is reviving[HTTP status code 402 Payment Required](https://www.zinc.com/blog/http-402-agentic-commerce) , which has been reserved in the HTTP spec since 1997 and rarely used. MPP defines a structured way for servers to say "you need to pay for this, here's how," and for agents to respond with a payment.


### The request / 402 / pay / receive flow


An agent requests a resource:


```text
POST     /agent/orders     HTTP/1.1
Host  :     api.zinc.com
Content-Type  :     application/json


{
"products"  :     [
{     "url"  :     "https://www.walmart.com/ip/514407302"  ,     "quantity"  :     1     }
]  ,
"max_price"  :     2499  ,
"shipping_address"  :     {     "..."     }
}
```


The service knows this request has a cost attached. Instead of rejecting the unauthenticated request, it responds with a 402 that lists the accepted payment methods, one` WWW-Authenticate` header per method (per RFC 9110). The price and challenge parameters are carried in the base64` request` blob:


```text
HTTP/1.1     402     Payment Required
WWW-Authenticate  :     Payment realm="zinc", method="tempo", id="ch_...", intent="charge", request="<base64>"
WWW-Authenticate  :     Payment realm="zinc", method="stripe", id="ch_...", intent="charge", request="<base64>"
```


The agent's MPP client (like the` mppx` CLI or Stripe's SDK) reads the 402, selects a payment method, authorizes the payment, and re-sends the request with the credential in a single` Authorization: Payment` header:


```text
POST     /agent/orders     HTTP/1.1
Host  :     api.zinc.com
Authorization  :     Payment <base64-encoded-credential>
Content-Type  :     application/json


{     ...  same body   as   before  ...     }
```


The service verifies the payment, places the order, and returns a` 201` with the order details, a` Payment-Receipt` , and a temporary` X-Api-Key` scoped to that order:


```text
HTTP/1.1     201     Created
X-Api-Key  :     zn_...
Payment-Receipt  :     tempo receipt=rcpt_...


{
"id": "3f2b8c7e-1a4d-4e6f-9c2b-5d8a3f1e7b9c",
"status": "pending"
}
```


That's the entire flow. One round trip becomes a handshake where the agent and service negotiate payment inline. The agent didn't need an account. The service didn't need to pre-provision an API key. The human didn't need to configure anything.


### Sessions: MPP's key innovation


MPP's specific contribution over raw HTTP 402 is the concept of a **session** . An agent pre-authorizes a spending limit once (say, $50), and streams micropayments within that session without an on-chain transaction per interaction. This is critical for high-frequency use cases (streaming an API, making thousands of small calls, running a long-horizon task).


Under the hood, payments settle on Tempo (an EVM-compatible blockchain built for payment primitives) for stablecoins, or through Stripe's Shared Payment Tokens (SPTs) for fiat. Visa extended MPP to traditional card payments. Lightspark extended it to Bitcoin Lightning.


For businesses on Stripe, these payments appear in the Stripe Dashboard like any other transaction. Same payout schedule, same balance, same fraud protection, same tax calculation, same refund flow. The agent world plugs into existing financial infrastructure instead of replacing it.


---


## MPP vs x402: the comparison everyone asks about


MPP and[x402](https://www.x402.org/) launched within weeks of each other, and both use[HTTP 402](https://www.zinc.com/blog/http-402-agentic-commerce) to let an agent pay for a request inline — which is exactly why they get confused. The real difference is *what* they settle and *who* they're built for. Here's the side-by-side; our[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) has the full breakdown.


x402 MPP


**Who** Coinbase (x402 Foundation, co-governed with Cloudflare) Stripe + Tempo


**Payment** Stablecoins only (USDC primary) — Base, Ethereum, Polygon, Solana, Avalanche, Sui Multi-rail: stablecoins on Tempo, fiat via Stripe SPTs, cards via Visa, Bitcoin via Lightning


**Settlement** Direct on-chain Tempo blockchain for stablecoins; Stripe's existing rails for fiat


**Fees** Zero protocol fees; users pay only blockchain gas (fractions of a cent on L2s) No native gas token on Tempo (fees paid in stablecoin via AMM); Stripe processing fees apply for fiat/card SPTs


**Best for** Fully crypto-native agent stacks; micropayments for APIs and data feeds Agents that pay for anything (APIs + physical goods + services) across mixed payment methods


**Launched** V2 December 2025 (Stripe added x402 for USDC on Base, February 2026) March 18, 2026


### When to pick which


Pick When


**x402** Your agents are fully crypto-native and want zero processing fees; you only accept stablecoins; you're in the Coinbase ecosystem or want direct Base/Ethereum settlement.


**MPP** Your agents pay for physical goods or services beyond pure API micropayments; you want both crypto and fiat from one protocol; you want enterprise controls (tax, fraud, refunds) via Stripe; you're already on Stripe and want one dashboard for human and agent transactions.


**Both** You're an infrastructure provider and want maximum agent coverage — Stripe backs both and expects most serious agent platforms to accept both.


In practice, MPP and x402 compete on the surface but solve overlapping problems. The honest read from[defiprime's analysis](https://defiprime.com/stripe-mpp-vs-x402) : Stripe is backing both, so for most builders this is a "support both" rather than "pick one."


---


## MPP vs ACP vs AP2: the full protocol landscape


Four protocols come up in every agentic commerce conversation. They solve different problems.


- **[ACP (Agentic Commerce Protocol)](https://www.zinc.com/blog/what-is-acp)** - OpenAI + Stripe. Defines **how agents check out** at merchants. Standardizes the cart, checkout, and order flow between an agent and a merchant catalog. Production deployment: ChatGPT Instant Checkout (February 2026), now shifting to app-based model.
- **[AP2 (Agent Payments Protocol)](https://www.zinc.com/blog/what-is-ap2)** - Google + 60 partners. Defines **how agents get authorized** to pay. Uses cryptographically signed ECDSA Mandates (Intent, Cart, Payment) so a third party can verify the human approved a specific transaction.
- **[x402](https://www.zinc.com/blog/mpp-vs-x402)** - Coinbase. Defines **how stablecoin payments settle** over HTTP using status code 402.
- **MPP** - Stripe + Tempo. Defines **how payments stream** in a session across stablecoin and fiat rails, using status code 402 as the payment negotiation mechanism.


ACP answers "where do I shop?" AP2 answers "am I allowed to buy this?" x402 answers "how do I pay with crypto?" MPP answers "how do I pay across any rail, at scale, with enterprise controls?"


A production agent might use all four: AP2 for authorization, ACP for merchant-side checkout, x402 or MPP for machine-to-machine payments, and MPP sessions specifically for high-frequency micropayments.


---


## What you can build with MPP today


MPP launched with 100+ integrated services. The real production use cases split into three categories:


### API and compute micropayments


- **[Parallel Web Systems](https://parallel.ai/)** : Agents pay per API call for web access
- **[Browserbase](https://www.browserbase.com/)** : Agents spin up headless browsers and pay per session
- **Data feeds, LLM inference APIs, search APIs, compute on-demand** : The classic "pay per unit" model


### Physical and digital services


- **PostalForm** : Agents pay to print and mail physical letters
- **Prospect Butcher Co.** : Agents order sandwiches for delivery in NYC
- **[Stripe Climate](https://stripe.com/climate)** : Agents contribute to carbon removal programmatically


### Physical retail (Zinc)


- **[Zinc](https://www.zinc.com/)** : Agents place orders at Amazon, Walmart, Target, Best Buy, and[50+ other retailers](https://www.zinc.com/integrations) through one MPP endpoint.


Zinc's integration is the most ambitious use case in the launch cohort. Every other MPP launch partner is selling a service or API the agent consumes directly. Zinc uses MPP as the entry point for an agent to buy real physical goods that get packed, shipped, and tracked across retailer systems that weren't designed for agents at all.


---


## How Zinc implements MPP for retail orders


Zinc exposes an MPP endpoint at` /agent/orders` . An agent (or any MPP-compatible client) posts a standard order payload without authentication, gets a 402 with the price, pays, and Zinc places the real order.


The agent uses the` mppx` CLI for this example, but any MPP-compliant client works:


```text
npx mppx https://api.zinc.com/agent/orders   \
--method POST   \
--body   '{
"products": [
{ "url": "https://www.walmart.com/ip/514407302", "quantity": 1 }
],
"max_price": 2499,
"shipping_address": {
"first_name": "Jane",
"last_name": "Smith",
"address_line1": "123 Main St",
"city": "Seattle",
"state": "WA",
"postal_code": "98101",
"country": "US",
"phone_number": "2065551234"
}
}'
```


Behind the scenes:


1. The` mppx` client sends the raw POST with no` Authorization` header.
2. Zinc responds with a 402 listing the price ($25.99) and accepted payment methods (` tempo` ,` stripe` ).
3. The client pays via the selected method — say, 25.99 USDC on Tempo.
4. It re-sends the request with the credential in an` Authorization: Payment` header.
5. Zinc validates the order, places it at Walmart, and returns a 201 with the order ID and a temporary API key scoped to that order.


```text
HTTP/1.1     201     Created
X-Api-Key  :     zn_...
Payment-Receipt  :     tempo receipt=rcpt_...


{
"id": "3f2b8c7e-1a4d-4e6f-9c2b-5d8a3f1e7b9c",
"status": "pending"
}
```


The` X-Api-Key` in the response is a **temporary** key **scoped to this order** . The agent uses it to poll the order's status via` GET /orders/{id}` without repeating the 402 payment flow — no account, no long-lived key to manage. The unauthenticated first call is all it takes to place the order and start tracking it.


Once the retailer ships, Zinc receives tracking and fires a webhook:


```text
{
"event"  :     "order.tracking_received"  ,
"order_id"  :     "3f2b8c7e-1a4d-4e6f-9c2b-5d8a3f1e7b9c"  ,
"status"  :     "shipped"  ,
"timestamp"  :     "2026-04-16T09:15:00Z"  ,
"data"  :     {
"tracking_numbers"  :     [
{
"carrier"  :     "UPS"  ,
"tracking_number"  :     "1Z999AA10123456784"
}
]
}
}
```


No account creation. No key management. No pre-configured billing. The agent went from "I need to buy party hats" to a placed order, paid in USDC, in one HTTP round trip.


**Try it live** :[agent.zinc.com](https://agent.zinc.com/) is a working MPP playground where your agent can place a real order using Tempo stablecoins.


---


## Should you use MPP?


MPP is a good fit if any of these are true:


- You're building an agent that needs to pay for services it discovers dynamically
- You can't predict in advance every service your agent will need to pay
- You want to avoid managing dozens of API keys and billing accounts
- You need enterprise financial controls (fraud, tax, refunds) out of the box
- Your agent needs to pay for physical goods, not just APIs
- You want a single dashboard (Stripe's) for both human and agent payments


MPP is not the right fit if:


- You're building a traditional consumer-facing checkout (use Stripe Checkout or ACP)
- You only need to authorize known transactions from known users (use Stripe with Shared Payment Tokens directly)
- You're purely crypto-native and don't want Tempo as a dependency (consider x402 directly)
- Your use case is fully accounted-for today with plain API keys (don't over-engineer)


The biggest practical test: **does your agent need to pay for something it hasn't been set up for in advance?** If yes, MPP is the protocol designed for that. If no, you probably don't need it.


---


## Getting started with MPP


Pick your side of the protocol and go:


**If you're building an agent that pays:**


1. Pick an MPP-compatible client (Tempo CLI, Stripe's Agent SDK, or roll your own HTTP 402 handler)
2. Fund the agent's wallet with stablecoin or configure an SPT with your Stripe account
3. Start calling MPP endpoints. Handle 402 responses by paying and retrying.
4. See a working reference in the[Zinc MPP launch post](https://www.zinc.com/blog/mpp)


**If you're building a service agents pay:**


1. Read the[MPP specification at mpp.dev](https://mpp.dev/)
2. Follow Stripe's integration guide at[docs.stripe.com/payments/machine](https://docs.stripe.com/payments/machine)
3. Expose 402 responses on paid endpoints with` WWW-Authenticate` headers listing supported methods
4. Verify payment credentials and deliver the resource
5. Accept payments directly into your existing Stripe balance with a few lines of code


**If you want to see MPP live** :[agent.zinc.com](https://agent.zinc.com/) lets you watch an agent place a real retail order through MPP in real time, paying in USDC.


---


## Bottom line


MPP is the first payment protocol designed for agents that get things done, not just agents that describe getting things done.


HTTP 402 has been a placeholder in the spec for 30 years. Stripe and Tempo finally built the handshake that makes it useful. The result: agents can pay for APIs, compute, services, and real physical goods over HTTP, across stablecoins and fiat, without any pre-existing account relationship.


If you're building agentic commerce infrastructure, MPP is worth supporting alongside[ACP](https://www.zinc.com/blog/what-is-acp) (for merchant checkout) and[x402](https://www.zinc.com/blog/mpp-vs-x402) (for pure crypto micropayments). If you're building an agent that needs to buy something real, the[Zinc MPP endpoint](https://www.zinc.com/docs/v2/mpp) is the fastest path from "idea" to "order shipped."


For the full developer walkthrough of Zinc's MPP integration, read[Zinc MPP: Let Your Agent Buy Anything Online](https://www.zinc.com/blog/mpp) . For the broader context on where MPP fits in the agentic commerce stack, read[Agentic Commerce in 2026: The Complete Developer Guide](https://www.zinc.com/blog/agentic-commerce) .


### Related reading


- [Zinc MPP Integration: Agents Buy Online, No Setup](https://www.zinc.com/blog/mpp)
- [What Is ACP? The Agentic Commerce Protocol Explained](https://www.zinc.com/blog/what-is-acp)
- [What Is AP2? The Agent Payments Protocol Explained](https://www.zinc.com/blog/what-is-ap2)
- [MPP vs x402: Which Agent Payment Protocol to Use?](https://www.zinc.com/blog/mpp-vs-x402)
- [What Is HTTP 402 Payment Required? A Complete Guide](https://www.zinc.com/blog/http-402-agentic-commerce)
- [How to Build an AI Shopping Agent: Step-by-Step Guide](https://www.zinc.com/blog/how-to-build-ai-shopping-agent)
- [Zinc GPT: Open-Source AI Shopping Agent with Checkout](https://www.zinc.com/blog/zinc-gpt)


[Get started with Zinc](https://app.zinc.com/) or try the live MPP playground at[agent.zinc.com](https://agent.zinc.com/) .


---


## Frequently Asked Questions


### What does MPP stand for?


MPP stands for Machine Payments Protocol. It's an open HTTP-based standard for AI agents and services to coordinate payments programmatically, co-developed by Stripe and Tempo. Launched March 18, 2026.


### Who created MPP?


MPP was co-authored by Stripe and Tempo. Launch partners include Visa, Lightspark, Anthropic, OpenAI, Shopify, Mastercard, and 100+ integrated services at launch. The specification is open and hosted at[mpp.dev](https://mpp.dev/) .


### How is MPP different from x402?


Both protocols revive[HTTP status code 402](https://www.zinc.com/blog/http-402-agentic-commerce) to let agents pay over HTTP. x402 (from Coinbase) is stablecoin-only and settles directly on-chain with zero protocol fees. MPP (from Stripe + Tempo) is multi-rail (stablecoin on Tempo, fiat via Stripe SPTs, cards via Visa, Bitcoin via Lightning) and includes enterprise financial controls like tax calculation, fraud protection, and refunds. Most serious agent platforms support both. See our full[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) for the deep dive.


### Is MPP only for crypto payments?


No. MPP supports stablecoins on Tempo blockchain, fiat via Stripe Shared Payment Tokens, card networks via Visa's extension, and Bitcoin via Lightspark's Lightning extension. An agent using MPP can pay in USDC, dollars, or any supported rail.


### Can MPP be used for physical goods commerce?


Yes. Zinc uses MPP to let agents place orders at Amazon, Walmart, Target, Best Buy, and 50+ other retailers. Other MPP integrations include PostalForm (physical mail) and Prospect Butcher Co. (food delivery). MPP is not limited to API or digital service payments.


### How does an agent authenticate with MPP?


It doesn't, at least not in the traditional sense. The first request is unauthenticated. The server responds with HTTP 402 including payment instructions. The agent pays and retries the request with a payment credential. If the service supports it, the response can include a persistent API key tied to the agent's wallet, so subsequent requests use standard authentication.


### How much does MPP cost?


MPP itself has no protocol fees. Costs depend on the payment rail:


- Stablecoin on Tempo: fees paid in stablecoin via an integrated AMM, no native gas token
- Fiat via Stripe SPTs: standard Stripe processing fees (typically 2.9% + 30¢)
- Cards via Visa MPP extension: standard card network fees
- Bitcoin via Lightning: standard Lightning network fees


### Where can I read the full MPP specification?


The open specification is at[mpp.dev](https://mpp.dev/) . Stripe's implementation documentation is at[docs.stripe.com/payments/machine](https://docs.stripe.com/payments/machine) . Zinc's MPP integration docs for retail orders are at[zinc.com/docs/v2/mpp](https://www.zinc.com/docs/v2/mpp) .


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
