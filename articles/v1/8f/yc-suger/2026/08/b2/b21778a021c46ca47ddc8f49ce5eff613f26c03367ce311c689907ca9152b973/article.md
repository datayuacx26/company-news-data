---
schema_version: "1.0.0"
document_id: "b21778a021c46ca47ddc8f49ce5eff613f26c03367ce311c689907ca9152b973"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/connect-crm-to-cloud-marketplace-data/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T02:43:09.652657+00:00"
fetched_at: "2026-08-13T02:43:11.361615+00:00"
content_hash: "sha256:3f704abf25882d73d08f0e9728454edfd611cb84c3b7036dd4208c11a92dd174"
---

# Connecting Your CRM to Cloud Marketplace Data

*Your CRM is where the revenue conversation happens, and it has no idea a marketplace exists. Offers, entitlements, metered usage and payouts sit in consoles the sales team cannot open. Closing that gap is a data-modelling problem before it is an integration problem — and the modelling is the part teams skip.*


---


The symptom is always the same. An account executive asks whether a renewal is safe and nobody can answer without opening a second console. Finance asks why bookings and payouts disagree and the reconciliation happens in a spreadsheet. A partner manager registers a deal that has already been transacted through a reseller.


None of that is a missing API. All of it is marketplace records sitting in different systems from the records they describe.


Here is what each marketplace actually emits, the four patterns teams use to move it, and the decision that makes the difference: which records belong in the CRM at all.


---


## **What does connecting a CRM to marketplace data mean?**


**It means keeping the CRM’s account, opportunity and contract records in agreement with the offers, agreements and usage that exist inside AWS, Microsoft and Google Cloud.** Those systems hold the authoritative commercial record once a deal transacts through a marketplace. Your CRM holds everything before and around it.


The connection has to carry four kinds of record, and they have different owners and different failure modes:


Record Lives with The CRM needs it because Failure looks like


Offer Deal desk It is the quote the AE is waiting on Pipeline shows a deal nobody sent


Agreement / entitlement The marketplace It is the signed contract Renewals surprise everybody


Metered usage Your product It drives overage and expansion Overage invoices nobody predicted


Payout Finance Bookings are not cash Revenue never reconciles


Most integrations move the first two well and stop. That is why the reconciliation spreadsheet survives.


---


## **What each marketplace gives you to work with**


**AWS emits events; Microsoft calls your webhook; Google Cloud publishes to Pub/Sub.** All three are push, and all three describe subscription state rather than sales state.


**AWS Marketplace** provides two topics for SaaS products. The` aws-mp-subscription-notification` topic sends` subscribe-success` ,` subscribe-fail` ,` unsubscribe-pending` and` unsubscribe-success` . The` aws-mp-entitlement-notification` topic sends a single` entitlement-updated` action for new contracts, upgrades, renewals and expiry alike — AWS is explicit that “regardless of the action (new, upgrade, renewal, or expired), the message is the same,” and that “a subsequent call to` GetEntitlement` is required to discover the content of the update.”


Two operational details matter for a CRM sync.` unsubscribe-pending` gives you “about one hour” to send final metering records before cancellation completes — so a CRM that closes the opportunity on that event closes it too early. And AWS has stated that SNS notifications for SaaS products “are being replaced with Amazon EventBridge notifications,” with existing SNS integrations continuing to work. If you are building the pipe now, build it against the destination.


**Microsoft** requires a connection webhook for transactable SaaS offers, described as the only way you are notified about updates to customers’ subscriptions. Separately, for pre-purchase leads, Microsoft supports sending directly to Dynamics 365, Marketo and Salesforce, plus an Azure table or an HTTPS endpoint configured through Power Automate. Note that this is the *leads* path, not the subscription path — a lead is an interested visitor, not a customer, and putting the two on one integration is a common early mistake.


**Google Cloud Marketplace** uses the Partner Procurement API for purchase information and Pub/Sub for notifications, with the Service Control API for usage reporting on consumption-priced products.


---


## **The four patterns, and what each costs**


### Pattern 1: Manual export


Someone downloads a report and pastes it in. It works at ten agreements and stops working somewhere around forty, usually when the person who does it takes a holiday. The cost is not the labour; it is that nobody trusts a number whose freshness they cannot see.


### Pattern 2: Point-to-point per marketplace


One integration per marketplace, each writing to the CRM in its own vocabulary. This is where most in-house builds land, and it looks reasonable until the third marketplace. The real cost is that each cloud’s model leaks into your CRM schema — you end up with an “AWS agreement ID” field, a “Microsoft subscription ID” field and a “GCP entitlement name” field on the same object, and every report has to know which one to read.


### Pattern 3: A canonical layer in the middle


One normalised model — product, offer, entitlement, usage, payout — that every marketplace maps into, and one integration from that model to the CRM. The CRM gets one field for the contract, whichever cloud it came from.


