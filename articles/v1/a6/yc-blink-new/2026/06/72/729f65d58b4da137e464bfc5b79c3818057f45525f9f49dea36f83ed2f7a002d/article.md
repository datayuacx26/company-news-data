---
schema_version: "1.0.0"
document_id: "729f65d58b4da137e464bfc5b79c3818057f45525f9f49dea36f83ed2f7a002d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-crm-automation-hubspot"
published_at: "2026-06-12T02:39:21+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:b58260af66306091c68a811bdd6e876f767f21dfdc218fb5ebdc5568f1a38429"
---

# OpenClaw CRM Automation: Sync Your Agent with HubSpot and Salesforce

## Salesforce Integration: Setup


The Salesforce skill follows the same pattern:


```text
/skill install clawhub:claw-crm-salesforce


```


Authenticate with a connected app or session-based access token:


```text
/secret set SF_ACCESS_TOKEN your_token
/secret set SF_INSTANCE_URL https://yourorg.salesforce.com


```


Your` SOUL.md` CRM rules apply equally to Salesforce — the agent uses the same reasoning logic regardless of CRM. The Salesforce skill handles the API translation.


Salesforce-specific considerations:


**Opportunity stage updates.** Define your stage criteria in` SOUL.md` explicitly. "Discovery" to "Qualified" should require a clear signal — don't let the agent update stages on its own unless you've defined the threshold.


**Activity logging.** The skill creates tasks and logs activities to the correct opportunity automatically. Map your activity types in` SOUL.md` so the agent uses your taxonomy, not a generic one.


**Custom fields.** If your Salesforce org has custom fields, list them in` SOUL.md` . The agent reads them by field API name — include them if they affect lead quality decisions.


Try Blink free — ship your first app today


Describe what you want to build. Get a working app with database, auth, and hosting in minutes.


[Start free](https://blink.new/)


## The 5 CRM Workflows Teams Run on OpenClaw


**Workflow 1: Lead research enrichment**


The agent pulls each new lead, researches the company (LinkedIn, website, recent news, tech stack), and writes a one-paragraph qualification summary back to the CRM contact record. Reps open a contact and find it already researched.


**Workflow 2: Deal stage update from email conversations**


Forward a thread to the agent or connect it to your email inbox. It reads the thread, identifies stage-change signals (budget confirmed, decision maker engaged, timeline discussed), and updates the opportunity stage with a note explaining why.


**Workflow 3: Follow-up reminder and draft**


The agent checks every deal for last-contact date. Deals past their follow-up window get a drafted email — personalized to the last conversation and what the agent found in recent company news. Rep reviews, edits if needed, sends.


**Workflow 4: Pipeline health check**


Every Monday at 7 AM: list all open deals with no activity in the past 14 days, flag deals with overdue close dates, and post a prioritized "deals at risk" list to #sales-ops. No surprise pipeline reviews.


**Workflow 5: Win/loss analysis**


After a deal closes (won or lost), the agent pulls the full history — contacts, activities, emails, stage timeline — and writes a structured win/loss summary. What signals correlated with the close? What broke down? Posted to your team's Notion page or Slack channel automatically.


## What the Agent Logs vs. What Requires Human Judgment


The agent is good at finding and recording facts. It's not good at relationship judgment.


**Agent logs automatically:**


- Contact info changes (new title, company, phone number)
- Company details (headcount, funding stage, recent news)
- Email timestamps and response status
- Call notes from connected dialers
- Stage change rationale (from defined triggers in` SOUL.md` )


**Human judgment required:**


- Pricing decisions and custom terms
- Relationship-sensitive responses (a deal in trouble, a frustrated buyer)
- Contract language and legal review
- Anything where tone matters more than content


**Human in the loop for email drafts.** The agent drafts follow-up emails; reps approve before sending. This is intentional — and set in` SOUL.md` :


```text
## Email Rules


Draft follow-up emails for rep review. Never send directly.
Stage outreach based on company size:
-   Enterprise (500+): formal, reference specific pain, no pricing
-   Mid-market (50-500): direct, lead with value, include next step
-   SMB (<50): short, conversational, single CTA


Flag emails that touch pricing or legal language — do not draft these.
```


This keeps the human in control of the relationship while the agent handles the research and structure.


OpenClaw connecting to HubSpot, Salesforce, email, and Slack — the agent at the center of your sales stack


Blink


## Blink Claw: Running It 24/7


Self-hosted OpenClaw runs when your machine is on. The HEARTBEAT.md schedule fires at 6:30 AM only if your laptop is running, connected, and the server hasn't crashed.


[Blink Claw](https://blink.new/claw) is managed OpenClaw hosting. It runs continuously — no Docker setup, no VPS configuration, no keeping your laptop open.


Self-hosted Blink Claw


Infra cost $5-7/mo VPS Included


LLM costs $20-80/mo (varies by usage) Included


**Total** **$25-87/mo** **$22/mo**


Uptime When laptop is on 24/7


Setup Docker + config Deploy in minutes


LLM models Your API keys 200+ models included


At $22/mo all-in with 200+ models included, Blink Claw is cheaper than the VPS-only option before LLM costs.


Install the` claw-crm-hubspot` skill, point it at your HubSpot API key, and run the first morning sync. Your team arrives to pre-researched contacts and drafted follow-ups.


**Related reading:**[OpenClaw Skills for Developers](https://blink.new/blog/openclaw-skills-for-developers) ·[OpenClaw for Solopreneurs](https://blink.new/blog/openclaw-for-solopreneurs) ·[How to Build a Sales Pipeline Agent](https://blink.new/blog/openclaw-sales-agent-pipeline)


## Frequently Asked Questions


OpenClaw connects directly to HubSpot and Salesforce via their official APIs using your credentials, stored in the secrets manager. No third-party connector sits in the middle — the agent reads and writes to your CRM just like a native integration. Your data doesn't pass through any intermediary service.


Yes. You can configure separate secrets for multiple HubSpot portals or Salesforce orgs and reference them in` SOUL.md` . For team use on Blink Claw, a single agent instance can manage CRM operations across multiple accounts by parameterizing which credentials to use for each workflow run.


OpenClaw logs every action it takes — you can see exactly what changed and why in the agent's activity log. To minimize incorrect updates, define your stage criteria precisely in` SOUL.md` . For high-stakes stages like "Proposal Sent" or "Closed Won", configure the agent to draft the update for human approval rather than applying it automatically. The HEARTBEAT.md schedule can be set to route stage changes to a Slack channel for review before writing to the CRM.


It depends on contact count and research depth. A typical sync covering 50 contacts with full company research (LinkedIn, website, news) runs 15-25 minutes. For larger pipelines, stagger the HEARTBEAT.md schedule — run contact research at 6:30 AM and deal updates at 7:00 AM. On Blink Claw, the agent runs in the background; your team never waits for it to finish.
