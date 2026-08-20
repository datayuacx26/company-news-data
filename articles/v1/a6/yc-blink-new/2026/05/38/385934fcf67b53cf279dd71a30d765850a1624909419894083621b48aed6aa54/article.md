---
schema_version: "1.0.0"
document_id: "385934fcf67b53cf279dd71a30d765850a1624909419894083621b48aed6aa54"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-google-calendar-automation"
published_at: "2026-05-17T00:26:30+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:78cdef36605001dae19d1722adaa5e48e4290abcc36ea435d32d5892dc32a679"
---

# OpenClaw Google Calendar Automation: Let Your Agent Own Your Schedule

## Configure Calendar Automation


1


#### Install the Google Calendar skill


In your agent workspace, run:


```text
openclaw   skills   add   adrianmiller99/google-calendar
```


The skill prompts you to authenticate via Google OAuth. Approve the calendar scopes — you need` calendar.readonly` at minimum for read operations, or the full` calendar` scope for write access.


Credentials are stored in the agent's secrets manager, not in any file the agent can leak.


2


#### Verify the skill works


Send your agent a quick test message:


```text
What meetings do I have today?


```


If the skill is installed correctly, the agent fetches today's events and replies with a formatted list. If it returns an auth error, re-run` openclaw skills auth adrianmiller99/google-calendar` .


3


#### Create your HEARTBEAT.md


In your agent workspace directory, create a` HEARTBEAT.md` file. This defines what the agent checks on each heartbeat tick.


```text
# HEARTBEAT


tasks:
-   name: morning-briefing
interval: 24h
prompt: >
Fetch today's Google Calendar events. Send a briefing to Telegram
with time, title, location, and any prep notes from the event
description. Flag back-to-back meetings or gaps under 15 minutes.
If the calendar is empty, send "Clear day today."


-   name: meeting-prep
interval: 30m
prompt: >
Check if any meeting starts in the next 30 minutes. If yes,
note the attendees and any description context, then send a
3-bullet prep brief to Telegram. If nothing is upcoming,
reply HEARTBEAT_OK.


## Additional instructions
-   Keep briefings under 5 lines.
-   If nothing needs attention after all due tasks, reply HEARTBEAT_OK.
-   Never send the same alert twice in a row.
```


OpenClaw parses the` tasks:` block and only runs each task when its interval is due. The morning briefing fires once every 24 hours. The meeting-prep check runs every 30 minutes but only sends a message when a meeting is actually approaching.


4


#### Configure heartbeat delivery in openclaw.json


Edit your` openclaw.json` to enable heartbeat and route messages to your preferred channel:


```text
{
"agents"  : {
"defaults"  : {
"heartbeat"  : {
"every"  :   "30m"  ,
"target"  :   "telegram"  ,
"lightContext"  :   true  ,
"isolatedSession"  :   true  ,
"activeHours"  : {
"start"  :   "07:00"  ,
"end"  :   "21:00"  ,
"timezone"  :   "America/New_York"
}
}
}
}
}
```


Setting` lightContext: true` limits the bootstrap context to just` HEARTBEAT.md` , cutting per-heartbeat token cost significantly. Setting` isolatedSession: true` runs each heartbeat in a fresh session — this reduces cost from roughly 100K tokens per run down to 2-5K.


5


#### Test end-to-end


Trigger an immediate heartbeat to confirm everything works:


```text
openclaw   system   event   --text   "Run morning calendar briefing now"   --mode   now
```


Check your Telegram. Within 15-30 seconds, your agent should send today's schedule. If you see` HEARTBEAT_OK` instead, your calendar may be empty — add a test event and trigger again.


Set` activeHours` in your heartbeat config to restrict runs to business hours. This prevents 3am notifications when someone in a different timezone schedules a meeting.


## Use Case Examples


**Daily morning briefing.** The most common setup. At 8am every day, the agent sends a structured agenda to Telegram: event title, time, location, and any description notes. Users report saving 10-15 minutes every morning by eliminating the "what's today look like?" mental load.


**Conflict detection.** Two meetings overlapping, or back-to-back with under 10 minutes between them? The agent flags the conflict automatically and suggests open slots for rescheduling. No more discovering the double-booking five minutes before a call.


**Pre-meeting prep briefs.** Thirty minutes before a client call, the agent compiles a short context brief — who the attendees are, what the meeting is about, and any relevant notes from the event description. You arrive at every call prepared.


**End-of-day recap.** A 5pm heartbeat task summarizes the day's completed meetings and previews tomorrow's first three events. Useful for async teams where a nightly status note keeps everyone aligned.


**Cross-timezone scheduling.** Tell your agent "find a 45-minute slot this week that works for someone in Tokyo." The agent checks your calendar for open windows, converts to JST, and suggests three options with a draft invite message ready to send.


