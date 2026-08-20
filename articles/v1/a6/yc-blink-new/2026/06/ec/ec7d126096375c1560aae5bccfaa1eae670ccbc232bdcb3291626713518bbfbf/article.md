---
schema_version: "1.0.0"
document_id: "ec7d126096375c1560aae5bccfaa1eae670ccbc232bdcb3291626713518bbfbf"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-getting-started"
published_at: "2026-06-02T01:14:26+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:1657d36201d6711cbf76a64073179b04044e5ca4d9f2349dbc80c0ed01701593"
---

# OpenClaw Getting Started: Your First AI Agent in 30 Minutes

## Path A: Blink Claw (the 5-minute path)


1


#### Start the free trial


Go to[blink.new/claw](https://blink.new/claw) . 14-day free trial, no credit card required.


2


#### Choose your plan


Starter is $22/mo all-in on annual billing. That includes the LLM — no separate Anthropic or OpenAI account needed.


3


#### Name your agent and pick a model


Claude Opus 4.5 is the recommended default. You can switch between 200+ models anytime without touching configuration.


4


#### Connect a channel


Telegram is the fastest to set up. Paste your bot token from @BotFather and you're connected in under 2 minutes. Slack, Discord, WhatsApp, and iMessage also connect in this step.


5


#### Send your first message


Message your agent on Telegram. It replies. Your agent is live and running 24/7.


Signup to first reply: under 5 minutes.


## Path B: Self-hosted quickstart


If you want to run OpenClaw on your own machine, requires Node.js 22+:


```text
# macOS / Linux
curl   -fsSL   https://openclaw.ai/install.sh   |   bash


# Windows (PowerShell)
iwr   -useb   https://openclaw.ai/install.ps1   |   iex


# Run the onboarding wizard — takes ~2 minutes
openclaw   onboard   --install-daemon
```


The wizard walks through model provider selection, API key setup, and Gateway configuration. After that:


```text
openclaw   gateway   status     # Should show: listening on port 18789
openclaw   dashboard          # Opens the Control UI in your browser
```


If the dashboard loads, everything is working. The[official getting started docs](https://docs.openclaw.ai/start/getting-started) cover Windows-specific setup, Docker installs, and advanced configurations.


For self-hosting on Raspberry Pi or a remote Linux VPS, the setup is identical — but you'll want Tailscale Serve for secure remote access without exposing your Gateway to the public internet.


## Your first real task


Don't test your new agent with "hello." Give it something that shows what it can actually do:


> *"Summarize my unread emails from the last 24 hours and flag anything that looks urgent."*


Or a recurring one:


> *"Every Monday at 8am, check my Notion task list and send me a prioritized summary."*


That second request is where the distinction becomes clear. A chatbot responds with "Sure, here's how you'd set that reminder..." OpenClaw actually sets it. Then runs it. Every Monday. Whether you're awake or not.


Your OpenClaw agent checking in via Telegram with completed task summaries and structured briefings


Blink


## SOUL.md and HEARTBEAT.md


Every OpenClaw agent has two files that define its behavior:


**SOUL.md** is the agent's identity: its name, personality, working style, and standing instructions. Think of it as the agent's memory of who it is and how it should operate. Without a SOUL.md, your agent runs with sensible defaults. With one, it becomes specifically yours.


**HEARTBEAT.md** is the list of recurring tasks — the "pulse" of what the agent does proactively. This is where you define the 7am email briefing, the daily stock alert, the weekly Jira ticket cleanup. Without HEARTBEAT.md, your agent only responds when you message it. With it, your agent starts working without you.


See the[full SOUL.md and HEARTBEAT.md setup guide](https://blink.new/blog/openclaw-soul-heartbeat-setup) for detailed customization.


## What OpenClaw can do (the concrete list)


The 47,094+ ClawHub skills cover the long tail. Here's what most people use OpenClaw for from day one:


- **Morning briefing** — email summary + calendar preview, delivered to Telegram on schedule (see the[morning briefing setup guide](https://blink.new/blog/openclaw-morning-briefing-telegram) )
- **Email triage** — flag urgent messages, draft replies for your review
- **Calendar management** — schedule meetings, set reminders, decline conflicts
- **Research tasks** — web search + summarize, delivered when done
- **Slack and Discord monitoring** — surface messages that mention keywords you care about
- **Jira and Linear** — create tickets, update statuses, query your backlog by voice
- **File operations** — read, write, and organize files in your workspace
- **Web browsing** — the agent controls a Chrome/Chromium browser to fill forms, scrape pages, and automate web tasks
- **Cron automation** — recurring jobs without a separate cron service
- **Multi-agent coordination** — route different tasks to isolated agent workspaces


For the best starting point, the[best OpenClaw skills guide](https://blink.new/blog/best-openclaw-skills) covers top community picks with setup instructions.


## Frequently asked questions


No. The onboarding wizard handles all configuration through prompts. Blink Claw requires zero command-line work — you configure everything in a web interface. Self-hosting requires running a few terminal commands, but no programming knowledge.


ChatGPT responds when you ask it something. OpenClaw acts while you're not watching. You configure OpenClaw with a HEARTBEAT.md that defines recurring tasks — and it executes them on schedule, whether you're at your desk or asleep. For a full comparison, see[OpenClaw vs ChatGPT](https://blink.new/blog/openclaw-vs-chatgpt) .


Anthropic's Claude Opus 4.5 is the recommended default — strong reasoning, reliable long-context performance, and good prompt-injection resistance. For lighter or faster tasks, Claude Sonnet 4.5 is a solid alternative. Blink Claw gives you access to 200+ models with no configuration changes required when switching.


Yes. OpenClaw runs on any ARM Linux device with Node.js 22+. The Raspberry Pi 5 is the most common dedicated home agent setup. The install is identical to standard Linux. Use Tailscale Serve for secure remote access. If you want to avoid the hardware cost and maintenance, Blink Claw starts at $22/mo all-in.


Yes. Your SOUL.md, HEARTBEAT.md, and custom skills are yours. Blink Claw manages the infrastructure; you control the agent's identity and behavior. Your configuration is exportable at any time.
