---
schema_version: "1.0.0"
document_id: "788dd1488015aa198f9346aee006fca3144e9d62c53cc98ddce165e2e17ced42"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-hubspot-crm-automation"
published_at: "2026-05-17T00:30:21+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:d3b16f4f25ec777ddf0e47400d97c71deaf34e1c2423019473c9cb8cbb4b19d1"
---

# OpenClaw HubSpot Automation: Let Your Agent Run Your CRM

## Step-by-Step Setup


1


#### Add your HubSpot token to OpenClaw secrets


In your OpenClaw workspace, store your HubSpot token as a secret. If you're on Blink Claw, go to your agent's Settings → Secrets and add` HUBSPOT_API_TOKEN` with your private app token. OpenClaw agents access secrets through environment variables — your HEARTBEAT.md can reference` $HUBSPOT_API_TOKEN` without exposing the value.


2


#### Install the HubSpot skill (optional but recommended)


OpenClaw's skill ecosystem includes a community-maintained` hubspot-api` skill. Run:


```text
/skill install hubspot-api


```


This gives your agent pre-built functions for common HubSpot operations: searching contacts, reading deals, adding notes, and updating deal stages. You can also skip the skill and call the HubSpot API directly via` curl` or Python — the skill just makes the prompts shorter.


3


#### Write your HEARTBEAT.md file


HEARTBEAT.md is where you define scheduled tasks. OpenClaw reads this file and runs each task at the specified time. Create or update this file in your agent's workspace. A starter configuration for sales automation is in the next section.


4


#### Test with a manual trigger


Before relying on scheduled runs, test your HubSpot connection manually. Message your agent:


> "Pull the last 5 open deals from HubSpot and tell me each deal name, stage, and the date of last activity."


If your token is configured correctly, you'll get a response within seconds. If you see an authentication error, double-check your scopes — the most common issue is missing` crm.objects.deals.read` .


5


#### Deploy and let it run


