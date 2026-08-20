---
schema_version: "1.0.0"
document_id: "8b951bab07088892e6fcbe800c030d92220cbb298d9c9d48fa7d827a8dfc54db"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-6c10cddc543b"
canonical_url: "https://www.pagerduty.com/blog/automation/get-the-context-your-alerts-are-missing-with-event-enrichment/"
published_at: "2026-08-05T11:00:15+00:00"
first_seen_at: "2026-08-05T13:12:01.402416+00:00"
fetched_at: "2026-08-05T13:12:02.429109+00:00"
content_hash: "sha256:4d69fe868e5aef45e9d7fc5e181b080ed2dc0a4da184fab5069dbdba5fc14c20"
---

# Get the Context Your Alerts Are Missing with Event Enrichment by Ariel Russo

This blog post is part of PagerDuty’s ongoing series on how we’re helping customers navigate their journey towards autonomous operations. Read on to learn about how Event Enrichment builds towards this vision.


---


Every on-call engineer knows the drill. An alert fires. It tells you


*something* is wrong, but not


*what it means* . Is this asset in maintenance? Which team owns it? Is it customer-facing? To find out, you’re off — tabbing between your CMDB, your monitoring tool, a spreadsheet someone maintains “temporarily,” and half a dozen Slack threads.


That gap between “an event happened” and “I understand what this event means” is where minutes turn into missed SLAs. Incoming events from monitoring tools typically lack context around infrastructure status, service structure and ownership, and business impact — so responders are left to fill in the gaps manually. The result: slower MTTR, frustrated on-call engineers, and SLAs that quietly slip.


**Event Enrichment is built to close that gap.**


#### **Events Without Context**


Monitoring tools are great at detecting that something changed. They’re not so great at telling you why it matters. They don’t natively know that the server throwing errors is in a scheduled maintenance window, or which team actually owns the failing service, or that this particular alert affects a revenue-critical customer flow.


That missing context doesn’t disappear — it just gets pushed onto the human being holding the pager, at 2 am, under pressure.


#### **Meet Event Enrichment**


Event Enrichment, now available for Early Access, automatically pulls context from across your systems (like CMDB data) directly into your events. Account-level defaults, service-specific overrides, and team-based access give you flexible control over exactly what gets added and where.


The payoff isn’t just cleaner alerts. In the future, it will give tools like SRE Agent a full, 360-degree view of your environment, so responders spend less time hunting for context and more time actually resolving incidents.


#### **How It Works, Under the Hood**


##### **1. PagerDuty’s contextual data layer**


Monitoring tools typically lack context from external systems to which they don’t natively have access. PagerDuty’s contextual data layer solves this by pulling data from systems like ServiceNow CMDB and attaching it directly to events — so third-party latency never delays a notification.


Setup starts with


**schemas** : think of each one as your own cached mini-database, with one schema per data source. Load data through a direct ServiceNow integration or CSV-formatted API calls, with more direct integrations on the way. ServiceNow syncs every 12 hours, plus real-time updates for select fields like maintenance status — so you can manage maintenance windows per-event instead of pausing an entire service.


##### **2. Enrichment Rules**


Once your CDP is configured, you create


**Event Enrichments** — the rules that determine which data gets added to which events. Enrichments can be owned by specific teams, or configured as the account “Default” (one per account) when you need standard enrichment data applied regardless of service ownership.


Conditions let you control exactly which events get enriched. You select a schema and the event field it should match against, then define where in Custom Details the matched data lands.


##### **3. Extract and Compose**


Enrichment rules can do more than attach data — they can also transform it. Using


**Extract** , you can pull a specific value out of a field (for example, using regex to extract a PagerDuty service ID into a standard location so Dynamic Routing can use it). Using


**Compose** , you can build new values out of existing ones. And


**Connect Mode** lets you build full logic trees that recombine rules, so you’re not duplicating logic across the board.


##### **4. Powering Orchestration**


Here’s the real unlock: enrichment happens


*before* orchestration. That means Orchestration can act on data that was never in the original event.


If ServiceNow says an asset is in maintenance, the alert gets suppressed automatically — no noise, no page. Events route automatically to the owning team based on enriched data, so you can finally retire the giant hard-coded routing table.


##### **5. Seeing It in Action**


Trigger an event, and you’ll see it enriched in real time — for example, with


enriched_infra


and


enriched_service


fields populated straight from the contextual data layer. That enriched data is what suppresses the noisy, in-maintenance alert and routes the real one to the correct service, even when the monitoring tool itself didn’t have the right service mapping.


#### **The Bigger Picture: Fueling Autonomous Operations**


Event Enrichment standardizes and enriches inbound signals to unlock better correlation, routing, and automation. In turn, that will give features like SRE Agent a clearer understanding of service and infrastructure relationships — and a 360-degree view it can use to make smarter recommendations for resolving incidents.


That’s the data flywheel effect: every incident becomes organizational intelligence that helps prevent the next one. Data at scale, intelligent automation, and a system that gets smarter over time — that’s how PagerDuty delivers on Autonomous Operations.


#### **See It for Yourself**


Context shouldn’t be something your team has to go find manually, incident after incident. Event Enrichment brings it to you, automatically, before it’s even needed.


**Ready to see it in action?** Watch the


full product demo


and sign up for


[Early Access](https://www.pagerduty.com/early-access/) .
