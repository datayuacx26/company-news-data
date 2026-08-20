---
schema_version: "1.0.0"
document_id: "ca37ff85088243f9c77322edf65d483a7f6e96a1f07b578354ea71171904112c"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-custom-skill-tutorial"
published_at: "2026-04-28T00:45:12+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:52:06.558365+00:00"
content_hash: "sha256:fb2b51e0b10749745e1ff62ffbf930baa305eb45e8db6084cffbc58444796438"
---

# How to Write Your First OpenClaw Skill: A Complete Guide

## Building the Morning Briefing Skill Step by Step


### Step 1: Define exactly what the skill does


Before writing a single file, write one paragraph describing the skill as if explaining it to someone else:


> "This skill runs every morning at 7am. It checks the weather API for today's forecast, does a web search for the top 3 headlines, reads the user's calendar for today's events, and reads the user's task list for anything due today or overdue. It formats all of that into a clean digest and delivers it as an agent notification."


That paragraph becomes your IDENTITY.md instruction and your HEARTBEAT.md description. Writing it first prevents scope creep.


### Step 2: Create the skill directory


```text
mkdir   -p   ~/.openclaw/skills/morning-briefing
cd   ~/.openclaw/skills/morning-briefing
```


Or, if you're using ClawHub's skill template:


```text
npx   clawhub   create   morning-briefing
```


This scaffolds the three core files with placeholder content.


### Step 3: Write skill.yaml


```text
name  :   morning-briefing
version  :   1.0.0
description  :   >
Delivers a personalized morning digest at 7am: weather,
top headlines, today's calendar events, and open tasks.
author  :   your-name
homepage  :   https://github.com/your-name/morning-briefing
tags  :
-   productivity
-   daily
-   briefing


tasks  :
-   name  :   deliver_briefing
description  :   Compile and deliver the morning digest
trigger  :   heartbeat
output  :   notification
timeout  :   60s


capabilities  :
-   web_search
-   calendar_read
-   weather
-   task_read


settings  :
timezone  :
type  :   string
default  :   UTC
description  :   Your local timezone (e.g., America/New_York)
news_topics  :
type  :   string
default  :   "technology, world news"
description  :   Topics to search for headlines (comma-separated)
```


The` settings` block is optional but useful. It lets users customize the skill without editing YAML directly. Settings appear in the ClawHub install wizard and in the OpenClaw skill manager UI.


### Step 4: Write IDENTITY.md


```text
# Morning Briefing Agent


You are a precise morning briefing assistant. You run once per day at 7am.
Your output is a clean, scannable digest that takes under 60 seconds to read.


## Gathering data


1.   Get today's weather using the weather capability for the user's location
2.   Search the web for top 3 headlines matching the user's configured news_topics
3.   Read today's calendar events (from midnight to midnight local time)
4.   Read open tasks: anything due today or overdue


Gather all four data sources in parallel if possible. Do not wait for one
before starting another.


## Output format


Structure your response exactly like this:


---
**Good morning. Here's your [  Day  ], [  Date  ] briefing.**


🌤   **Weather:**   [  condition  ], [  high  ]°/[  low  ]° — [one-line forecast]


📰   **Top Stories:**
-   [Headline 1] — [one-sentence summary]
-   [Headline 2] — [one-sentence summary]
-   [Headline 3] — [one-sentence summary]


📅   **Today's Schedule:**
-   [  time  ] [event name]
-   [  time  ] [event name]


✅   **Open Tasks:**
-   [task name] (due: [  date  ])
-   [task name] (overdue)
---


## Error handling


-   If weather fails: skip the weather line entirely
-   If no calendar events: "No meetings scheduled today"
-   If no open tasks: "No open tasks"
-   If web search fails: use one placeholder: "Unable to fetch headlines today"
-   Never explain why something is missing. Just handle it silently.


## Tone


Direct and neutral. This is a tool, not a conversation partner.
No "I hope you have a great day!" No filler. Start immediately with the output.
```


The morning briefing skill pulls four data sources in parallel and assembles them into a clean digest


Blink


### Step 5: Write HEARTBEAT.md


```text
# Heartbeat: Morning Briefing


SCHEDULE: 0 7 * * *


Runs the deliver_briefing task every day at 7:00 AM in the configured timezone.


## Schedule variations


Daily at 7am (default): 0 7 * * *
Weekdays only at 7am:   0 7 * * 1-5
Every morning at 6am:   0 6 * * *
```


### Step 6: Test the skill locally


OpenClaw's skill testing command runs the skill immediately, outside of the schedule:


```text
openclaw   skill   test   morning-briefing   --task   deliver_briefing
```


