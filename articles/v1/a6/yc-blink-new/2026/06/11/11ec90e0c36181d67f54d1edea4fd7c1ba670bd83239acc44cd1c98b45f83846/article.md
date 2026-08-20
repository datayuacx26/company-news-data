---
schema_version: "1.0.0"
document_id: "11ec90e0c36181d67f54d1edea4fd7c1ba670bd83239acc44cd1c98b45f83846"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-morning-briefing"
published_at: "2026-06-09T00:54:35+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:47bfec44b5201d13fd4c28342e0e8f79d1fbc3c948719d5044e6c5ac647cd3a6"
---

# OpenClaw Morning Briefing: Wake Up to Your AI Agent's Daily Report

## The HEARTBEAT.md File: Your Briefing Schedule


HEARTBEAT.md is a markdown file with one section per scheduled behavior. Here is a complete configuration for a morning briefing:


```text
# HEARTBEAT SCHEDULE


## Morning Briefing — 7:00 AM daily


Time: 7:00 AM in user's timezone
Trigger: Daily (Mon–Fri) or Daily (all days) — choose based on preference
Delivery: Telegram


### What to include:
1.   Weather for user's city (current + today's high/low)
2.   Calendar: next 3 meetings today (time, title, key attendees)
3.   News: 3 headlines matching user's tracked topics from SOUL.md
4.   Email: scan inbox for unread emails in last 12h, summarize the highest-priority one
5.   Top priority: first uncompleted task from user's task list


### Format:
-   Keep it scannable — use short lines, not paragraphs
-   No fluff — only include something if it needs action or awareness
-   Use emoji sparingly for section headers (optional)
-   Total length: under 300 words
```


Set the time to whatever works for your schedule. 6:30 AM works well for people with early meetings. 8:00 AM works better if you prefer to exercise first.


## The SOUL.md File: What Your Agent Knows


SOUL.md is where you tell your agent who you are and what matters to you. A morning briefing without a good SOUL.md will pull generic news and the wrong calendar. Here is a practical starting point:


```text
# ABOUT ME


Name: [Your name]
Timezone: America/Los_Angeles (or your timezone)
Location: San Francisco, CA
Role: [Your job title and company]


# WHAT MATTERS TO ME


## Industry & Topics
-   [Your industry] — track funding, acquisitions, major product launches
-   Competitors: [List 2-3 competitor names to track news for]
-   Keywords: [2-3 specific topics you care about, e.g. "AI regulation", "B2B SaaS pricing"]


## Email Priority Rules
-   Flag anything from [your boss/board/key clients]
-   Flag anything with subject line containing: "urgent", "action required", "invoice", "contract"
-   Everything else can wait


## Calendar
-   My work calendar is [Google Calendar / Outlook / etc.]
-   Flag meetings that have no agenda notes as "prep needed"
-   Flag back-to-back meetings (no break)


# HOW I COMMUNICATE
-   Prefer: brief, direct, no filler
-   Telegram handle: @[  your-handle  ]
```


The more specific your SOUL.md, the more useful the briefing. Generic configuration gives you generic results.


How OpenClaw's morning briefing works: HEARTBEAT.md triggers the agent, SOUL.md tells it what you care about


Blink


## Step-by-Step: Set Up Your Morning Briefing


1


#### Get an OpenClaw agent on Blink Claw


