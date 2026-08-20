---
schema_version: "1.0.0"
document_id: "6fccf1c9ecf15f70536136bf8eb59780f8d01fcae62257595696f422d2efac7f"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/how-to-sync-campfire-with-salesforce"
published_at: "2026-07-21T11:10:00+00:00"
first_seen_at: "2026-07-22T16:41:29.064477+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:e79cbab28daad0e18c57f7984d4f8c9fe4ae1375c4ed3be1af54170624052286"
---

# From Closed-Won to Posted Invoice: Wiring Salesforce Into Campfire

Salesforce knows who agreed to buy. Campfire knows what they were actually billed, what they paid, and what the revenue schedule looks like. Between those two facts sits the handoff that most finance teams still do by hand: someone reads a closed-won opportunity, retypes the customer and the line items into the ledger, and then answers the rep in Slack a week later when they ask whether the invoice went out.


That handoff is a sync problem, not a process problem. This guide walks through wiring Salesforce into Campfire so the deal becomes an invoice on its own, and so the invoice, the balance, and the payment status come back to the rep without anyone opening the ledger. It covers the object mapping, the keys that stop duplicate customers, and how changes are detected on each side.


Three moves: connect, map, sync. The detail that decides whether it works long term is in the second one, so most of this guide sits there. If you are choosing a platform to run it on, start with our guide to[what a Campfire integration platform has to get right](https://www.stacksync.com/blog/enterprise-ipaas-campfire-erp) .


## Step 1: connect both sides


Salesforce connects over OAuth, so the sync acts as an integration user with the permissions you grant it and nothing more. Campfire connects with an API key you mint yourself: create a dedicated API user under Settings, give it a role, and copy the key once. The key is sent as an` Authorization: Token` header on every request.


Use a separate API user for the integration rather than a person's credentials. It means the audit log shows clearly which writes came from the sync, and it means you can rotate or revoke that one key without locking a controller out of their own ledger. If you only need a read-only feed into a warehouse, the view only role is enough; a quote-to-cash sync that creates invoices needs write access.


## Step 2: map the objects, and pick a key


This is where the sync is won or lost. Salesforce and Campfire model the same customer differently, so decide up front which system owns which field, then match records on a stable id rather than a name.


Salesforce Campfire Direction


Account Customer Two-way, Salesforce owns the name and address


Contact Billing contact on the customer Two-way


Opportunity, closed-won Invoice with line items Salesforce to Campfire


Order and order products Invoice lines Salesforce to Campfire


Invoice number, balance, status Invoice and payments Campfire to Salesforce, read-only for reps


The mapping most teams start with. Ownership per field matters more than the object list.


For the key, write the Campfire customer id onto the Salesforce account and the Salesforce account id onto the Campfire customer. Every later change then updates the existing pair. Teams that skip this and match on company name end up with two customers for the same buyer within a month, usually because someone typed a trailing Inc. Campfire also supports custom fields and custom dimensions on its objects, which is the natural place to park that external id.


The quote-to-cash path, from a closed-won opportunity to a posted invoice and back.


## Step 3: sync both ways, and watch the round-trip


With the mapping in place, the engine watches both sides. On the Salesforce side it picks up the change as soon as the record is saved. On the Campfire side it subscribes to webhooks, with` last_modified_at` filtering on the list endpoints as the backfill path and the safety net. Nothing here runs on a nightly timer.


One round-trip: closed-won to invoice, then invoice and payment status back to the account.


The step people forget is the return leg. Once Campfire posts the invoice and generates its journal entry, the invoice number, balance, due date, and payment status belong back on the Salesforce account. That is what stops the Slack question. It is also what lets customer success see that an account is 60 days past due before they renew it, without giving every rep a seat in the ledger.


Origin tracking is what keeps the round-trip from becoming a loop. When the engine writes the invoice number into Salesforce, it tags that write as its own, so the resulting Salesforce update is not read back as a fresh change and pushed into Campfire again. Without it, two systems that both watch for changes will happily ping the same record back and forth forever.
