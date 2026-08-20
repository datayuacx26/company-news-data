---
schema_version: "1.0.0"
document_id: "67166dbf78f4c16a7ee56ccb3126dc4a989804d9363a5e2e1065c39d8d6a5967"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/integrations/architecting-crm-native-messaging-twilio-for-salesforce"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:37.657414+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:2d61dd5a881c5678be0326da7ef829164447be5154a2d7c3a6d09c0d8a265201"
---

# Architecting a CRM-Native Messaging Platform on Salesforce with Twilio

Time to read:


-
-
-
-
-


June 17, 2026


**Written by**[Devam Gupta](https://www.twilio.com/en-us/blog/authors/author.dgupta) Twilion


**Reviewed by**[Daniel Arturo Cañón Molina](https://www.twilio.com/en-us/blog/authors/author.dcanonmolina) Twilion


[Paul Kamp](https://www.twilio.com/en-us/blog/authors/author.pkamp) Twilion


---


## Copy of Salesforce_Twilio_CRM_Architecture


If you're evaluating whether to build messaging natively inside Salesforce or bolt on an external connector – or you're already on the platform and hitting governor limit, async, or multi-tenant walls – this post documents how[Twilio for Salesforce](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000EtEuBUAV) , or *TFS* , embeds a full messaging system inside the CRM, and the architectural decisions that got us there.


Most enterprise messaging integrations are connectors. They sit outside the CRM, receive a webhook, write a note, and call it done. The messages live in the messaging platform, the CRM records live in the CRM, and somewhere between them is a field mapping and eventual consistency.


But with[Twilio for Salesforce](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000EtEuBUAV) , we took a different path: build the messaging system *inside* the CRM. Every message is a native Salesforce record. Every piece of state – delivery status, consent, conversation history – is Salesforce data, queryable with standard reports, accessible to the same Flows and automation the rest of the business already runs on.


As the technical architect of Twilio for Salesforce – a managed package serving enterprise customers in *healthcare, sales operations, field services, and customer support* since 2020 – I'll walk through the architectural challenges we encountered while building and how we solved them with Platform Event decoupling, async self-chaining, consent as a first-class data model, and multi-tenant reliability.


## Background


Twilio for Salesforce is a managed package listed on the Salesforce AppExchange. It embeds[Twilio's Messaging API](https://www.twilio.com/docs/messaging/api) ,[Studio](https://www.twilio.com/docs/studio) , and[TaskRouter](https://www.twilio.com/docs/taskrouter) natively into Salesforce, serving enterprise customers across healthcare, sales operations, field services, and customer support.


The system has to handle one-to-one conversational messaging, bulk campaign sends to hundreds of thousands of recipients, inbound webhook reliability, consent management, and delivery status tracking. It does this all within Salesforce's governor limits, across many organizations with different permission models and automation configurations.


### 1. The core architectural decision: where does the message record live ?


In a connector model, message records live in the messaging platform and are mirrored into the CRM. Every report or workflow that involves message data crosses an API boundary or depends on a sync job that may be minutes behind.


In a CRM-native model, **the CRM is the system of record** . Twilio's Messaging API is the transport layer – it carries the message and returns a status. The message record in Salesforce is the source of truth for everything else.


This has one specific consequence: the webhook writes into Salesforce, not the other way around. When an inbound SMS arrives or a message status changes, Twilio's messaging platform fires a webhook to a registered Salesforce REST endpoint. That endpoint creates or updates a record.


### 2. The data model


The core data model has six objects:


- **Message record** — individual SMS (inbound or outbound). Carries polymorphic lookups to Contact, Lead, Account, and a generic parent ID field for custom objects.
- **Campaign send record** — bulk send job. Parent for batch campaign messages.
- **Consent record** — opt-in/out state per phone number per keyword. ****
- **Standalone** — no foreign key to Contact.
- **Configuration setting** — hierarchy custom setting storing Twilio credentials at org and user level.
- **Service registry** — maps Twilio Messaging Service SIDs to Teams.
- **Team / Team membership** — shared inbox group. Maps users to messaging services.


The message record is the center. A` direction` field (` inbound` /` outbound` ) on every record drives conversation thread rendering – sorting by created date with direction gives a threaded view with no external state required.


The polymorphic lookup design – Contact, Lead, Account, plus a generic parent ID field – is exists because custom object support was retrofitted in a later version. In a first-generation managed package (1GP) managed package, schema changes are additive only.


### 3. The outbound send path: record-first, send async


Outbound messages follow a deliberate two-step pattern: **insert the record first, then send via Twilio's API in an async job.**


The reason is Salesforce's execution model.[A DML operation](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_dml_section.htm) and an outbound HTTP callout cannot occur in the same synchronous transaction. The trigger inserts the message record synchronously. A Queueable job runs after the transaction commits, makes the Twilio API callout, and updates the record with the external message SID and status.


This pattern has a secondary benefit: the message record exists before the API call succeeds. If the Queueable fails, the record is still there.


Here is the simplified pattern:


Copy code


```text
// Trigger inserts record, enqueues send job
trigger MessageTrigger on Message__c (after insert) {
List<Id> messageIds = new List<Id>();
for (Message__c msg : Trigger.new) {
if (msg.Direction__c == 'outbound') {
messageIds.add(msg.Id);
}
}
if (!messageIds.isEmpty()) {
System.enqueueJob(new MessageSendQueueable(messageIds));
}
}


// Queueable makes the Twilio API callout
public class MessageSendQueueable implements Queueable, Database.AllowsCallouts {
private List<Id> messageIds;


public MessageSendQueueable(List<Id> ids) {
this.messageIds = ids;
}


public void execute(QueueableContext ctx) {
List<Message__c> messages = [
SELECT Id, Body__c, To_Number__c, From_Number__c
FROM Message__c
WHERE Id IN :messageIds
];
for (Message__c msg : messages) {
// Call Twilio Messaging API
TwilioApiResponse resp = TwilioClient.sendMessage(msg);
msg.External_Id__c = resp.sid;
msg.Status__c = resp.status;
}
Data.modify(messages, true, true, false); // CRUD + FLS enforced
}
}
```


**The callout limit problem at bulk scale:** Salesforce allows 100 HTTP[callouts](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm) per Queueable execution so bulk campaigns sending more messages cannot be processed in one job. The solution is self-chaining .The job tracks callout count and chains a new Queueable with remaining IDs when the limit approaches.


Copy code


```text
// Self-chaining pattern — remaining messages passed to a new job
if (calloutCount >= 95 && index < messages.size()) {
List<Id> remaining = new List<Id>();
for (Integer i = index; i < messages.size(); i++) {
remaining.add(messages[i].Id);
}
System.enqueueJob(new MessageSendQueueable(remaining));
break;
}
```


**Fallback scheduler:** When the Salesforce async job queue is full – which happens when customer automation is also queuing jobs – enqueue fails. A fallback scheduler fires the remaining send 20 seconds later as a scheduled job.


### 4. The inbound path: Platform Events as the decoupling layer


The entry point is a Salesforce REST endpoint that ships with the managed package and is registered as the Twilio webhook URL during initial configuration in the Twilio Configuration tab. When a message arrives, Twilio fires an HTTP POST to this endpoint.


The naive approach would have been to insert a message record directly in the handler. The problem: the handler would need to respond quickly so it wouldn’t exceed Twilio’s webhook timeout and create duplicate records.


Twilio for Salesforce uses a **Salesforce Platform Event** as a decoupling layer:


1. Webhook handler validates the request (account ID, auth token, HMAC signature)


2. Publishes a lightweight Platform Event


3. Returns` HTTP 202` to Twilio


4. A separate async trigger processes the event – inserts the record, links to CRM objects, evaluates consent, dispatches notifications


Copy code


```text
@RestResource(urlMapping='/smswebhook/*')
global class TwilioSMSWebhook {
@HttpPost
global static void callBack() {
RestRequest req = RestContext.request;
Map<String, String> params = req.params;
String twilioSignature = req.headers.get('X-Twilio-Signature');
String endpoint = 'https://' + req.headers.get('Host') + '/services/apexrest' + req.requestURI;
TwilioRestClient client;
try {
client = TwilioAPI.getDefaultClientForWebhook();
} catch (Exception ex) {
RestContext.response.statusCode = 400;
return;
}
// Validate account SID and HMAC signature
if (client.getAccountSid() != params.get('AccountSid') ||
!client.validateRequest(twilioSignature, endpoint, params)) {
RestContext.response.statusCode = 403;
return;
}
// Publish Platform Event — structured fields, not a payload blob
Twilio_Message_Status__e evt = new Twilio_Message_Status__e(
Body__c             = params.get('Body'),
To__c               = params.get('To'),
From__c             = params.get('From'),
AccountSid__c       = params.get('AccountSid'),
MessageSid__c       = params.get('SmsSid'),
MessagingServiceSid__c = params.get('MessagingServiceSid'),
SmsStatus__c        = params.get('SmsStatus'),
ErrorCode__c        = params.get('ErrorCode')
);
EventBus.publish(evt);
RestContext.response.statusCode = 202;
}
}
```


Platform Event ordering is not guaranteed. An inbound message and its delivery status update can arrive out of sequence. The event handler resolves this with an explicit status precedence check –` delivered` beats` sent` ,` sent` beats` queued` – so out-of-order processing does not corrupt the status field.


### 5. CRM record linking: resolving a phone number in bulk


When an inbound message arrives, the system knows the sender's phone number. It needs to resolve that to a *Contact* , *Lead* , or *Account* . This resolution must happen in bulk – a trigger may process[200 inbound messages](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm) simultaneously – and cannot afford a query per message.


The pattern:


1. Collect all unique phone numbers from the full trigger batch


2. Issue **one SOQL (SOQL —**[Salesforce's structured query language, analogous to SQL](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) **) query per object type** filtering against the full set — not one-at-a-time


3. Build a map: phone number → matched record ID


4. For each message in the batch, populate the appropriate lookup field from the map


Copy code


```text
Set<String> phoneNumbers = new Set<String>();
for (Twilio_Message_Status__e evt : events) {
phoneNumbers.add(evt.From__c);
}
// One query per object — never N+1
Map<String, Id> contactsByPhone = new Map<String, Id>();
for (Contact c : [SELECT Id, Phone FROM Contact WHERE Phone IN :phoneNumbers]) {
contactsByPhone.put(c.Phone, c.Id);
}
Map<String, Id> leadsByPhone = new Map<String, Id>();
for (Lead l : [SELECT Id, Phone FROM Lead WHERE Phone IN :phoneNumbers]) {
leadsByPhone.put(l.Phone, l.Id);
}
// Assign lookup fields from maps — no per-record queries
for (Message__c msg : messagesToInsert) {
String phone = msg.From_Number__c;
if (contactsByPhone.containsKey(phone)) {
msg.Contact__c = contactsByPhone.get(phone);
} else if (leadsByPhone.containsKey(phone)) {
msg.Lead__c = leadsByPhone.get(phone);
}
}
```


For custom objects,[a configuration registry](https://help.salesforce.com/s/articleView?id=platform.custommetadatatypes_overview.htm&type=5) stores which object and field to query. The same bulk resolution logic applies – the query is dynamically built from the registered configuration. This is how the Twilio for Salesforce package can support healthcare patient records, recruiting candidate objects, and retail account objects with no code change.


### 6. Consent as a first-class data model


Most messaging integrations store consent as a boolean field on the Contact record. This is the wrong model:


-


A Contact[may have multiple phone numbers](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_contact.htm) — consent is per number, not per Contact


-


A boolean has no history — you cannot audit when consent was given or revoked


-


Checking consent for a bulk campaign means loading every Contact record


The model we chose: **consent is a separate object, keyed by phone number and keyword.**


Copy code


```text
Opt_In__c (Opt In record)
──────────────────────────────────
UniqueId__c                         ← composite hash of phone + keyword, used for upsert deduplication
Opt_In_Phone_Number_Unformatted__c  ← standalone — not a Contact lookup
Opt_In_Keyword__c                   ← lookup to keyword record ("STOP", "START", or campaign keyword)
Active__c                           ← true / false
Opt_In_Date_Time__c
Opt_Out_Date_Time__c
```


A bulk campaign consent check before sending is a single SOQL query against the consent object for all phone numbers in the batch:


Set<String> phoneNumbers = getAllCampaignPhoneNumbers(); // collected once


Copy code


```text
List<Opt_In__c> optOuts = [
SELECT Opt_In_Phone_Number_Unformatted__c
FROM Opt_In__c
WHERE Opt_In_Phone_Number_Unformatted__c IN :phoneNumbers
AND Active__c = false
];


Set<String> suppressedNumbers = new Set<String>();
for (Opt_In__c c : optOuts) {
suppressedNumbers.add(c.Opt_In_Phone_Number_Unformatted__c);
}


// Filter messages before send — not per-record
for (Message__c msg : outboundMessages) {
if (!suppressedNumbers.contains(msg.To_Number__c)) {
messagesToSend.add(msg);
}
}
```


[Opt-out processing from inbound Twilio keywords](https://help.twilio.com/articles/223134027-Twilio-support-for-opt-out-keywords-SMS-STOP-filtering-) ("STOP", "START") uses upsert


on the composite key – idempotent even if the same keyword is processed twice:


Copy code


```text
// keywordRecordId is the Id of the Opt_In_Keyword__c record for "STOP" or "START"
// buildUniqueHash concatenates the unformatted phone number + keyword record Id
Opt_In__c optInRecord = new Opt_In__c(
UniqueId__c                        = OptInService.buildUniqueHash(phoneNumber, keywordRecordId),
Opt_In_Phone_Number_Unformatted__c = phoneNumber,
Opt_In_Keyword__c                  = keywordRecordId,
Active__c                          = isOptIn,
Opt_In_Date_Time__c                = isOptIn ? System.now() : null,
Opt_Out_Date_Time__c               = isOptIn ? null : System.now()
);
upsert optInRecord Opt_In__c.UniqueId__c;
```


### 7. Status sync: two-path reliability


Delivery status follows two independent paths by design:


**Path 1 — Twilio status callback (fast):** When message status changes ( queued


→ sending


→ sent


→ delivered


/ failed


), Twilio fires a status callback webhook. The Platform Event pattern handles this identically to inbound messages : fast 202 acknowledgment, async record update.


**Path 2 — Scheduled polling fallback (reliable):** A scheduled Apex job runs on a configurable interval. It queries Twilio's API for messages sent since the last sync timestamp and reconciles status against existing records.


Two paths exist because webhooks fail in practice:


-


Salesforce orgs have maintenance windows, and webhooks delivered during downtime are silently dropped


-


High-volume bulk sends generate more status callbacks than can be processed synchronously


-


Network timeouts between Twilio and Salesforce happen under load


The polling job is idempotent : processing the same status update twice is harmless. A system that relies only on webhooks will have stale status on every interruption. The polling fallback degrades gracefully.


### 8. Multi-tenancy: what changes across hundreds of orgs


Permission model variability. With Twilio for Salesforce, our customers have many different profile and permission set configurations. All[DML](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_dml_section.htm) in TFS routes through a centralized data access framework with explicit CRUD and[FLS](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_perms_enforcing.htm) enforcement flags. When Salesforce's AppExchange Security Review identifies violations, the fixes are isolated to one layer and not scattered across hundreds of classes.


Copy code


```text
// Every DML goes through the framework — never raw insert/update
Data.create(messages, true, true, false);  // enforceCRUD=true, enforceFLS=true
Data.modify(messages, true, true, false);
Data.remove(recordIds, true, true, false);
```


**Trigger conflicts.** Customers have their own triggers on Contact and Lead. When TFS creates a new Contact from an inbound message, the customer's trigger fires too. The package exposes a bypass flag that customer triggers can check to skip logic that should not run during messaging operations.


**Governor limit pressure.** Customers wire Twilio For Salesforce Flow actions into complex automation. When a Flow fires in a context already under governor limit pressure, the async job queue competes with customer automation. The fallback scheduler handles the worst case where queuing fails, the send is deferred 20 seconds rather than dropped.


**Schema permanence.** In a 1GP managed package, schema changes are additive only. Objects and fields cannot be deleted after release. Every early data model decision is permanent — a constraint that does not exist in single-org development and shapes every architecture decision from day one.


## What the CRM-native model enables


Building messaging natively inside the CRM means the platform's own capabilities – reporting, automation, and AI orchestration – work on message data without any additional integration layer.


**Native reporting without ETL.** Every message is a Salesforce record. Managers report on delivery rates, response times, and campaign performance using standard Salesforce reports — no external BI tool, no data export.


**Zero-code automation.** The Twilio send method is exposed as a Salesforce Flow invocable action. Any Flow can trigger messaging without code: a lead status change, a case escalation, an appointment reminder.


**AI agent compatibility.** The Flow invocable action is already callable from Salesforce's Agentforce platform as an agent action . An AI agent can send messages as part of an automated workflow the moment Agentforce is configured in the org.


## Where the model breaks down


Although this architecture delivers native reporting, zero-code automation, and multi-tenant reliability, building inside the CRM means accepting the platform's constraints as your own – governor limits, async execution boundaries, and schema permanence are not abstractions you can work around, only design for.


**Async debugging is hard.** When a Queueable send job fails, error context is limited. Without deliberate instrumentation – logging the failing record ID, the Twilio API response, the exception – diagnosing a failed bulk send in production is guesswork.


**Chained job limits under extreme volume.** Under very high volume, self-chaining Queueables can hit Salesforce's[concurrent async job ceiling](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_queueing_jobs.htm) . Twilio for Salesforce’s fallback scheduler absorbs this, but it introduces visible latency.


**Schema permanence as a design tax.** Every early schema decision is permanent under[1 GP rule](https://developer.salesforce.com/docs/atlas.en-us.packagingGuide.meta/packagingGuide/packaging_intro.htm) s . The generic parent ID field in the current data model is a visible scar from retrofitting custom object support. It cannot be removed and future versions will continue to build alongside it.


## What's next


The patterns in this post – *Platform Event decoupling, async self-chaining, consent-first data model, two-path status sync* – are the core of how Twilio for Salesforce handles enterprise messaging at scale inside Salesforce.


If you came to this post evaluating whether to build messaging natively inside Salesforce or bolt on an external connector – or already hitting governor limit, async, or multi-tenant walls – hopefully the architecture above shows how the CRM-native approach handles both without stepping outside the platform. And if you like our approach, you can find the[Twilio for Salesforce Agentforce listing](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000EtEuBUAV) here.


If you are building on Twilio's messaging platform or exploring AppExchange ISV development, these resources are a useful starting point:


-


[Twilio Messaging API documentation](https://www.twilio.com/docs/messaging)


-


[Twilio Studio — visual flow builder](https://www.twilio.com/docs/studio)


-


[Salesforce Platform Events documentation](https://developer.salesforce.com/docs/atlas.en-us.platform_events.meta/platform_events/)


-


[AppExchange Security Review requirements](https://developer.salesforce.com/docs/atlas.en-us.packagingGuide.meta/packagingGuide/security_review_overview.htm)


---


*Devam Gupta is a Staff Engineer at Twilio and the technical architect of Twilio for Salesforce (TFS), a managed application on the Salesforce AppExchange serving enterprise customers in healthcare, sales operations, and field services. He focuses on CRM-native integration architecture, AppExchange ISV development, and enterprise messaging systems at scale. Connect with Devam on[LinkedIn](https://linkedin.com/in/devam-gupta-a0465693)*
