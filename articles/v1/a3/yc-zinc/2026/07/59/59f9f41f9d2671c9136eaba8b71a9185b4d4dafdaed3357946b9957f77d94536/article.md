---
schema_version: "1.0.0"
document_id: "59f9f41f9d2671c9136eaba8b71a9185b4d4dafdaed3357946b9957f77d94536"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/shipment-tracking-api"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-24T09:14:52.140847+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:aa27bebf591a185f28800c9b640485ad342f656a141e28b8c51e317d5e85aa4d"
---

# Shipment Tracking API for Developers: Polling, Webhooks, and Multi-Retailer Tracking (2026)

You're building procurement automation, a rewards catalog, a gifting platform, or an[AI shopping agent](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) . Orders go out to Amazon, Walmart, Target, and Best Buy. Your users ask one question over and over: **where is my stuff?**


A **shipment tracking API** should answer that without five retailer integrations, three carrier accounts, and a cron job that scrapes order history pages every hour. Most teams nail checkout automation first, then spend months patching together tracking from emails, retailer portals, and carrier aggregators that were never designed to work together.


This is the full **developer integration guide** — not the[Launch Week tracking announcement](https://www.zinc.com/blog/tracking) , which covers what Zinc ships. Here you'll learn what a unified **ecommerce order tracking API** needs to do, why DIY approaches break when you **track shipments across multiple retailers** , and how to wire up **programmatic order tracking** with polling,[webhooks](https://www.zinc.com/blog/webhooks) , and customer-facing links. Field reference:[Order tracking API](https://www.zinc.com/docs/v2/api-reference/orders/tracking) and[Webhooks](https://www.zinc.com/docs/v2/api-reference/introduction/webhooks) .


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


## What is a shipment tracking API?


A **shipment tracking API** is an interface that returns structured shipment status — carrier, tracking number, checkpoints, and delivery state — for orders your software placed or manages. A good one does not stop at label creation. It ingests post-purchase signals from retailers, normalizes status across carriers, and pushes updates to your app through webhooks or a pollable order object.


That is different from a carrier-only API, which assumes you already have a tracking number, and from marketplace seller APIs, which only cover shipments from stores you operate. For **unified order tracking** across Amazon, Walmart, Target, and other retailers you buy from but do not own, you need an API that owns both purchase execution and post-checkout status.


**Key takeaways:**


- Retailers rarely expose clean tracking APIs when you buy as an automation layer. Extraction from post-purchase signals is the hard part.
- Carrier-only APIs (EasyPost, Shippo, AfterShip, etc.) track packages *after* you have a number. They do not solve getting that number from Amazon, Walmart, or Target accounts.
- Zinc attaches` tracking_numbers\[\]` to the order object automatically after placement. There is no separate "create tracking" endpoint.
- Use webhooks (` order.tracking_received` ,` order.delivered` ) in production. Poll` GET /orders/{order_id}` as a fallback or for backfill.
- Split shipments are normal. One order can have multiple tracking numbers across different carriers.
- Zinc tracking is included when you buy through Zinc. It is not a standalone carrier tracking API for orders placed elsewhere.


## Why multi-retailer tracking is harder than it looks


The naive plan looks like this: place orders, wait for a tracking number, hand it to a carrier API, show status in your app. Three steps. Should take a week.


It usually takes a quarter, because step two is where everything falls apart.


### Retailers do not give you a tracking API


When you automate purchases at consumer retailers, you are not a seller on their marketplace. You do not get SP-API or Walmart Marketplace webhooks for outbound shipments. You get an order confirmation email, maybe a "your item has shipped" email, and a "Track Package" button that sometimes requires a logged-in session to reveal anything useful.


The catch: every retailer formats this differently. Amazon Logistics uses TBA numbers. Walmart may route through FedEx or OnTrac. Target splits items across fulfillment centers. Some emails include the tracking number inline. Some bury it behind an authenticated redirect. There is no standard REST endpoint that says` GET /my-orders/{id}/tracking` .


If you already integrated[Amazon programmatic buying](https://www.zinc.com/blog/amazon-api) or[Walmart ordering](https://www.zinc.com/blog/walmart-api) through a unified API, you solved checkout. Tracking is a separate extraction problem on the same fragile surface.


### Carrier APIs solve a different job


Shipping label APIs and carrier aggregators are excellent at what they do: given a tracking number and carrier, return normalized status events, ETAs, and delivery confirmation. EasyPost, Shippo, AfterShip, and similar tools assume **you already have the tracking number** because you generated the label or the merchant pushed it to you through a platform you control.


That assumption breaks for tail-spend procurement, rewards fulfillment, and agentic commerce. You placed the order through a retailer account. The label was created on their side. The tracking number lives in their notification pipeline, not yours. Wiring carrier APIs alone leaves a gap between "order placed" and "tracking number acquired."


### Scraping retailer order pages fails the same way checkout scrapers do


The fallback is obvious: log into each retailer's order history, scrape the tracking field, poll until it appears. This is the tracking equivalent of the browser automation pain described in[web scraping vs ecommerce API](https://www.zinc.com/blog/web-scraping-vs-ecommerce-api) guides.


Bot detection, session expiry, 2FA prompts, A/B tested page layouts, and quarterly UI redesigns all apply. You are not maintaining one scraper. You are maintaining one per retailer, plus logic to map each retailer's status vocabulary into something your app understands. That is not a shipment tracking API. That is an ops team.


## What a shipment tracking API should actually do


Strip away the vendor marketing. A **shipment tracking API** for multi-retailer ecommerce needs to do four things:


1. **Ingest post-purchase signals** from retailers after an order is placed (emails, account pages, ship notifications).
2. **Extract and normalize** carrier, tracking number, and shipment status into a stable schema.
3. **Enrich with carrier checkpoints** so your app can show "out for delivery" without calling UPS yourself.
4. **Push updates** to your system via webhooks or a pollable order object, including split shipments.


If any of those steps is missing, your "tracking integration" is a script, not an API.


### Unified tracking across retailers


Use a unified purchasing API that owns the full post-checkout lifecycle. You place orders through one integration. Tracking numbers appear on the same order object regardless of whether the underlying retailer was Amazon, Walmart, or Target. Your app reads` tracking_numbers\[\]` from` GET /orders/{order_id}` or subscribes to` order.tracking_received` webhooks. You do not integrate each retailer's notification format separately.


That is the model Zinc uses:[search → order → track → return](https://www.zinc.com/docs/quickstart) through one API surface, with tracking included rather than bolted on.


### Comparison: retailer scraping vs carrier-only vs unified API


Approach Gets tracking numbers Multi-retailer Webhooks Maintenance burden


Scrape retailer order pages Sometimes Per-retailer build DIY High — breaks on UI and auth changes


Carrier aggregator only No — needs number first Carrier-agnostic Varies by vendor Medium — you still extract numbers


Marketplace seller APIs (SP-API, etc.) For your own seller shipments Single marketplace Yes Low per platform, but seller-only


Unified purchasing + tracking API (Zinc) Yes — auto-extracted 50+ retailers, one schema Yes Low — same payload across retailers


The right row depends on your workflow. If you only ship from your own Shopify store, Shopify's native tracking is enough. If you buy from retailers you do not own on behalf of users, you need the last row or equivalent in-house infrastructure.


## How Zinc's shipment tracking API works


Zinc is a unified[ecommerce purchasing API](https://www.zinc.com/blog/best-ecommerce-purchasing-apis) : search products, place orders, track shipments, and initiate returns across[50+ retailers](https://www.zinc.com/integrations) . Tracking is not a separate product you enable. It is part of the order lifecycle once you place an order through Zinc — the same capability[announced at Launch Week](https://www.zinc.com/blog/tracking) , documented here as a step-by-step integration guide.


Here is the flow:


1. **Order placed.** Your app calls` POST /orders` (or uses[MPP agent payments](https://www.zinc.com/blog/what-is-mpp) for wallet-native checkout). Zinc executes the retailer-specific purchase.
2. **Retailer ships.** The retailer creates a label and sends a ship notification through their normal channels.
3. **Zinc extracts tracking.** Zinc parses retailer notifications and account signals, resolves the carrier, and attaches tracking to your order object. Supported carriers are auto-detected:` ups` ,` fedex` ,` usps` ,` amazon` ,` dhl` .
4. **Tracking appears on the order.** Poll` GET /orders/{order_id}` or list orders with checkpoint timelines via` GET /orders?include=tracking_events` .
5. **Webhooks fire.** Configure a webhook URL and receive` order.tracking_received` when numbers arrive, then` order.delivered` when all packages are delivered.


There is no` POST /tracking` endpoint. You do not register packages manually. If Zinc placed the order, tracking shows up on that order when the retailer ships.


### Status and checkpoints in plain English


Each entry in` tracking_numbers\[\]` includes:


Field Meaning


` id` Unique identifier for this tracking record


` carrier` Normalized carrier slug (` ups` ,` fedex` ,` usps` ,` amazon` ,` dhl` )


` tracking_number` The carrier-facing tracking ID


` status` Aggregate status:` pending` ,` in_transit` , or` delivered`


` checkpoints\[\]` Carrier scan events, most recent first


` created_at` When Zinc first recorded this tracking number


Each checkpoint includes` checkpoint_time` ,` status` ,` message` , and location fields (` city` ,` state` ,` country` ,` zip` ) when the carrier provides them. Your UI can show a timeline without calling carrier APIs directly.


**Multiple tracking numbers** are common. Items ship from different fulfillment centers, or one line item ships today and another ships next week. Your integration should iterate` tracking_numbers\[\]` , not assume a single package.


**Timing:** Tracking may not exist immediately after order placement. Retailers often take hours or days to ship. Build for empty` tracking_numbers\[\]` on fresh orders and handle updates asynchronously.


## Code: poll an order for tracking


The simplest integration path: poll the order until tracking appears, then refresh until delivered.


```text
GET https://api.zinc.com/orders/550e8400-e29b-41d4-a716-446655440000
Authorization  :     Bearer YOUR_API_KEY
```


Example response with two tracking numbers and checkpoints:


```text
{
"id"  :     "550e8400-e29b-41d4-a716-446655440000"  ,
"status"  :     "shipped"  ,
"retailer"  :     "amazon"  ,
"tracking_numbers"  :     [
{
"id"  :     "trk_01HQ8X2K9M4N"  ,
"carrier"  :     "amazon"  ,
"tracking_number"  :     "TBA123456789000"  ,
"status"  :     "in_transit"  ,
"created_at"  :     "2026-07-05T14:22:00Z"  ,
"checkpoints"  :     [
{
"checkpoint_time"  :     "2026-07-06T09:15:00Z"  ,
"status"  :     "in_transit"  ,
"message"  :     "Package arrived at a carrier facility"  ,
"city"  :     "Phoenix"  ,
"state"  :     "AZ"  ,
"country"  :     "US"  ,
"zip"  :     "85043"
}  ,
{
"checkpoint_time"  :     "2026-07-05T18:40:00Z"  ,
"status"  :     "pending"  ,
"message"  :     "Shipping label created"  ,
"city"  :     "Goodyear"  ,
"state"  :     "AZ"  ,
"country"  :     "US"  ,
"zip"  :     "85338"
}
]
}  ,
{
"id"  :     "trk_01HQ8X2K9M4P"  ,
"carrier"  :     "ups"  ,
"tracking_number"  :     "1Z999AA10123456784"  ,
"status"  :     "delivered"  ,
"created_at"  :     "2026-07-04T11:05:00Z"  ,
"checkpoints"  :     [
{
"checkpoint_time"  :     "2026-07-06T16:30:00Z"  ,
"status"  :     "delivered"  ,
"message"  :     "Delivered to front door"  ,
"city"  :     "Austin"  ,
"state"  :     "TX"  ,
"country"  :     "US"  ,
"zip"  :     "78701"
}
]
}
]
}
```


For ops dashboards that need checkpoint timelines across many orders, use the list endpoint with the include flag (omitted by default to keep payloads small):


```text
GET https://api.zinc.com/orders?include=tracking_events&limit=50
Authorization  :     Bearer YOUR_API_KEY
```


Full field reference:[Order tracking docs](https://www.zinc.com/docs/v2/api-reference/orders/tracking) and[Get order](https://www.zinc.com/docs/v2/api-reference/orders/get-order) .


**Don't poll aggressively.** Tracking often takes 24–48 hours to appear after an order is placed, so a tight polling loop mostly burns requests. Back off exponentially (15m → 30m → 1h) until the first tracking number lands, and prefer webhooks (below) once you're past prototyping.


## Code: webhook handler


Polling works for prototypes. Production apps should subscribe to[webhooks](https://www.zinc.com/blog/webhooks) and treat polling as backfill.


Configure your webhook URL in the[Zinc dashboard](https://app.zinc.com/settings) . Zinc POSTs to your endpoint on lifecycle events:


- ` order.placed` — retailer confirmed the purchase
- ` order.failed` — order could not be completed after retries
- ` order.cancelled` — order was cancelled
- ` order.tracking_received` — one or more tracking numbers extracted
- ` order.delivered` — all packages on the order delivered


Example Node.js handler with signature verification and idempotency:


```text
import     crypto     from     'crypto'  ;
import     express     from     'express'  ;


const   app   =     express  (  )  ;
const     WEBHOOK_SECRET     =   process  .  env  .  ZINC_WEBHOOK_SECRET  ;
const   processedEvents   =     new     Set  (  )  ;     // use Redis/DB in production


app  .  post  (  '/webhooks/zinc'  ,   express  .  raw  (  {     type  :     'application/json'     }  )  ,     (  req  ,   res  )     =>     {
const   signature   =   req  .  headers  [  'x-webhook-signature'  ]  ;
const   expected   =   crypto
.  createHmac  (  'sha256'  ,     WEBHOOK_SECRET  )
.  update  (  req  .  body  )
.  digest  (  'hex'  )  ;


if     (  signature   !==   expected  )     {
return   res  .  status  (  401  )  .  send  (  'Invalid signature'  )  ;
}


const   payload   =     JSON  .  parse  (  req  .  body  )  ;
const   dedupeKey   =     `  ${  payload  .  event  }  :  ${  payload  .  order_id  }  :  ${  payload  .  timestamp  }  `  ;


if     (  processedEvents  .  has  (  dedupeKey  )  )     {
return   res  .  status  (  200  )  .  send  (  'Already processed'  )  ;
}
processedEvents  .  add  (  dedupeKey  )  ;


switch     (  payload  .  event  )     {
case     'order.tracking_received'  :
notifyCustomer  (  payload  .  order_id  ,   payload  .  data  .  tracking_numbers  )  ;
break  ;
case     'order.delivered'  :
closeLoop  (  payload  .  order_id  )  ;
break  ;
case     'order.failed'  :
alertOps  (  payload  .  order_id  ,   payload  .  data  )  ;
break  ;
}


res  .  status  (  200  )  .  send  (  'OK'  )  ;
}  )  ;
```


Python equivalent for the verification step:


```text
import   hmac
import   hashlib


def     verify_zinc_webhook  (  body  :     bytes  ,   signature  :     str  ,   secret  :     str  )     -  >     bool  :
expected   =   hmac  .  new  (  secret  .  encode  (  )  ,   body  ,   hashlib  .  sha256  )  .  hexdigest  (  )
return   hmac  .  compare_digest  (  signature  ,   expected  )
```


**Idempotency matters.** Webhooks can retry. Key handlers on` order_id` + event type + timestamp (or a webhook delivery ID if present). Updating customer-facing status should be safe to run twice.


Store routing metadata in` client_notes` when placing the order (user ID, ticket ID, chat session) so webhook handlers know who to notify. See[Webhooks documentation](https://www.zinc.com/docs/v2/api-reference/introduction/webhooks) for payload shapes and signature verification.


## Code: customer-facing tracking links


Your app has normalized carrier slugs. Map them to public tracking URLs so users can click through without you building a full tracking UI on day one.


```text
const     CARRIER_TRACKING_URLS     =     {
ups  :     (  n  )     =>     `  https://www.ups.com/track?tracknum=  ${  n  }  `  ,
fedex  :     (  n  )     =>     `  https://www.fedex.com/fedextrack/?trknbr=  ${  n  }  `  ,
usps  :     (  n  )     =>     `  https://tools.usps.com/go/TrackConfirmAction?tLabels=  ${  n  }  `  ,
amazon  :     (  n  )     =>     `  https://track.amazon.com/tracking/  ${  n  }  `  ,
dhl  :     (  n  )     =>     `  https://www.dhl.com/us-en/home/tracking.html?tracking-id=  ${  n  }  `  ,
}  ;


function     trackingUrl  (  carrier  ,   trackingNumber  )     {
const   builder   =     CARRIER_TRACKING_URLS  [  carrier  ]  ;
return   builder   ?     builder  (  encodeURIComponent  (  trackingNumber  )  )     :     null  ;
}
```


For a richer experience, render` checkpoints\[\]` directly in your app and use carrier URLs as a fallback link. That is usually enough for procurement requesters, rewards recipients, and agent chat surfaces without embedding third-party widgets.


## Multi-retailer reality: one integration, many sources


The maintenance argument is straightforward. Without a unified layer, tracking Amazon means parsing Amazon ship emails and order pages. Tracking Walmart means a different email format, different carrier mix, different auth flow. Tracking Target adds a third pipeline. Each one breaks independently.


With Zinc, your integration code does not branch on retailer for tracking:


```text
async     function     getTrackingStatus  (  orderId  )     {
const   res   =     await     fetch  (  `  https://api.zinc.com/orders/  ${  orderId  }  `  ,     {
headers  :     {     Authorization  :     `  Bearer   ${  API_KEY  }  `     }  ,
}  )  ;
const   order   =     await   res  .  json  (  )  ;
return   order  .  tracking_numbers  .  map  (  (  t  )     =>     (  {
carrier  :   t  .  carrier  ,
number  :   t  .  tracking_number  ,
status  :   t  .  status  ,
lastUpdate  :   t  .  checkpoints  [  0  ]  ?.  message   ??     'Label created'  ,
}  )  )  ;
}
```


The same function works whether the order was placed at[Amazon](https://www.zinc.com/blog/amazon-api) ,[Walmart](https://www.zinc.com/blog/walmart-api) , Target, or Best Buy. Split shipments across retailers in one platform (common in procurement and gifting) still surface as separate orders with their own` tracking_numbers\[\]` , but your schema stays consistent.


That consistency matters for[agentic commerce](https://www.zinc.com/blog/agentic-commerce) stacks where an agent places orders, waits for delivery confirmation, and only then marks a task complete. The agent should not need retailer-specific tracking tools in its MCP server.


## Use cases


### Procurement automation and restock alerts


A requisition gets approved, Zinc places the order at the cheapest in-stock retailer, and your ERP stays in "awaiting delivery" until` order.delivered` fires. No one checks Amazon order history manually. Delivery confirmation closes the PO loop automatically — and if something arrives damaged, your[returns workflow](https://www.zinc.com/blog/best-returns-management-software) picks up from there. Pairs naturally with[procurement automation API](https://www.zinc.com/blog/procurement-automation-api) workflows.


### Rewards and loyalty fulfillment


Points redeemed for physical goods need shipping notifications. When` order.tracking_received` arrives, email the recipient with a tracking link. When` order.delivered` fires, trigger a satisfaction survey or loyalty bonus. One webhook handler covers Amazon, Walmart, and Target rewards catalogs.


### BNPL and marketplace order status pages


Marketplaces that buy from external retailers on a user's behalf need an order status page that is not a lie. Poll or webhook-driven updates from Zinc keep "processing → shipped → delivered" accurate without building retailer scrapers per merchant.


### AI shopping agents closing the loop


An[AI shopping agent](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) that places orders should confirm delivery before archiving the conversation. Webhook-driven` order.delivered` events let the agent message the user proactively: "Your order arrived." That is the difference between a shopping chatbot and a shopping agent.


### Ops dashboards for bulk CSV orders


Teams uploading hundreds of orders via CSV or internal tools still need visibility. List orders with` include=tracking_events` to populate an ops dashboard. Filter by` status` and surface stalled shipments (order placed five days ago, no tracking yet) for manual review.


## What Zinc tracking is not


Honest limits keep integrations sane:


- **Not a standalone carrier tracking API for external orders.** If you did not place the order through Zinc, you cannot attach it retroactively. Use a carrier aggregator for packages you already have numbers for.
- **Not real-time GPS.** Checkpoints reflect carrier scan events. "Out for delivery" means the carrier reported it, not that your app has live truck coordinates.
- **Not instant.** Tracking depends on retailer ship speed and when they send notifications. An order placed at 2pm may show empty` tracking_numbers\[\]` until the next business day.
- **Not a replacement for your own OMS** if you operate a merchant storefront with first-party fulfillment. Shopify and BigCommerce have native tracking for orders you ship yourself. Zinc is for orders placed at retailers you do not control.


## Frequently asked questions


### What is a shipment tracking API?


A shipment tracking API returns structured delivery status — carrier, tracking number, checkpoints, and delivery state — for orders your application manages. The best ones extract tracking from retailer post-purchase signals automatically, not just poll carrier sites after you manually copy a number.


### How do I track shipments from multiple retailers with one API?


Place orders through a unified purchasing API that attaches` tracking_numbers\[\]` to each order object regardless of retailer. Read status with` GET /orders/{order_id}` or subscribe to` order.tracking_received` and` order.delivered` webhooks. You do not build separate Amazon, Walmart, and Target tracking integrations.


### What is the difference between a carrier tracking API and an ecommerce order tracking API?


Carrier APIs (EasyPost, Shippo, AfterShip) track packages once you have a tracking number and carrier. An **ecommerce order tracking API** also solves extraction: getting that number from retailer ship notifications when you bought through Amazon, Walmart, or Target rather than shipping from your own warehouse.


### Does Zinc have a separate tracking API endpoint?


No. There is no` POST /tracking` endpoint. Tracking numbers are extracted automatically after you place an order through Zinc and appear on the order object. Poll` GET /orders/{order_id}` or use webhooks for updates.


### Can I use Zinc tracking for orders I did not place through Zinc?


No. Zinc tracking covers orders placed through Zinc's[Ordering API](https://www.zinc.com/docs/v2/api-reference/orders/create-order) . For packages where you already have a tracking number from another source, use a carrier aggregator instead.


### How long until tracking appears after placing an order?


Often 24–48 hours, sometimes longer. Retailers ship on their own schedule and send notifications asynchronously. Build for empty` tracking_numbers\[\]` on fresh orders and use webhooks rather than aggressive polling.


### What is programmatic order tracking?


**Programmatic order tracking** means your software reads and reacts to shipment status without a human checking retailer portals or carrier websites. Webhooks fire when tracking arrives or delivery completes; your app updates ERP records, sends customer emails, or closes agent tasks automatically.


## Bottom line


If you are building multi-retailer purchasing, do not rebuild tracking as a side project. The hard part is extracting numbers and normalizing status across retailers with no standard API. That is the job a **shipment tracking API** should absorb so your app reads one order object and moves on.


Zinc includes tracking in the same API you use to search and buy. Place an order, wait for` order.tracking_received` , show checkpoints to your users, close the loop on` order.delivered` . No scraper fleet. No carrier API patchwork. No per-retailer maintenance.


**Next steps:**


- [Quickstart](https://www.zinc.com/docs/quickstart) — place your first order and watch tracking appear
- [Order tracking reference](https://www.zinc.com/docs/v2/api-reference/orders/tracking) — fields, carriers, and checkpoint schema
- [Webhooks](https://www.zinc.com/docs/v2/api-reference/introduction/webhooks) — configure push notifications and verify signatures


Already shipping with Zinc? You have tracking. Wire up webhooks before you write another polling loop.


### Related reading


- [Best Ecommerce Purchasing APIs for Developers](https://www.zinc.com/blog/best-ecommerce-purchasing-apis)
- [Web Scraping vs Ecommerce APIs](https://www.zinc.com/blog/web-scraping-vs-ecommerce-api)
- [Procurement Automation API](https://www.zinc.com/blog/procurement-automation-api)
- [Programmatic Returns API](https://www.zinc.com/blog/programmatic-returns-api)
- [Amazon Shopping API](https://www.zinc.com/blog/amazon-api)
- [Walmart Shopping API](https://www.zinc.com/blog/walmart-api)
- [How to Build an AI Shopping Agent](https://www.zinc.com/blog/how-to-build-ai-shopping-agent)
- [Launch Week: Tracking](https://www.zinc.com/blog/tracking) — product announcement; this post is the integration guide
- [Launch Week: Webhooks](https://www.zinc.com/blog/webhooks)
- [Best Returns Management Software](https://www.zinc.com/blog/best-returns-management-software)


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