This runs` deliver_briefing` right now and shows the output in your terminal. Use this to verify the skill works before scheduling it.


To test with custom settings:


```text
openclaw   skill   test   morning-briefing   --task   deliver_briefing   \
--setting   timezone=America/New_York   \
--setting   news_topics="AI, startups"
```


Common issues in testing:


- **Capability not granted** : add the missing capability to` skill.yaml`
- **Calendar read returning empty** : check that your calendar integration is connected in OpenClaw settings
- **Weather API errors** : verify you have a location configured in your agent profile


### Step 7: Install the skill in your agent


```text
openclaw   skill   install   ./morning-briefing
```


Or from a GitHub repo:


```text
openclaw   skill   install   github:your-name/morning-briefing
```


The skill is now active. It will run at 7am per the schedule. You can verify it's loaded:


```text
openclaw   skill   list
```


## Common Skill Patterns


Beyond the morning briefing, most productivity skills follow one of four patterns:


**Time-gated digest** : gather data, format it, deliver. (Morning briefing, weekly review, daily standup)


**Watch-and-react** : monitor a source, trigger an action when something matches. (New email from client → draft a reply, new PR opened → summarize the diff)


**Transform-and-store** : take input, process it, save the output. (Meeting ends → write summary to Notion, article read → extract key points to reading list)


**Cross-service sync** : pull from A, push to B. (Tasks in Todoist → daily slack message, GitHub issues → Jira tickets)


The pattern determines which capabilities you need and whether you want heartbeat or event triggers.


## Sharing on ClawHub


ClawHub has 5,400+ skills and growing. Contributing one is worth doing — both for the community and because published skills get regular feedback that improves them.


To publish:


1. Create a GitHub repo with your skill files
2. Add a` README.md` with a clear description, setup steps, and required capabilities
3. Submit via[clawhub.io/submit](https://clawhub.io/submit) — paste your GitHub URL and fill in the category tags
4. ClawHub reviews for safety (no unauthorized API calls, reasonable capability requests) in 1-3 days


Once published, users can install your skill with:


```text
npx   clawhub   install   your-name/morning-briefing
```


## Running Skills Without Managing Infrastructure


Writing skills is the easy part. Running them reliably is where most self-hosted setups break down. Your agent needs to be online at 7am every day — no Docker downtime, no VPS reboots that interrupt the schedule, no missed heartbeats.


[Blink Claw](https://blink.new/claw) handles the always-on runtime. Your OpenClaw agent runs on managed infrastructure — 24/7, with LLM costs included in the monthly price. Skills run on schedule without you thinking about it. Starting at $22/month, it's cheaper than running a VPS with the same reliability guarantees.


## Frequently Asked Questions


A task is a unit of work your skill can perform — the action itself. A trigger is what causes the task to run. In the morning briefing example,` deliver_briefing` is the task; the heartbeat schedule is the trigger. One skill can have multiple tasks (each potentially with different triggers), or one task with multiple trigger types.


Declare the` web_search` capability to make HTTP requests to external URLs, or use OpenClaw's named integration capabilities (` calendar_read` ,` email_read` , etc.) for common services. For custom APIs without a named capability, use` http_request` and configure the API credentials in your agent's secrets store — then reference them via` {{secret.MY_API_KEY}}` in your IDENTITY instructions.


Yes — use the appropriate capabilities.` filesystem_write` for local file output,` calendar_write` for creating calendar events,` notion_write` for Notion pages,` email_send` for outbound email. Always declare the write capability explicitly — skills default to read-only for safety.


It sets the maximum time OpenClaw waits for a task to complete before cancelling it and logging a timeout error. Default is 120 seconds. For fast tasks like the morning briefing (web searches + calendar read = usually under 20s), set it lower. For long-running tasks (processing a full document, complex multi-step research), raise it to 300-600s.


First, test it manually with` openclaw skill test \[name\]` . If it works manually but not on schedule, check: (1) your agent's timezone setting vs the cron schedule, (2) whether the agent was online at the scheduled time, (3) the schedule syntax in HEARTBEAT.md. Run` openclaw log --filter skill:morning-briefing` to see the last 50 execution attempts with status.


Yes. Define multiple tasks in skill.yaml — one with` trigger: heartbeat` and another with` trigger: event` . Or define one task with both triggers in the triggers array. The agent handles deduplication if both fire simultaneously.


OpenClaw itself has no hard limit. The practical limit is the compute and API rate limits of your agent environment. If you're self-hosting, your machine's resources are the ceiling. On Blink Claw's managed runtime, you can run as many skills as your subscription tier allows — the infrastructure scales automatically.
