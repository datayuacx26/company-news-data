---
schema_version: "1.0.0"
document_id: "8dd8bd70b0935f129c901928dcb6e278d8e8420d22b0c2da9dd551fdc55fa7a2"
company_key: "yc-paragon"
company: "Paragon"
source_id: "yc-paragon-news-import-425709159824"
canonical_url: "https://www.useparagon.com/blog/introducing-actionkit-triggers"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-25T18:46:04.995159+00:00"
fetched_at: "2026-07-28T22:12:46.949292+00:00"
content_hash: "sha256:8cd93c31547886c3ab5fc4f2d962a0425e02f085244fe6266560d74b8c52507b"
---

# Introducing ActionKit Triggers: Subscribe to any integration event

Today, we're launching **ActionKit Triggers** , a single API to subscribe to your users' integration events, like "Slack message received," "HubSpot record updated," and "Notion Page created."


With ActionKit Triggers (or Triggers for short), integrations aren't just active when users are actively using your product. They're **reactive** . Your product can listen to events from your users' integrated tools around the clock for use cases like:


1.


User-defined workflows in a **workflow builder** application


2.


User-defined agents in **agent builder** applications


3.


**Background agents** that trigger from events like Slack messages or GitHub PRs


4.


**Real-time automations** where your application is listening for integration webhooks


## Why ActionKit Triggers


In the past, subscribing to different webhook events from different integration providers was an arduous and disparate process. Each new webhook subscription requires:


◻ Research into that integration's unique webhook policies (i.e. retries, signatures, handshake mechanisms, delivery policies)


◻ Additional infrastructure that requires high-uptime and high-durability, as webhooks can be easily missed


◻ New UI for end-users to configure their unique integration settings


◻ Tracing and observability across all webhook events


◻ Maintenance for all of the above, especially as downstream integration APIs change


We developed Triggers for customers that wanted a **simple and standard** way to reliably configure and subscribe to their users' integration events. Addressing the pain points of integration webhooks:


✅ Triggers is a **unified API** for subscribing to ALL of your integration webhooks


✅ **Paragon manages the webhook infrastructure** with the Triggers API to list available webhook subscriptions, subscribe to event buses, unsubscribe from events, and more*


✅ Triggers provides **drop-in configuration** so your end-users can select exactly what and how events are delivered


✅ **All events are logged** and traced in Paragon's[Event Logs](https://docs.useparagon.com/monitoring/event-logs#event-logs) so you can see all of your users' events in the same interface as Workflows, Syncs, and ActionKit


✅ **Paragon manages API drift** to stay on top of breaking changes from downstream providers


* **Paragon even creates polling infrastructure to create webhook-like events for integrations without webhook support**


## How ActionKit Triggers Work


ActionKit Triggers was built to be not only a **universal API** to subscribe to all of your users' events, but also a **universal subscription model** for these events.


**Universal API**


Use the` POST trigger-subscriptions` to subscribe to any integration event just by switching out the payload options.


```text
const    options   =  {
method  :    'POST'  ,
headers  :    {
Authorization  :    'Bearer <token>'  ,
'Content-Type'  :    'application/json'
}  ,
body  :    JSON  . stringify  (  {
integration  :    'salesforce'  ,
type  :    'SALESFORCE_RECORD_CREATED'  ,
parameters  :    {    recordType  :    'Opportunity'    }  ,
webhookOverride  :    {
url  :    '<https://example.com>'  ,
headers  :    {  }  ,
metadata  :    {  }
}
}  )
}  ;


fetch  (  '<https://actionkit.useparagon.com/projects/{project_id}/trigger-subscriptions>'  ,    options  )
. then  (  res    =>    res  . json  (  )  )
. then  (  res    =>    console  . log  (  res  )  )
. catch  (  err    =>    console  . error  (  err  )  )  ;
```


```text
const    options   =  {
method  :    'POST'  ,
headers  :    {
Authorization  :    'Bearer <token>'  ,
'Content-Type'  :    'application/json'
}  ,
body  :    JSON  . stringify  (  {
integration  :    'salesforce'  ,
type  :    'SALESFORCE_RECORD_CREATED'  ,
parameters  :    {    recordType  :    'Opportunity'    }  ,
webhookOverride  :    {
url  :    '<https://example.com>'  ,
headers  :    {  }  ,
metadata  :    {  }
}
}  )
}  ;


. then  (  res    =>    res  . json  (  )  )
. then  (  res    =>    console  . log  (  res  )  )
. catch  (  err    =>    console  . error  (  err  )  )  ;
```


