---
schema_version: "1.0.0"
document_id: "c39af37d2fe3963d912b91de682b0e48d2166998ca959f8d535dc6daa736fb4e"
company_key: "yc-nash"
company: "Nash"
source_id: "yc-nash-news-import-bf9592126cf4"
canonical_url: "https://www.nash.ai/release-notes/multi-quote-visibility"
published_at: "2025-06-09T00:00:00+00:00"
first_seen_at: "2026-07-24T12:02:47.144757+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:295b3a5760310ae4901adf6d58ba07bd29e8b9c068aeab824a4cf5381d6e0213"
---

# Enhanced Quote Visibility

Release notes


# Enhanced Quote Visibility


June 8, 2025


Nash now displays multiple quote options per provider simultaneously when requesting delivery providers:


- **Multiple quotes per provider** - See different service levels and pricing options
- **Individual quotes from each driver group** - Compare rates and availability across your internal fleet
- **Real-time availability** - View which options are available right now
- **Side-by-side comparison** - All information in one view for confident decisions


‍


##### How It Works


1. Click "Get Quotes" or "Assign Provider" on any delivery
2. View all available options with pricing and availability
3. Select your preferred option
4. Nash handles the rest


##### Getting Started


This feature is being rolled out progressively. Contact your Nash representative to enable multi-quote visibility for your organization.


‍


‍


#### Under the Hood: Logistics Enhancements


🚚 **Fleet**


+


- **Shift template CSV upload** - Fleet managers can bulk upload shift templates via CSV, streamlining the setup process for driver schedules across multiple locations. This significantly reduces the time needed to configure recurring shift patterns for large fleets.
- **Route balancing strategies** - Organizations can choose their preferred route optimization strategy, allowing them to either minimize the number of drivers needed or balance deliveries evenly across available drivers. This gives dispatchers more control over how routes are distributed based on their operational needs.


⚙️ **Dispatch**


+


- **Enhanced provider assignment for orders** - Dispatchers can assign providers and drivers directly from the orders view with an improved workflow. The new interface allows users to either "Assign & Save" to pre-assign drivers or "Assign & Dispatch" for immediate dispatch, providing greater flexibility in managing delivery assignments.
- **Multiple quotes visibility** - The platform displays multiple quote options from providers and internal fleet driver groups, enabling dispatchers to compare and select the best option for each delivery. This transparency helps optimize dispatch decisions based on cost and availability.
- **Flexible route optimization timing** - Routes can now be optimized even when the start time is in the past, as long as the end time is still in the future. This provides more flexibility for mid-day route adjustments and re-optimization.


🕹️ **Control**


+


- **Archive orders guidance** - A new label has been added to the Archive Orders view, helping users understand the purpose and functionality of archived deliveries at a glance.
- **Streamlined refund request management** - Providers can more efficiently process refund decisions with dedicated fields for refunded amounts and response explanations. The refund request export and import process has been significantly enhanced with reorganized columns, clearer naming conventions, and improved field requirements.
- **Organization preferences** - Organizations can configure their preferred units and formats including weight (kg vs lbs), distance (km vs miles), date/time formats, and language settings. This localization capability makes the platform more accessible for international users.


🎯 **Promise**


+


- **Enhanced Shopify checkout customization** - Merchants can better control their checkout experience with new capabilities to hide local pickup rates and properly filter delivery methods based on customer selections. This provides a cleaner, more tailored checkout flow for end customers.
- **Shopify risk flag synchronization** - Risk flags and tags from Shopify orders are automatically synced to Nash, helping merchants better identify and manage potentially fraudulent or high-risk deliveries.
- **Multi-zone delivery window export** - Organizations can select and export delivery windows from multiple zones simultaneously, eliminating the need to export each zone individually.


🖥️ **Platform**


+


- **Australia Post ETA integration** - Deliveries handled through Australia Post include estimated time of arrival calculations, providing better visibility and tracking information for both merchants and customers.
- **Enhanced API field mapping** - The job payload API includes additional fields such as \`pickupStoreLocationExternalId\`, improving integration capabilities and data completeness for API users.
- **Multi-organization SSO support** - Single sign-on (SSO) supports multi-organization setups, allowing users to access multiple organizations with one login, enhancing security and user experience for enterprise customers.
- **Extended Menulog order timeout** - The cancellation timeout for Menulog orders has been increased from 10 to 30 minutes, giving more time to find available providers before orders are automatically cancelled.
- **Activity logs for delivery windows and shifts** - Track all changes made to delivery windows and shifts with new comprehensive activity logs. View who made changes, when they were made, and what was modified, providing complete audit trail visibility for operational accountability.
- **Enhanced CSV upload capabilities** - CSV order uploads now support additional fields including pickup store location IDs and geographic coordinates (latitude/longitude). The system also better handles various timestamp formats including milliseconds, reducing upload errors.


‍


‍


[← Back to release notes](https://www.nash.ai/release-notes)


See it run


### Built for the reality of logistics.


15 minutes. Real ops, your data.


[Get a demo](https://www.nash.ai/demo)
