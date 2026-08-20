---
schema_version: "1.0.0"
document_id: "c2d7ed679d6cab0bd8073bb3cbaaa0b3c9fc6f2a44bc116fc23ed1612f0d9f06"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-meeting-prep"
published_at: "2026-05-28T12:42:47+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:03.604597+00:00"
content_hash: "sha256:1bf6115fc502620e52ca011f37000a52f818ca01432cf66b00c2c331d3074a6a"
---

# OpenClaw Meeting Prep: Let Your Agent Brief You Before Every Call

## During the meeting: structured note capture


**What it does:** After the meeting ends (detected from calendar), the agent sends a prompt to your phone asking for quick voice or text notes. It takes your raw input — however messy — and structures it into a meeting summary.


**The output format:**


- Summary: 2-3 sentences on what was discussed
- Key decisions made
- Open questions that need answers
- Action items with owners and deadlines


**The agent posts this summary to your CRM as a meeting note and copies it to your team's shared Notion doc.**


This replaces the 10-minute "write up my notes while they're still fresh" block that most people skip and then regret.


## After the meeting: follow-up email draft


**What it does:** Within 10 minutes of the meeting ending, the agent drafts a follow-up email based on the structured notes. The draft captures the main discussion points, confirms next steps, and includes any resources that were mentioned.


OpenClaw auto-drafted follow-up email with action items and next steps highlighted


Blink


**The draft goes to your email as a draft — not sent.** You review, make any edits, and send. The agent handles the first-draft lift so you're not staring at a blank email at 4pm trying to remember what you agreed to.


**Setup:** Connect the agent to your email via Gmail or Outlook API. Define a follow-up template structure. The agent reads the meeting notes, fills the template, and creates the draft. A Telegram message notifies you it's ready.


## Action item tracking


**What it does:** The agent extracts action items from meeting notes and adds them to a shared tracker — with the owner, meeting context, and a follow-up reminder set for three days before the deadline.


**Why it matters:** Action items from meetings disappear into calendars, email threads, and half-remembered conversations. The agent turns every action item into a tracked task the moment the meeting ends.


**Setup:** Connect to your task management tool (Notion, Linear, or Asana). The agent parses structured notes, identifies action items, creates tasks, and assigns them based on names mentioned.


## Setting up the meeting prep agent


The full workflow requires four connections:


1. **Calendar** (Google Calendar or Outlook) — triggers the briefing 30 minutes before meetings
2. **CRM** (HubSpot, Salesforce, or Pipedrive) — pulls contact history and posts meeting notes
3. **LinkedIn** — researches contact and company background
4. **Email** (Gmail or Outlook) — receives follow-up draft


All four are available as OpenClaw skills in the ClawHub registry. Initial setup takes 2-4 hours. After that, the agent runs without any manual input.


Define what meetings to prep for in the agent's system prompt — you may want to exclude internal team syncs and only trigger for external contacts.


## Why Blink Claw keeps this running reliably


A meeting prep agent that's down when you need it isn't useful. The 8:45am briefing has to arrive at 8:45am — not when you remember to restart your Docker container.


Blink Claw runs your OpenClaw agent 24/7 from $22/month all-in — LLM costs included. No separate API key for the language model. No infrastructure to maintain.


Your agent runs 24/7 — not just when your laptop is on. The briefing lands in your Telegram or Slack whether you're on your work machine or picking up your phone from your nightstand.


**Security patches applied automatically.** Your meeting prep agent has access to your calendar, CRM, and email. You want the hosting infrastructure to stay updated without manual effort.


For related automation setups, see[OpenClaw inbox zero](https://blink.new/blog/openclaw-inbox-zero) and[OpenClaw CRM automation](https://blink.new/blog/openclaw-crm-automation) .


Try Blink free — ship your first app today


Describe what you want to build. Get a working app with database, auth, and hosting in minutes.


[Start free](https://blink.new/)


## Frequently asked questions


You define the trigger in the agent's system prompt. The most common rule: "prepare a briefing for any calendar event with external email addresses as attendees." This automatically skips internal team meetings. You can also exclude specific meeting types by title (skip "1:1" or "standup").


The CRM connection is optional. Without it, the agent skips the interaction history section and focuses on LinkedIn research, company background, and news. Many solopreneurs and consultants run the meeting prep workflow without a CRM connected.


Yes — configure a lighter internal briefing that pulls the person's role, current project context from Notion or Linear, and any open action items between you and them. Different trigger rules for internal vs external attendees.


No — by default, the agent creates a draft that you review and send manually. This is the right default for any customer-facing communication. If you want to auto-send for specific low-stakes meeting types (internal status updates, thank-you notes), you can configure that as a separate rule.


The agent queues briefings independently for each meeting. It processes them in sequence and delivers each one 30 minutes before its start time. If two meetings overlap in their briefing windows, the agent delivers both and flags the conflict.


With Blink Claw, LLM costs are included in the $22/month plan. For self-hosted OpenClaw, processing 5-10 meeting briefings per day typically costs $5-15/month in LLM API fees depending on research depth and note length.


Yes. With Blink Claw, your agent runs on their infrastructure. Briefings deliver to Telegram or Slack, which you receive on your phone. You don't need a local machine running — the agent operates from Blink's servers around the clock.
