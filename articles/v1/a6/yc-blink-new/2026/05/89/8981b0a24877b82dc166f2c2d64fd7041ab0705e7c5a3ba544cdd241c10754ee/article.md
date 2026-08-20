---
schema_version: "1.0.0"
document_id: "8981b0a24877b82dc166f2c2d64fd7041ab0705e7c5a3ba544cdd241c10754ee"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-personal-assistant-complete-guide"
published_at: "2026-05-15T01:30:22+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:13:38.923429+00:00"
content_hash: "sha256:128adc5a3d7924a42150ea28d03509189f9fd60da529a5d5d84418dd230a05e1"
---

# OpenClaw Personal Assistant Setup: The Complete Guide

## Getting Started: SOUL.md and HEARTBEAT.md


The two most important files for a personal assistant OpenClaw configuration:


**SOUL.md** defines who your assistant is and what it knows about you. A good personal assistant soul file includes:


```text
## Identity
You are [Name]'s personal AI assistant. You know their priorities, schedule, and preferences.
You are brief, specific, and action-oriented in all communications.


## My Context
- Work timezone: [timezone]
- Primary communication channel: Telegram
- Daily schedule anchor: 7am briefing, 9pm review
- Top priorities: [work context, personal goals]


## Operating Rules
- Never send more than 5 items in a briefing without ranking them
- When uncertain, ask one clarifying question before acting
- Escalate anything involving financial transactions or calendar changes affecting others
- Default to short messages — max 3 sentences unless I ask for more


## Services Available
[list connected services]


## What to Escalate
[list things that require human confirmation]


```


**HEARTBEAT.md** is your schedule file. It drives all time-based automations:


```text
# Daily Schedule
07:00 - morning briefing (calendar + tasks + weather + headlines)
09:00 - inbox triage summary
16:00 - afternoon inbox pass
21:00 - habit check-in
23:00 - end-of-day journal prompt


# Weekly
Friday 18:00 - weekly review and synthesis
Sunday 20:00 - next week prep (calendar review + goal check)


# On-demand
respond to Telegram messages within 5 minutes during waking hours


```


Start with one automation before building a full schedule. The morning briefing is the highest-leverage entry point — it's the first thing you encounter each day and sets the tone for everything that follows.


## The 3 Skills Every Personal Assistant Needs First


Install these three skills from the OpenClaw skill hub before any others:


[telegram-messenger Two-way Telegram integration. Your agent sends briefings, receives your replies, and handles conversational inputs. The primary human-agent communication layer for most personal assistant setups.](https://blink.new/claw)[calendar-reader Reads events from Google Calendar (or CalDAV-compatible calendars). Powers morning briefings, daily prep, and meeting summaries. Required for any time-aware automation.](https://blink.new/claw)[note-capture Writes structured notes to Obsidian, Notion, or a plain Markdown directory. Powers second brain capture, journal logging, and meeting notes. The memory layer for your assistant.](https://blink.new/claw)


Once these three are working, add the email skill, the habit-tracker skill, and the voice-to-text skill based on which automation you're building next.


## Why Always-On Hosting Changes Everything


A personal assistant that only runs when your laptop is open isn't a personal assistant. It's a script.


The morning briefing that doesn't send because your machine was sleeping. The inbox triage that didn't run because you were on a flight. The habit check-in that failed because your VPN disconnected.


Every gap in uptime is an automation that didn't fire, a loop that didn't close, a reminder that you'll have to do manually. The value of a personal assistant compounds over time only if it runs reliably every single day.


This is the core case for[Blink Claw](https://blink.new/claw) : your OpenClaw agent runs in a managed cloud container, 24/7, regardless of whether your laptop is on or off. No Docker setup. No VPS to maintain. Security patches applied automatically. You message it from Telegram, Discord, or Slack.


At **$22/mo all-in** — LLM API costs included via the 200+ model router — it costs less per month than one dinner out. For the number of hours a well-configured personal assistant reclaims from routine work, the math is straightforward.


Complete OpenClaw personal assistant setup running 6 daily automations simultaneously


Blink


Self-hosting OpenClaw is fully supported and well-documented. If you prefer to manage your own infrastructure — Docker on a VPS or a home server — OpenClaw runs there too. Blink Claw is the managed path for developers who want the personal assistant without the infrastructure overhead.


Related:[Blink Claw pricing and plan comparison](https://blink.new/blog/blink-claw-pricing) and[OpenClaw skill hub: the 20 most useful personal productivity skills](https://blink.new/blog/openclaw-best-productivity-skills)


A 2025 Forrester study found that knowledge workers using AI-powered automation for daily administrative tasks reclaimed an average of[60–90 minutes per day](https://www.forrester.com/) — time previously spent on email triage, scheduling, and information retrieval.


---


ChatGPT responds when you open it. An OpenClaw personal assistant runs on a schedule, connects to your real services (calendar, email, notes), and delivers output to wherever you check — Telegram, Discord, Slack. It takes action: sending messages, creating events, writing notes, processing your inbox. ChatGPT requires you to copy-paste context and ask for help. OpenClaw operates without you present.


Time saved depends heavily on which automations you run. Morning briefings typically save 15–20 minutes of morning news and calendar scanning. Inbox triage saves 30–60 minutes per day for high-volume email users. Second brain capture eliminates the friction of filing notes, which typically saves 10–15 minutes per day for knowledge workers who journal or take regular notes. Habit tracking prevents the 5–10 minutes per day of checking multiple apps to see where you stand. Combined: 60–90 minutes per day is achievable with a fully configured setup.


The OpenClaw skill hub includes integrations for Gmail, Google Calendar, Outlook, Notion, Obsidian, Telegram, Discord, Slack, Todoist, Linear, GitHub, and dozens more via the MCP protocol. Any service with an API can be connected with a custom skill. The most common personal assistant stack is Telegram (communication) + Google Calendar (scheduling) + Notion or Obsidian (notes) + Gmail (email).


No. The SOUL.md and HEARTBEAT.md files are plain text written in natural language — there's no code syntax to learn. Installing skills is a one-click operation in the skill hub. Connecting services requires generating API tokens, which each service's documentation covers in a few steps. Most personal assistant setups are completed without writing a single line of code.


The morning briefing. It's self-contained, delivers immediate value on day one, and gives you a working mental model of how schedules, outputs, and Telegram communication work. Once your morning briefing fires reliably for a week, you understand the system well enough to add the next automation — usually inbox triage or habit check-ins.


Yes. Many OpenClaw users run two agents: a work agent connected to work email, calendar, Slack, and GitHub — and a personal agent connected to personal email, Obsidian, Telegram, and health apps. Each has its own SOUL.md tuned to its context. Blink Claw supports multiple agents on a single account, each with independent schedules and skill sets.
