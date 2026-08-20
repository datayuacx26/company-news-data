---
schema_version: "1.0.0"
document_id: "a3ef13958366b7a2782b29301182c4c6f414c92ac0d1962c07f560bb1f243992"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/tracking"
published_at: "2026-01-22T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:e217220edbf47a9d324209d85f763f73e87b37e5cb47da6dc4439cffce83b1c0"
---

# Order Tracking Across Every Retailer: Automatic Carrier & Tracking Numbers

Zinc now extracts **tracking numbers and carriers automatically** when an order ships—across every retailer we support. No extra API calls. No parsing emails yourself. Tracking just shows up on the order.


**In this post:**


- Why this is harder than it sounds — retailers hide tracking behind emails and login walls
- How it works — tracking on the order response and webhooks
- Try it for yourself — place an order and watch tracking appear


When an order ships, we attach the tracking number and carrier (UPS, FedEx, USPS, Amazon Logistics, DHL) to your order. This existed in v1 for a handful of retailers. Now it works everywhere we do.


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


## Why this is harder than it sounds


Retailers don't give you tracking numbers in an API response. They send an email that says "your order has shipped" with a button that says "Track Package."


Sometimes the tracking number is in the email. Sometimes it isn't. Sometimes you click the button and land on a page with the tracking info. Sometimes you click the button and hit a login wall, and you need to be signed into your account to see anything.


There's no standard. Every retailer does it differently.


So we built a system that handles all of it. We parse the email. If the tracking number is there, we grab it. If it's behind a link, we follow the link. If that link requires authentication, we spin up a browser, sign in, and extract the tracking info from the page.


Same adaptive approach as the ordering engine we talked about on Day 1. We're not hard-coding every retailer's email format. We're building a system that figures it out.


## How it works


Tracking info shows up automatically in the order response:


```text
{
"id"  :     "550e8400-e29b-41d4-a716-446655440000"  ,
"status"  :     "order_placed"  ,
"tracking_numbers"  :     [
{
"carrier"  :     "ups"  ,
"tracking_number"  :     "1Z999AA10123456784"  ,
"created_at"  :     "2026-01-15T14:30:00Z"
}
]
}
```


If items ship separately — from different fulfillment centers, or on different days — you'll see multiple tracking numbers. We handle that automatically.


And if you set up[Zinc order webhooks](https://www.zinc.com/blog/webhooks) , you'll get an` order.tracking_received` event the moment we have it. No polling required.


## Try it for yourself


Place an order and wait for it to ship. The tracking info will appear in the order response and (if configured) fire a webhook.


For supported carriers and how to construct tracking URLs, check out[Order Tracking](https://www.zinc.com/docs/v2/api-reference/orders/tracking) in our docs. For a full integration guide, see[Shipment Tracking API for Developers](https://www.zinc.com/blog/shipment-tracking-api) . Pair tracking with[order webhooks](https://www.zinc.com/blog/webhooks) or[returns APIs](https://www.zinc.com/blog/best-returns-management-software) when you need the rest of the post-purchase loop.


Tomorrow we're wrapping up launch week with a demo app you can actually use.


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
