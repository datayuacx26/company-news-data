---
schema_version: "1.0.0"
document_id: "262dc4b1e74640c8647d6e22cc85a3bc3a3f6a063d5b39c8947fcaaa52f76717"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-a-discord-bot"
published_at: "2026-06-13T12:44:33+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:35c93c99455c9914dbbb5d2a7a5bd316ea6839d82cbb6140eac9800ea880ce30"
---

# How to Build a Discord Bot with AI: No Coding Required (2026)

## Step 1: Create a Discord App and Get Your Bot Token


Go to the[Discord Developer Portal](https://discord.com/developers/applications) and create a new application. The name you give the application becomes your bot's display name across every server it joins.


Navigate to **Bot** in the left sidebar and click **Add Bot** . Copy the token that appears — treat it like a password. It is your bot's API key for all Discord interactions.


Enable these under **Privileged Gateway Intents** :


- **Server Members Intent** — required to receive join and leave events
- **Message Content Intent** — required to read message text for moderation and auto-responder commands


Under **OAuth2 > URL Generator** , select the` bot` and` applications.commands` scopes, then add these permissions: Send Messages, Manage Roles, Kick Members, Ban Members, Manage Messages, Read Message History. Copy the generated invite URL — you use it in Step 4.


## Step 2: Prompt Blink to Build Your Bot


Open[Blink](https://blink.new/) and start a new project. Use this prompt:


> "Build a Discord bot with welcome messages for new members (configurable per server), a FAQ slash command that pulls answers from a database, reaction-based role assignment, and auto-moderation that deletes spam and flags banned words. Include a web dashboard where I can configure all settings without touching code. Use slash commands throughout."


Blink generates the complete stack: the bot process with event listeners, the database schema for server configurations and command data, the slash command handlers, and the web dashboard with auth — all wired together in one project.


Prompting Blink to generate a full-stack Discord bot in one session


Blink


## Step 3: Add Your Bot Token and Deploy


In Blink's environment variables panel, add your Discord bot token. This panel encrypts secrets at rest — no` .env` files to manage.


Blink deploys the bot process and web dashboard together. The bot comes online and registers its slash commands with Discord automatically. From the web dashboard, you configure:


- Welcome message text and target channel per server
- FAQ entries (question and answer pairs, editable without redeploy)
- Emoji-to-role mappings for reaction roles
- Moderation rules: spam detection thresholds, banned word list, auto-kick conditions


Changes in the dashboard take effect in seconds without redeployment.


## Step 4: Invite the Bot to Your Server


Use the OAuth2 invite URL from Step 1. Share it with any Discord server admin — they approve in one click. The bot joins and activates all configured features immediately.


Each server gets its own configuration stored in your Blink database. The same deployment handles unlimited servers. Per-server admins configure their own settings through the web dashboard using their Discord login.


## What Blink Handles vs. What You Do Manually


Piece Blink handles You do manually


Bot process (24/7) Generated and deployed —


Database schema Generated and migrated —


Slash command handlers Written for each feature —


Event listeners (join, leave, reaction) Written automatically —


Web dashboard with auth Generated —


Hosting infrastructure Included —


Discord app setup — Create app in Developer Portal


Bot token — Copy from Developer Portal


Server invite — Share invite URL


The three things you do manually take about 5 minutes total.


## DIY Stack vs. Blink: Monthly Cost


Cost item Traditional DIY Blink


Bot hosting (Railway) $7–20/mo Included


Database (Supabase) $25/mo Included


Dashboard auth (Clerk) $25/mo Included


Dashboard hosting $20/mo Included


Setup time 4–8 hours Under 1 hour


**Monthly total** **$77–90+** **$0 (free tier)**


At the free tier, Blink covers small and medium servers. Paid plans add higher usage limits, custom domains, and more compute for high-traffic bots.


## Common Features to Add After Launch


Starting with welcome messages and moderation, you can extend the same Blink project:


- **AI Q&A in channels** — connect an OpenAI or Anthropic model to answer natural language questions. Blink includes 200+ models — pick one based on your speed and quality requirements.
- **Polls with auto-close** — reaction-based voting with a timer that announces results automatically
- **Scheduled announcements** — post messages to any channel on a configured schedule
- **Ticket system** — open a private thread for support requests, assign to mods, close when resolved
- **Live data commands** —` /price BTC` ,` /score \[team\]` pulling from external APIs
- **Member analytics** — engagement stats, growth trends, and retention by role


Each feature is a prompt away from being added to the same project.


For a broader look at what AI can build, see[best vibe coding tools](https://blink.new/blog/best-vibe-coding-tools) and[vibe coding for beginners](https://blink.new/blog/vibe-coding-beginners) .


## Frequently Asked Questions


No. Blink hosts the bot on its own infrastructure. The bot process runs 24/7 on Blink's servers — your computer does not need to be on. This is the main advantage over running a bot locally with discord.py: if your computer sleeps or the terminal closes, your bot goes offline. With Blink, uptime is managed automatically.


Yes. One Blink deployment handles unlimited servers. Each server's configuration is stored separately in the database. When the bot joins a new server, the web dashboard shows a new server panel where you configure that server's specific settings independently.


From your Blink project, add an AI integration: specify which channels get AI responses, set the system prompt that defines how the bot behaves, and add your AI provider API key as an environment variable. The bot routes messages in configured channels to the model and returns the response. You can switch models anytime from the dashboard.


Yes. Add Stripe billing (Blink can generate this) where server admins pay for premium features. Gate features by subscription status stored in your Blink database. Some bot developers charge $5–20/month per server for premium features. The web dashboard can include a billing section for subscription management.


Immediately go to the Discord Developer Portal, navigate to your application's Bot section, and click "Reset Token." Update the new token in Blink's environment variables panel. Your bot restarts with the new token automatically. Store tokens only in environment variables — never in code repositories or public configs.


Discord allows up to 200 application command creates per day. Blink registers commands once at startup, not on every interaction. If you hit rate limits during development, wait 15 minutes before retrying. In production, slash commands are cached by Discord and do not re-register on each bot restart unless the command definitions change.
