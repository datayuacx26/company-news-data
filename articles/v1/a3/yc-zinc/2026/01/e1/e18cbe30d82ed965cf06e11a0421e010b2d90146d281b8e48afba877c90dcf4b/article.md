---
schema_version: "1.0.0"
document_id: "e18cbe30d82ed965cf06e11a0421e010b2d90146d281b8e48afba877c90dcf4b"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/webhooks"
published_at: "2026-01-21T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:e9f804c5f8bd01589ee7ff226c97f1753265cc2722df062c17627720624766fe"
---

# Zinc Order Webhooks: Real-Time Order Status Updates

**Zinc order webhooks** push HTTP notifications to your server when an order changes state. Configure one URL in the dashboard, and Zinc POSTs structured JSON for each lifecycle event— **no polling loop** required.


Polling works until status changes land between intervals. Your customer asks where their order is, but your last` GET /orders/{order_id}` ran nine minutes ago and the retailer shipped eight minutes ago. Webhooks close that gap so your backend and buyer-facing UI stay in sync.


Below you'll see why webhooks matter, how to configure them, which events Zinc sends, and how to validate handlers in[test mode](https://www.zinc.com/blog/test-mode) before going live. For payload schemas and signature verification, see the[Webhooks API reference](https://www.zinc.com/docs/v2/api-reference/introduction/webhooks) . When tracking arrives, webhooks tie into[order tracking](https://www.zinc.com/docs/v2/api-reference/orders/tracking) , the[tracking overview](https://www.zinc.com/blog/tracking) , and the full[shipment tracking API guide](https://www.zinc.com/blog/shipment-tracking-api) .


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


A lot happens after you submit an order. The retailer confirms it. They pick and pack. They create a shipping label. The carrier picks it up. Each step can take minutes, hours, or days, and your customers want to know what's going on.


Without webhooks, you're polling. Hitting the API over and over, waiting for something to change.


The real pain isn't the wasted API calls. It's the lag. Your customer asks "where's my order?" and your system doesn't know yet because you're polling every 10 minutes and the update came in 9 minutes ago. Or you're polling once an hour to save resources and now you're 59 minutes behind.


Webhooks fix this. We tell you the moment something changes.


## How it works


Configure your webhook URL in the Zinc dashboard under Settings. One endpoint, all events. If you used v1 webhooks, this is simpler. No more separate URLs for each event type.


Once configured, we'll POST to your URL whenever an event occurs:


```text
{
"event"  :     "order.placed"  ,
"order_id"  :     "550e8400-e29b-41d4-a716-446655440000"  ,
"status"  :     "order_placed"  ,
"timestamp"  :     "2026-01-15T14:30:00Z"  ,
"data"  :     {
"price_components"  :     {
"subtotal"  :     1999  ,
"shipping"  :     499  ,
"tax"  :     150  ,
"total"  :     2648
}
}
}
```


We send webhooks for the key moments in an order's lifecycle:


- ` order.started` — order created and queued for processing
- ` order.placed` — order successfully placed with the retailer
- ` order.failed` — order failed after all retry attempts exhausted
- ` order.tracking_received` — tracking number received from the retailer


Each event includes relevant data: price components when the order is placed, error details if it fails, carrier and tracking number when tracking arrives. For tracking-specific fields and multi-retailer patterns, see[Order tracking](https://www.zinc.com/docs/v2/api-reference/orders/tracking) and[Shipment Tracking API: How to Track Orders Across Multiple Retailers](https://www.zinc.com/blog/shipment-tracking-api) .


## Try it for yourself


Configure your webhook URL in the[dashboard](https://app.zinc.com/settings) under Settings. Create a test order in[test mode](https://www.zinc.com/blog/test-mode) and watch the events come through.


For payload structure, signature verification, and best practices, check out[Webhooks](https://www.zinc.com/docs/v2/api-reference/introduction/webhooks) in our docs.


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
