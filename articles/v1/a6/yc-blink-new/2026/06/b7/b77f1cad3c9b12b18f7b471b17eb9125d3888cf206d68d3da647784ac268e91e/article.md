---
schema_version: "1.0.0"
document_id: "b77f1cad3c9b12b18f7b471b17eb9125d3888cf206d68d3da647784ac268e91e"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-discord-bot-with-ai"
published_at: "2026-06-03T13:02:35+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:35.808686+00:00"
content_hash: "sha256:b097a3f6d47d96f714536f7d32ad159445f585bcd8c97dafaef05e6ee0cfca76"
---

# How to Build a Discord Bot with AI — No discord.js Required

## How to Build a Discord Bot with Blink (Step by Step)


You need two things: a Discord application for the token and a Blink project to run the bot. Blink handles the database, hosting, and code generation. The bot lives in production from day one.


1


#### Open Blink and describe your bot


Go to[blink.new](https://blink.new/) and start a new project. Paste this prompt directly:


> *"Build a Discord community bot for my server. When new members join, send them a welcome DM with the server rules and automatically assign the Member role. Add a /ticket command that creates a private support thread, collects a category and description, and assigns it to the Support role. Log all moderation actions to a #mod-logs channel. Store all member data, ticket history, and moderation records in a database so everything persists across restarts."*


Blink generates the full application: Discord event handlers, a database schema, welcome DM flow, ticket system, moderation logger, and an admin settings panel. No npm install. No database provisioning. No config files to touch.


2


#### Add more features by describing them


Each additional feature is one more prompt. Copy any of these directly:


- *"Add an XP system. Every message earns the member 10 XP. At 500 XP, automatically assign the Active Member role. Members check their XP with /rank."*
- *"Build reaction roles in the #roles channel. Reacting with ✅ assigns the Announcements role. Store all role assignments in the database with timestamps."*
- *"Create a FAQ command. When a member asks about verification in any channel, the bot detects the question pattern and posts the verification steps as a reply."*
- *"Add a poll command. /poll creates a timed vote with up to 4 options. Store results in the database and post the final tally when time expires."*


No code editing required. Describe the change; Blink updates the application.


3


#### Get your Discord bot token


Go to[discord.com/developers/applications](https://discord.com/developers/applications) and set up your bot:


1. Click **New Application** and name your bot
2. Open the **Bot** tab, click **Reset Token** , and copy the token
3. Enable all three **Privileged Gateway Intents** : Message Content, Server Members, and Presence
4. Go to **OAuth2 → URL Generator** , select` bot` and` applications.commands` scopes
5. Copy the generated invite URL and use it to add the bot to your server


Paste the token into Blink's environment variables. That is the only manual configuration step.


4


#### Test your bot in the server


Your bot is live immediately after connecting the token. Test` /ticket` in a channel. Join with a secondary account to trigger the welcome DM. Check the admin dashboard at your Blink project URL — member records and ticket logs populate in real time.


Blink's built-in database stores every event. Nothing is held in memory. A process restart leaves all data intact. The bot that existed before the restart is the same bot that comes back online.


5


#### Deploy to production


One-click deploy from Blink. Your bot runs on production infrastructure — no VPS to manage, no nginx config, no uptime monitor to set up. Custom domain available. The bot process runs always-on with automatic restarts on crashes, so your server never notices a hiccup.


## 8 Bot Features — and Why the Database Makes Each One Work


Every feature has a fragile version (no database) and a production version (database included). Here's the difference.


**1. Welcome automation** — Personalized welcome DM on member join, auto-role assignment, and onboarding questionnaire. The database tracks which members completed onboarding, so you never send duplicates.


**2. Role assignment** — Reaction roles, level-based roles, command-based roles. The database stores the full assignment history and creates an audit trail for every change — critical for large servers with multiple moderators.


**3. Moderation (automod)** — Keyword filters, account age checks, progressive actions: warn → mute → ban. The database stores strike counts per user, ban reasons, and appeal status. Without it, moderators have no record to review.


**4. Community announcements** — Scheduled @role pings, embed-formatted posts, announcement queuing. The database archives every post so you never duplicate an announcement by accident.


**5. Polls and voting** — Reaction-based polls, timed polls, live results. The database prevents double-voting and archives results for post-event review.


**6. Ticket system** —` /ticket` creates a private thread, collects category and description, and assigns it to a support role. The database stores logs, resolution status, and full transcripts. A bot that knows a user's previous two tickets is a bot that actually helps.


**7. Event scheduling** —` /event` creates an embed with RSVP tracking and reminder DMs at set intervals. The database stores RSVP lists and sends reminders only to users who opted in.


**8. Custom slash commands** — Any behavior, any trigger. The database stores per-server settings so your` /rules` command shows different content in different servers without duplicating your bot.


8 Discord bot features powered by a persistent database — built with Blink so member data never disappears on restart


Blink


## Blink vs VibeBot vs discord.js


Three approaches. Three different outcomes.


discord.js (DIY) VibeBot Blink


Time to first working bot 2–40 hours 10 minutes 5 minutes


Coding required JavaScript / Python None None


Database included You manage ❌ No ✅ Yes


Auth & permissions You manage ❌ No ✅ Built in


Data persists on restart Depends on your setup ❌ Often lost ✅ Always


Custom domain VPS + nginx setup ❌ No ✅ Yes


Hosting cost $5–20/mo VPS Included Included


Full web dashboard You build it Limited ✅ Yes


Add a new feature Rewrite code Limited builders Describe to AI


Starting price $5–20/mo + dev time $5–10/mo Free to start


[VibeBot](https://vibebot.gg/) is genuinely good for standard Discord command bots. 2,500+ servers use it, with a 4.9/5 rating from 200+ reviews. Its 37+ pre-built command builders handle common features quickly. For standard commands without a backend, it works well.


The structural limit: VibeBot doesn't include a database. Data stored in memory disappears when the bot process restarts. For bots managing XP systems, ticket logs, or custom server settings — anything where members expect the bot to remember them — that gap matters.


discord.js gives developers complete control. Plan for 40+ hours on a full-featured bot with a real database, authentication, deployment pipeline, and ongoing maintenance. The right choice if you're a developer who needs custom integrations at the deepest level.


Blink generates a full-stack application from a description — database included, auth built in, settings UI generated, custom domain available, production hosting on. For community managers who want a bot that actually remembers users, the infrastructure overhead disappears entirely.


**Your bot shouldn't lose its data every time it restarts. Blink gives every Discord bot a real database, built-in auth, and production hosting — describe it in plain English and deploy in minutes. →[blink.new](https://blink.new/)**


Discord bot data persistence — memory-only bot loses data on restart vs Blink-powered bot with full persistent database


Blink


## Six Mistakes That Kill Discord Bots


Most community bot failures come from these six patterns.


**1. Storing everything in memory** — Any crash or restart wipes the state. Start with a real database from day one, not after you've lost three months of data.


**2. Running the bot locally** — Your internet drops; the bot goes offline. Your laptop sleeps; the bot goes offline. Use production hosting.


**3. No rate limiting on AI responses** — If you add GPT or Claude responses to your bot, every message triggers an API call. Set hard limits from day one to control costs.


**4. No moderation logging** — Without logs, you can't review actions, handle ban appeals, or audit moderators. Every moderation action needs a database record with timestamp, actor, and reason.


**5. Over-automating social interaction** — The 50,000-member community data is clear: FAQ bots with specific triggers outperform general AI chatbots in adoption. Visible automation makes servers feel like customer service queues. Keep bots invisible until useful.


**6. Sharing your bot token** — Rotate it immediately if this happens. A leaked token gives complete bot control to anyone who holds it. Never commit tokens to version control.


## Frequently Asked Questions


No. AI app builders like Blink let you describe what you want in plain English — Blink generates the discord.js code and wires up the database and hosting automatically. You need coding knowledge only for highly custom integrations that go beyond what you can describe to an AI.


Blink is free to start — no credit card required. Traditional discord.js development requires a VPS at $5–20/month plus your own database, auth, and monitoring setup. VibeBot runs $5–10/month but doesn't include a database or auth. Conferbot starts at $29/month. The largest hidden cost in every approach is developer time — which Blink eliminates.


Discord bots automate moderation (spam filters, automatic timeouts, progressive warnings), handle member onboarding (welcome DMs, role assignment), manage support tickets, run polls and events, track XP and engagement, post scheduled announcements, and run custom slash commands. Servers with onboarding bots see 30-day member retention improve from 25–35% to 50–65%. Moderation response time drops from 15–60 minutes to under one second.


Yes. Blink can build a bot that connects to OpenAI or Anthropic APIs for natural-language responses — answering member questions, moderating content with context, or running an AI chat persona in a designated channel. The AI integration is a feature you describe, not code you write. Set rate limits from day one to keep API costs predictable.


VibeBot specializes in Discord commands — 37+ pre-built builders, good for teams that need standard features quickly without a backend. It doesn't include a database, so member data can be lost on restarts. Blink builds a full-stack application: database, auth, settings UI, custom domain, and any feature you can describe. For data persistence, custom logic, or a real backend behind your bot, Blink is the right tool.


Create a Discord Application at discord.com/developers/applications, copy the bot token from the Bot tab, enable Privileged Gateway Intents, then generate an OAuth2 invite URL with` bot` and` applications.commands` scopes. Paste the URL in your browser to add the bot to your server. Blink walks you through this step in under 2 minutes — you only need to paste the token into your Blink project settings.
