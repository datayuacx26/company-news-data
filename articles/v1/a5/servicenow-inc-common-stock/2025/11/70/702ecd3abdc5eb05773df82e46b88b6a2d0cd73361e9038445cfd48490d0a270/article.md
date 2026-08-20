---
schema_version: "1.0.0"
document_id: "702ecd3abdc5eb05773df82e46b88b6a2d0cd73361e9038445cfd48490d0a270"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/learnings-service-bridge-amp-domain-separation/ba-p/3249940"
published_at: "2025-11-07T23:40:41+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:983a427b6c35c648e62567928e5663959d9ac38703c5d17e19bd4bbbd8a60c5a"
---

# Learnings Service Bridge & Domain Separation

Service Bridge natively support multiple integrations between the 2 same instances. So I went about identifying whether it could be supported for a Remote Task between a Domain Separated 'Consumer' and a 'Provider'.


The scenario.


A service provider runs a Domain Separated instance to host shared fulfilment/resolvers, etc, of many customer environment/s. They work with a Service Desk (either ran by the Service Provider or by the customer or both) within the ITSM suite. The Service Desk would need to collaborate or hand off that Incident to a shared services ran by the Service Provider as a Case, so they would trigger a Remote Task (Case) with them on another instance from the business impacting Incident.


Consumer


Provider


incident


sn_customerservice_case (Case)


The challenge:


- Service Bridge only allows 1 URL endpoint between the same provider & consumer instance, and the Connection is tied to a 'Company' of the Provider.


The solution idea:


- Build another 'lane' inside the Remote Task definition, to store the Company being referred to, and ensure it finds the right 'lane' to land on at either end (Company or Domain)


The only 'Data' pre-requisite:


- The Domain Display value on the Consumer (Domain Separated instance), must match the Company Display Value on the Provider instance.


Direction : Provider (non Domain Separated) to Consumer (Domain Separated)


Addition of a field to 'customer_account' to connect the Service Bridge > Consumer Connection


- Customer_account.u_consumer – Reference to ‘sn_sb_pro_consumer_connection’. (Scope: Service Bridge for Providers)


- When provisioning the customer account it will need this populated.


Addition of a new UI Action to call a SubFlow to trigger the Remote Task to the Consumer - 'Create Remote Task on MSP Consumer' - this is displayed if there is a Remote Task Definition for the Current Record’s Table & there is a Consumer Reference from the Current Account record Only. ( Note


: this may have to be extended to other tables where the Company field is required at a later point)


- This triggers a SubFlow in the Foreground, with Inputs for Reference to Case record :sn_sb_pro.create_remote_task_for_related_companies


- If successful, returns a Remote Task number for Display as an Information message, otherwise, an Error Message is returned to the user.


- In addition, requires the Role ‘sn_sb.remote_task_creator’ (Note: CS8023999 & PRB1863882 has been raised for similar role for OOTB UI Action)


SubFlow : Create Remote Task for Related Companies


- Security: Run as User and requires role ‘sn_sb.remote_task_creator’


- BEGIN - If Parent Remote Task does not exist.


Error handling logic has been included to display message to the User. If this occurs, we hope your Datacom Case Agents will be in touch with you!


The Hello World Remote Task Definition


- Packaged in ‘Global’ scope – theoretically this should work in scope ‘Service Bridge for Providers’ - Name = Case to Incident MSP V1


- Send Attachments & Maintain SysID


- Consumer Criteria – All Accounts


- Provider : sn_customerservice_case to incident (direct field unless stated)


- Consumer : incident to sn_customerservice_case


Consumer Business Rule:


- Set Company - Provider Remote Task (sn_sb_con_remote_task) – Global


Direction : Consumer (Domain Separated) to Provider (Non Domain Separated)


Provider Business Rule


- Set Company - Consumer Remote Task (sn_sb_pro_remote_task) – Global


sys_remote_update_set_78189a03935e6ad0e31030d74dba10d2.xml


sys_remote_update_set_c3181207935e6ad0e31030d74dba10fd.xml


sys_script_d6c485c38314a610f19b75326daad339.xml


sys_script_3106c5cb939ca21039a230d74dba106c.xml


sys_remote_update_set_c3181207935e6ad0e31030d74dba10fd.xml


sys_remote_update_set_78189a03935e6ad0e31030d74dba10d2.xml
