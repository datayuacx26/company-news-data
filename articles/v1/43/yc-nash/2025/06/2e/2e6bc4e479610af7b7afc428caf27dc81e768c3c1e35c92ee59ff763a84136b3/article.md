---
schema_version: "1.0.0"
document_id: "2e6bc4e479610af7b7afc428caf27dc81e768c3c1e35c92ee59ff763a84136b3"
company_key: "yc-nash"
company: "Nash"
source_id: "yc-nash-news-import-bf9592126cf4"
canonical_url: "https://www.nash.ai/release-notes/weekly-dispatch-june"
published_at: "2025-06-18T00:00:00+00:00"
first_seen_at: "2026-07-24T12:02:47.144757+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:954a150ed4e463f84c7b2360df3003d2065569676c22c0dfca584532aadd339c"
---

# Bulk select all orders across pages

Release notes


# Bulk select all orders across pages


June 17, 2025


Select **all orders** across your entire order list with a single click, not just those visible on the current page. This powerful feature enables bulk actions (optimize, dispatch, archive) on hundreds or thousands of orders at once, eliminating the need to process page by page.


After initiating a bulk action, you'll receive a status confirmation showing the progress and completion of your request. This enhancement is essential for high-volume operations that need to efficiently manage large order batches.


‍


#### Under the Hood: Logistics Enhancements


🕹️ **Control**


+


- **Assign providers and drivers directly from orders** - Quickly assign providers or drivers to orders directly from the orders and routes pages without navigating to separate screens. View and select from available quotes with the option to auto-dispatch, streamlining your dispatch workflow.
- **Cancel order action** - A new cancel order action has been added for **Shopify Merchants** to the interface, providing a streamlined way to cancel orders when needed.


🚚 **Fleet**


+


- **Enhanced shift CSV uploads** - Shift CSV uploads now support the` PICK_SHIFT_NAME` field, enabling more detailed shift configuration and better alignment with delivery window assignments during optimization.
- **Driver-level weight and dimension configuration** - Configure weight and dimension parameters at the individual driver level rather than splitting by contract. This provides greater flexibility in optimization by allowing driver-specific capacity settings that override contract defaults.


⚙️ **Dispatch**


+


- **External store location references in CSV uploads** - Reference stored pickup locations using external IDs when uploading orders via CSV. This enhancement aligns CSV functionality with API behavior, making bulk uploads more efficient by leveraging pre-configured location data.


🖥️ **Platform**


+


- **Japanese Yen (JPY) currency support** - Nash now supports Japanese Yen (JPY) as a currency option throughout the platform for contracts and orders, enabling seamless operations for Japanese market expansion.
- **Third-party quotes in eligibleDeliveryWindows API** - The eligibleDeliveryWindows API response now includes quotes from third-party providers in addition to internal providers, giving API users access to a complete view of available delivery options.
- **Update package items via API** - The job-update API now supports updating package items after job creation, providing greater flexibility for managing delivery contents throughout the fulfillment process.
- **Provider organization visibility** - When your organization type is set as provider, you can now see which client organization each order comes from, along with the customer fee being paid. This transparency helps providers better understand their revenue and client relationships.


🛒 **Promise**


+


- **Dynamic pricing for tiered rate cards** - Shopify Merchants can enable dynamic pricing within tiered pricing structures. When defining tiers, you can now choose between manual fixed pricing or dynamic pricing that adjusts based on real-time factors, providing more flexible pricing strategies.


[← Back to release notes](https://www.nash.ai/release-notes)


See it run


### Built for the reality of logistics.


15 minutes. Real ops, your data.


[Get a demo](https://www.nash.ai/demo)
