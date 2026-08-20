---
schema_version: "1.0.0"
document_id: "edc5e0bf4f618e7bfb9e16026c4983ef88c9b35082439933d9d2185c82607162"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/accessing-and-using-service-bridge-remote-task-variables-in-an/ba-p/2886966"
published_at: "2024-07-18T20:45:00+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:6cd4577d11caa5dde1a8a90211d5f1496d545263124b992afa3aee34298b6495"
---

# Accessing and using Service Bridge Remote Task variables in an Advanced Transform script

The Service Bridge **Remote Task Definition** allows an admin to specify what fields are to be passed back and forth between a connected Provider and Consumer using a **Remote Task** . **Transforms** can be specified that will alter a field value and/or label as it is passed from (outbound transform) and/or to (inbound transform) the Provider or Consumer instances. An **Advanced Transform** allows the admin to include a script that will transform the data between the instances as required.


Service Bridge provides an **object_data.remote_task_vars** object that enables the reference and use in a transform script of any of the fields that have been included in the Remote Task Definition. The purpose of this article is to describe how to reference Remote Task variables from a transform script.


I will use a simple example to demonstrate how this can be done. My Remote Task Definition will trigger a Remote Task on the Consumer instance when an incident is assigned to the “Network Support” group, and the task will flow to the Provider instance and create a corresponding case. In the Remote Task Definition, I have specified that the inbound **Description** field will synchronize between the incident and the case on **Insert** , and I have included the consumer incident **Number** as an inbound field but have specified that it will **Never** synch (i.e. it will not be written to a corresponding provider case field).


I will use an Advanced Transform to add the customer’s incident number to the provider’s case description so the provider will have an easily visible reference to use in related discussions with the customer.


As mentioned above, the specified Remote Task variables can be referenced using the **object_data.remote_task_vars** object, but I need to know what the related field name for the incident number is so I can use it in my script. To find a Remote Task variable field name, open the related Remote Task Definition and select the **Remote tasks variables** related list.


I can see that the field name for the inbound incident number is: **number50270d9ec31c8210075c35aa05013126** .


Now I can create an Advanced Transform with a script that uses the number field to transform the provider case description.


See that the script adds the customer’s incident number to the case description by referring to the number field in the remote task variables object:


And see the result…


I hope this article helps you better understand how you can pass fields using Service Bridge Remote Tasks and then use them in your Transform scripts!
