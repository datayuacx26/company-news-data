---
schema_version: "1.0.0"
document_id: "c562614293de78ecac83e9d97e5f2edcf95d76174be0bd12ba1fe43465a15720"
company_key: "yc-rutter"
company: "Rutter"
source_id: "yc-rutter-news-import-c12c34cd87f8"
canonical_url: "https://www.rutter.com/blog/rutter-news-august"
published_at: "2025-09-12T18:26:40.255+00:00"
first_seen_at: "2026-07-22T12:26:40.375919+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:e4bfd9aaa148a28003b1e0594f7a27583dd904db20bca55e3c5cf617bd023abc"
---

# Rutter News: August

#### Product Update


## Breaking through integration complexity


##### 🧩 **Platform-specific flexibility meets unified simplicity**


This month we shipped field-level passthrough, fundamentally changing how developers can leverage platform-specific functionality while maintaining the power of our unified API.


Combined with our new pre-built UI for bank feeds and new endpoints for transaction reporting, August represents a big leap forward in how Rutter reduces integration complexity for fintechs everywhere. As always, head to our[changelog](https://docs.rutter.com/changelog?utm_source=hs_email&utm_medium=email&_hsenc=p2ANqtz--Ek-JQHhAfwI3BiXREghqIrS327qpDYpCSuiyLCIN3aF4uJBMNwBEhJFzZ87WpheRS1wnq) for the full technical details, or read our executive summary below.


#### Featured release


## Field-Level Passthrough


##### 🔁 **Introducing Field-Level Passthrough**


We're excited to announce field-level passthrough, a breakthrough feature that allows you to inject platform-specific fields directly into Rutter API requests. With field-level passthrough, you can access advanced features and custom fields based on your customers' specific needs, with minimal custom development.


#### Bank Feeds


## Account Mapping UI


##### 📦 **Pre-Built UI Components for Xero and NetSuite**


Rutter now offers hosted UI components to assist your Xero and NetSuite bank feed customers in mapping their bank or card accounts to GL accounts. Our hosted UI enables end users to select bank feed accounts for syncing, map them to general ledger accounts, create new GL accounts, configure date ranges, and manage linked accounts, all without custom development on your end.


#### What's new


## API Enhancements


##### 🧾 **Enhanced Transaction Fetching**


We've added powerful new capabilities to our Accounting Transaction endpoints. Now, Rutter supports tying these high-level transactions back to detailed Rutter entities, allowing you to better track and reconcile transactions across your customers’ integrated systems.


##### 🪝 **Expanded Webhook Support**


We’ve introduced **BACKFILL_COMPLETED** and **INCREMENTAL_SYNC_COMPLETED** webhooks across all platforms, giving you real-time visibility into data synchronization processes and enabling more responsive user experiences.


#### Developer UX


## Platform-Specific Enhancements


##### 📒 **QuickBooks Desktop Expansions**


We’re continuing to improve our QBD product, adding a new **DELETE /invoices** endpoint and enhancing invoice creation with support for setting internal memos. We also introduced the **GET /currencies** endpoint for better multi-currency workflow management.


##### 🌐 **Cross-Platform Improvements**


Enhanced expense management with support for memos on QuickBooks **PATCH /expenses** , tax-inclusive billing options on **POST /bills** , and expanded Zoho Books invoice functionality with memo field support.


##### ➕ **FreeAgent Integration**


Extended our platform coverage with a new **GET /credentials** endpoint for FreeAgent, allowing the use of passthrough for this platform.


##### ⚙️ **Enhanced Platform URLs**


Added **platform_url** support across QuickBooks transactions and NetSuite departments, providing better visibility and direct linking to source platform records.


Ready to get started with Rutter? ****[Get a demo](https://www.rutter.com/get-a-demo)
