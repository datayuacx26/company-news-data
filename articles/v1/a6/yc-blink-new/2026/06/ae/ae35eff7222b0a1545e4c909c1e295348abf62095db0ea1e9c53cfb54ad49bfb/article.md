---
schema_version: "1.0.0"
document_id: "ae35eff7222b0a1545e4c909c1e295348abf62095db0ea1e9c53cfb54ad49bfb"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-daily-briefing-telegram"
published_at: "2026-06-10T00:29:32+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:afbfa9812c70beae5c9117f20e082d0e11906995a0b971535f0ff36ec1a1ced3"
---

# OpenClaw Daily Briefing on Telegram: Build a 4-Source Morning Digest

## Setting Up the Four Skills


Four skills power the comprehensive briefing. Each needs a one-time configuration in your agent's settings.


### Skill 1: openclaw/telegram-notification


Handles message delivery. In your Blink Claw settings, navigate to **Integrations > Telegram** :


```text
# In SOUL.md — Telegram section


TELEGRAM_BOT_TOKEN: [your bot token from @BotFather]
TELEGRAM_CHAT_ID: [your chat ID — send /start to your bot to get it]
TELEGRAM_PARSE_MODE: Markdown
MAX_MESSAGE_LENGTH: 4096
```


Create your bot via[@BotFather](https://t.me/botfather) in 3 minutes. Message it` /newbot` , follow the prompts, copy the token. To get your chat ID: message your new bot once, then fetch` https://api.telegram.org/bot{YOUR_TOKEN}/getUpdates` — your` chat.id` is in the response.


### Skill 2: openclaw/calendar-summary


Reads your calendar and formats meeting summaries. Add to SOUL.md:


```text
# Calendar settings


CALENDAR_PROVIDER: google  # or: outlook, apple
CALENDAR_LOOKAHEAD: today
CALENDAR_FLAGS:
-   no_agenda: "⚠️ No agenda"
-   back_to_back: "⚠️ No buffer"
-   external_attendees: "👥 External"
CALENDAR_MAX_EVENTS: 5
```


Google Calendar: connect via OAuth in Blink Claw's integrations panel. Outlook: same. No manual API keys needed — Blink Claw handles the OAuth flow.


### Skill 3: openclaw/email-digest


Scans your inbox and surfaces priority emails. SOUL.md configuration:


```text
# Email settings


EMAIL_PROVIDER: gmail  # or: outlook
EMAIL_SCAN_WINDOW: 12h
EMAIL_MAX_RESULTS: 5
EMAIL_PRIORITY_SIGNALS:
-   from: [your-boss@company.com, board@company.com]
-   subject_contains: ["urgent", "action required", "invoice", "contract"]
-   has_attachment: true
EMAIL_FILTER_OUT:
-   newsletters
-   automated_notifications
-   social_media_alerts
```


The agent doesn't read email body content by default — only sender, subject, and snippet. If you want the agent to summarize body content for a specific email, add` EMAIL_BODY_SUMMARY: true` .


### Skill 4: openclaw/habit-tracker


Tracks daily habits. SOUL.md configuration:


```text
# Habits to track


HABITS:
-   name: Exercise
target: daily
check_method: ask  # agent asks you each day
-   name: Reading
target: 30min/day
check_method: ask
-   name: Writing
target: 500 words/day
check_method: ask


HABIT_STREAK_DISPLAY: true
HABIT_REMINDER_IF_MISSED: true
```


The` check_method: ask` setting means the agent messages you each evening to log completion. You reply with a quick "yes/no" or a number. The agent stores the response and surfaces it in the next morning's briefing.


SOUL.md and HEARTBEAT.md configuration — the two files that power OpenClaw's daily briefing


Blink


## Why Hosted Beats Self-Hosted for Daily Briefings


A daily briefing agent is only useful if it runs reliably at 7am — every day, without you touching it. That's where self-hosted setups consistently fail.


Self-hosted problems:


- Agent only runs when your laptop is on. Leave it at the office → no briefing.
- VPS requires manual setup, patching, and monitoring.
- LLM API keys managed separately, billed per-call with unpredictable monthly totals.
- If your VPS goes down at 3am, you don't know until 7am when no message arrives.


**Blink Claw solves this:**


- **$22/month all-in** — LLM costs included via a 200+ model router. No separate API key bills.
- **No Docker, no VPS** — the agent is fully managed. You configure it via a UI.
- **24/7 uptime** — runs in 30+ data center regions. Your briefing arrives at 7am even if you're offline.
- **Automatic failover** — if one region goes down, another picks up within seconds.


For a daily briefing that runs before you wake up, hosted is the only reliable choice. The math is also straightforward: $22/month vs. $10–15/month for a VPS + separate LLM API costs that often run $15–40/month for an active agent = comparable or cheaper total, with zero maintenance.


Blink Claw 24/7 uptime — hosted OpenClaw agent runs reliably every morning while you sleep


Blink


## Setting Custom Timing for Your Schedule


The default` 7:00 AM daily` setup works for most people. Here are common customizations:


**Different times on weekdays vs. weekends:**


```text
## Weekday Briefing — 6:30 AM
Repeat: Daily (Mon–Fri)
[standard briefing components]


## Weekend Briefing — 9:00 AM
Repeat: Daily (Sat–Sun)
[shorter format, drop email and habits, keep calendar and news]
```


**Pre-meeting briefing (30 minutes before each meeting):**


```text
## Pre-Meeting Brief
Trigger: 30 minutes before each calendar event
Delivery: Telegram
Include: attendees, any prep notes, last email thread with attendees
```


**End-of-day summary:**


```text
## Evening Wrap-Up — 6:00 PM
Repeat: Daily (Mon–Fri)
Include:
-   Tasks completed today
-   Emails still requiring response
-   Tomorrow's first 2 meetings
-   Habit check-in prompt
```


The agent treats each section in HEARTBEAT.md as an independent schedule. Multiple schedules stack without conflict.


## Troubleshooting Reliability


**Briefing arrives at the wrong time:** Check that your` Timezone` field in HEARTBEAT.md matches your local timezone exactly. Use[IANA timezone names](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) like` America/New_York` , not abbreviations like` EST` .


**Email section is empty:** The agent requires calendar/email OAuth permissions. In Blink Claw, navigate to **Integrations** and re-authenticate your email provider. Check that the` EMAIL_SCAN_WINDOW` covers the time period you expect.


**No news items:** Verify that your SOUL.md has` TRACKED_TOPICS` defined. The agent won't guess — if topics are empty, the news section is empty.


**Briefing stopped arriving:** Check the[Blink Claw status page](https://blink.new/status) . If status is green, check your Telegram bot hasn't been blocked — send it a direct message and confirm it responds.


## Frequently Asked Questions


The[morning briefing guide](https://blink.new/blog/openclaw-morning-briefing) covers basic Telegram setup with 5 default data sources. This article covers building a comprehensive 4+ source digest, custom HEARTBEAT.md timing for multiple schedules, the four specific skills and their SOUL.md configuration, and reliability troubleshooting for 24/7 operation.


$22/month flat — includes hosting, LLM API costs via the 200+ model router, 24/7 uptime, and automatic security patches. No separate LLM API keys, no VPS bills, no per-message charges. The daily briefing alone costs a fraction of that in actual LLM usage (typically under $0.05/day).


Yes. Common additions from the OpenClaw community: Reddit digests (monitor specific subreddits), stock prices, GitHub activity (PRs merged, CI failures), shipping/delivery tracking, and sports scores. Each is a new section in HEARTBEAT.md and a corresponding SOUL.md entry for personal preferences (which subreddits, which tickers, etc.).


The` openclaw/telegram-notification` skill has a Slack variant. In HEARTBEAT.md, change` Delivery: Telegram` to` Delivery: Slack` and add your Slack webhook URL to SOUL.md. Discord is also supported. Most users prefer Telegram for its bot-first design and message formatting.


Only sender, subject line, and snippet by default. The agent ranks emails by priority signals (sender importance, subject keywords) without reading full body content. Enable` EMAIL_BODY_SUMMARY: true` in SOUL.md if you want one-sentence summaries of body content for flagged emails. Body content is processed locally by the LLM — Blink Claw does not store email content.


Yes. OpenClaw agents are conversational — reply to any Telegram message and the agent responds. Ask it to draft a reply to the priority email, add a task to your list based on a meeting, or dig deeper into a news item. The briefing is a scheduled output; back-and-forth conversation is always available. This is what separates OpenClaw from Zapier or IFTTT automations.


*Also read:[OpenClaw morning briefing setup](https://blink.new/blog/openclaw-morning-briefing) ·[OpenClaw second brain with Obsidian](https://blink.new/blog/openclaw-second-brain-obsidian) ·[OpenClaw inbox zero](https://blink.new/blog/openclaw-inbox-zero-email)*