```text
const    options   =  {
method  :    'POST'  ,
headers  :    {
Authorization  :    'Bearer <token>'  ,
'Content-Type'  :    'application/json'
}  ,
body  :    JSON  . stringify  (  {
integration  :    'salesforce'  ,
type  :    'SALESFORCE_RECORD_CREATED'  ,
parameters  :    {    recordType  :    'Opportunity'    }  ,
webhookOverride  :    {
url  :    '<https://example.com>'  ,
headers  :    {  }  ,
metadata  :    {  }
}
}  )
}  ;


. then  (  res    =>    res  . json  (  )  )
. then  (  res    =>    console  . log  (  res  )  )
. catch  (  err    =>    console  . error  (  err  )  )  ;
```


```text
const    options   =  {
method  :    'POST'  ,
headers  :    {
Authorization  :    'Bearer <token>'  ,
'Content-Type'  :    'application/json'
}  ,
body  :    JSON  . stringify  (  {
integration  :    'salesforce'  ,
type  :    'SALESFORCE_RECORD_CREATED'  ,
parameters  :    {    recordType  :    'Opportunity'    }  ,
webhookOverride  :    {
url  :    '<https://example.com>'  ,
headers  :    {  }  ,
metadata  :    {  }
}
}  )
}  ;


. then  (  res    =>    res  . json  (  )  )
. then  (  res    =>    console  . log  (  res  )  )
. catch  (  err    =>    console  . error  (  err  )  )  ;
```


**Universal Subscription**


All Triggers payloads are in a standard format and have the same validation (HMAC-256 signature), retry, backoff, and rate limiting policies.


```text
{
"eventId"  :  "[uuid]"  ,
"userId"  :  "12345"  ,
"integration"  :  "salesforce"  ,
"triggerType"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerSubscriptionId"  :  "[uuid]"  ,
"credentialId"  :  "[uuid]"  ,
"projectId"  :  "[uuid]"  ,
"dateReceived"  :  "2025-10-14T00:00:00Z"  ,
"payload"  : { ...  },
"triggerSubscriptionMetadata"  : { ...


```


```text
{
"eventId"  :  "[uuid]"  ,
"userId"  :  "12345"  ,
"integration"  :  "salesforce"  ,
"triggerType"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerSubscriptionId"  :  "[uuid]"  ,
"credentialId"  :  "[uuid]"  ,
"projectId"  :  "[uuid]"  ,
"dateReceived"  :  "2025-10-14T00:00:00Z"  ,
"payload"  : { ...  },
"triggerSubscriptionMetadata"  : { ...


```


```text
{
"eventId"  :  "[uuid]"  ,
"userId"  :  "12345"  ,
"integration"  :  "salesforce"  ,
"triggerType"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerSubscriptionId"  :  "[uuid]"  ,
"credentialId"  :  "[uuid]"  ,
"projectId"  :  "[uuid]"  ,
"dateReceived"  :  "2025-10-14T00:00:00Z"  ,
"payload"  : { ...  },
"triggerSubscriptionMetadata"  : { ...


```


```text
{
"eventId"  :  "[uuid]"  ,
"userId"  :  "12345"  ,
"integration"  :  "salesforce"  ,
"triggerType"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerSubscriptionId"  :  "[uuid]"  ,
"credentialId"  :  "[uuid]"  ,
"projectId"  :  "[uuid]"  ,
"dateReceived"  :  "2025-10-14T00:00:00Z"  ,
"payload"  : { ...  },
"triggerSubscriptionMetadata"  : { ...


```


ActionKit Triggers also features a` GET /triggers` endpoint for listing all triggers that a user can subscribe to, with their respective configurations and descriptions.


```text
{
"type"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerModel"  :  "POLLING"  ,
"title"  :  "New Record"  ,
"description"  :  "Trigger when a new record is created in Salesforce"  ,
"parameters"  : [
{
"id"  :  "recordType"  ,
"type"  :  "TEXT_NO_VARS"  ,
"label"  :  "Record Type"  ,
"required"  :  true


```


