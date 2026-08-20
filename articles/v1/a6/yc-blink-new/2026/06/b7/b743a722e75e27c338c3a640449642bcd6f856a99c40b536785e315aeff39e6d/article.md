---
schema_version: "1.0.0"
document_id: "b743a722e75e27c338c3a640449642bcd6f856a99c40b536785e315aeff39e6d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-habit-tracking"
published_at: "2026-06-10T00:20:58+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:1fcebab1e3aed07281b33342a56620869838f22d695051bd429bf72000eea945"
---

# OpenClaw Habit Tracking: Automate Daily Check-Ins, Streaks, and Weekly Reports

## HEARTBEAT.md: The Scheduling File


HEARTBEAT.md defines when your agent runs automated tasks. Here's a complete habit tracking configuration:


```text
habits  :
-   name  :   Morning habit check-in
cron  :   "0 7 * * *"
action  :   >
Send me a Telegram message asking about yesterday's exercise,
sleep quality (1-10), and reading pages. Log my response using
the habit-tracker skill. Show current streaks after logging.


-   name  :   Evening sleep log
cron  :   "0 22 * * *"
action  :   >
Ask me what time I went to bed last night and my sleep quality
(1-10). Log the response.


-   name  :   Deep work check-in
cron  :   "30 17 * * 1-5"
action  :   >
Ask me how many hours of focused work I completed today.
Log the response.


-   name  :   Weekly habit report
cron  :   "0 9 * * 0"
action  :   >
Generate and send my full weekly habit report: all tracked
habits, streak lengths, monthly consistency percentages, and
the habit most needing attention next week.
```


The cron syntax` 0 7 * * *` means 7:00am every day.` 0 9 * * 0` means 9:00am every Sunday. Your agent reads these instructions on schedule and executes them automatically.


## The 3 Best Skills for Habit Tracking


OpenClaw habit streaks — exercise, reading, and sleep progress displayed after each morning check-in


Blink


**1.` habit-tracker`** — The core skill. Provides structured storage for daily habit logs, streak calculations, and trend generation. Install with:


```text
openclaw   skills   install   habit-tracker
```


**2.` daily-reflection`** — Adds a nightly reflection prompt that captures mood, blockers, and wins alongside habit data. Pairs with` habit-tracker` to give the weekly report more context than raw numbers.


**3.` streak-counter`** — Standalone streak engine with milestone alerts. Your agent sends a specific message when you hit 7, 30, or 60 consecutive days. Compatible with any habit type tracked in HEARTBEAT.md.


For note-taking integration — automatically syncing habit logs to your Obsidian vault — see[OpenClaw second brain and Obsidian setup](https://blink.new/blog/openclaw-second-brain-obsidian) . The` memory-wiki` skill handles cross-session referencing across both habit data and notes.


## Why Hosted Beats Running It on Your Laptop


OpenClaw weekly habit report with streak stats, consistency percentages, and accountability notes


Blink


Habit automation only works if it runs reliably at the scheduled time. Three failure modes kill self-hosted setups:


- **Laptop is closed.** The 7am Telegram message never sends because your machine is asleep.
- **VPN or network issues.** The agent can't reach Telegram or Discord during the check-in window.
- **Update breaks the runtime.** A Docker or Node version update corrupts the process. You lose 3 days before you notice.


[Blink Claw](https://blink.new/claw) runs your OpenClaw agent 24/7 on hosted infrastructure for $22/month, LLM costs included via a 200+ model router. No Docker setup. No VPS configuration. No security patches to manage.


Your agent sends the 7am check-in whether your laptop is open or not — whether you're traveling, on a weekend, or in a meeting. That reliability is what makes the streaks meaningful.


For delivering check-ins via Telegram specifically, see[OpenClaw daily briefing on Telegram](https://blink.new/blog/openclaw-daily-briefing-telegram) — the same setup handles both briefings and habit check-ins from one agent.


Yes. Message your agent directly: "I want to start tracking water intake — 8 glasses a day. Check in at 4pm." The agent adds it immediately for the current session. For persistent scheduled check-ins across sessions, add the habit to HEARTBEAT.md. Both methods log data to the same habit-tracker storage.


The agent logs the day as no-data rather than automatically marking it a miss. You can reply to any old check-in retroactively and the agent will log it. Streaks only break for confirmed missed days — not missed check-ins — unless you configure it otherwise in your HEARTBEAT.md action instructions.


Yes. OpenClaw agents work across Telegram, Discord, Slack, and WhatsApp. Many users set morning check-ins on Telegram (personal phone) and end-of-day work check-ins on Slack. The habit-tracker skill stores data centrally regardless of which channel you reply from.


The habit-tracker skill itself is free. Running your OpenClaw agent on Blink Claw costs $22/month all-in — that includes compute, LLM usage through the 200+ model router, Telegram/Discord messaging, and storage. No separate LLM API key needed.


There's no hard technical limit, but 4-6 habits is the practical sweet spot. More than that, daily check-in messages become long enough that you start skipping the reply. Build consistency with 3 core habits first, then add. Your agent adapts as your habit stack grows.
