---
schema_version: "1.0.0"
document_id: "bd62797fe3d396a86599f09c0ee178bec48adbaf9638719548c6304aafa495c1"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/how-to-change-the-account-and-order-overview-content-on-the/ba-p/3132431"
published_at: "2024-12-19T19:42:59+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:3060a772a347ecd3e1f7e2750ab7d1e85ab6d0ef4a34a08648f9481226cc7332"
---

# How to change the account and order overview content on the order form in CSM/FSM Workspace

I recently helped a customer understand how to change the information presented in the customer account information and order overview sections on the Order form in CSM/FSM Workspace. Here's how...


Here we see an Order form with account information and order overview sections highlighted.


To change the information presented in these sections we need to modify the


script include OrderUtil which is an OOB script include designated for customer overrides.


After overriding you can review the returned JSON by running the following from Scripts – Background.


```text
var orderInfo = new sn_ind_tmt_orm.OrderUtil().getOrderDetailsJSON('SysID of the Order');
gs.info(JSON.stringify(orderInfo));
```


**Overriding OrderUtilOOB**


The script include OrderUtil extends the script include OrderUtilOOB.


The method in


**OderUtilOOB** which needs to be overridden is


**generateDetailsJSON** .


In the method **generateDetailsJSON,** customer account information is contained in **contact_card** and Order Overview information is contained in **label_value_stacked_items.**


Copy the method


**generateDetailsJSON** and put it into the script include OrderUtil and then modify contact_card and label_value_stacked_items to display the desired information. For example you could remove State from the contact_fields. In the image below, State was below city but it has been removed.
