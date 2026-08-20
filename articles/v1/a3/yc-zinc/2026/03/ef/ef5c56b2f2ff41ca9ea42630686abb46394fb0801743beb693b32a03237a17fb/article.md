---
schema_version: "1.0.0"
document_id: "ef5c56b2f2ff41ca9ea42630686abb46394fb0801743beb693b32a03237a17fb"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/mpp"
published_at: "2026-03-26T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T22:17:35.086279+00:00"
content_hash: "sha256:730b0235c17d14e9eb0c9bdf5146bf883ee9d92c1298b70d60718fd1f9edd3a7"
---

# Zinc MPP Integration: Agents Buy Online, No Setup

Today we're launching[MPP](https://mpp.dev/) support on Zinc — the first retail integration in the protocol's launch cohort where agents pay and receive **physical goods** , not just API access. Any MPP-compatible client can hit` /agent/orders` , handle the[HTTP 402](https://www.zinc.com/blog/http-402-agentic-commerce) challenge, pay with Tempo stablecoins or Stripe, and get a real order placed at Amazon, Walmart, Target, and 50+ other retailers. **No Zinc account. No API key. No checkout page.**


New to the protocol? Read[What Is MPP?](https://www.zinc.com/blog/what-is-mpp) for the full spec walkthrough. Choosing between agent payment standards? See[MPP vs x402](https://www.zinc.com/blog/mpp-vs-x402) . Ready to ship? Follow our[AI shopping agent tutorial](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) or explore[AI agent commerce with Zinc](https://www.zinc.com/solutions/ai) .


**In this post, you'll see:**


- Why agents still hit a wall when a plan needs physical purchases
- How the[Zinc MPP integration](https://www.zinc.com/docs/v2/mpp) turns one POST into a placed order
- The full 402 challenge-and-pay flow with request and response examples
- Where to try the live playground


Here's the request flow from "buy party hats" to a tracking number.


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


## Agents can't buy things


AI agents can build entire codebases, research any topic in seconds, and write better than most people. But ask one to do something that involves buying a physical thing and it falls apart.


Tell an agent to plan your five-year-old's birthday party. It'll find venues, draft the invite list, suggest a theme, and build you a timeline. But who's buying the party hats? The balloons? The cake? At some point the plan hits a wall because the agent has no way to actually purchase anything.


[MPP](https://www.zinc.com/blog/what-is-mpp) changes that. It embeds payment negotiation directly into HTTP using[402 Payment Required](https://www.zinc.com/blog/http-402-agentic-commerce) . When an agent hits a 402, it knows what to do: see the price, pay, and get what it asked for. No signup, no dashboard.


## Enter Zinc + MPP


Your agent can't buy party hats from Amazon because Amazon doesn't support MPP. But Zinc does.


Zinc is an API for buying things. You give us a product URL and a shipping address, and we place the order on any major retailer. We handle the checkout, the payment to the retailer, the shipping, and the tracking. Until now, that required a Zinc account and API keys, which meant a human had to set things up first.


With MPP, your agent can go from "I need to buy party hats" to a placed order without any prior setup. It finds the product, sends a request to Zinc, pays inline, and the order is placed. Zinc handles everything from there: purchasing from the retailer, shipping, tracking, and delivery. The agent gets back an API key tied to its wallet that works across all of Zinc's endpoints, so it can track the order, place more orders, and operate just like any other Zinc customer. For a full build walkthrough, see[How to Build an AI Shopping Agent](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) .


No account creation. No key management. No pre-configured billing. The agent that planned the birthday party can now actually throw it.


## How it works


The agent runs a single command using the Tempo CLI, passing the order details to Zinc's MPP endpoint:


```text
tempo request -X POST   \
--json   '{
"products": [{ "url": "https://www.amazon.com/dp/B0EXAMPLE", "quantity": 1 }],
"max_price": 2499,
"shipping_address": {
"name": "Jane Smith",
"address_line1": "123 Main St",
"city": "Seattle",
"state": "WA",
"postal_code": "98101",
"country": "US",
"phone": "2065551234"
}
}'     \
https://api.zinc.com/agent/orders
```


Under the hood, Tempo sends the request to Zinc. Zinc doesn't see any payment yet, so it responds with a 402 containing the price and accepted payment methods:


```text
HTTP/1.1     402     Payment Required
Content-Type  :     application/json
WWW-Authenticate  :     stripe realm="zinc" charge="..." amount="25.99" currency="usd"
WWW-Authenticate  :     tempo realm="zinc" charge="..." amount="25.99" currency="usd"


{
"error"  :     {
"code"  :     "payment_method_required"  ,
"message"  :     "Payment required"  ,
"details"  :     {
"methods"  :     [
{
"method"  :     "stripe"  ,
"intent"  :     "charge"  ,
"amount"  :     "25.99"  ,
"currency"  :     "usd"
}  ,
{
"method"  :     "tempo"  ,
"intent"  :     "charge"  ,
"amount"  :     "25.99"  ,
"currency"  :     "usd"
}
]
}
}
}
```


Tempo sees the 402, pays 25.99 USDC, and resubmits the request with a payment credential. Zinc verifies the payment and confirms the order:


```text
HTTP/1.1     201     Created
Content-Type  :     application/json
X-Api-Key  :     zn_live_abc123...
Payment-Receipt  :     tempo receipt=...


{
"id"  :     "3f2b8c7e-1a4d-4e6f-9c2b-5d8a3f1e7b9c"  ,
"status"  :     "in_progress"
}
```


The` X-Api-Key` in the response is a persistent key tied to the agent's wallet. The same wallet always gets the same key, so the agent can use it to check order status and interact with Zinc's other endpoints just like any API customer:


```text
curl   https://api.zinc.com/orders/3f2b8c7e-1a4d-4e6f-9c2b-5d8a3f1e7b9c   \
-H   "Authorization: Bearer zn_live_abc123..."
```


## Try it


We built an[interactive MPP playground](https://agent.zinc.com/) where you can place a real order using Tempo stablecoins. Pick a cheap item, pay, and watch the order go through. Stripe support is coming soon.


For full integration details, request schemas, and response examples, check out the[MPP documentation](https://www.zinc.com/docs/v2/mpp) .


If you have feedback or ideas, reach out atmachine-payments​@zinc.com .


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
