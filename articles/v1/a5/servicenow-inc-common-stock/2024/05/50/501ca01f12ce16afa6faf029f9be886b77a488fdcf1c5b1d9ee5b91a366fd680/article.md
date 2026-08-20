---
schema_version: "1.0.0"
document_id: "501ca01f12ce16afa6faf029f9be886b77a488fdcf1c5b1d9ee5b91a366fd680"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/service-bridge-remote-record-producers-using-rrp-variables-in/ba-p/2941470"
published_at: "2024-05-23T15:51:09+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:4532aee108328c00b8e544c3b150083366bce1ad6c1b4c6972f2a3041fc4f6ec"
---

# Service Bridge - Remote Record Producers - Using RRP variables in your flows

One of the powerful outcomes of using Remote Record Producers (RRP) with Service Bridge, is that once a consumer submits an RRP, you will collect structured data that can be used to automate your workflows on your Provider instance. When configuring a Remote Record Producer, a subflow is linked to drive the fulfillment. It is quick and easy to extract the variables and pass them along to other tasks and drive automation. Here is an example on this is done:


1. I started by making a copy of the OOTB Service Bridge subflow "Create Incident from Provider Task".


2. Next we will add an action before the existing "Create Incident Record" action. We will select the "Get Catalog Variables" action that is under the ServiceNow Core spoke.


3. In this action, we need to fill in the following fields:


- **Submitted Request** - This will be the "Provider Task" record that is provided in the Subflow Inputs
- **Template Catalog Items and Variable Sets** - From the picker, we will find the name of the RRP that this subflow will be used for.
- **Catalog Variables** - Once you have filled in the above field, you will now see the variables that you have defined in your RRP show in the "Available" section on the left. Select the variables that you will use later in the subflow and click the right arrow to move them into the "Selected" section.


4. Now those variables will show up as pills that can be used in later steps in the Subflow.


5. As an example, I have updated the OOTB Create Incident Record action to use variables that I have collected:


- Short Description is populated by the "title" variable
- Description is populated by the "summary" variable
- Priority is populated by to the "priority" variable
- Category is populated by the "category" variable
- Subcategory uses a script to determine which of the variable to populate, based on the value of the "category" variable


6. Now when my incident is created, I am automatically setting the short description, description, priority, category, and sub-category.


Using the Get Catalog Variables action, it is easy to use the variables from your RRP in your flow.
