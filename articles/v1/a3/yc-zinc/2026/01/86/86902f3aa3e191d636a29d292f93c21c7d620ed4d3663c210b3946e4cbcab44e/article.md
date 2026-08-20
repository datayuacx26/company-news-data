---
schema_version: "1.0.0"
document_id: "86902f3aa3e191d636a29d292f93c21c7d620ed4d3663c210b3946e4cbcab44e"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/test-mode"
published_at: "2026-01-20T00:00:00+00:00"
first_seen_at: "2026-07-24T09:14:52.140847+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:c07dcea6527027c73755a3487321505fb3a0dca49db65a31b3fa173ad67dfb6d"
---

# Zinc API Sandbox: Test Ecommerce Integrations Without Real Orders

The Zinc API sandbox is an isolated test environment for building and validating ecommerce integrations without placing real orders, incurring charges, or touching production data. Flip on test mode with a` zn_test_` API key or the` X-Test-Mode` header, simulate order flows end to end, and verify that your app handles both success and failure before you process live purchases.


New to Zinc? Start with the[quickstart](https://www.zinc.com/docs/quickstart) or browse the full[API docs](https://www.zinc.com/docs) . For sandbox setup, test products, and error scenarios, see[Sandbox & Testing](https://www.zinc.com/docs/v2/api-reference/introduction/sandbox) . When you're ready to go live, check[pricing](https://www.zinc.com/pricing) —and pair test mode with[webhooks](https://www.zinc.com/blog/webhooks) so async order updates reach your app in real time instead of relying on polling.


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


## Why this matters


Until now, testing meant one of two things: place real orders and cancel them, or just hope your code works when it hits production.


Neither is great. Real orders cost real money, even if you cancel them. And "hope it works" isn't a testing strategy. It's how you find bugs in production when a customer's order fails and you don't know why.


**We wanted something better.** If you've ever integrated with Stripe, you know what good looks like: a test environment that behaves exactly like production, but with fake data and no real consequences. Flip a toggle, use test credentials, and build with confidence.


That's what we built.


## How it works


There are two ways to enable test mode.


You can use a test API key, which is prefixed with` zn_test_` :


```text
curl   https://api.zinc.io/v2/orders   \
-H   "Authorization: Bearer zn_test_abc123..."
```


Or you can add a header to any request:


```text
curl   https://api.zinc.io/v2/orders   \
-H   "Authorization: Bearer zn_live_abc123..."     \
-H   "X-Test-Mode: true"
```


## Test products


The point of a test environment isn't just to see the happy path. It's to verify your integration handles failures correctly.


Real orders fail in specific ways. An item goes out of stock mid-checkout. The price changes and exceeds your max. The address doesn't validate. A variant is required but wasn't selected. Your wallet balance is too low.


You need to know your app handles all of these before you're processing real orders for real customers.


So we created test product URLs that trigger each scenario:


- ` test-success` — order completes successfully
- ` test-out-of-stock` — product unavailable
- ` test-price-exceeded` — total exceeds your max_price
- ` test-invalid-address` — shipping address fails validation
- ` test-invalid-variant` — variant selection required
- ` test-shipping-unavailable` — can't ship to that address
- ` test-insufficient-funds` — wallet balance too low


```text
curl   -X POST https://api.zinc.io/v2/orders   \
-H   "Authorization: Bearer zn_test_abc123..."     \
-H   "Content-Type: application/json"     \
-d   '{
"products": [{"url": "https://zinc.io/shop/products/test-out-of-stock"}],
"shipping_address": {...},
"max_price": 5000
}'
```


That order will fail with` product_out_of_stock` , exactly like it would in production if the item wasn't available.


## Synchronous vs asynchronous errors


Some errors happen immediately, some happen later.


**Synchronous errors** , like an invalid address or unreachable URL, come back right away in the API response. You can catch these and show the user an error before anything else happens.


**Asynchronous errors** , like out of stock or price exceeded, happen during order processing. The order is created successfully, but fails later when we're actually checking out on the retailer's site. These show up when you poll the order status.


This matches how real orders behave. Some problems we can catch upfront. Some we only discover mid-checkout. Your integration needs to handle both, and now you can test both.


## Data isolation


Test mode and production are completely separate. Test orders don't appear in your production order list. Production orders don't appear in test mode.


You can build your entire integration in test mode, verify everything works, then swap to a live API key and go.


## Try it for yourself!


Switch to test mode directly in the[dashboard](https://app.zinc.com/) . Create orders, trigger errors, and verify your integration handles everything correctly without spending a dollar.


For the full list of test products and error scenarios, check out[Sandbox & Testing](https://www.zinc.com/docs/v2/api-reference/introduction/sandbox#sandbox-and-testing) in our docs.


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
