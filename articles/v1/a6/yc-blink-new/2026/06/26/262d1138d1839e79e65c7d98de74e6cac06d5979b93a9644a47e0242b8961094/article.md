---
schema_version: "1.0.0"
document_id: "262d1138d1839e79e65c7d98de74e6cac06d5979b93a9644a47e0242b8961094"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-morning-briefing-telegram"
published_at: "2026-06-12T12:50:02+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:3d26015f4177795155682da3ceb8fe6a447cf1d16f001710b0cd2054a529411d"
---

# OpenClaw Morning Briefing: Wake Up to a Full Daily Digest on Telegram

## Setting up your OpenClaw morning briefing


1


#### Connect Telegram to OpenClaw


Open Telegram and search for **@BotFather** . Start a conversation and run` /newbot` . Follow the prompts — pick a name, pick a username, copy the API token it returns.


In your OpenClaw workspace, run:


```text
openclaw   channel   connect   telegram   --token   YOUR_BOT_TOKEN
```


Then open a chat with your new bot in Telegram and send it any message. This creates the chat session OpenClaw uses to reach you.


Test the connection:


```text
openclaw   channel   test   telegram
```


If you see **"Channel connected. Message delivered."** — you're ready.


If OpenClaw can't find your Telegram session, make sure you sent the bot at least one message first. The bot needs an active chat before it can send to you.


2


#### Create the morning briefing cron job


This is the core command. Copy it exactly, then edit the city placeholder:


```text
openclaw   cron   create   "0 7 * * *"   \
"Check my calendar for today's events, scan inbox for urgent emails, get weather forecast for [your city], summarize top tech news, then send a formatted morning brief to Telegram"   \
--name   "Morning Brief"   \
--channel   telegram
```


The` "0 7 * * *"` is standard cron syntax. It means: every day, at 7:00 AM. Change the` 7` to whatever hour works for you —` 6` for early,` 8` for late. The time zone uses your server's locale.


OpenClaw confirms the job with an ID:


```text
Cron job created: morning-brief-a3f9
Schedule: 0 7 * * * (next run: tomorrow at 07:00)
Channel: telegram


```


To verify it's registered:


```text
openclaw   cron   list
```


3


#### Customize what's in your briefing


The prompt in the cron command is what your agent reads and executes. Edit it to match your actual morning needs.


**Want to add GitHub activity?**


```text
openclaw   cron   update   morning-brief-a3f9   \
--prompt   "Check my calendar for today's events, scan inbox for urgent emails, get weather in [city], check GitHub for open PRs needing my review, summarize top tech news, send formatted morning brief to Telegram"
```


**Want to add a to-do list?**


Reference your task source directly:


```text
...check my Todoist for tasks due today, then send...


```


**Want a specific format?**


Tell the agent how to structure the output:


```text
...format as: 📅 Calendar | 📧 Email | 🌤 Weather | 📰 News | ✅ To-do. Keep each section to 3 lines max.


```


The agent reads the prompt literally. Specificity gets better results.


OpenClaw uses a HEARTBEAT.md file for routine check-ins (default: every 30 minutes). The morning briefing cron runs independently of HEARTBEAT — it fires exactly at the scheduled time regardless of the heartbeat interval.


4


#### Keep the agent running 24/7


Here's the problem with running OpenClaw on your laptop: your agent only fires the 7 AM cron if your laptop is on at 7 AM.


Close the lid, shut it down overnight, or lose WiFi — no briefing.


This is where most self-hosted setups quietly break. You set up the cron, go to sleep, wake up, nothing in Telegram.


**Blink Claw solves this.** Your agent runs 24/7 on managed infrastructure — even when your laptop is off. The 7 AM cron fires from a server in one of 30+ data center regions worldwide. You get the message on your phone whether you're home, traveling, or still asleep.


No Docker setup. No VPS to manage. No separate API keys for each model — LLM costs are included via a 200+ model router. $22/mo all-in, billed annually.


To migrate your existing OpenClaw setup to Blink Claw:


```text
# Export your current config
openclaw   export   --output   my-agent-config.json


# Import on Blink Claw (run from the Blink Claw dashboard)
blink   claw   import   --config   my-agent-config.json
```


Your cron jobs, channel connections, and HEARTBEAT.md all transfer.


Telegram morning notification from an AI agent — the daily briefing format that OpenClaw users wake up to every day


Blink


*Telegram morning notification from an AI agent — the daily briefing format that OpenClaw users wake up to every day*


## Adding more data sources


Once the basic briefing is running, extending it takes one line in your prompt.


**Hacker News top posts:**


```text
...fetch the top 3 Hacker News posts from today, include in news section...


```


**Crypto prices:**


```text
...check BTC and ETH prices, include in a brief "Markets" section if movement > 5%...


```