Once your HEARTBEAT.md is configured and the manual test passes, your agent will start running the scheduled tasks automatically. If you're self-hosting OpenClaw, it needs to stay running — which means your laptop needs to stay on. If you're using[Blink Claw](https://blink.new/claw) , it runs 24/7 in the cloud with no infrastructure to manage.


## HEARTBEAT.md Configuration for Sales Automation


HEARTBEAT.md is a Markdown file your agent reads to understand what to do and when. Here's a production-ready configuration for HubSpot sales automation:


```text
## Daily Pipeline Briefing (8:00 AM weekdays)
-   Pull all open HubSpot deals
-   For each deal: get deal name, stage, amount, and date of last activity
-   Flag any deal with no activity in 5+ days
-   Calculate total pipeline value by stage
-   Send briefing to Telegram with deal names, stage, last activity date, and a summary line
-   Format: daily-pipeline-brief


## New Lead Research (trigger: new HubSpot contact created)
-   When a new contact is added to HubSpot (check every 30 minutes)
-   Look up the contact's company website and summarize what they do in 2 sentences
-   Search for any recent news about the company (last 90 days)
-   Check if the company is on LinkedIn and note employee count
-   Write a research summary to the contact's HubSpot notes field
-   Tag the contact as "researched"


## Post-Call Follow-Up (trigger: call logged in HubSpot)
-   When a call activity is logged in HubSpot (check hourly)
-   Read the call notes
-   Draft a follow-up email: thank for time, summarize next steps, include one relevant resource
-   Save draft to Gmail Drafts folder
-   Add note to HubSpot deal: "Follow-up draft ready in Gmail Drafts"


## Weekly Deal Health Check (Monday 7:00 AM)
-   Pull all open deals
-   Flag: no activity in 7+ days
-   Flag: no next step logged in deal notes
-   Flag: amount field empty
-   Flag: close date in the past with no status update
-   Send report to Telegram with prioritized action list
```


Keep each HEARTBEAT.md task focused on one outcome. An agent that tries to do five things in one scheduled run is harder to debug when something goes wrong. Split complex workflows into separate tasks.


## Use Case Deep Dives


### Morning Pipeline Summary


Every weekday at 8am, your agent connects to HubSpot, pulls your open pipeline, and sends a Telegram message before you've opened your laptop. The message includes:


- Total number of open deals and combined pipeline value
- Deals with no activity in the last 5 days (the ones at risk)
- Deals with activity yesterday (momentum to follow up on)
- One-line summary per flagged deal


You arrive at your desk already knowing what to focus on. No CRM login required.


### Auto-Research New Leads


When a lead fills out a HubSpot form and gets added as a contact, your agent has 30 minutes to do what a good SDR does manually: look up the company, find their recent news, check headcount, and write a brief summary.


By the time you open the contact record, there's already a note: "Acme Corp — $12M Series B in March 2026, 45 employees, building ERP for logistics. CEO recently spoke at SaaStr about pipeline efficiency." That context changes your first conversation.


### Follow-Up Emails After Calls


You log a call in HubSpot. Your next task is already waiting in Gmail Drafts. The agent read your call notes, pulled the deal context, and wrote a follow-up that names the specific next steps you agreed on. You review, adjust the tone if needed, and send.


The average sales rep sends fewer than 30% of planned follow-ups within 24 hours, according to multiple industry studies. Your agent sends 100% — or at least gets a draft ready within the hour.


### Weekly Deal Health Check


Monday morning, before the pipeline review, your agent sends you a ranked list of problem deals. The ranking uses simple rules:


- Deals with no activity in 7+ days → high priority
- Deals with a close date that has passed → high priority
- Deals missing a next step → medium priority
- Deals with no amount logged → low priority


This takes a human 20-30 minutes to build manually from CRM reports. Your agent does it in seconds and delivers it to your phone.


Four key OpenClaw HubSpot automation workflows that replace hours of manual CRM work


Blink


*Four key OpenClaw HubSpot automation workflows that replace hours of manual CRM work*


## Running 24/7 with Blink Claw


Self-hosting OpenClaw works, but it comes with a catch: your agent only runs when your machine is on. A scheduled 8am briefing doesn't fire if your laptop is closed. A 30-minute lead research check doesn't run during the weekend.


[Blink Claw](https://blink.new/claw) solves this. Your agent runs in the cloud — no Docker, no VPS, no server management. $22/mo all-in, LLM costs included via a 200+ model router. Your agent runs 24/7 across 30+ data center regions globally. You message it from Telegram, Discord, or Slack.


For HubSpot automation specifically, this matters because:


- New leads can arrive at any time — you want research done before the next business day
- Scheduled briefings need to fire reliably, not "whenever the laptop is on"
- Deal health checks are most useful when they run before your Monday meeting, not when you remember to trigger them manually


Blink Claw handles security patches automatically. You never need to track OpenClaw CVEs or update Docker images. The agent is always running the latest stable version.


OpenClaw running HubSpot automation 24/7 with Blink Claw — your sales operations never sleep


Blink


*OpenClaw running HubSpot automation 24/7 with Blink Claw — your sales operations never sleep*


## Frequently Asked Questions


Yes. HubSpot launched an official MCP server at` https://mcp.hubspot.com` in early 2026. OpenClaw can connect to it using OAuth authentication. This gives your agent natural-language access to all CRM objects — contacts, deals, companies, tickets, activities — without you writing custom API integration code. The Private App token method (described in this guide) still works and is often simpler for scheduled automation tasks.


Any HubSpot plan works, including the free CRM tier. The Private App API access and MCP server are available on all tiers. Some features like sequences and forecasting tools require Sales Hub Starter ($15/mo) or higher, but the core automation tasks in this guide — deal monitoring, contact notes, call follow-ups — work with the free CRM.


OpenClaw can both read and write to HubSpot. With the right Private App scopes enabled, your agent can create and update contacts, add notes to deals, update deal stages, log call activities, and write to custom properties. Write operations require the corresponding write scopes — for example,` crm.objects.notes.write` to add notes.


Never put your HubSpot token in HEARTBEAT.md, IDENTITY.md, or any file your agent reads as context. Store it as an environment variable (` HUBSPOT_API_TOKEN` ) in your OpenClaw secrets — Blink Claw provides a secrets management UI for this. Your HEARTBEAT.md references the variable name, not the value. The token is never logged or exposed in agent responses.


HubSpot allows 100 API requests per 10 seconds on most plans (higher on Enterprise). A well-written HEARTBEAT.md that batches requests — pulling all deals in one call rather than looping per deal — stays well within this limit. For high-volume pipelines (500+ deals), consider adding a delay between operations or using HubSpot's search API with filters to reduce result sets. OpenClaw agents handle HTTP 429 errors gracefully by default and will retry after the rate limit window resets.


Yes. Message your agent on Telegram: "Pull the pipeline and tell me which deals need attention." Your agent will run the same HubSpot queries as the scheduled version, but on demand. This is useful for ad-hoc requests before a client call or when you want a current status outside of the normal briefing window. Blink Claw connects to Telegram, Discord, and Slack out of the box.


HubSpot's native AI (Breeze) operates inside the HubSpot interface and follows HubSpot's predefined workflow templates. OpenClaw is a general-purpose agent that can cross multiple systems — it can research a lead on LinkedIn, check their company's recent press, query your internal notes, and then write a summary to HubSpot in a single task. It's not limited to what HubSpot's workflow builder supports. You write the logic in plain English in HEARTBEAT.md, and the agent executes it.
