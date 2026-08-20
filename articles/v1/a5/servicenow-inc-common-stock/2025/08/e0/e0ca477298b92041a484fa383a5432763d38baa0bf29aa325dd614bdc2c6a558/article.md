---
schema_version: "1.0.0"
document_id: "e0ca477298b92041a484fa383a5432763d38baa0bf29aa325dd614bdc2c6a558"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/foundation-data-synch-and-cmdb-collaboration/ba-p/3359949"
published_at: "2025-08-22T23:48:21+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:967bfde232fc9063360ba67599cfb9fe435c0970640eb614a34fe181749aa672"
---

# Foundation Data Synch and CMDB collaboration

I have been kicking the tyres of Foundation Data Synch (sn_fds) plug as part of August 2025 release of Service Bridge v2.2.2 (sn_sb), and I thought I would take you through the configuration process and try to sum it up in a diagram.


Foundation data synch at this release allows for a Provider to offer CMDB, Asset, User, Group, Location, Company & Department tables to a Consumer over Serice Bridge.


The Consumer requires an import set map to load the offered data into the platform on a schedule, and in the case of the CMDB using Integration Hub ETL as a custom Service Graph Connector.


For demonstration I will use an Apache Web Server running on a Linux Server to show how this data comes into the Consumer.


The pre-requisites are that the Provider and Consumer are connected and have the Provider and Consumer plugins for Service Bridge and Foundation Data Sync.


A diagram is at the bottom that explains the steps, if you don't like following step-by-step instructions.


First step for Foundation Data Sync for CMDB is to ensure the Company is set on the Provider side for the Configuration Items.


See below - Apache Web Server runs on Linux Server


Before doing any configuration, a key point to note, is to read all the informational


'Messages'.


They are actually quite helpful..!!


STEP 1


Service Bridge Provider > Administration > FDS Offering Definitions > New


Give it a name and description and click 'Save'


Before adding Offering Items, we need to ensure at a minimum the IRE fields are sent across.


This may have to be worked out with the Consumer in the event there are differences in their IRE rules for the CI classes being sent.


See CI Class Manager.


STEP 2a


Offering Item - click New, add fields.


Also, click AccountSecure to ensure they go to the Company of the Service Bridge connection.


When completed, click Save.


Now Click 'Create Dependent Offering Item', as the Apache Web Server has a dependent Linux Server.


The add fields like Serial Number, etc.


STEP 2b and STEP 3


Back on the Offering, Add Consumer Criteria - e.g. All Accounts


Then click Publish.


I have left Auto-acknowledge and auto-publish set to true, to reduce the amount of overhead (i.e. it is already quite a long process).


The Confirmation messages explain why.


STEP 4


Now lets move the Consumer.


Service Bridge Consumer > Request FDS Offerings


Select Provider and Offerings, and set Synch Interval.


Click Submit


This moves into a Request Process.


STEP 5


Wait for this to move from New to Work In Progress.


STEP 6


Then refresh the list for Subscriptions, which should be in Awaiting Validation.


Open that then the Subscription Items, which should have Apache Web Server.


STEP 7


Click on 'ETL Transform Map Assistance'


This opens IntegrationHub ETL


Step 1 - mark as Complete as the import set is already in place.


Step 2 - Preview and Prepare Data.


This is where you can transform the data.


You should see the Apache and related Linux Server data in the tree.


Once transform completed, Mark as Complete


Step 3 - Map Data to CMDB and Add relationships


Add Apache Web Server Class, and Setup Mapping


Add attributes and fill out with data pills, here is the mapping I completed here. It is important to put the source native key in there to ensure the Connector has an identifier to coalesce before going into the CMDB.


Do the same for Linux Server, e.g. Source Native Key, Serial number, OS version, Operating System, etc.


Then mark as complete and move to 'Add Relationships'


The mark as complete.


The Step 4 - test and rollback integration results, making sure the CMDB would be updated with an Apache and Linux server & relationship.


Mark as Complete & Perform Rollback


STEP 8 & 9


Back to Subscription Item > this should move to Validated.


Navigate back the Subscription & Click 'Accept' - this should move to 'Active'


Ok, where not done yet…this is where we can send the initial payload of all the data.


Once this is done, we can shut our eyes and hope the integration works peacefully into the future, with data updates, etc, flowing over the bridge.


STEP 10


Back the Provider > FDS Subscription


On agreement with the consumer, we can 'Send Initial Payload' - remember in real scenarios this could have ,000s of Configuration item records.


Back to the Consumer > Subscription Item - we should see the 'Next Action' set.


STEP 11


If your inpatient like me, back to the Provider - we can 'Send Payload Now' or wait for the scheduled job to run.


Some happy snaps back on the Consumer.


And a diagram describing all the steps we mentioned above. Note all the tables are not shown in ERD, more to help with the flow of configuration.
