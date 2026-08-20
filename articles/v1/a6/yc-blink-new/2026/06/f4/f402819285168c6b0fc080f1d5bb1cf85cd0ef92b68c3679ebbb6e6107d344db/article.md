---
schema_version: "1.0.0"
document_id: "f402819285168c6b0fc080f1d5bb1cf85cd0ef92b68c3679ebbb6e6107d344db"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-soul-heartbeat-setup"
published_at: "2026-06-05T12:57:26+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:00077284d06f7e7e7f77f36b2cd4989be05e83cc11a7530d0571edb65f87912a"
---

# OpenClaw SOUL.md and HEARTBEAT.md: The Complete Setup Guide for Daily Automations

## The Minimal Working SOUL.md


Start with this template. Replace every` \[bracketed\]` value:


```text
# Agent Identity


My name is [  AGENT_NAME  ]. I work for [YOUR_NAME / YOUR_COMPANY].


## My Role


I am a personal AI assistant. My primary job is to save you time on recurring tasks
and keep you informed without you having to check multiple apps.


## Communication Style


-   Direct and brief — get to the point in the first sentence
-   Use bullet points for lists of 3 or more items
-   Always confirm what action I took, not just that I'll take it
-   Use Telegram when sending proactive messages unless told otherwise


## Memory


-   Your timezone is [TIMEZONE, e.g. "America/Los_Angeles"]
-   Your location is [  CITY  ] for weather and local context
-   Your primary task system is [Notion / Linear / Todoist / other]
-   Your calendar is [Google Calendar / Outlook]


## Morning Briefing Protocol


At the scheduled Heartbeat time, I send a Telegram message with:
🌤️ Weather — [current + high/low]
📅 Schedule — [today's meetings, chronological]
📬 Email — [top 3 overnight priority items]
📰 Headlines — [3 top stories, tech-focused]
✅ Tasks — [due today, from [  TASK_SYSTEM  ]]


Keep each section to 3-5 items max. No padding. Send it even if one source fails.
```


This file covers the 90% case. Add more sections as you find gaps in how the agent behaves.


## The Minimal Working HEARTBEAT.md


```text
# Heartbeat Schedule


## Morning Briefing


SCHEDULE: 0 7 * * *


TASK: Send the daily morning briefing to Telegram.
Follow the Morning Briefing Protocol in SOUL.md.
Use the telegram-notify skill to deliver the formatted message.


---


## Weekly Review (Friday)


SCHEDULE: 0 16 * * 5


TASK: Generate a weekly summary.
1.   List all tasks completed this week (from [  TASK_SYSTEM  ])
2.   List any recurring issues or blockers
3.   List next week's top 3 priorities
4.   Send as a formatted Telegram message


---


## Nightly Log


SCHEDULE: 0 22 * * *


TASK: Save a brief daily log to memory.
What was accomplished today? What's outstanding?
Store to memory under key: "daily-log-[today's date]"
Do not send a notification — just log silently.
```


Three entries is a good starting point. Add more as you identify what you check manually each day.


## Cron Expression Reference


Expression Meaning


` 0 7 * * *` 7:00 AM every day


` 0 7 * * 1-5` 7:00 AM weekdays only


` 0 9 * * 6,0` 9:00 AM weekends only


` 0 16 * * 5` 4:00 PM every Friday


` 0 */4 * * *` Every 4 hours


` */30 * * * *` Every 30 minutes


All times are in the timezone set in your SOUL.md. If you don't set a timezone, OpenClaw defaults to UTC — which means your "7 AM briefing" arrives at 11 PM Pacific.


How SOUL.md and HEARTBEAT.md work together — identity definition plus scheduled task execution in OpenClaw


Blink


## Common Protocols to Add to SOUL.md


### Email Priority Protocol


```text
## Email Priority Protocol


When reading my inbox, flag as URGENT:
-   Anything from my boss or direct reports
-   Subject lines containing: "urgent", "action needed", "approval required"
-   Emails older than 48 hours that I haven't responded to


Everything else is FYI or low priority.
```


### Task Management Protocol


```text
## Task Protocol


My task system is [Notion / Linear].
When I ask you to "add a task", create it there with:
-   Title (clear action verb)
-   Due date (if mentioned)
-   Priority: P1/P2/P3
-   Assigned to: me (unless specified otherwise)


Always confirm the task was created by showing the link.
```


### Communication Style Protocol


```text
## Response Format


Unless I ask for detail:
-   Answer in 1-3 sentences
-   Lead with the key fact or action
-   Use bullets for 3+ items
-   Never repeat what I just said back to me
-   Skip filler phrases ("Great question!", "Certainly!")
```


## Advanced: Conditional Heartbeat Logic


You can add conditional logic to Heartbeat tasks with plain English:


```text
## Daily Market Report


SCHEDULE: 0 8 * * 1-5


TASK: Check if [  TICKER  ] is down more than 3% from yesterday's close.
If yes: send a Telegram alert with the exact percentage and current price.
If no: do nothing. Do not send a message.
```


Your agent interprets "if yes / if no" correctly and only notifies you when the threshold is crossed. This prevents notification fatigue from automations that trigger constantly regardless of relevance.


## Deploying Your Configuration


### On Blink Claw (Recommended)


1. Go to your agent's dashboard → **Configuration**
2. Paste your` SOUL.md` content into the Identity field
3. Paste your` HEARTBEAT.md` content into the Heartbeat field
4. Click **Save and Restart**


Your agent restarts in under 60 seconds with the new configuration live.


Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo →[blink.new/claw](https://blink.new/claw)


### On Self-Hosted OpenClaw


1. Edit` ~/.openclaw/SOUL.md` with your configuration
2. Edit` ~/.openclaw/HEARTBEAT.md` with your schedule
3. Restart the Docker container:` docker restart openclaw`
4. Check logs:` docker logs openclaw -f`


## Testing Your Setup


After deploying, test Heartbeat manually:


In your agent chat:


```text
Run the "Morning Briefing" Heartbeat task now.


```


The agent executes the task immediately instead of waiting for the scheduled time. If it completes correctly, the scheduled automation will too.


## What Not to Put in SOUL.md


**Too long:** SOUL.md above 500 words starts cutting into your conversation context window. If it's getting long, move topic-specific protocols to separate skill files.


**Outdated information:** Don't hardcode things that change. Use references like "today's priorities from my task system" instead of "my top priority is launching X."


**Duplicate content:** If you have a skill for calendar reading, don't also write calendar logic in SOUL.md. Let skills handle tool-specific behavior.


OpenClaw loads one SOUL.md per agent. If you want different behavior for work vs personal use, run two separate agents with different SOUL.md configurations. Blink Claw lets you run multiple agents from the same dashboard.


OpenClaw logs the failure to the task history. On Blink Claw, you can view failed Heartbeat runs in Agent Logs → Scheduled Tasks. The agent doesn't retry automatically by default, but you can add retry logic to your HEARTBEAT.md: "If the task fails, wait 10 minutes and try once more."


You set it explicitly in SOUL.md. OpenClaw doesn't auto-detect location. If you don't set it, all cron times run in UTC — which means your 7 AM briefing actually fires at midnight Pacific. Always set your timezone.


Yes. A Heartbeat task can instruct the agent to "run the email summary skill, then trigger the daily report template." The agent follows the chain. You're limited by the 200,000-token context window, so don't chain more than 4-5 dependent steps without checkpointing results to memory.
