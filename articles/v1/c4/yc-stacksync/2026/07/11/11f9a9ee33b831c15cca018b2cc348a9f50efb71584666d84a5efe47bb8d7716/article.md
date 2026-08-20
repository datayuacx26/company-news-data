---
schema_version: "1.0.0"
document_id: "11f9a9ee33b831c15cca018b2cc348a9f50efb71584666d84a5efe47bb8d7716"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/how-to-sync-tiktok-shop-with-shopify"
published_at: "2026-07-22T10:05:00+00:00"
first_seen_at: "2026-07-22T16:41:29.064477+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:80d9d1e691b2c871de1d38d3a8ee98ef28c5dec863f72322b4b9db9282edabc7"
---

# Selling the Same Stock Twice: Putting TikTok Shop and Shopify on One Count

The problem is easy to describe and annoying to live with. You have two storefronts and one warehouse. TikTok Shop sells a unit, Shopify does not find out for twenty minutes, and in those twenty minutes Shopify sells the same unit to someone else. One of those two buyers is about to get an apology email.


Everything else follows from that. Prices drift because someone updated one catalog. Tracking numbers get rekeyed by hand because they live in Shopify and are needed in TikTok Shop. Cancellations land in one place and never reach the other.


This walks through the setup end to end. If you are still choosing the layer underneath, the wider view is in the guide to an[enterprise iPaaS for TikTok Shop](https://www.stacksync.com/blog/enterprise-grade-ipaas-for-tiktok-shop) .


## Step 1: decide which system owns what


Do this before you connect anything. Almost every failed multi-channel setup failed here, not in the wiring, because two systems were both allowed to be right about the same field.


The usual split: Shopify owns the catalog, because it already has the product data and the merchandising team lives there. Inventory is shared, with one available-to-sell number both channels read and write. TikTok Shop owns the orders it creates, including their status and the buyer's edits. Fulfilment is owned by whoever picks the box, which is Shopify or a 3PL behind it.


Object Owner Direction


Product and variant Shopify Shopify to TikTok Shop


Price Shopify Shopify to TikTok Shop


Available to sell Shared pool Both ways


Order and buyer edits TikTok Shop TikTok Shop to Shopify


Fulfilment and tracking Shopify or 3PL Shopify to TikTok Shop


Cancellation and refund TikTok Shop Both ways


One owner per object. Inventory is the exception, and that is exactly why it needs a conflict rule.


## Step 2: match the SKUs once


Matching is the part that decides whether the rest works. Pair each TikTok Shop listing to its Shopify variant on SKU first, then barcode as a fallback. Anything that matches neither goes to a review list rather than creating a new product, because a sync that creates on no-match will quietly double your catalog.


Two cases deserve attention. Variants have to pair at the variant level, not the product level, or a size run of eight will share one stock number. Bundles need a component map: selling one bundle should deduct each component from the pool, otherwise the components stay available and you oversell the parts instead of the kit.


## Step 3: share one inventory pool


This is the step that stops the apology emails. Instead of two counts that are reconciled on a schedule, both channels read and write the same available-to-sell number, and every order on either side moves it in seconds.


The whole point of the loop is step 04. Until the quantity moves, the other channel is still selling the unit.


Keep a buffer. During a spike there is always a short window where orders exist but the deduction has not landed everywhere, and holding back a few units per SKU absorbs it. Set the buffer per SKU rather than globally, because the risk is concentrated in the handful of products a video actually featured.


Set the conflict rule while you are here. Inventory is the one object both sides write, so decide explicitly what happens when both changed: for stock the usual answer is that the warehouse count wins, since it is the only number backed by physical units on a shelf.


## Step 4: close the loop on fulfilment


An order that reaches Shopify is only half the job. TikTok Shop measures how quickly orders are dispatched, so the tracking number has to come back before that window closes, and rekeying it by hand is how the window gets missed on the busiest days.


One order, out and back. Origin tags on each write are what stop the return trip being read as a new change.


The same path carries the exceptions. A cancellation matches back to the original order, updates Shopify, and releases the stock. A partial refund adjusts the order rather than creating a new one. These are the cases a one-way order feed cannot handle, and they are the reason most sellers rebuild the integration rather than patch it.


## What to watch in the first week


Three numbers tell you whether the setup is healthy. How many listings are sitting in the unmatched review list, how long a stock change takes to appear on the other channel, and how many orders were dispatched outside the window. If the first is shrinking and the other two are flat during a spike, it is working.


One thing worth planning for separately: TikTok Shop changed how seller shipping works, and it affects which fulfilment path you can use. We covered it in[TikTok Shop ending seller shipping](https://www.stacksync.com/blog/what-changed-tiktok-shop-ends-seller-shipping) . The connector details are on the[Shopify and TikTok Shop integration page](https://www.stacksync.com/integrations/shopify-and-tiktok) .


## Two shopfronts, one truth


The goal is not that TikTok Shop holds a copy of your Shopify catalog. It is that a buyer in the TikTok app and a buyer on your own site are looking at the same stock, the same price, and the same delivery promise, and that neither of them is buying a unit that left the warehouse an hour ago.


If your orders also have to reach an ERP, that is the next piece:[TikTok Shop and NetSuite](https://www.stacksync.com/blog/two-way-sync-solutions-tiktok-shop-netsuite) . Or[book a demo](https://www.stacksync.com/book-a-demo) and we will match a real SKU across both of your channels.