**Stock watchlist:**


```text
...check prices for AAPL, NVDA, MSFT at market open, flag anything down more than 2%...


```


**Weather for multiple cities** (useful if you travel often):


```text
...get weather for New York and London, I'm traveling to London on Thursday...


```


Each data source adds a few seconds to the agent's run time. Keep the total prompt scope reasonable — a briefing that takes five minutes to compile defeats the point.


## What the output looks like


Here's a real example of a briefing delivered to Telegram:


> **☀️ Morning Brief — Thursday, June 12**
>
>
> **📅 Calendar (3 events)** — 9:00 AM: Team standup (Zoom link in calendar) — 1:00 PM: Design review with Sara — 4:30 PM: 1:1 with manager
>
>
> **📧 Email (2 urgent)** — "Deploy blocker: prod API returning 500s" — DevOps (08:42) — "Contract signed — need your countersignature" — Legal (07:15)
>
>
> **🌤 Weather — New York** 64°F, partly cloudy. Rain at noon (bring umbrella). High of 71°F.
>
>
> **📰 News** — Anthropic releases Claude 3.8 with extended context — GitHub Copilot adds terminal autocomplete — Y Combinator S26 batch announced
>
>
> **✅ To-do today** — Review PR #847 (assigned to you) — Respond to contract email — Prep design review notes


This takes your agent about 40-60 seconds to compile. It arrives at 7:00 AM sharp.


Blink Claw keeps your OpenClaw agent running 24/7 — the morning briefing fires at 7am whether your laptop is on or not


Blink


*Blink Claw keeps your OpenClaw agent running 24/7 — the morning briefing fires at 7am whether your laptop is on or not*


## Before you set yours up


Three quick checks that save most people the debug loop:


1.


**Test the Telegram channel first** — run` openclaw channel test telegram` before creating any cron job. If this fails, the briefing will silently fail every morning.


2.


**Start the prompt simple** — calendar + email only on day one. Add weather and news once you've seen a successful delivery. Debugging a simple prompt is ten times faster.


3.


**Check your time zone** —` openclaw cron list` shows next run time. If it says 15:00 when you expected 07:00, your server is in a different time zone. Add` --tz "America/New_York"` (or your zone) to the cron create command.


The agent doesn't judge how minimal the first version is. Blink Claw from $22/mo → blink.new/claw runs it the same whether you're checking one calendar or five.


By default, OpenClaw uses the server's system time zone. On Blink Claw, this is UTC. To schedule at 7 AM in your local time, either convert to UTC manually (e.g.,` 0 12 * * *` for 7 AM EST in winter) or pass the` --tz` flag:` openclaw cron create "0 7 * * *" "..." --tz "America/New_York"` . The` --tz` flag accepts any IANA time zone string.


Yes. Create two cron jobs with the same schedule but different` --channel` flags:


```text
openclaw   cron   create   "0 7 * * *"   "Send morning brief"   --name   "Brief-Telegram"   --channel   telegram
openclaw   cron   create   "0 7 * * *"   "Send morning brief"   --name   "Brief-Slack"   --channel   slack
```


Or update your existing job to use multiple channels:` --channel telegram,slack` . Both receive the message at the same time.


Three most common causes:


1. **Bot wasn't messaged first** — open Telegram, find your bot, send it any message to activate the chat session.
2. **Token expired or wrong** — run` openclaw channel test telegram` . If it fails, reconnect with the correct bot token.
3. **Agent wasn't running at 7 AM** — self-hosted OpenClaw only runs crons while the process is active. If your laptop was off or asleep, the cron missed. Blink Claw keeps the agent alive 24/7 so this never happens.


Change the cron schedule to weekdays only:


```text
openclaw   cron   update   morning-brief-a3f9   --schedule   "0 7 * * 1-5"
```


` 1-5` means Monday through Friday. Your agent skips Saturday and Sunday automatically.


Each briefing run makes one LLM call to compile and format the digest. Using a mid-tier model like Claude Haiku or GPT-4o Mini, the cost is roughly $0.002-0.005 per run — less than a cent per morning. On Blink Claw, LLM costs are included in the $22/mo plan. No separate API key, no surprise bills at the end of the month.


Yes — update the prompt at any time:


```text
openclaw   cron   update   morning-brief-a3f9   \
--prompt   "Your new full prompt here"
```


The change takes effect on the next scheduled run. No need to delete and recreate the cron job. Your agent picks up the new instructions automatically.


The standup bot still messages me at 6 PM. This one messages me at 7 AM. Both run whether my laptop is open or not — and that's the entire point.


Set up the cron. Test the Telegram channel. Run it for a week. The first morning it fires and everything is there before your alarm, you'll understand why this is the most common reason people install OpenClaw in the first place.
