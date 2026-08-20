---
schema_version: "1.0.0"
document_id: "0880bcf2f188979dcc0a13c39254c13e3e27716ec01e6587581e3fa6dc331049"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-crm-automation"
published_at: "2026-06-01T01:24:59+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:0777ca948ff4dd6d1284b13d50eaaeec8568eccd9d379f11a1c04d5485610474"
---

# OpenClaw CRM Automation: Sync Your Agent With HubSpot and Salesforce

## Salesforce integration


The setup pattern is the same. You need a Salesforce Connected App with OAuth credentials:


```text
## CRM
crm_tool: salesforce
salesforce_instance_url: https://yourorg.salesforce.com
salesforce_client_id: [Connected App consumer key]
salesforce_client_secret: [Connected App consumer secret]
salesforce_username: [your Salesforce username]
salesforce_password: [your password + security token concatenated]


## Salesforce Automation Rules
-   Create an Opportunity when a new B2B lead qualifies
-   Auto-populate Stage field from email context
-   Add call transcript summary as a Note on the Account record
-   Sync meeting outcomes to Salesforce Activities
-   Query pipeline for weekly review every Monday at 9am
```


The agent works with standard Opportunity pipelines. When a call transcript comes in, it reads the key points and adds them to the right Account record. You stop copy-pasting call notes into Salesforce after every meeting.


One configuration detail to get right: the Connected App needs the Salesforce API scope enabled. Without it, the agent can read CRM records but can't write. That's the most common reason a Salesforce integration appears to work but doesn't update anything.


The weekly pipeline query is worth setting up from day one. Every Monday morning, the agent reads your Salesforce pipeline and sends a Telegram summary of deals by stage, which ones have been stale, and which ones need action. Your pipeline review goes from 30 minutes of clicking to a two-minute read.


## Google Sheets for teams without a CRM


Not every team has Salesforce. Some teams have a spreadsheet that actually works, and adding a formal CRM is on the roadmap but not this quarter.


OpenClaw connects to Google Sheets through the built-in Sheets skill. The SOUL.md block:


```text
## CRM
crm_tool: sheets
sheets_id: [your Google Sheet ID from the URL bar]
sheets_range: Leads!A:H


## CRM Automation Rules
-   Append a new row for each qualified lead with name, company, email, source, and date
-   Update the "Last Contacted" column after each outreach email
-   Flag "Follow-up Needed" in column H when there's been no reply after 3 business days
-   Read the sheet before composing any outreach so you know the contact history
```


The agent appends rows, updates cells, and reads the sheet to check what needs attention before composing any message. It has the full contact history every time it acts — no more "did I already email this person?" moments.


This is a solid bridge while evaluating a formal CRM. When you're ready to move to HubSpot, update one line in SOUL.md (` crm_tool: hubspot` ) and add the token. The automation rules carry over as written.


If you've already set up email automation with OpenClaw, the CRM sync builds directly on that foundation — see the[OpenClaw email automation guide](https://blink.new/blog/openclaw-email-automation) for how both work together.


## The 24/7 advantage


Here's the scenario that changes the math.


A prospect replies at 11pm from Singapore. On a local OpenClaw install, the laptop is closed. The reply sits until morning — which means it sits for 9 hours. That prospect is evaluating three vendors. By the time your agent processes the reply, two of them have already responded.


[HubSpot's research on follow-up timing](https://blog.hubspot.com/sales/follow-up-timing-data) puts a number on it: leads contacted within the first hour are 7× more likely to qualify than those contacted later. The window closes fast.


On[Blink Claw](https://blink.new/claw) , the agent runs on hosted infrastructure. It doesn't go offline when your laptop closes. It doesn't sleep between 11pm and 8am. The reply gets processed in minutes. The CRM updates. The follow-up goes out.


$22/mo all-in — LLM costs included. No Docker needed, no VPS to configure, no server to patch at 2am when HubSpot pushes an API change.


Compare that to a part-time admin handling CRM updates. At 15 hours/week at minimum wage, you're looking at $350-500/month — and they're not available at 11pm. They also make mistakes; manual CRM data entry produces[data that's roughly 32% inaccurate on average](https://www.gartner.com/en/documents/3889339) . The agent writes what it reads from the actual email. No transcription errors.


You can message it from Telegram, Discord, or Slack while you're traveling — "What deals need attention today?" — and get an answer sourced directly from your live CRM data.


For a complete picture of what a full sales workflow looks like with OpenClaw, see the[OpenClaw sales agent pipeline guide](https://blink.new/blog/openclaw-sales-agent-pipeline) and the[AI agents for sales overview](https://blink.new/blog/ai-agents-for-sales) .


Blink Claw running OpenClaw 24/7 — CRM updates, follow-up emails, and deal stage changes continue processing through the night without the agent needing to be online


Blink


## Frequently Asked Questions


HubSpot, Salesforce, and Google Sheets work through built-in skills with no additional setup beyond the SOUL.md config shown above. Airtable works through the Airtable skill. For any other CRM with a REST API, you can configure a custom tool using OpenClaw's tools framework — most API-based CRMs come online within an hour of configuration once you have the endpoint documentation.


SOUL.md lives locally in your agent's config directory and isn't transmitted to any third party outside of your agent's execution context. On Blink Claw, each agent runs in an isolated instance — other users can't access your config. For extra security, use environment variable references in SOUL.md (` hubspot_token: $HUBSPOT_TOKEN` ) and set the value through your agent's secrets manager, keeping the token out of the file entirely.


Yes — add an approval rule to your automation config: "Ask me before moving any deal past Proposal stage" or "Confirm before updating any deal value over $25,000." The agent pauses and sends a Telegram message with the proposed change and a yes/no confirmation. This is especially useful during the first few weeks while you're calibrating the rules to your actual workflow.


OpenClaw queues failed operations and retries with exponential backoff. If HubSpot has a brief outage, the agent logs the activity locally and retries when the API responds. You'll get a Telegram notification if retries fail beyond the backoff window. Nothing gets silently dropped — every failed write is logged with enough context to reconstruct the update manually if needed.


Each agent has its own SOUL.md and its own CRM connection. Two agents — say, one for inbound leads and one for outbound sequences — can point to the same HubSpot account using the same private app token. They operate independently on the same CRM data. For Salesforce, use the same Connected App credentials for both. Neither agent knows the other is running; they just both write to the same Opportunities and Contacts.


Add explicit field mapping to your automation rules: "Map company headcount to HubSpot's 'numberofemployees' field" or "Write the prospect's tech stack to the custom field 'tech_stack_notes'." The agent reads these as natural-language policies and applies them when writing to the CRM. You don't configure field mappings through a GUI — you describe the mapping in plain English and the agent handles the API field names behind the scenes.
