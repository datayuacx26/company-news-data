---
schema_version: "1.0.0"
document_id: "938e2bb82396ad9e8f83c192f21a8577da53d121ef152af0eb4e50aae9181dbb"
company_key: "yc-coris"
company: "Coris"
source_id: "yc-coris-news-import-dd89c9ff7287"
canonical_url: "https://www.coris.ai/blogs/launching-salesforce-integration"
published_at: null
first_seen_at: "2026-07-25T00:43:25.568731+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:04d3179818131ed4a9b5a942bc6c5f679b8c8681a03f1f80d6520405b0c25e01"
---

# Launching Salesforce Integration

Salesforce is by far the most popular CRM and where many commercial teams already operate. It is where deals are tracked, account history lives, and customer communication is managed by a lot of the teams. However, risk and underwriting teams don’t always directly use Salesforce but still have to access it for information or keep it updated from a data hygiene standpoint.


Until now, bringing that information into risk platforms such as Coris required copy and paste work, shared links, or duplicate entry. This slowed underwriters and risk analysts and increased manual effort as merchant volume grew.


Our new integration changes that.


Coris can now pull new merchant and opportunity records directly from Salesforce, which means risk workflows begin with the context that already exists in their CRM. Underwriters can see sales notes, expected volume, use case information, and history without switching tools.


Also, as decisions are made in Coris, updates can flow back into Salesforce automatically. Status, data fields, and notes stay aligned without anyone typing the same information twice.


The result is faster onboarding, cleaner communication between teams, and a shared view of the merchant from first conversation to approval.


‍


## **What we are launching**


We have built a native integration between Coris and[Salesforce](https://www.salesforce.com/ap/form/sem/salesforce-crm/?d=701ed00000agEdSAAU&nc=701ed00000ar2XLAAY&utm_content=701ed00000agEdSAAU&gclsrc=aw.ds&gad_source=1&gad_campaignid=22817000634&gbraid=0AAAABAVuWUlzaSczzxw1Cqg0ul6WJYPPO&gclid=Cj0KCQiAubrJBhCbARIsAHIdxD9XI_jn8zQDTczpvmD7h03fWxpLfhjFa7R9U9F03Mj_OsdV4-0H0GgaAmRqEALw_wcB) so information can move in both directions without engineering work. With this integration, Salesforce admins at our customers can connect their Salesforce instances to Coris through a simple OAuth and configure the mapping between Coris and Salesforce.


Once connected, Coris can pull context from Salesforce and push risk outcomes back, all in real time.The integration is designed for two groups:


- teams that manage deals and customers in Salesforce
- risk and underwriting teams that run their workflows inside Coris


## **How the integration works**


### **No code setup**


Customers do not need to involve their engineering teams. A Salesforce admin can:


1. Create and approve the Coris app in Salesforce
2. Authorize the connection using OAuth
3. Map relevant objects and fields between Salesforce and Coris


After that, Coris can read and write data within the scope of the integration.


‍


### **Pull Salesforce data into Coris**


Once connected, Coris can use Salesforce as a source of merchant and opportunity data.


Example uses:


- **New opportunities** When a new deal or opportunity is created in Salesforce, Coris can automatically create a corresponding merchant account and have our AI agent underwrite based on the set rules and standard operating procedures (SOP). This removes the need to re-enter basic details into Coris.


- **Context for underwriting** Coris can pull account information, opportunity details, and related fields from Salesforce. This can include things like expected volume, pricing, industry, or sales notes that describe how the customer plans to use the platform. In turn, these can be factored in the underwriting and ongoing monitoring of these accounts.


- **Notes and history** Existing notes on the account in Salesforce can be imported into Coris so risk reviewers have access to previous conversations and commitments without switching tools.


This gives risk teams a richer view of the merchant from the moment an opportunity is created.


‍


### **Push Coris decisions back into Salesforce**


The integration also works in the other direction. Coris can write data back to Salesforce so sales and operations stay current without manual updates.


Customers can:


- map fields in Coris to specific fields in Salesforce
- choose which events in Coris should trigger updates


Example uses:


- **Underwriting status** When a merchant is approved by our AI agent or manually in Coris, the corresponding fields in Salesforce can be updated automatically.


- **Risk fields and notes** Custom fields in Coris, such as risk scores, flags, or payout settings, can be automatically written into Salesforce. Notes recorded during reviews can also be synced so account teams see why a decision was made.


- **Operational changes** As data changes in Coris, Salesforce can stay aligned without anyone retyping the same information.


This removes duplicate work and keeps everyone looking at the same version of the truth.


‍


## **Example workflows**


### **1. Deal created in Salesforce, underwriting begins in Coris**


A new opportunity is entered in Salesforce. Coris’ AI agent pulls the record, creates a merchant profile, enriches it, and routes it into underwriting. Status updates sync back automatically so sales can track progress without leaving Salesforce.


**Result** : Faster onboarding and fewer manual handoffs.


### **2. Risk events and payout holds**


When a risk rule fires in Coris, the event can be surfaced in Salesforce for account visibility. Case automation for Salesforce is coming next, enabling real time merchant notification and case creation without manual tickets.


**Result** : Shared awareness during critical events and less work stitching systems together.


### **3. Chargebacks and disputes**


Disputes surfaced through processors like Stripe or Adyen appear inside Coris. Salesforce can receive updates so operators do not maintain those timelines in two places.


**Result** : Fewer missed follow ups and consistent communication across teams.


‍


## **Why this matters**


The Salesforce integration is designed to remove friction in how commercial teams and risk teams work together.


With both systems in sync:


- New merchant data flows into Coris automatically


- Underwriting begins with more context


- Teams do not need to retype information across tools


- Status stays current in Salesforce without reminders


- Operations can scale without adding manual updates


Coris remains the core system for risk evaluation, but Salesforce is now connected to it in real time.


‍


## **What is coming next**


Phase one supports bidirectional field sync, opportunity to merchant creation, and underwriting visibility across systems.


Next we are working on:


- Automated case creation inside Salesforce triggered by Coris events


- Merchant notifications for events such as payout holds and disputes


- More granular configuration options for objects and field behavior


As these ships, teams will automate more of the operational lift around onboarding, event handling, and communication.


The outcome is simple. Less time moving information. More time making decisions.
