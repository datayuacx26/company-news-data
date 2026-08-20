---
schema_version: "1.0.0"
document_id: "9a7d2c8ff2f20c203e786c1e1ca10c0a860bd9ac45ba269dab9cad7b20d7317f"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/openclaw-automation-heartbeat-scheduled-tasks/"
published_at: "2026-04-10T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T22:15:54.285032+00:00"
content_hash: "sha256:a398abf88d5aab7abaab884f135d373e3b7d463a8a5f4529510ccb332f10d568"
---

# OpenClaw Automation: Heartbeat, Scheduled Tasks, and What Runs Without You

Most of our customers’ agents sit idle until someone types a message. The ones getting the most value are the ones where the agent works while the owner doesn’t. Daily briefings delivered before they wake up. Inbox scanned every 30 minutes. Weekly reports compiled and posted to Slack overnight.


The configurations in this post come from watching how hundreds of instances actually use Heartbeat and cron, not from reading the docs once. These are the patterns that work and the mistakes that cost people money.


## How Does OpenClaw Run Tasks Without You?


OpenClaw automation uses two systems that handle proactive tasks: Heartbeat and cron. They solve different problems, and picking the wrong one for a given job has real consequences.


**Heartbeat** is a periodic wake-up inside the main conversation session. Every 30 minutes by default, the Gateway sends your agent a prompt. The agent reads a short checklist from a file called HEARTBEAT.md, checks if anything needs attention, and either alerts you or stays silent ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ). If nothing is happening, the agent responds with` HEARTBEAT_OK` and the message gets dropped. You never see it.


**Cron** is the Gateway’s built-in scheduler for jobs that need precise timing. You set a schedule (cron expression, fixed interval, or one-shot), and the Gateway fires it at the right time. Jobs persist at` ~/.openclaw/cron/jobs.json` , so they survive restarts. Every cron execution creates a task record you can inspect later ([OpenClaw Docs: Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) ).


