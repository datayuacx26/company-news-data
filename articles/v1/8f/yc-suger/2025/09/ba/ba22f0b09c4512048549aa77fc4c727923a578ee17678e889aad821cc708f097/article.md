---
schema_version: "1.0.0"
document_id: "ba22f0b09c4512048549aa77fc4c727923a578ee17678e889aad821cc708f097"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/product-updates-august-2025/"
published_at: "2025-09-02T00:00:00+00:00"
first_seen_at: "2026-07-22T15:09:34.706489+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:5a6b5b11707f5777c464bac6eae2a963f530db8b08658f0b4f9a65424d10c394"
---

# Product Updates: August 2025

This month’s release strengthens financial visibility, improves partner collaboration and attribution, empowers resellers, and deepens CRM integration across marketplaces.


Highlights include a new Billing analytics dashboard, expanded revenue and marketplace tables, and the ability for resellers to create CPPOs directly in Salesforce. We also launched Referral-to-Opportunity and Entitlement-to-Opportunity linking, enabling greater accuracy between marketplace activity and CRM records.


## **Billing Dashboard & Revenue Columns**


We continue to give customers deeper insights and easier data access out-of-the-box.


**New Billing**[Analytics](https://doc.suger.io/analytics/) **dashboard** : Provides clearer visibility into disbursements, pending payments, and overdue items—with more dashboards coming soon!


**New Revenue table columns** : Added five new fields—Refund Invoice Date, Refund Invoice Amount, Refund Disburse Date, Refund Disburse Amount, and Bank Trace ID. The bank-assigned trace ID helps correlate deposit notifications and reports with invoices in AWS Marketplace reporting.


## **Salesforce Table & Reporting Upgrades**


We expanded Salesforce tables to make data more accessible and reporting more flexible:


- Support column visibility and reordering
- Opportunity ID column added to AWS, Azure, and GCP tables
- Sales Rep column added to the AWS table


With the **Opportunity ID column** , you can connect marketplace activity directly to your CRM pipeline, making it easier to see which deals each referral, offer, or entitlement rolls up to. This improves reporting accuracy and streamlines attribution so finance, sales, and operations are working from the same source of truth.


With the **Sales Rep column** , you can reference AWS reps you’ve worked with on prior deals when reaching out to contacts—for example: *“I saw you’re connected with Ben—we worked with him on deals like Glean and Intel.”* This column also improves AWS rep attribution in reporting.


## **Reseller CPPOs in Salesforce**


Resellers can now create and manage AWS Channel Partner Private Offers (CPPOs) directly from resale authorizations in Salesforce. This removes the need to switch between portals, keeping the entire process inside existing opportunity records and workflows. The result: faster, simpler, and more scalable reseller-led marketplace transactions.


## **AWS Referral-to-Opportunity Linking**


Connecting inbound AWS referrals to your sales pipeline is now seamless. Once a referral is accepted, you can link it to an existing Salesforce account or opportunity. You can also create a new one directly from the referral detail page and automatically link it to the referral. This ensures referrals flow smoothly into your pipeline, reduces manual work, and keeps CRM records accurate and aligned.


## **Entitlement-to-Opportunity Linking**


You can now link individual entitlements directly to Salesforce Opportunities or HubSpot Deals from the Suger Console, with full visibility in your CRM.


For customers managing multiple entitlements from a single public offer, this provides granular control and clarity. Every entitlement can be tied to the right deal, ensuring clean records, accurate forecasts, and a stronger connection between marketplace activity and your sales pipeline.


## **More Updates**


### **Console**


- New login page design
- Support 8-decimal precision for AWS usage pricing
- Show calculated total when creating Azure private offers with flexible billing
- Refactored Settings page (part 1)
- Allow setting AWS private offer expiration to today’s date
- Support syncing multiple entitlements from AWS
- Choose single or multiple dimensions when creating SaaS contract pricing


### **Salesforce**


- Create Azure professional service offers
- Updated referral types to distinguish inbound vs. outbound (AWS, Azure, GCP)
- Updated Azure solution area & play picklists for outbound referrals
- Added Azure flexible total amount label


### **Hubspot**


- New entitlements tab in HubSpot panel
- Edit entitlements directly
- Clone offers
- Link CRM Opportunity ID when editing offer info
