---
schema_version: "1.0.0"
document_id: "78f91a86bf591add893c1c8ba4c33be177521bdd4428b507d79936df7f7c996f"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/what-is-acp"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:c15a845a28db277ba83771dfcabff8179c776dcc1cc1ae3d91f10330541f1e7b"
---

# What Is ACP? The Agentic Commerce Protocol Explained

ACP is an open protocol for AI agents to complete checkout at merchants. It defines how a merchant exposes its products for discovery, how an agent tells a business what's in the cart, how the business confirms pricing and availability, and how a single-use payment token flows through the transaction. Co-authored by OpenAI and Stripe, released in September 2025 under Apache 2.0, and first deployed inside ChatGPT's Instant Checkout in February 2026.


ACP matters if you're building an agent that buys from online merchants - but with a real caveat: it only works where the merchant has already implemented ACP and where your agent is approved on a platform that supports it. Today that's a narrow slice (ChatGPT plus a set of Stripe- and Shopify-backed merchants), not the open web. This guide walks through what ACP is, its four checkout endpoints, how Shared Payment Tokens work, where it actually fits among the other agentic commerce protocols, and what it doesn't solve.


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


## ACP in one paragraph


ACP is a specification for four REST endpoints that a merchant exposes so an AI agent can create, update, complete, or cancel a checkout session programmatically. Payment flows through a Shared Payment Token (SPT), a single-use, time-bound, amount-restricted credential that the agent can present on the user's behalf without ever seeing raw card data. The spec lives at[agenticcommerce.dev](https://www.agenticcommerce.dev/) and is PSP-agnostic, though Stripe is the first payment service provider to implement SPTs.


---


## The problem ACP solves


Online commerce was built for a human clicking through a website: browse, add to cart, type in a card, confirm. An AI agent acting for that human has none of that UI to lean on, and two things were missing before ACP.


1. **No common language for agent-to-business transactions.** Every merchant describes products, carries a cart, and confirms an order differently. An agent had to reverse-engineer each site or hope the merchant had a usable public API - most don't, and the ones that do (Shopify Storefront, BigCommerce) are platform-specific.
2. **No safe way for an agent to pay on the user's behalf.** Handing an agent a raw card number is a non-starter for the user, the merchant, and the payment network.


ACP standardizes both. It defines a common interface in three parts: a **product feed** so agents can discover and rank a merchant's items, a **checkout** flow (the four endpoints below) so an agent can build and confirm a cart against the merchant's real pricing and inventory, and a **delegated payment** step that passes a scoped, single-use token to the merchant's payment processor - so the agent never touches card data. The key design choice is that the merchant stays the merchant of record and keeps its existing payment, fulfillment, and support systems; it adds a feed and a few endpoints rather than rebuilding the store, and controls what gets sold, to whom, and at what price.


---


## The four ACP endpoints


ACP defines four operations, all REST, all aligned to how a traditional merchant checkout already works:


### 1. Create Checkout


The agent initiates a checkout with cart contents, customer info, and shipping details.


```text
POST /checkout_sessions
Authorization  :     Bearer <agent_credential>
Content-Type  :     application/json


{
"buyer"  :     {
"email"  :     "jane@example.com"  ,
"first_name"  :     "Jane"  ,
"last_name"  :     "Doe"
}  ,
"items"  :     [
{
"product_id"  :     "prod_abc123"  ,
"quantity"  :     1
}
]  ,
"shipping_address"  :     {
"line_1"  :     "123 Main St"  ,
"city"  :     "San Francisco"  ,
"state"  :     "CA"  ,
"postal_code"  :     "94105"  ,
"country"  :     "US"
}
}
```


The merchant responds with a` checkout_session_id` , pricing breakdown (subtotal, tax, shipping), and any required fields the agent still needs to fill in.


### 2. Update Checkout


The agent can update the session before completion - change quantities, swap shipping methods, apply promo codes, update the address.


```text
POST /checkout_sessions/{id}
Content-Type  :     application/json


{
"items"  :     [
{     "product_id"  :     "prod_abc123"  ,     "quantity"  :     2     }
]
}
```


The merchant recalculates pricing and returns the updated session state.


### 3. Complete Checkout


The agent submits the Shared Payment Token and the merchant charges the payment method and confirms the order.


```text
POST /checkout_sessions/{id}/complete
Content-Type  :     application/json


{
"payment"  :     {
"shared_payment_token"  :     "spt_test_abc123..."
}
}
```


If payment succeeds, the merchant returns an` order_id` and the purchase is real.


### 4. Cancel Checkout


Before completion, the agent (or the merchant) can cancel cleanly.


```text
POST /checkout_sessions/{id}/cancel
```


No charge, no order, session closed.


