---
schema_version: "1.0.0"
document_id: "e32b45dfd7e7d19947f080a1b965fde40cf919444e0de32db64c027c388c31d3"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/service-bridge-v2-1-february-2025-store-release-new-feature/ba-p/3168550"
published_at: "2025-02-05T22:54:25+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:744f87bc00319ae4ccdc48b6ae784730041e9ac33b7d6ce2c10b02424d52ab5e"
---

# Service Bridge v2.1: February 2025 Store Release New Feature - Consumer Variables for RRPs

In the Xanadu release of Service Bridge, we added a[Consumer Pre-Flows feature](https://www.servicenow.com/docs/bundle/yokohama-service-bridge/page/product/tmt-service-bridge-2/task/service-bridge-v2-conf-consumer-flow.html) that allows Service Bridge consumers to introduce a flow for controlling when Remote Catalog Item / RRP request data should be synced to the providers instance. The new February 2025 Store Release of Service Bridge (v2.1) extends the consumers control of provider RRPs even further by enabling a consumer administrator to add additional variables to an RRP that can be used in managing the request content and flow.


To use this new Consumer Pre-Flows Service Bridge feature you must:


- be using Service Bridge version 2.1.x+
- add the required variables to a variable set (see[Using variable sets with Remote Record Producers](https://www.servicenow.com/docs/bundle/yokohama-service-bridge/page/product/tmt-service-bridge-2/concept/service-bridge-v2-variable-sets.html) for help)


You can only add one variable set to a providers RRP in your consumer instance, but that set may contain multiple variables that you wish to gather.


Here's an example to illustrate how this new feature works...


This "Upgrade Server" Service Bridge RRP from the provider allows a consumer user to request a server upgrade:


The consumer leadership needs to make sure upgrade requests are justified before allowing them to be sent to the provider for fulfillment, so they want to add two additional variables to the RRP for capturing business justification and impact.


The consumer administrator creates a new single-row variable set called Server Upgrade Approval that contains two variables - a multi-line text variable for capturing the business justification , and a select box variable for capturing the impact:


The administrator then navigates to **All > Service Bridge Consumer > Provider Connections** and selects the Number link for the provider of the Upgrade Server RRP. In the **Provider Connection** page, the administrator selects the Remote record producers related list, and then selects the Upgrade Server remote record producer.


In the **Consumer variables** reference field the administrator selects the Server Upgrade Approval variable set that they created, and then selects the **Activate** UI action to activate the RRP in their instance, or the **Update** UI action if the RRP has previously been activated.


The variables contained in the variable set are now included when a consumer user requests the RRP from the catalog:


The consumer administrator can access and use the added variables in the Consumer Pre-flow as required to review a request and control if and when it is sync'd back to the provider instance. The variables can be accessed in the *vars_json* field of the newly created Provider Task (in a future release we will be adding a flow action that will simplify variable access):


One thing to note is that consumer variables are visible/accessible


as part of a resulting Provider Task on the


consumer instance but are not passed to the


provider instance. They are only visible to the


consumer.


​


Allowing this additional consumer control of Service Bridge RRP requests will enhance Service Bridge useability, elevate the customer experience, and driver higher adoption rates. Enjoy!
