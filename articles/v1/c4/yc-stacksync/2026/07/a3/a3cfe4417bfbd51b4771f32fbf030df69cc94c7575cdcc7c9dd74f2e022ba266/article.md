---
schema_version: "1.0.0"
document_id: "a3cfe4417bfbd51b4771f32fbf030df69cc94c7575cdcc7c9dd74f2e022ba266"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/acumatica-salesforce-sales-order-sync"
published_at: "2026-07-20T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:38.087450+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:c2ad9cff40e66ca6fbf6905e1d0e21a1d6471964707519d56967c4ac8626cd0c"
---

# Acumatica to Salesforce Sales Order Sync: Mapping, Status, and Backfill

Syncing Acumatica sales orders into Salesforce is a common first integration: orders and fulfillment live in Acumatica, sales works in Salesforce, and reps need to see order status without opening the ERP. The concept is simple, but teams evaluating it ask the same concrete questions, because the details decide whether the sync is trustworthy.


Does the data map straight across or does it need transformation? How do Acumatica order statuses line up with Salesforce, since they are not the same values? Can a custom field like the Acumatica Note ID carry over? How do you keep Acumatica the one-way source of truth so no order gets overwritten? How do you backfill the orders that already exist? And how do you avoid the Salesforce API limits and per-task costs that catch teams on tools like Zapier? This guide answers each one.


The setup below assumes a managed sync platform such as Stacksync connecting[Acumatica](https://www.stacksync.com/connectors/acumatica) and[Salesforce](https://www.stacksync.com/connectors/salesforce) . For the broader Acumatica to CRM picture across both Salesforce and HubSpot, see the[Acumatica to Salesforce and HubSpot integration guide](https://www.stacksync.com/blog/acumatica-salesforce-hubspot-integration) . This post goes deep on the sales order sync itself.


## Field mapping: straight across, or transformed?


The most common question is whether Acumatica data needs transformation on the way into Salesforce or whether it is the same content flowing straight across. For a sales order, most fields are a direct copy. Shipping address, quantities, and line totals move field for field with no reshaping. Two things do need handling: matching records to the right target, and translating values that differ between the systems.


The key detail is the **match key** . Line items match on the Acumatica inventory ID, so each order line links to the correct Salesforce product instead of guessing by name. At the order and customer level, a stable identifier (the Acumatica customer ID or Note ID) is stored as a Salesforce external ID so the same record is never created twice.


Most fields copy across. Line items match on inventory ID, status is translated, and the Note ID becomes an external ID. Acumatica sales order Salesforce How it maps


Customer Account Matched on external ID, not created twice


Shipping address Shipping address Copied field for field


Line items Order products Matched on inventory ID


Quantity and price Quantity and price Copied as-is


Order status Status Translated to your values


Note ID (custom) External ID Carried for record linking


A typical Acumatica sales order to Salesforce field map.


## Status mapping: Acumatica and Salesforce are not 1:1


Order status is the one field that always needs translation. Acumatica and Salesforce use different status vocabularies, so they do not line up one to one. The fix is a mapping table: you decide which Acumatica status becomes which Salesforce value, and the sync applies it every time a status changes. The table below is an example, not a fixed rule, since your Salesforce status picklist is your own.


Acumatica order status Example Salesforce status


Open Draft


On Hold On Hold


Awaiting Approval In Review


Back Order Processing


Shipping Processing


Completed Activated


Cancelled Cancelled


Statuses do not match 1:1, so you define the mapping. Example values only.


The same idea covers custom fields. Many teams add a field during their Acumatica implementation, such as the **Acumatica Note ID** , to link records back to the source. That field maps into Salesforce like any other, usually to an external ID, so the connection between an Acumatica order and its Salesforce record is explicit and survives re-syncs. Getting this key in place up front is also what makes the historical backfill match cleanly instead of duplicating.


## Keeping Acumatica the one-way source of truth


For sales orders, most teams want Acumatica to stay the single source of truth. Orders are raised, approved, and fulfilled in the ERP, so the sync should push order data out to Salesforce and never let a Salesforce edit change the order. That is a one-way sync: Acumatica to Salesforce, with orders read-only on the Salesforce side.


Direction is set per object, not globally. You can keep sales orders strictly one-way while still running[accounts and contacts two-way](https://www.stacksync.com/two-way-sync) if sales needs to edit those. The point is that no rep action in Salesforce ever writes back to an Acumatica order, so finance and operations always work from one authoritative version.


Orders flow one way and stay read-only in Salesforce. Accounts and contacts can run two-way if you want.


This is also where a purpose-built sync beats a stack of scripts. Because direction and read-only rules are enforced by the platform, there is no path for an accidental Salesforce edit to reach Acumatica, and no custom guard code to maintain.


## Backfilling historical orders


Turning on real-time sync only handles orders from that moment forward. Teams that went live on Acumatica months ago also want the existing orders in Salesforce, often a thousand or more since go-live. That is a one-time historical backfill, and it runs on the same mapping as the live sync.


1. 01


Set the mapping and keys first


Lock in the field map, the status mapping, and the external ID (Note ID or customer ID) so historical records match instead of duplicating.


2. 02


Run the historical load


Read the existing Acumatica orders in one managed backfill, applying the same field and status mapping as the live sync.


3. 03


Match, do not duplicate


Records match on inventory ID and external ID, so an order that later updates lands on the same Salesforce record it was backfilled to.


4. 04


Switch to real time


Once the backfill reconciles, enable real-time sync so every new order and status change flows automatically from that point on.


Because the backfill and the ongoing sync share one mapping, every order from launch and the order created five minutes ago end up in exactly the same shape in Salesforce, with no separate import script to reconcile later.


## Staying inside Salesforce API limits, without per-task bills


Two costs surprise teams that build this on a generic automation tool: Salesforce API limits and per-task pricing. Because a tool like Zapier bills per task, an Acumatica trigger that fires on every order and line-item change burns tasks quickly. As order volume grows, monthly task usage can climb from a few thousand into five figures, and the plan jumps a tier with it, so the integration gets more expensive exactly as the business scales.


Each of those tasks is also a separate Salesforce API call, which is how high-volume syncs bump into Salesforce rate limits during busy periods. A sync built for this handles both problems the same way.


Concern Per-task automation Stacksync


Billing model Per task, every triggered call counts Not billed per record


Volume spikes Task usage and cost climb with orders Batched, so spikes are absorbed


Salesforce API limits Each task is a separate API call Grouped writes stay inside limits


Historical backfill Extra tasks, extra cost Part of the initial managed load


Why per-task automation gets expensive at order volume, and how a managed sync avoids it.


Stacksync uses[change data capture](https://www.stacksync.com/two-way-sync) to react only when data actually changes, and it groups those changes into batched Salesforce writes with retries and replay. That keeps the sync inside API limits during peak order periods and keeps cost flat as volume grows, instead of scaling your bill with every task.


## Bringing it together


An Acumatica to Salesforce sales order sync comes down to a handful of decisions: most fields copy straight across while line items match on inventory ID, order status is translated through a mapping you define, custom keys like the Note ID carry over as external IDs, orders stay one-way with Acumatica as the source of truth, history is backfilled on the same mapping, and the whole thing runs inside Salesforce API limits without per-task bills.


Get those right and reps see live order status in Salesforce while Acumatica stays authoritative. To map your own orders, statuses, and custom fields on real data,[see the Acumatica and Salesforce integration](https://www.stacksync.com/integrations/acumatica-and-salesforce) or[book a demo](https://www.stacksync.com/book-a-demo) .