The core difference comes down to context and precision ([OpenClaw Docs: Cron vs Heartbeat](https://docs.openclaw.ai/automation/cron-vs-heartbeat) ):


Heartbeat Cron


**Timing** Approximate (default every 30m) Exact (cron expressions, one-shot)


**Session** Main session (full conversation context) Fresh isolated session (default)


**Task records** Never created Always created


**Delivery** Inline in main session Channel, webhook, or silent


**Best for** Monitoring, triage, quick checks Reports, reminders, background jobs


Heartbeat is for “check if something changed.” Cron is for “do this at 9 AM sharp.”


If you use Heartbeat to send a daily briefing, it fires at whatever 30-minute mark is closest to your target time. Your team might get the briefing at 6:45 or 7:15, not 7:00. If you use cron for inbox monitoring, you’re spinning up an isolated session every 30 minutes when a quick check inside the main session would have been cheaper and faster.


## What Goes in HEARTBEAT.md?


HEARTBEAT.md is a plain text file your agent reads every Heartbeat cycle. It lives in your workspace at` ~/.openclaw/workspace/HEARTBEAT.md` . If the file is empty or contains only comments, the Heartbeat skips the API call entirely, costing you nothing ([OpenClaw Docs: HEARTBEAT.md Template](https://docs.openclaw.ai/reference/templates/HEARTBEAT) ).


A minimal HEARTBEAT.md looks like this:


```text
# Heartbeat checklist


-   Check Gmail for urgent unread messages from priority senders
-   Check calendar for meetings in the next 2 hours
-   If nothing needs attention, reply HEARTBEAT_OK
```


That’s it. Three lines. The agent reads them, does the checks, and either alerts you or stays silent.


For more granular control, use a` tasks:` block with interval-based sub-checks. Each task runs on its own schedule within the Heartbeat cycle ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ):


```text
tasks  :
-   name  :   inbox-triage
interval  :   30m
prompt  :   "Check for urgent unread emails from contacts in USER.md"
-   name  :   calendar-scan
interval  :   2h
prompt  :   "Check upcoming meetings, flag any without agendas"
-   name  :   weekly-metrics
interval  :   168h
prompt  :   "Pull this week's Stripe revenue and compare to last week"
```


Only due tasks get included in each Heartbeat run. If the` calendar-scan` task ran an hour ago, the next Heartbeat skips it and only processes` inbox-triage` . This saves tokens because the agent only sees the prompts that are actually due.


Here’s where people make mistakes. Every line in HEARTBEAT.md gets injected into the prompt on every cycle. A 3KB checklist with 20 items means the agent processes all 20 items every 30 minutes, even when only 1 is relevant. Token cost scales linearly with file size. Agents already make 3 to 10x more LLM calls than simple chatbots ([Zylos Research](https://zylos.ai/research/2026-02-19-ai-agent-cost-optimization-token-economics) ). A bloated HEARTBEAT.md amplifies that.


One more thing: do not put secrets in HEARTBEAT.md. No API keys, no phone numbers, no private tokens. The file becomes part of the prompt context ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ). If you need the agent to access credentials during a Heartbeat check, store them in environment variables or a secrets manager and reference them through tool calls.


## Five Automated Workflows That Actually Run on Klaus


These are real patterns we see across Klaus instances. Each one includes the configuration and the reasoning behind the Heartbeat-vs-cron choice.


### Morning Briefing to Telegram


A cron job fires at 7 AM local time: pull the calendar, scan email for urgent items, check the weather, and deliver a consolidated summary to Telegram.


```text
openclaw   cron   add   \
--name   "Morning brief"   \
--cron   "0 7 * * *"   \
--tz   "America/Los_Angeles"   \
--session   isolated   \
--message   "Compile my morning briefing: today's calendar, urgent emails, weather"   \
--announce   \
--channel   telegram
```


This uses cron because the timing matters. If you want the briefing at 7 AM, you want it at 7 AM, not at 6:45 or 7:15 ([OpenClaw Docs: Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) ). The` --session isolated` flag means the briefing runs in a clean context, so it doesn’t pollute your main conversation.


### Inbox Monitoring Every 30 Minutes


A two-line HEARTBEAT.md that checks for urgent emails from priority senders and flags them if the unread count spikes:


```text
-   Check Gmail for unread messages from contacts in USER.md. Alert if any are urgent.
-   If unread count exceeds 10, send a summary of senders and subjects.
```


The Heartbeat config restricts this to business hours with` lightContext: true` so only HEARTBEAT.md loads each cycle ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ):


```text
{
"heartbeat"  : {
"every"  :   "30m"  ,
"lightContext"  :   true  ,
"activeHours"  : {
"start"  :   "08:00"  ,
"end"  :   "20:00"  ,
"timezone"  :   "America/New_York"
}
}
}
```


Without` activeHours` , the agent checks email at 3 AM and burns credits on an empty inbox. Without` lightContext` , every check loads your full conversation history, turning a 2K-token operation into a 100K-token operation.


### Weekly Metric Report to Slack


A Monday-morning cron job that pulls Stripe revenue, compares to the previous week, and posts a summary to #metrics:


```text
openclaw   cron   add   \
--name   "Weekly metrics"   \
--cron   "0 8 * * MON"   \
--tz   "America/New_York"   \
--session   isolated   \
--message   "Pull Stripe revenue for the past 7 days. Compare to the week before. Format as a summary table."   \
--announce   \
--channel   slack   \
--to   "channel:C1234567890"
```


Isolated session keeps the report out of the main conversation. The` --announce` and` --channel slack` flags deliver the output directly to a Slack channel ([OpenClaw Docs: Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) ). Your team sees the numbers without anyone having to ask for them.


### Lead Enrichment on New CRM Entries


When a new lead enters your CRM, a webhook triggers an isolated cron job to enrich the contact via Apollo and Exa (both available through Orthogonal on Klaus instances):


The Gateway exposes HTTP endpoints for this. A POST to` /hooks/agent` fires an isolated agent run with your payload ([OpenClaw Docs: Scheduled Tasks](https://docs.openclaw.ai/automation/cron-jobs) ). The agent enriches the lead record, finds the company website, pulls recent news, and writes a summary back to your CRM. No human interaction needed.


### End-of-Day Task Checkpoint


A Heartbeat task that runs once per day. At 6 PM, the agent summarizes what was completed, flags anything blocked, and delivers it inline in the main session:


```text
tasks  :
-   name  :   eod-checkpoint
interval  :   24h
prompt  :   "Summarize tasks completed today. Flag anything that's blocked or waiting on someone. Keep it under 200 words."
```


This stays in Heartbeat (not cron) because the summary benefits from full session context. The agent can reference conversations from earlier in the day. If this ran in an isolated cron session, it would have no memory of what happened.


## Heartbeat Configuration That Keeps Your Costs Down


The default Heartbeat configuration works out of the box. It also leaves significant cost savings on the table. Here’s what each setting does and what happens when you change it ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ).


**` every`** controls the interval. Default is 30 minutes. Extending to 60 minutes cuts daily turns in half. Setting it to 5 minutes means your agent wakes up 192 times during a 16-hour active window instead of 32 times. I recommend doing every 55 minutes (for caching reasons) or once every couple hours.


**` lightContext`** controls which bootstrap files load. When set to` true` , only HEARTBEAT.md loads. When` false` (the default), the agent loads AGENTS.md, TOOLS.md, MEMORY.md, SOUL.md, USER.md, and HEARTBEAT.md. On a busy agent with weeks of conversation history, that’s the difference between 2 to 5K tokens per turn and 100K+ tokens per turn.


**` isolatedSession`** starts a fresh session for each Heartbeat run. No conversation history, no previous context. Combined with` lightContext: true` , this is the minimum-cost configuration for simple monitoring tasks.


**` activeHours`** restricts when Heartbeat runs. The` start` and` end` fields use HH:MM format, and you can specify a timezone ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ). Outside the window, Heartbeat skips entirely. A 16-hour window (8 AM to midnight) eliminates 8 hours of wasted checks every day.


**Model routing** is the single biggest cost lever. Heartbeat tasks are typically lightweight: read a checklist, do a few quick checks, respond. Routing Heartbeat to a cheaper model while keeping a more capable model for main conversations can cut Heartbeat costs dramatically ([OpenRouter Models](https://openrouter.ai/models) ). Use the` model` field in your Heartbeat config to override the default.


Here’s a full optimized configuration:


```text
{
"agents"  : {
"defaults"  : {
"heartbeat"  : {
"every"  :   "30m"  ,
"target"  :   "last"  ,
"lightContext"  :   true  ,
"isolatedSession"  :   true  ,
"model"  :   "anthropic/claude-haiku-4.5"  ,
"activeHours"  : {
"start"  :   "08:00"  ,
"end"  :   "22:00"  ,
"timezone"  :   "America/New_York"
}
}
}
}
}
```


With all of these applied, a Heartbeat turn costs roughly 800 to 2,500 tokens depending on your HEARTBEAT.md size. At 28 turns per day (every 30 minutes over 14 active hours), that’s 22K to 70K tokens daily. On a cheap model, that’s pennies. Without optimization, the same Heartbeat schedule could run 100K+ tokens per turn, costing 10 to 50x more.


Agents already generate 3 to 10x more LLM calls than simple chatbots ([Zylos Research](https://zylos.ai/research/2026-02-19-ai-agent-cost-optimization-token-economics) ). Heartbeat adds recurring calls on top of that. The optimization settings exist for a reason.


## Frequently Asked Questions


### What happens if my Heartbeat and a cron job fire at the same time?


The Gateway skips the Heartbeat if the main queue is already busy processing a cron job or user message. Cron jobs running in isolated sessions are unaffected and execute independently ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ). There’s no conflict, but the Heartbeat check gets delayed until the next cycle.


### Can I trigger a Heartbeat manually?


Yes. Run` openclaw system event --text "Check urgent items" --mode now` to enqueue an immediate Heartbeat turn ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ). This is useful for testing your HEARTBEAT.md before waiting 30 minutes to see if it works.


### How much does Heartbeat cost per day?


It depends on model, context size, and frequency. With` lightContext: true` ,` isolatedSession: true` , and Claude Haiku 4.5 ($1/$5 per million tokens on[OpenRouter](https://openrouter.ai/models) ), expect 800 to 2,500 tokens per turn. At 28 to 32 turns per day, that’s roughly 22K to 80K tokens daily, costing well under $0.10/day. Without those optimizations, the same schedule on a capable model with full context can run $5 to $10/day.


### Does Heartbeat work on Klaus out of the box?


Yes. Klaus pre-configures the Heartbeat system on every instance. You just need to create a HEARTBEAT.md file in your workspace at` ~/.openclaw/workspace/HEARTBEAT.md` and the agent starts checking it on the next cycle.


### What happens if my credits run out while Heartbeat is active?


This is a known issue ([#3181](https://github.com/openclaw/openclaw/issues/3181) ). The Heartbeat scheduler retries failed runs every second with no exponential backoff, even for permanent errors like billing failures. Your session file can grow to 50-100MB with failed requests, and the instance will keep hammering the API with doomed calls. If you’re running low on credits, either top up before they hit zero or temporarily rename your HEARTBEAT.md (e.g.,` mv HEARTBEAT.md HEARTBEAT.md.disabled` ) until you do. Cron jobs don’t have this problem — they correctly classify billing errors as permanent and stop retrying.


### What is the difference between HEARTBEAT.md and AGENTS.md?


AGENTS.md defines your agent’s behavior rules, personality, and operational constraints. It shapes how the agent thinks across all interactions. HEARTBEAT.md is a periodic checklist the agent reviews on a schedule. It tells the agent what to check proactively ([OpenClaw Docs: Heartbeat](https://docs.openclaw.ai/gateway/heartbeat) ). One shapes behavior. The other triggers action.


## Key Takeaways


- OpenClaw has two automation systems: Heartbeat for approximate-timing monitoring with full session context, and cron for precisely scheduled jobs with isolated execution.
- HEARTBEAT.md is a plain text checklist your agent reads every cycle. If it’s empty, the Heartbeat skips the API call and costs nothing.
- Use the` tasks:` block for interval-based sub-checks so only due tasks run each cycle, saving tokens on items that don’t need checking yet.
- The` lightContext` ,` isolatedSession` ,` activeHours` , and model routing settings are your primary cost controls. Leaving them at defaults can cost 10 to 50x more than an optimized configuration.
- Heartbeat stays in the main session and benefits from conversation context. Cron creates isolated sessions by default and produces task records you can audit.
- For a deeper look at how Heartbeat fits into[OpenClaw’s architecture](https://klausai.com/blog/how-openclaw-actually-works-architecture-for-non-engineers) , see our architecture explainer. For the workflows that Heartbeat powers (daily briefings, email triage), see our[executive assistant guide](https://klausai.com/blog/how-to-use-openclaw-as-your-executive-assistant) and[use case roundup](https://klausai.com/blog/openclaw-use-cases-10-ways-our-customers-actually-use-it) .
- I (Robbie) personally almost never use heartbeat. I put everything into cron jobs, so that I have more say over how often they trigger. One reason: there’s a[known OpenClaw bug](https://github.com/openclaw/openclaw/issues/3181) where Heartbeat retries failed runs every second with no backoff. If your API credits run out, the Heartbeat scheduler keeps firing, generating tens of thousands of failed requests in a tight loop. Your session file balloons to 50-100MB and your instance hammers OpenRouter with doomed 402s until you manually intervene. Cron jobs handle this correctly — they classify billing errors as permanent and stop retrying. Until the Heartbeat retry logic gets the same treatment, cron gives you more predictable failure behavior. That said, the community loves heartbeats, and the simplicity they provide in having your agent keep working all day. Often I think just having one massive heartbeat is the future, something we’ll all be doing as the models get smarter, and cron jobs are a kludge for the models of today.


---


Want to try this? Klaus pre-configures Heartbeat on every instance. Sign up at[klausai.com](https://klausai.com/) and create your first HEARTBEAT.md.


## Sources


### Primary Sources


- OpenClaw. “Heartbeat.”[https://docs.openclaw.ai/gateway/heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- OpenClaw. “Cron vs Heartbeat.”[https://docs.openclaw.ai/automation/cron-vs-heartbeat](https://docs.openclaw.ai/automation/cron-vs-heartbeat)
- OpenClaw. “Scheduled Tasks (Cron).”[https://docs.openclaw.ai/automation/cron-jobs](https://docs.openclaw.ai/automation/cron-jobs)
- OpenClaw. “HEARTBEAT.md Template.”[https://docs.openclaw.ai/reference/templates/HEARTBEAT](https://docs.openclaw.ai/reference/templates/HEARTBEAT)
- OpenClaw GitHub. “heartbeat.md.”[https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md](https://github.com/openclaw/openclaw/blob/main/docs/gateway/heartbeat.md)
- OpenRouter. “Models.”[https://openrouter.ai/models](https://openrouter.ai/models)


### Secondary Sources


- Zylos Research. “AI Agent Cost Optimization: Token Economics and FinOps in Production.” 2026.[https://zylos.ai/research/2026-02-19-ai-agent-cost-optimization-token-economics](https://zylos.ai/research/2026-02-19-ai-agent-cost-optimization-token-economics)
