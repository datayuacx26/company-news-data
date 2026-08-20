---
schema_version: "1.0.0"
document_id: "516383163b6673675606c2685da3b8d55ffdef5ba4d14d8b480f7d1eba5a5040"
company_key: "servicenow-inc-common-stock"
company: "ServiceNow Inc."
source_id: "servicenow-inc-common-stock-rss-e68ea5e3c60f"
canonical_url: "https://www.servicenow.com/community/technology-blog/service-bridge-v2-1-february25-store-release-publish-catalog/ba-p/3152692"
published_at: "2025-01-31T14:55:30+00:00"
first_seen_at: "2026-07-20T04:36:33.238428+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:21f343712c11e53479e98c0bc17f683d9da40e97a7f2e3025063d44d01072268"
---

# Service Bridge v2.1: February25 Store Release - Publish catalog items as remote record producers

This time saving feature allows providers to publish local catalog items to Service Bridge as remote record producers.


A provider can create a remote record producer from the following catalog classes.


1. Catalog Item


2. Record Producer


3. Hardware Catalog


4. Software Catalog


The catalog item must meet the following requirements.


1. Must be in a published state


2. Must be one of the supported classes


3. Cannot already have been published to service bridge


Catalog items can be published to Service Bridge individually or in bulk. When publishing catalog items in bulk, the default limit is 20 items. The limit can be changed by defining and setting the system property **sn_sb_pro.max_batch_size_covertable_catalog_items.**


Publishing a catalog item to Service Bridge creates a copy of the item as a remote record producer. The remote record producer is associated with the original catalog item via the reference field **Created from cat item** .


**Publish individual catalog item to Service Bridge**


1. Navigate to the catalog to be published.
2. Click the Publish to Service Bridge Related Link


3. A remote record producer is created from the catalog item and navigation is directed to the newly created remote record producer.


**Publish catalog items in bulk to Service Bridge**


1. Navigate to a list of catalog items to be Published to Service Bridge.
2. On the list, select the checkboxes for the catalog items to be Published to Service Bridge.
3. In the **Actions on selected rows…** menu, click Publish to Service Bridge.


4. Publishing to Service Bridge modal is displayed while in progress.


5. On completion, the results are displayed.


**After publishing to Service Bridge** , items need to be finalized prior to Publishing for synching to consumer instances. Finalization steps include.


1. Adding a required flow
2. Adding Consumer Criteria
3. Adding Persona if required
4. Verify and update remote record producer content and variables. Example reference variables may need to be changed to remote choices.
