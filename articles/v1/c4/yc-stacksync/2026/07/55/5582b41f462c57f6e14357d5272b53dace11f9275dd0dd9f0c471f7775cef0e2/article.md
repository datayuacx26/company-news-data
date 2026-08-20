---
schema_version: "1.0.0"
document_id: "5582b41f462c57f6e14357d5272b53dace11f9275dd0dd9f0c471f7775cef0e2"
company_key: "yc-stacksync"
company: "Stacksync"
source_id: "yc-stacksync-rss-96a17fe536ae"
canonical_url: "https://www.stacksync.com/blog/how-to-sync-zendesk-with-salesforce"
published_at: "2026-07-21T14:00:00+00:00"
first_seen_at: "2026-07-21T19:30:15.029798+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:57147a083a5447f995dd130229c2c3740001219da5dd406185973928a5b99c73"
---

# How to Sync Zendesk with Salesforce: A Two-Way Integration Guide

Support lives in[Zendesk](https://www.stacksync.com/connectors/zendesk) and sales lives in[Salesforce](https://www.stacksync.com/connectors/salesforce) , and they are looking at the same customers from two sides. An agent working a ticket needs the account owner, the renewal date, and the plan tier that sit in Salesforce. An account executive needs to know their customer has three open tickets before they push a renewal. When the two systems are not synced, each team keeps its own half of the picture and fills the gaps by hand.


The fix is a two-way sync between the two. Match the records once, map the fields that both teams rely on, and let a sync layer keep them in step so a change in either tool reaches the other in real time. This guide covers what to sync, how the two-way sync works, and how to set it up without creating duplicates or update loops.


Done properly, the sync keeps three things in step: contacts and accounts, ticket status and case fields, and the single view of the customer both teams work from. Get those right and support and sales stop re-keying and re-checking each other's tools.


## Why Zendesk and Salesforce drift apart


The two systems drift because they are edited independently all day. Support creates and updates tickets in Zendesk; sales edits accounts and contacts in Salesforce. Nothing connects the two edits, so the same customer ends up with a slightly different story in each tool: a contact with one email in Zendesk and another in Salesforce, a ticket marked solved that the linked case still shows open, an account whose owner changed last week that support never saw.


Teams paper over it with manual work. Someone exports a report and reconciles it, an agent alt-tabs to the CRM on every ticket, or a one-way connector copies contacts across on a schedule and quietly duplicates half of them. None of it holds up under volume, and the cost is real: a customer told something out of date, or a renewal worked without knowing about an escalation.


Without a sync What it causes


Contacts edited in both tools Duplicate and conflicting records


Ticket status not reflected in the case Sales works from a stale support picture


Account changes never reach Zendesk Agents miss owner, tier, and renewal context


Manual reconciliation and tab-switching Slow, error-prone, and never current


What happens when Zendesk and Salesforce are edited independently with nothing keeping them in step.


## What to sync: contacts, accounts, tickets, and cases


Start by deciding which objects and fields belong in the sync. The mapping is straightforward because the two systems model the same ideas: a Zendesk user is a Salesforce contact, a Zendesk organization is an account, and a Zendesk ticket is a case. Within each, you sync the fields both teams act on and leave internal-only fields local.


Zendesk Salesforce What syncs


User Contact Name, email, phone, matched on email


Organization Account Company, domain, tier; matched on domain


Ticket Case Status, priority, subject, owner, both ways


Custom fields Custom fields Plan, renewal date, region, mapped per field


A typical Zendesk to Salesforce object and field map. Match keys keep records paired; custom fields map one to one.


The match keys matter as much as the fields. Email pairs a Zendesk user with the right Salesforce contact, and company domain pairs an organization with the right account, so updates land on the existing record instead of spawning a duplicate. Set those before anything else.


## How the two-way sync works


With the objects mapped, a sync engine sits between Zendesk and Salesforce and keeps the paired records in step. It matches records on the keys you set, maps the fields across the two schemas, and applies each change to the other side. Neither team logs into the other's tool; both keep working where they already work.


Each tool stays where it is. The engine matches records, maps fields, and keeps both sides in step both ways.


Three details make it trustworthy. **Identity match** pairs each Zendesk record with the right Salesforce one on email and domain, so nothing duplicates. **Field mapping** translates status, priority, and custom fields between the two shapes. **Origin tracking** tags every write as coming from the sync, so when the other system reports the change back it is recognized and not resent, which is what stops a single edit from looping between the tools.


## A change crossing both ways


It helps to follow one change through the sync. Say an agent moves a ticket to solved in Zendesk. The engine picks up the change, finds the linked Salesforce case, and updates it, along with any mapped fields, then confirms without writing an echo back to Zendesk. The same path runs in reverse when sales changes an account owner or renewal date and it needs to appear on the ticket.


One change crossing the sync. The origin tag stops the acknowledgement from returning as a new change.


Because the engine reacts to changes in real time rather than on a schedule, the update lands on the other side in seconds. Support and sales are never more than a moment out of step, and neither team has to check the other's tool to trust what they see.


## Setting up the Zendesk and Salesforce sync


The setup is a short sequence, and after it the sync runs unattended.


1. 01


Connect both systems


Authorize Zendesk and Salesforce to the sync layer; no login into either tool from the other.


2. 02


Pick the match keys


Use email for contacts and company domain for accounts so records pair instead of duplicating.


3. 03


Map the objects and fields


Map users to contacts, organizations to accounts, and tickets to cases, including the custom fields both teams use.


4. 04


Set direction and conflict rules


Decide which fields are two-way and which are one-way, and how conflicts resolve when both sides edit the same field.


5. 05


Run an initial match, then turn on real-time sync


Pair existing records first, then enable the two-way sync with origin tracking so updates flow both ways without looping.


This same pattern is how any pair joins a Zendesk integration layer. If you are connecting more than these two, see[an enterprise-grade iPaaS for Zendesk](https://www.stacksync.com/blog/enterprise-grade-ipaas-for-zendesk) , and if you want agents to see this synced context without leaving a ticket, see[full customer context in the helpdesk](https://www.stacksync.com/blog/full-customer-context-in-helpdesk-no-tab-switching) .


## Bringing it together


Syncing Zendesk and Salesforce is about giving support and sales one current view of the same customer. Match contacts and accounts on a stable key so nothing duplicates, map ticket status and case fields both ways, and use origin tracking so updates do not loop. Then a ticket resolved in Zendesk and an account changed in Salesforce both show up where the other team works, in seconds.


To connect[Zendesk](https://www.stacksync.com/connectors/zendesk) and[Salesforce](https://www.stacksync.com/connectors/salesforce) with real-time two-way sync,[see how two-way sync works](https://www.stacksync.com/two-way-sync) or[book a demo](https://www.stacksync.com/book-a-demo) . To route and escalate tickets on that live account data once it is synced, see[auto-routing support tickets](https://www.stacksync.com/blog/auto-route-support-tickets-region-type-crm) .