The fastest way to run OpenClaw without Docker, a VPS, or any server configuration is[Blink Claw](https://blink.new/claw) . It's $22/month, all-in — LLM costs included via a 200+ model router, 24/7 uptime, automatic security patches, 30+ data center regions. No self-hosting required. Your agent is live in under 5 minutes.


2


#### Set up your delivery channel


For Telegram: create a new bot via[@BotFather](https://t.me/botfather) , get your bot token, then message the bot once to get your chat ID. Add both to your agent's environment. For Slack or Discord, create a webhook URL via your workspace settings and add it the same way. Blink Claw's UI walks you through this.


3


#### Configure HEARTBEAT.md


In your Blink Claw agent settings, navigate to the agent files section and create or edit` HEARTBEAT.md` . Paste the template above, adjust the time to your preferred wake time, and set your timezone. Save — the agent picks up the new schedule within minutes.


4


#### Add your preferences to SOUL.md


Create or edit` SOUL.md` with your details. At minimum: your name, timezone, location, and the 2–3 topics you want tracked in the news section. The email and calendar sections require connecting your accounts — Blink Claw's integrations panel handles this with OAuth (no API keys to manage manually).


5


#### Wait for tomorrow morning


The agent runs on schedule. The first briefing arrives at your set time. Expect to iterate — your first SOUL.md will be missing some things and have some wrong assumptions. Adjust after the first few days based on what's useful and what's noise.


## Example Briefing Output


Here's what a real morning briefing looks like in Telegram:


> **☀️ Good morning, Alex — Tuesday, June 10**
>
>
> **Weather** — San Francisco: 62°F, partly cloudy. High 68°F, no rain.
>
>
> **Today's meetings:** • 9:30 AM — Product sync (5 people) — no agenda yet • 1:00 PM — Q3 planning with Sarah and Marcus • 4:00 PM — Customer call: Acme Corp
>
>
> **News:** • Anthropic raised $2.5B Series E — valuations rising across the sector • Salesforce acquires Spiff (commission software) for $419M • Your competitor X launched enterprise tier — announced this morning
>
>
> **Priority email:** From: Marcus Chen | Subject: Contract renewal — action needed Summary: Client wants to renew at current pricing but needs approval letter by Friday.
>
>
> **Top priority today:** Finish Q2 board update — due tomorrow


Clean. Scannable. No dashboard required.


## Customizing Your Briefing


The five default sources (weather, calendar, news, email, tasks) are the base. Teams in the OpenClaw community have added:


**Reddit digests** — monitor specific subreddits for relevant threads (r/SaaS, r/startups, r/\[your industry\])


**Stock and crypto prices** — if you hold positions or track specific tickers


**Sports scores** — if you want to know the result before opening social media


**GitHub activity** — PRs merged, issues filed, any CI failures on your repos


**Shipping and delivery** — track packages and surface any that need attention


Each addition is a new section in HEARTBEAT.md and, if it requires personal context (which tickers, which subreddits), a new section in SOUL.md.


## Comparison: OpenClaw vs. Other Automation Tools


IFTTT Zapier OpenClaw on Blink Claw


Morning briefing Basic Basic Fully customizable


Processes email No Limited Yes


Reads calendar Yes (Google) Yes Yes


Remembers your preferences No No Yes (SOUL.md)


Replies to your messages No No Yes


Cost Free–$10/mo $20–100/mo $22/mo all-in (LLMs included)


Requires setup 30 min 1–2h 1h (no code)


IFTTT and Zapier trigger automations. They do not understand context or maintain state across interactions. Your OpenClaw agent knows who you are, what you care about, and can have a conversation with you about anything in the briefing — because it's an agent, not a workflow.


A fully customized OpenClaw morning briefing — every source you care about, delivered before your coffee is ready


Blink


## Frequently Asked Questions


On Blink Claw, the flat $22/month covers everything — LLM API costs, hosting, uptime, and security patches. The morning briefing alone costs a fraction of that in actual LLM usage (typically under $0.05/day for a standard briefing). There's no per-message billing on Blink Claw. If you self-host OpenClaw, you pay your cloud provider ($5–15/month for a VPS) plus LLM API costs separately.


Telegram, Discord, and Slack are the most commonly configured delivery channels. Telegram is the most popular choice in the OpenClaw community because bots are first-class citizens and message formatting is excellent. You can also configure email delivery, but most users prefer the push notification from a messaging app.


HEARTBEAT.md supports multiple schedules. Add two sections — one for weekday briefings (Mon–Fri, 7:00 AM) and one for weekend briefings (Sat–Sun, 9:00 AM) with different content or a shorter format. The agent treats each section as an independent schedule.


Yes. OpenClaw agents are conversational — the morning briefing is a scheduled output, but you can reply to any message and the agent responds. Ask it to dig deeper into a news item, draft a reply to the priority email, or add a task to your list based on what's in the briefing. This is what separates it from Zapier.


No. HEARTBEAT.md and SOUL.md are plain markdown files — formatted text, no programming. Connecting calendar and email accounts is handled via OAuth in Blink Claw's UI. The Telegram bot setup requires copying a token from @BotFather, which takes about 5 minutes. The total setup time for a first briefing is under an hour.


The[OpenClaw GitHub repository](https://github.com/openclaw/openclaw) has community-contributed HEARTBEAT.md examples in the` examples/` directory. The morning briefing pattern is one of the most thoroughly documented, with variants for different timezones, delivery channels, and content sources.


---


*Also read:[OpenClaw skills guide](https://blink.new/blog/openclaw-skills-guide) ·[How to run an AI agent 24/7](https://blink.new/blog/how-to-run-ai-agent-24-7) ·[AI agent morning routine](https://blink.new/blog/ai-agent-morning-routine)*