```text
{
"type"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerModel"  :  "POLLING"  ,
"title"  :  "New Record"  ,
"description"  :  "Trigger when a new record is created in Salesforce"  ,
"parameters"  : [
{
"id"  :  "recordType"  ,
"type"  :  "TEXT_NO_VARS"  ,
"label"  :  "Record Type"  ,
"required"  :  true


```


```text
{
"type"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerModel"  :  "POLLING"  ,
"title"  :  "New Record"  ,
"description"  :  "Trigger when a new record is created in Salesforce"  ,
"parameters"  : [
{
"id"  :  "recordType"  ,
"type"  :  "TEXT_NO_VARS"  ,
"label"  :  "Record Type"  ,
"required"  :  true


```


```text
{
"type"  :  "SALESFORCE_RECORD_CREATED"  ,
"triggerModel"  :  "POLLING"  ,
"title"  :  "New Record"  ,
"description"  :  "Trigger when a new record is created in Salesforce"  ,
"parameters"  : [
{
"id"  :  "recordType"  ,
"type"  :  "TEXT_NO_VARS"  ,
"label"  :  "Record Type"  ,
"required"  :  true


```


The` GET /trigger-subscriptions` allows you to fetch example webhook payloads that the subscription will deliver to your application.


```text
[
{
"id"  :  "001xx000003DHP0AAO"  ,
"name"  :  "Sample Opportunity"  ,
"amount"  :  50000  ,
"stage"  :  "Prospecting"  ,
"closeDate"  :  "2025-12-31"


```


```text
[
{
"id"  :  "001xx000003DHP0AAO"  ,
"name"  :  "Sample Opportunity"  ,
"amount"  :  50000  ,
"stage"  :  "Prospecting"  ,
"closeDate"  :  "2025-12-31"


```


```text
[
{
"id"  :  "001xx000003DHP0AAO"  ,
"name"  :  "Sample Opportunity"  ,
"amount"  :  50000  ,
"stage"  :  "Prospecting"  ,
"closeDate"  :  "2025-12-31"


```


```text
[
{
"id"  :  "001xx000003DHP0AAO"  ,
"name"  :  "Sample Opportunity"  ,
"amount"  :  50000  ,
"stage"  :  "Prospecting"  ,
"closeDate"  :  "2025-12-31"


```


These Triggers APIs **enable workflow builders and agents** to configure parameters and understand expected payloads directly.


## ActionKit: Your Full Suite for Building Integration Features


ActionKit has two components: Triggers and Tools. Triggers are the focus of this announcement, but ActionKit Tools is the complement to Triggers.


ActionKit Tools is a unified API that provides hundreds of integration actions, like "Slack Send Message" and "Notion Create Page."


**Tools** is how your application interacts with integration APIs. **Triggers** is how your application reacts to integration events.


In tandem, ActionKit provides all of the primitives you need to build any integration feature.


-


**For your workflow builder** : Listen to your users' "Salesforce Record Created" event Trigger to initiate your users' defined workflow and call the "Slack Send Message" Tool to perform actions in Slack's API


-


**For your background agent** : Listen to your users' "Slack Message Received" event Trigger to kick off your AI agent with Tools like "Jira Update Ticket"


-


**For real-time automations** : Listen to your users' "HubSpot Record Created" event Trigger, perform data enrichment, and call the "HubSpot Record Update" Tool with the enriched data


Your application reacts to your users' integration events in real time and performs actions, all with Paragon-managed infrastructure.


This means **unified APIs** . **No maintenance** on 3rd-party APIs. **Guaranteed performance** on Paragon's infrastructure. **Built-in monitoring** and observability. And **first-class developer experience** for building integrations for agents and workflow products.


## Get Started With ActionKit Triggers & Tools


ActionKit, with Triggers and Tools, are the best way to build fully featured integrations. Ship reactive integrations that perform actions for your users around the clock. Triggers keep your app subscribed to your users' integration events and Tools perform actions in those integrations.


If you’re ready to get started,[sign up for Paragon](https://dashboard.useparagon.com/signup) on a free trial and check out the ActionKit docs to start building.