This is the pattern that survives a fourth marketplace, and the reason is boring: the mapping problem is solved once per cloud instead of once per cloud per destination. When the data warehouse and the billing system want the same records, they read the same model.[Offer vs entitlement](https://www.suger.io/resources/blog/offer-vs-entitlement/) covers the two objects that model has to get right.


### Pattern 4: Reverse sync


The CRM writes back — an AE creates an offer from an opportunity without leaving Salesforce. This is the one that changes behaviour rather than reporting, and it is also the one with real blast radius: a bad write creates a real commercial document. Worth doing, worth doing last, and worth restricting to a role rather than everybody with a login.


---


## **Which records actually belong in the CRM**


The instinct is to sync everything. Resist it. A CRM is a system for managing a sales conversation, not a data warehouse, and every field you add is a field someone has to interpret.


**Sync into the CRM:**


- The agreement, as a contract record on the account — with start date, end date, and committed value.
- Offer status, so the AE can see whether the thing they are waiting on has been sent, viewed or accepted.
- Renewal date, on the object your renewals process actually reads.
- A pointer — the marketplace identifier — so anybody can get to the authoritative record in one click.


**Leave out:**


- Raw metering records. They are high volume, they change by the hour, and no salesperson makes a decision from one. Aggregate them somewhere else and sync a rollup if you need it.
- Payout detail. Finance needs it; it does not belong on an opportunity.[Marketplace revenue reconciliation](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) covers where that work should sit.
- Every offer revision. Sync the current state, keep the history where the history is authoritative.


The test for any field: name the person who makes a different decision because it is there. If you can’t, it is clutter that will be stale within a quarter.


---


## **Where these integrations break**


**Matching accounts.** The marketplace knows a buyer by an AWS account ID or an Azure tenant ID. Your CRM knows them by company name and domain. Nothing in either record reliably matches the other, and a subsidiary buying on a parent’s account breaks the guess entirely. Decide the matching rule deliberately, and decide what happens when it fails — an unmatched agreement queued for a human is far better than one silently attached to the wrong account.


**Treating events as state.** Events arrive out of order, get retried, and occasionally arrive twice. An integration that applies each message as an instruction rather than reconciling against the current entitlement will eventually record a cancellation after a renewal.[When a buyer pays but provisioning never happens](https://www.suger.io/resources/blog/marketplace-provisioning-failures/) covers the same failure on the product side.


**Syncing leads into the pipeline.** Marketplace leads are interested visitors. Dropping them in as opportunities inflates the pipeline and teaches sales to distrust the source within a quarter.


**One-way assumptions in a two-way world.** If a deal desk can amend an offer in the marketplace console, the CRM’s copy is wrong the moment they do. Either the console is off-limits or the sync is genuinely bidirectional; “mostly one-way” is the state that produces disagreements nobody can explain.


---


## **Frequently asked questions**


**How do I get AWS Marketplace data into Salesforce?** Subscribe to the AWS SaaS notification topics, resolve each event to the current entitlement with a` GetEntitlement` call, normalise it, and write the agreement to the account. Do not write raw metering records into the CRM.


**Does AWS still use SNS for marketplace notifications?** AWS has stated that SNS notifications for SaaS products are being replaced with Amazon EventBridge notifications, and that existing SNS integrations continue to work. New builds should target EventBridge.


**Which CRMs does Microsoft support for marketplace leads?** Microsoft supports Dynamics 365, Marketo and Salesforce for customer leads, plus an Azure table or an HTTPS endpoint configured with Power Automate. Subscription events are a separate webhook.


**Should marketplace usage data go into the CRM?** No, as a rule. Metering is high volume and changes hourly. Aggregate it in a warehouse or billing system and sync a rollup only if a salesperson makes a decision from it.


**What is the hardest part of a marketplace-to-CRM sync?** Account matching. The marketplace identifies a buyer by cloud account or tenant ID; your CRM identifies them by company. Define the matching rule, and queue failures for a human rather than guessing.


---


## **Takeaways**


- Four record types need to move: offers, agreements, metered usage and payouts. Most integrations carry the first two and leave finance with a spreadsheet.
- AWS pushes events on two SNS topics and is moving that to Amazon EventBridge. Microsoft calls a connection webhook. Google Cloud publishes to Pub/Sub.
- AWS’s entitlement topic sends one action value for every change, so the event is a prompt to re-read state, not the state itself.
- A canonical model in the middle beats one integration per marketplace as soon as there is a third marketplace.
- Sync the agreement, offer status, renewal date and a pointer. Leave raw metering and payout detail out of the CRM.
- Account matching is the hardest part, and an unmatched record queued for a human beats a confident wrong match.


---


Marketplace records only help the revenue team when they arrive as one shape rather than three. See how Suger’s[CRM and business system integrations](https://www.suger.io/platform/integrations/) map offers, agreements and usage from every marketplace onto the objects your team already works in.
