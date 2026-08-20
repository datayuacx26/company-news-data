---
schema_version: "1.0.0"
document_id: "010cc0be77c578a31e1fc985b48ff500b61adbde7b07125478c7a011058c2dfc"
company_key: "yc-junction"
company: "Junction"
source_id: "yc-junction-news-import-b3b1cd4ba92d"
canonical_url: "https://docs.junction.com/changelog/core/api"
published_at: null
first_seen_at: "2026-07-25T10:23:23.528404+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:0d2f810fc8cd5432b4a8bd331a44381fcaf67bbdfb9e9ae90361a072840c0c8a"
---

# API - Junction

### ​


Error for closed beta endpoint calls is now JSON (Nov 2025)


Previously, if you tried to access an endpoint in closed beta and for which your team was not enabled, it would return a plain text error body. It now returns a JSON error body to align with the rest of our API.


### ​


Lab Test Result Interpretation Filtering (June 2025)


You can now filter lab test orders by result interpretation using the new` interpretation` query parameter in the[GET /orders](https://docs.junction.com/api-reference/lab-testing/get-orders) endpoint.


Details


The new filtering capability allows you to query orders based on their clinical interpretation:


**Query Parameter:**


- **` interpretation`** - Filter by result interpretation of the lab test


- Type:` enum<string> | null`
- Available options:` normal` ,` abnormal` ,` critical`
- Note: This enum is non-exhaustive


**Response Enhancement:** Order responses now include a new` interpretation` field providing the clinical assessment of the test results:


- **` interpretation`** - Interpretation of the order result


- Type:` enum<string> | null`
- Available options:` normal` ,` abnormal` ,` critical`
- Note: This enum is non-exhaustive


This enhancement enables you to programmatically identify and prioritize critical lab results, improving clinical workflow efficiency and patient care monitoring.


Check out the[GET /orders](https://docs.junction.com/api-reference/lab-testing/get-orders) endpoint documentation.


### ​


Webhook Management Endpoints (May 2025)


You can now programmatically manage your webhooks via the[Webhooks API](https://docs.junction.com/api-reference/org-management/team-webhook/list) . The new endpoints allow you to:


- **CRUD** (create/read/update/delete) your webhooks
- Manage webhook **headers**
- Update webhook **secrets**


Org Management API is available for[the Scale plan](https://tryvital.io/pricing) .


### ​


Team Management Keys (May 2025)


[Junction Management API](https://docs.junction.com/api-details/junction-management-api) now supports Management Keys (previously *Org Keys* ) that are scoped to one or more Teams.


As a recap, there are now two types of Management Keys:


Type Remarks


Org Management Key Full control of the organization — notably can create or delete Teams.


Team Management Key Scoped control over one or more Teams. But it cannot access Org-level resources or actions, e.g., creating or deleting Teams.


Details


All Management Keys — Org or Team — are accepted by the` X-Management-Key` header, as well as the deprecated` X-Vital-Org-Key` header.


Selected customers can now manage Management Keys in the[Junction Dashboard](https://app.junction.com/) through:


Type Remarks


Org Management Key The **Org Config** page; accessible via the top-left corner Dropdown Menu.


Team Management Key The **Team Config** page.


Check out:


- the[Create Management Key](https://docs.junction.com/api-reference/org-management/management-keys/create-management-key) endpoint documentation;
- the[List Management Keys](https://docs.junction.com/api-reference/org-management/management-keys/list-management-keys) endpoint documentation; and
- the[Delete Management Key](https://docs.junction.com/api-reference/org-management/management-keys/delete-management-key) endpoint documentation.


If you intend to create a Team Management Key that binds to 2 or more Teams, you must use the[Create Management Key](https://docs.junction.com/api-reference/org-management/management-keys/create-management-key) API endpoint. Junction Dashboard does not support creating a key for more than one Team.


Note that Management Keys cannot be used as Team API Keys to access the Junction API. However, you can[manage Team API Keys](https://docs.junction.com/api-reference/org-management/team-api-keys/create-team-api-key) through a Management Key.


[Junction Management API](https://docs.junction.com/api-details/junction-management-api) is available for[the Scale plan](https://tryvital.io/pricing) .


### ​


` X-Management-Key` header for Junction Management API (April 2025)


[Junction Management API](https://docs.junction.com/api-details/junction-management-api) now accepts Management Keys (previously *Org Keys* ) in the` X-Management-Key` header, in addition to the` X-Vital-Org-Key` header.


We will continue to support the` X-Vital-Org-Key` header, though we recommend moving over to` X-Management-Key` to avoid confusion, especially if you do plan to adopt Team Management Keys.


### ​


Enhanced Historical Data Pull Status Tracking (May 2025)


We’ve added a new “Retrying” state to the Historical Pull Status page and the historical introspection endpoint, helping you distinguish temporary issues from permanent failures. Additionally, failed historical pulls now include extra error information, making troubleshooting easier and more efficient.


This applies to connections established after May 9th.


### ​


Prepare Team Custom Credentials endpoint (Aug 2024)


The new[Prepare Team Custom Credentials](https://docs.junction.com/api-reference/org-management/team-custom-credentials/prepare-team-custom-credentials) endpoint provides instructions for preparation of[Bring Your Own OAuth](https://docs.junction.com/wearables/connecting-providers/bring-your-own-oauth/overview) custom credentials.


You can use the information to configure things like OAuth callback URI and the Webhook URI (if applicable), before activating it on your Junction Team through the[Set Team Custom Credential](https://docs.junction.com/api-reference/org-management/team-custom-credentials/upsert-team-custom-credentials) endpoint.


### ​


Azure Event Hub: Flexible routing (Aug 2024)


You can now configure your[Azure Event Hub destination in ETL Pipelines](https://docs.junction.com/webhooks/etl-pipelines/azure-event-hubs) to route Junction data events to different Event Hubs based on their event type prefix.


Check out the[ETL Pipelines - Azure Event Hub](https://docs.junction.com/webhooks/etl-pipelines/azure-event-hubs#multiple-event-hubs) documentation.


ETL Pipelines are available for[the Scale plan](https://tryvital.io/pricing) .


### ​


Azure Event Hubs as ETL Pipeline destination (Jun 2024)


You can now receive[events](https://docs.junction.com/webhooks/introduction) from Junction directly with your Azure Event Hubs.


Check out the[ETL Pipelines](https://docs.junction.com/webhooks/etl-pipelines/azure-event-hubs) documentation.


ETL Pipelines are available for[the Scale plan](https://tryvital.io/pricing) .


### ​


Manage Team Brand Information (Apr 2024)


You can now manage Brand Information of your Junction Teams through the Org Management API.


Details


Team Brand Information is used in:


1. the Junction-hosted[Link widget](https://docs.junction.com/wearables/connecting-providers/introduction) ;
2. all user communications in[Junction Lab Testing](https://docs.junction.com/lab/overview/introduction) sent on your behalf; and
3. the Junction-hosted Appointment Booking page for[Junction Lab Testing](https://docs.junction.com/lab/overview/introduction) .


Check out the[Update Team](https://docs.junction.com/api-reference/org-management/team/update-team) and[Create Team](https://docs.junction.com/api-reference/org-management/team/create-team) endpoint documentation.


### ​


Junction Orgs and Org Management API (Apr 2024)


We have introduced Junction Org, a new level that groups all your Junction Teams. Your Junction Teams have been transparently grouped and migrated to the new structure.


Details


We introduced this to provide a unified billing and administrative experience for customers having these use cases:


1. multi-region presence; or
2. user organization with diverging team-level configurations.


We introduced the Org Management API because customers have asked for programmatic access to dynamically create Junction Teams and manage different aspects of their Junction Teams.


Check out the[Org Management API](https://docs.junction.com/api-reference/org-management) documentation.


### ​


New webhook event top-level fields (Mar 2024)


All events now include Team ID, User ID and Client User ID as top-level fields.


Details


We introduced this because this helps reduce a Junction User ID → Client User ID database lookup on your end.


Check out the[Webhook Event Structure](https://docs.junction.com/webhooks/event-structure) documentation.


#### ​


Before


Basic event structure


```text
{
"data"  : {
#   ...   event   specific   data
},
"event_type"  :   "daily.data.glucose.created"  ,
}


```


#### ​


After


Basic event structure


```text
{
"data"  : {
#   ...   event   specific   data
},
"event_type"  :   "daily.data.glucose.created"  ,
"user_id"  :   "4a29dbc7-6db3-4c83-bfac-70a20a4be1b2"  ,
"client_user_id"  :   "01HW3FSNVCHC3B2QB5N0ZAAAVG"  ,
"team_id"  :   "6b74423d-0504-4470-9afb-477252ccf67a"
}


```


### ​


Improved error response for User creation conflicts (Mar 2024)


The[Create User](https://docs.junction.com/api-reference/user/create-user) endpoint has improved handling of conflicts in Client User ID.


Details


We introduced this because the user creation endpoint being idempotent can help simplify your application logic.


When the supplied` client_user_id` conflicts with an existing user, the 400 Bad Request response now includes the Junction User ID (` user_id` ) and the creation date (` created_on` ) of the conflicting user.


Check out the[Create User](https://docs.junction.com/api-reference/user/create-user) endpoint documentation.


### ​


User Undo Deletion (Feb 2024)


You can now undo user deletion that is still in the 7-day grace period.


Details


Check out the[User Undo Deletion](https://docs.junction.com/api-reference/user/undo-delete-user) endpoint documentation.
