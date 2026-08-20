---
schema_version: "1.0.0"
document_id: "d6106f749a1451829af872bff908da3970c5cb5f8ed89dafc34f248ce0818578"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-clawhub-vs-custom-skills"
published_at: "2026-05-07T00:23:11+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:16.827560+00:00"
content_hash: "sha256:afbc610d7f039f3558a57503635c65dca4d9723dd4f669508ff08a60b9e759b6"
---

# ClawHub vs Custom OpenClaw Skills: When to Build vs When to Install

## When to Install a ClawHub Skill


Install from ClawHub when:


- The task is common enough that someone has already built a good version
- You need the skill working in minutes, not hours
- The skill is actively maintained (check last commit date and star count)
- The integration doesn’t require your private credentials or internal data to work


**Good ClawHub skills to install:**


- **Voice journaling** — transcribes audio notes and saves structured entries
- **Telegram notifications** — sends messages to a Telegram channel or bot
- **GitHub integration** — creates issues, comments on PRs, reads repo data
- **Web search** — runs searches and returns structured results
- **Calendar access** — reads and writes Google Calendar or Outlook events
- **Slack posting** — sends messages to a specified Slack channel or user


These are maintained by active community contributors. Rebuilding them from scratch gains you nothing. Install, configure via your` SOUL.md` , and move on.


## When to Build a Custom Skill


Build a custom skill when:


- You’re connecting to a proprietary or internal API not in ClawHub
- The logic is specific to your workflow or business rules
- The skill would expose credentials or data you don’t want in a public registry
- An existing ClawHub skill is close but needs non-trivial modification


**Good reasons to build custom:**


- Reading from your company’s internal CRM (Salesforce, HubSpot custom objects)
- Posting to a private Slack channel with company-specific formatting rules
- Pulling data from a Google Sheet that contains proprietary business data
- Integrating with a homegrown internal tool or legacy API
- Running a workflow with business logic that shouldn’t be shared publicly


Custom skills aren’t harder to build — they’re just more specific. Start from a SKILL.md template and describe exactly what the skill needs to do. If it needs code execution, add a function file.


## How to Install a ClawHub Skill


1


#### Find the skill


Search ClawHub from the terminal:


```text
clawhub   search   "telegram"
clawhub   inspect   telegram-notify
```


Or browse at[clawhub.ai](https://clawhub.ai/) .


2


#### Install it


```text
clawhub   install   telegram-notify
```


This downloads the SKILL.md and any supporting files into your agent’s` skills/` directory.


3


#### Configure in SOUL.md


Add the skill to your agent’s` SOUL.md` and provide any required environment variables or configuration. The skill’s README specifies what’s needed.


4


#### Test it


Run your agent and ask it to use the skill. For most ClawHub skills, this works on the first attempt if credentials are correctly configured.


## How to Write a Custom Skill


Every custom skill starts with a` SKILL.md` file that describes what the skill does, when to use it, and what inputs it needs:


```text
---
name  :   crm-contact-lookup
description  :   Look up a contact in the company CRM by email address.
metadata  :
openclaw  :
requires  :
env  :
-   CRM_API_KEY
---


## When to use this skill
Use when the user asks about a specific customer, client, or contact.
Always look up by email address.


## Inputs
-   `email`  : The contact’s email address


## Output
Returns name, company, last interaction date, and open deals.
```


If the skill needs to execute code, add a Python or JavaScript function alongside the SKILL.md. Register the skill in your agent’s` skills/` directory and it becomes available immediately.


Writing a custom OpenClaw skill for a proprietary API


Blink


## The Self-Hosting Problem


ClawHub skills and custom skills both only work when OpenClaw is actually running. On a local machine, that means only when your laptop is open and the process is active.


This breaks two categories of use cases:


1. **Scheduled tasks** — your agent can’t run a daily summary at 8am if your laptop is closed
2. **Event-triggered workflows** — your agent can’t respond to a Telegram message if the process isn’t running


Blink Claw runs your agent 24/7 — not just when your laptop is on. All ClawHub skills and custom skills work around the clock. You get the full OpenClaw runtime hosted on managed infrastructure, with $22/mo all-in pricing (LLM costs included), and no Docker setup, no VPS to manage.


The skills you install from ClawHub or write yourself work identically on Blink Claw — same SKILL.md format, same SOUL.md configuration. The only difference is the agent keeps running after you close your laptop.


Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo →[blink.new/claw](https://blink.new/claw)


ClawHub indexed 5,400+ skills as of 2026, with the registry growing continuously. The GitHub repository has 8,500+ stars and an active maintainer community. Skills cover productivity, developer tools, communication integrations, data access, and personal automation.


Yes. Once your custom skill is working, you can publish it with` clawhub skill publish <path>` . Published skills go through community review and become installable by other OpenClaw users. If your skill contains proprietary logic or credentials, keep it local — ClawHub is for general-purpose tools.


Yes. Blink Claw runs the full OpenClaw runtime, so all ClawHub skills install and work identically. You still configure them via` SOUL.md` as you would locally. The benefit is that Blink Claw runs 24/7 — your skills keep working even when your laptop is off. Blink Claw is $22/mo all-in with LLM costs included and no Docker setup required.


A basic custom skill with a SKILL.md file takes 15–30 minutes to write and test. A skill that executes code against an API takes 1–2 hours, depending on the API’s complexity. For most internal integrations, a morning is enough to go from zero to a working custom skill deployed on your agent.
