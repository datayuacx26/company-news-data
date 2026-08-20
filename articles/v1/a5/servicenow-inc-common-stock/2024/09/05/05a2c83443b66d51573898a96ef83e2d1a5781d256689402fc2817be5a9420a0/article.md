---
schema_version: "1.0.0"
document_id: "05a2c83443b66d51573898a96ef83e2d1a5781d256689402fc2817be5a9420a0"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/service-bridge-remote-tasks-sync-existing-attachments/ba-p/3041376"
published_at: "2024-09-13T14:59:22+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:006237dc3d3a02888b6e2a803cef85c82b45cd53b7e7751f2ac7066669fc513c"
---

# Service Bridge – Remote Tasks: Sync Existing Attachments

**[Remote Tasks](https://docs.servicenow.com/bundle/xanadu-service-bridge/page/product/tmt-service-bridge-2/concept/service-bridge-v2-remote-task-overview.html)**


are used to connect


tasks between two ServiceNow instances.


These


tasks


can be any task in a ServiceNow instance such as incident,


problem, change,


case, etc.


The connection is defined in the


**[Remote Task Definition](https://docs.servicenow.com/bundle/xanadu-service-bridge/page/product/tmt-service-bridge-2/task/service-bridge-v2-create-remote-tasks-defs.html)** .


When


the


connection is made


Service Bridge (v1 or v2) does not send attachments that exist on


a


task


before


the


connection is made


.


This is


by design


for security reasons


.


Because m


any different providers can be connected to a single task over time,


it was decided not to assume all attachments should be shared.


To sync existing attachments, the following Flow can be created in the Provider instance, Consumer instance, or both. If the instance which initiates the Remote Task has the Flow, then all attachments on the task will be synced.


Note: Based on feedback this post has been updated to use a flow instead of a business rule. An update set containing this Flow is included with this post. This is a custom solution not included in the Service Bridge application and is provided "AS-IS". Ensure to test thoroughly before implementing a solution in a production environment.


**Flow - Copy Attachment from Parent**


****


**Flow Properties**


****


**Flow Trigger**


****


**Flow Action 1**


****


**Flow Action 2**


****


**Flow Action 3**


****


Update Set - Service Bridge - Copy Attachment from Parent - Flow.xml
