---
schema_version: "1.0.0"
document_id: "bd4be4d32ecd5a66fe4875a6649816e0ae7b03c4d67f850db4e9237e88f407b7"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/product-updates-july-2025/"
published_at: "2025-08-04T00:00:00+00:00"
first_seen_at: "2026-07-22T15:09:34.706489+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:d35bfb695bc2553f604b86469b1d205659981749a63bebe0549ab23e9e3c5591"
---

# Product Updates: July 2025

This month, we rolled out major updates to make cloud GTM faster, smarter, and more connected—including enhanced analytics, data enrichment, global search, and ACE Partner Central API capabilities.


We also upgraded HubSpot to match our Salesforce integration, delivering a seamless, automated experience across CRMs. These improvements cut manual work and unlock insights—so your team can focus on driving revenue, not chasing tasks.


## Analytics Enhancements


You asked, we delivered: deeper insights and easier data access—providing teams clearer visibility into pipeline, revenue, and partner impact.


We’ve added three new[Analytics](https://doc.suger.io/analytics/) dashboards—Finance, Private Offers, and Co-Sell—with disbursements, pending and overdue disbursements, and more coming soon.


We’ve also unlocked full access to your analytics with powerful \[Data Exports\](/resources/blog/data-export-hub/, so you can:


- Export billing events, usage data, or co-sell records on demand
- Automate recurring exports for scheduled reporting
- Select flexible time ranges—from the last 30 days to your entire dataset
- Download in CSV, JSON, or PARQUET formats
- Send data to[Snowflake](https://doc.suger.io/integrations/snowflake) ,[BigQuery](https://doc.suger.io/integrations/bigquery) ,[Google Storage](https://doc.suger.io/integrations/google-cloud-storage) , or[Databricks](https://doc.suger.io/integrations/databricks)


## Outbound Referral Auto-Enrichment


Co-selling just got easier. Suger now[Auto-Enriches](https://doc.suger.io/cosell/cosell-outbound/#auto-enrich-referrals) required outbound referral fields—such as company name, website, address, industry, employee count, and contact phone—to streamline co-sell sharing across all marketplaces and CRMs.


This ensures referral records are always complete and compliant, enabling smoother auto-sharing, reducing manual effort, and keeping your co-sell motion fast and efficient.


## Company Page With Enriched Records


We’ve renamed the Buyer Page to “Company” to better reflect its expanded scope—now supporting buyers, partners, and other company record types.


The updated Company Detail Page includes enriched data (e.g. website, LinkedIn, location) and key metrics like entitlements, offers, and CPPOs for more actionable insights.


## Global Search


Global search is now live across the Suger Console, Salesforce app, and HubSpot app—giving you a faster, smarter way to find what you need, wherever you’re working.


You can now instantly search Offers, Products, Entitlements, Companies, Referrals, and Contacts. Soon, you’ll be able to search Hubspot deals, Salesforce opportunities, and additional third-party integrations like Stripe. Stay tuned!


## Metronome Integration Improvements


We’ve made key improvements to our[Metronome integration](https://doc.suger.io/integrations/metronome/) to ensure accurate entitlement handling and revenue reporting for AWS Marketplace.


**What’s New:**


- Support for voided and regenerated invoices
- Deduplication of usage data to prevent double-reporting
- Filters to exclude invoices with an external status of ‘paid’
- Automatic credits for charges already reported when an invoice is later voided


## More Updates


### Console


- Link Offers with Ace Referrals
- Link ACE Referrals with Salesforce CRM Opportunities
- Auto-sync buyer name after a private offer is created


### Salesforce


- ACE Partner Central API: Referral editor and actions, sortable opportunities in new ACE table


### Hubspot


- Create GCP SaaS offers directly in HubSpot
- Create GCP replacement offers from HubSpot
- Notify contacts for CPPO_Out and ABO workflows
- Add “Copy Offer URL” button in HubSpot
- Manually send ACE data to HubSpot from the Suger Console
- Inbound Azure pipeline now connects Suger to HubSpot
- New Resale-CPPO_Out flow for ISVs to authorize resellers
- Support flexible billing in Azure private offers
- Extend expiry dates or cancel private offers from HubSpot
- Support multiple EULA files on amendment offers
- Tooltip for commits and month-based contract length enhancements
- Auto-generate AWS POs with monthly installments in HubSpot
- Improved contact matching during PO creation
- Outbound Co-Sell Settings for ACE Partner Central API: Auto-Share Referrals, Auto-Enrich Referrals, Auto-Delete Referrals