That's the entire surface area. Four endpoints. Any merchant can implement them. Any agent platform can call them. The protocol doesn't care what's in the cart (physical goods, digital goods, subscriptions, async purchases are all supported).


---


## Shared Payment Tokens (SPTs)


The payment piece is where ACP's design matters most.


A Shared Payment Token is a single-use credential that represents permission to charge a specific amount, to a specific merchant, within a specific time window. The user or their wallet provider generates the SPT. The agent receives it scoped to exactly this transaction. The merchant charges it. The token expires.


**What an SPT carries:**


- Merchant identifier (can only be charged by this merchant)
- Maximum amount (can't be charged more than authorized)
- Expiration window (can't be charged past this time)
- Single-use marker (can only be charged once)


**What an SPT doesn't carry:**


- Raw card data (stays with the payment processor)
- User account credentials
- Long-lived payment authority


This is how ACP separates the agent's authority to act from the user's authority to pay. The agent can't drain a wallet. The merchant can't bill again later. The user's card details never reach the agent's runtime.


Stripe was the first PSP to implement SPTs. Any compatible PSP can issue tokens that ACP merchants accept. PayPal announced its own ACP server under development at launch.


---


## ACP in production: where it's live today


ACP's first real deployment was OpenAI's Instant Checkout feature in ChatGPT, rolled out to US Etsy sellers in February 2026. A user would ask ChatGPT for a product, ChatGPT would display options, and for participating Etsy sellers, a "Buy" button inside the chat completed the purchase through ACP.


In early March 2026, OpenAI scaled back in-chat purchasing and pivoted to an app-based model. The protocol continues. The specific ChatGPT UI changed.


The lesson that matters: ACP's long-term value is as an open standard, not as a feature of any single platform. Stripe, Shopify, Salesforce, and PayPal are all building or supporting ACP. OpenAI is the first agent platform. More will follow.


---


## What ACP doesn't solve


ACP gets a lot done with four endpoints. It doesn't do everything.


**ACP doesn't help you buy from non-ACP retailers.** Amazon, Walmart, Target, Best Buy, and most big-box stores haven't adopted ACP and aren't on public roadmaps. If your agent needs to buy from them, ACP won't help. You need an execution layer like[Zinc](https://www.zinc.com/) that handles programmatic ordering at 50+ retailers that don't speak ACP.


**ACP doesn't handle machine-to-machine API payments.** It's designed for human-present commerce where an agent assists a user. For pure agent-to-API micropayments, use[MPP](https://www.zinc.com/blog/what-is-mpp) or[x402](https://www.zinc.com/blog/mpp-vs-x402) instead.


**ACP doesn't handle long-running authorizations.** Each checkout session is a single transaction. For pre-authorized agents that act on a user's behalf over time (delegated shopping), you want[Google's AP2 protocol](https://www.zinc.com/blog/what-is-ap2) layered on top.


**ACP doesn't handle payment settlement.** It's a checkout protocol. The actual money movement happens through the underlying PSP (Stripe's SPTs today). If the PSP fails, ACP's checkout fails.


**ACP doesn't discover merchants.** There's no central directory of ACP-enabled merchants. Each agent platform manages its own merchant participation program. If you want to sell through ChatGPT, you apply to OpenAI. No global registry exists.


---


## ACP vs MPP vs x402 vs AP2


These four get mentioned in the same breath, but they aren't a stack you assemble together. They come from competing companies and mostly solve separate problems, so which one you touch depends on what your agent actually does - most agents use one, occasionally two, not all four.


- **ACP** (OpenAI + Stripe): **agent-to-merchant checkout.** Four endpoints, single-use payment tokens. Merchant-facing.
- **[MPP](https://www.zinc.com/blog/what-is-mpp)** (Stripe + Tempo): **agents paying for services over[HTTP 402](https://www.zinc.com/blog/http-402-agentic-commerce) .** Sessions, multi-rail (stablecoin + fiat + cards). Service-facing.
- **x402** (Coinbase): **agents paying in stablecoins over HTTP.** Zero protocol fees, purely crypto-native. Service-facing. See our[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) .
- **[AP2](https://www.zinc.com/blog/what-is-ap2)** (Google + 60 partners): **a signed-authorization layer** , payment-rail agnostic.


An agent buying from Shopify merchants inside ChatGPT uses ACP; an agent paying per call for a data API uses MPP or x402 and never touches ACP. AP2 is the one that can pair with another - it authorizes a payment that later settles over a rail - but that composition is still early, and ACP already carries its own delegated-payment step. And for retail orders at Amazon or Walmart, none of these protocols reach the retailer at all; that takes an execution layer like[Zinc](https://www.zinc.com/) .


---


## Should you implement ACP?


**Implement ACP as a merchant if:**


- You sell physical or digital goods directly and you're already on Stripe, Shopify, or another ACP-compatible PSP
- You want to be discoverable by agents on ChatGPT and future agent platforms
- You have the engineering capacity to expose four endpoints plus inventory, pricing, and order confirmation flows
- You want to participate in agent-driven commerce without giving up merchant-of-record status


**Implement ACP as an agent platform if:**


- Your agents help users discover and buy from merchants (not APIs, not big-box retailers)
- Your users want to transact inside your chat or app without being redirected
- You're willing to build a merchant approval and discovery layer on top of the protocol


**Skip ACP (at least for now) if:**


- Your agents only place orders at big retailers (use[Zinc](https://www.zinc.com/) instead)
- Your agents only pay for APIs and services (use[MPP](https://www.zinc.com/blog/what-is-mpp) or[x402](https://www.zinc.com/blog/mpp-vs-x402) )
- You don't have a clear merchant story


---


## Bottom line


ACP is a well-scoped protocol that solves a specific problem: letting agents complete checkout at merchants without scraping, without per-merchant custom integrations, and without the merchant giving up merchant-of-record status.


But it's gated on both sides. A merchant only transacts through ACP if it has implemented the feed and endpoints, and an agent only reaches those merchants if it's approved on a platform that supports ACP - today, effectively ChatGPT. Unless you have that "in," ACP doesn't get you access. And none of the big-box retailers (Amazon, Walmart, Target, Best Buy) have adopted it at all.


That's the gap[Zinc](https://www.zinc.com/) closes. Zinc gives agents programmatic ordering across 50+ retailers through one API, and no retailer has to implement ACP - or any agent protocol - for it to work. It isn't a competitor to ACP; it's how you reach everything ACP can't. See the[Zinc quickstart](https://www.zinc.com/docs/quickstart) to place your first order, compare[ecommerce purchasing APIs](https://www.zinc.com/blog/best-ecommerce-purchasing-apis) , or build the full flow with our guide to[building an AI shopping agent](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) and the[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) reference app.


For the full context on how agentic commerce stacks together, read[Agentic Commerce in 2026: The Complete Developer Guide](https://www.zinc.com/blog/agentic-commerce) . For the machine-to-machine payment layer, read[What Is MPP?](https://www.zinc.com/blog/what-is-mpp) and[Zinc MPP Integration](https://www.zinc.com/blog/mpp) . To place a real retail order right now, try[agent.zinc.com](https://agent.zinc.com/) .


---


## Frequently Asked Questions


### What does ACP stand for?


ACP stands for Agentic Commerce Protocol. It's an open specification for how AI agents complete checkout at merchants, co-developed by OpenAI and Stripe, released September 2025 under Apache 2.0 license.


### Who created ACP?


ACP was co-authored by Stripe and OpenAI. Shopify, Salesforce, and PayPal all support or are building ACP implementations. The specification is open source and community-contributed at[agenticcommerce.dev](https://www.agenticcommerce.dev/) .


### How does ACP work?


A merchant implements four REST endpoints (Create Checkout, Update Checkout, Complete Checkout, Cancel Checkout). An AI agent calls those endpoints to build a cart and complete a purchase on the user's behalf. Payment flows through a Shared Payment Token issued by the user's payment processor (like Stripe). The merchant remains the merchant of record.


### What is a Shared Payment Token?


A Shared Payment Token (SPT) is a single-use, time-bound, amount-restricted payment credential. It gives an agent permission to charge a specific amount at a specific merchant within a time window, without exposing raw card data. Stripe was the first PSP to support SPTs.


### How is ACP different from MPP?


ACP is a checkout protocol for agents buying from merchants.[MPP](https://www.zinc.com/blog/what-is-mpp) is a payment protocol for agents paying for services (APIs, compute, data, physical goods) over HTTP. ACP assumes a human-present shopping flow with a merchant relationship. MPP handles machine-to-machine payments where there's no pre-existing merchant relationship.


### Does ACP work with Amazon or Walmart?


No. Amazon, Walmart, Target, Best Buy, and most big-box retailers have not adopted ACP. For programmatic ordering at those retailers, use an execution layer like[Zinc](https://www.zinc.com/) , which covers 50+ retailers through a single API without requiring the retailer to implement ACP.


### Is ACP production-ready?


Yes. ACP launched in OpenAI's ChatGPT Instant Checkout in February 2026 with Etsy sellers. OpenAI has since pivoted to an app-based model, but the protocol remains an open standard with continued support from Stripe, Shopify, Salesforce, and PayPal. Merchant implementations are live in production today.


### Where can I read the ACP specification?


The open specification is at[agenticcommerce.dev](https://www.agenticcommerce.dev/) . Stripe's implementation documentation is at[docs.stripe.com/agentic-commerce](https://docs.stripe.com/agentic-commerce) .


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
