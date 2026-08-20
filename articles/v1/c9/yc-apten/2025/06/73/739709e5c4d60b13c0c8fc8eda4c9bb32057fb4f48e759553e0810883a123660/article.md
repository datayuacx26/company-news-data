---
schema_version: "1.0.0"
document_id: "739709e5c4d60b13c0c8fc8eda4c9bb32057fb4f48e759553e0810883a123660"
company_key: "yc-apten"
company: "Apten"
source_id: "yc-apten-news-import-6f51d8d43adf"
canonical_url: "https://www.apten.ai/blog/how-to-connect-an-ai-sms-bot-to-salesforce"
published_at: "2025-06-07T00:00:00+00:00"
first_seen_at: "2026-07-21T07:22:24.981357+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:cdc45f31c017ebbf5cf6f4cc1bd4716321064050d1cc2debe697b8914a6fed40"
---

# How to Connect an AI SMS Bot to Salesforce

If you're using Salesforce to manage leads, you already know how important it is to follow up fast, stay organized, and personalize your outreach. But let’s face it--manual follow-up and inconsistent messaging lose deals.


That’s where **Apten** , our AI-powered SMS and voice agent, comes in. With our Salesforce integration, you can sync lead data, automate follow-ups, and make your CRM work *for* you.


In this guide, we’ll show you exactly how to integrate Apten with Salesforce in under 10 minutes--and start automating your sales conversations instantly.


### Why Integrate Salesforce with an AI SMS Bot?


Here’s what you unlock by syncing Salesforce with Apten:


-


**Real-time SMS sync** : All lead conversations are pushed to Salesforce.


-


**Automatic lead tagging** : Tags like “Hot Lead” or “No Response” get synced directly.


-


**Conversation summaries** : Apten’s AI generates a concise summary of every conversation and stores it in Salesforce.


-


**Custom variable mapping** : Dynamic info like appointment times, budgets, or interests sync automatically to custom fields.


-


**Streamlined workflows** : No more switching between tools or guessing when to follow up.


This integration bridges the gap between CRM and real-time communication.


### Step-by-Step: Connecting Apten to Salesforce


Here’s how to go from setup to synced in minutes.


#### Step 1: Create an Integration User in Salesforce


Set up a dedicated user with API access. This keeps things secure and isolated.


1.


Go to` Setup > Users > New User`


2.


Fill in the basics (First Name: Apten, Last Name: Integration, etc.)


3.


Use **Salesforce Integration** as the license and **API Only System Integrations** as the profile


4.


Save


#### Step 2: Create a Permission Set for Apten


1.


` Setup > Permission Sets > New`


2.


Label it` Apten Integration Permissions`


3.


Enable:


-


API Access


-


Modify All Data


-


Customize Application


-


Read/Create/Edit for the objects you’re syncing (Leads, Contacts, etc.)


4.


Assign the permission set to the Apten Integration user


#### Step 3: Connect from Apten


1.


In Apten, go to **Integrations**


2.


Click **Connect** on the Salesforce card


3.


Log in with your integration user


4.


Choose which object to sync (Lead, Contact, etc.) – this can’t be changed later


5.


Optionally, enable **Tags** and **Summaries** to auto-create custom fields like` Apten_Tags__c` and` Apten_Summary__c` in Salesforce


> ✅ *Important: Be sure to add those new custom fields to the permission set, or syncing will fail.*


#### Step 4: Map Custom Variables (Optional)


Want to sync fields like “Appointment Date” or “Property Type”? You can map variables to Salesforce fields via the **Build & Test** tab in Apten.


Whenever your AI agent gathers this data, it will sync in real-time.


#### Step 5: Import Leads with Salesforce IDs


When importing leads into Apten, make sure to include the` integrationSource` and` salesforceId` :


This ensures every conversation, tag, and summary stays tied to the correct Salesforce record.


### From Static CRM to Dynamic AI Conversations


With this integration, Apten doesn’t just plug into Salesforce--it brings your CRM to life. Your team can see exactly what’s happening with every lead, in real-time, without lifting a finger.


Ready to automate your Salesforce outreach with AI?


👉[Connect Salesforce with Apten now](https://apten.ai/)


### 🛠️ Need a More Advanced Setup?


Whether you're working with custom Salesforce objects, multi-step workflows, or enterprise-scale data mapping, we've got you covered.


**Need a more complex integration?**
We offer **dedicated, done-for-you integration support** from our solutions team. From custom field mappings to API-level syncs, we’ll tailor everything to your exact setup--so you don’t have to lift a finger.


> ✅ *Enterprise-ready. Fully supported.*


Contact our team →