## HEARTBEAT.md Schedule for Calendar Tasks


The real power of OpenClaw calendar automation is structuring HEARTBEAT.md so each task runs at the right interval. The morning briefing fires once. The meeting-prep check fires every 30 minutes but only sends a notification when it matters.


Here's a production-ready HEARTBEAT.md for calendar management:


```text
# HEARTBEAT


tasks:
-   name: morning-briefing
interval: 24h
prompt: >
Fetch today's Google Calendar events. Format each as:
[  TIME  ] [  TITLE  ] — [LOCATION if set]
Flag any back-to-back gaps under 15 minutes.
Send to Telegram. If calendar is empty, send "Clear day today."


-   name: meeting-prep
interval: 30m
prompt: >
Is there a meeting starting in the next 30 minutes?
If yes: note the event description and attendees. Send a 3-bullet
prep brief to Telegram. If no: reply HEARTBEAT_OK.


-   name: end-of-day-recap
interval: 24h
prompt: >
At 5pm local time: list today's completed meetings and tomorrow's
first three events. Send to Telegram. Keep it under 5 lines.


## Additional instructions
-   Briefings must be under 5 lines.
-   If nothing needs attention, reply HEARTBEAT_OK.
-   Never send the same alert twice in a row.
```


Do not store OAuth tokens, API keys, or phone numbers directly in` HEARTBEAT.md` . This file becomes part of the prompt context on every heartbeat run. Store all credentials in the agent's secrets manager.


How HEARTBEAT.md drives OpenClaw's calendar automation schedule


Blink


## Running 24/7 With Blink Claw


Self-hosting OpenClaw works fine — until your laptop closes. The agent stops. Heartbeats stop. Your 8am briefing never arrives. The 9:28am meeting-prep check doesn't fire.


The fix is running your agent on a server that never sleeps. You can set up a VPS, configure Docker, manage TLS certificates, and track security patches yourself. Most users do this once, get it working, and then spend the following weeks maintaining it.


[Blink Claw](https://blink.new/claw) handles everything automatically — no Docker, no VPS, $22/month all-in. Your agent runs 24/7, not just when your laptop is on. LLM costs are included via a 200+ model router — no separate API bill when heartbeat volume increases.


Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo →[blink.new/claw](https://blink.new/claw)


With[Blink Claw](https://blink.new/claw) , your calendar agent runs across 30+ data center regions. You message it from Telegram, Discord, or Slack. Setup takes under 10 minutes: connect your Google Calendar OAuth once, add your HEARTBEAT.md, and the agent starts delivering briefings the same day.


Running OpenClaw's calendar automation 24/7 with Blink Claw — always on, even when your laptop is off


Blink


The` adrianmiller99/google-calendar` skill is the most actively maintained option. It supports read, create, update, and delete operations using Google Calendar API v3. For read-only setups,` mrgoodb/google-calendar` is a simpler alternative. For full automation — creating events and rescheduling — use` adrianmiller99` .


Yes. Install` adrianmiller99/google-calendar` and approve the full` calendar` OAuth scope (not just` calendar.readonly` ). The agent can then create, update, and delete events. Say "block Friday afternoon" or "move the 3pm call to Thursday at 2pm" and it modifies the event directly in Google Calendar.


Use` activeHours` in your` openclaw.json` heartbeat config. Set a start time, end time, and your local timezone:` "activeHours": { "start": "07:00", "end": "20:00", "timezone": "America/Chicago" }` . Outside this window, heartbeats are skipped automatically. The 24h morning-briefing task fires on the first tick inside your active window.


HEARTBEAT.md runs tasks in the agent's main session — the same context as chat. The agent can reference prior conversation history, check multiple sources in one turn, and apply judgment about what to send. Cron tasks run in isolated sessions and are better for fully deterministic, stateless jobs. For calendar briefings that need contextual judgment, HEARTBEAT.md is the right approach.


With` isolatedSession: true` and` lightContext: true` , each heartbeat uses roughly 2-5K tokens instead of 100K+. At Claude Haiku pricing (~$0.25/M input tokens), a 30-minute heartbeat interval costs under $0.10/day. On[Blink Claw](https://blink.new/claw) , LLM costs are included in the $22/mo plan — no separate API bill.


The` adrianmiller99/google-calendar` skill uses one OAuth credential by default. To connect multiple accounts (work and personal, for example), install the skill twice under different aliases, each authenticated with a separate Google account. Your HEARTBEAT.md prompts can then reference both explicitly.


The` adrianmiller99/google-calendar` skill uses refresh tokens, so the agent renews access automatically. If a refresh fails — for example, you revoked access in Google's security settings — the agent sends an alert to your configured Telegram channel and pauses calendar tasks until you re-authenticate.
