---
schema_version: "1.0.0"
document_id: "9be1af7ef241bfc2ba247e87f3d1ae362301d8eadf6ad8e2e3dc5e3b1db6c933"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-discord-bot"
published_at: "2026-06-08T00:42:08+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:88c76b9807fb594458131ea31f73b8b60549dc28a83e34af872d20f26e56ae9b"
---

# How to Build a Discord Bot with AI

## Step 2: Describe Your Bot to Blink


Open[Blink](https://blink.new/) and start a new project. Use this prompt:


> "Build me a Discord bot with welcome messages for new members, a FAQ command system, role assignment based on user reactions, and a moderation system that auto-deletes spam. Include a web dashboard where I can configure commands without touching code."


Blink generates the complete stack: the bot process, the database schema for server config and command data, the API layer, and the web dashboard — all wired together in one project.


This is the same approach you'd take for[building an AI chatbot for your website](https://blink.new/blog/how-to-build-ai-chatbot-website) : describe what you want, and Blink handles the infrastructure so you focus on behavior.


Blink deploys the full-stack Discord bot — bot process, database, API, and dashboard all live in under an hour


Blink


## Step 3: Connect Your Discord Token


Add your bot token to Blink's environment variables panel. This panel encrypts secrets at rest — no` .env` files to manage or server config vars to set.


Your bot process starts automatically. The web dashboard launches alongside it. From the dashboard, configure welcome messages, add FAQ entries, assign emoji-to-role mappings, and set moderation rules — no code edits needed.


Changes in the dashboard take effect immediately. Server admins can manage the bot without developer access.


## Step 4: Invite the Bot to Your Server


Generate an OAuth2 invite link from the Discord Developer Portal. Select the` bot` scope and the permissions your bot needs:` Send Messages` ,` Manage Roles` ,` Kick Members` , and` Manage Messages` for moderation.


Share the invite link with any Discord server admin. They approve it in one click. The bot joins and activates all configured features immediately.


The same Blink deployment serves every server the bot joins. Per-server configurations live in your Blink database, so each server gets its own settings.


## What Your Bot Can Handle


Once deployed, your bot replaces hours of manual server management:


- **Welcome messages** — greet new members with a custom message, assign a default role, and link server rules
- **FAQ auto-responder** —` /faq \[topic\]` pulls answers from your dashboard-configured database
- **Role assignment** — users react to a pinned message with an emoji to receive a role automatically
- **Auto-moderation** — detect and delete spam, repeated messages, and flagged keyword content
- **Scheduled announcements** — post messages to any channel at a configured time
- **Polls and voting** — reaction-based polls with auto-close timers
- **Custom slash commands** —` /rules` ,` /help` ,` /ticket` — all configured from the dashboard
- **Ticket system** — open a private support thread, assign to admins, close when resolved
- **Activity logging** — every join, ban, command, and deletion logged to a dedicated channel
- **External data commands** — pull live prices, scores, or stats from any public API with a custom slash command


All Discord bot feature panels — welcome, FAQ, moderation, roles, web dashboard, and analytics running from a single Blink project


Blink


## What to Build Next


A Discord bot is the foundation. Once moderation and FAQ run automatically, add features that create real community value:


- **AI-powered Q&A** — connect an OpenAI or Anthropic model to answer natural language questions in any channel. The same pattern powers the[AI chatbot builder](https://blink.new/blog/how-to-build-ai-chatbot-website) on the Blink blog.
- **Member analytics** — track server growth, engagement by role, and retention trends
- **Live data commands** —` /price BTC` ,` /score Lakers` ,` /status \[service\]` pulling from external APIs
- **Community portal** — a public-facing site for your server, backed by the same Blink database


The web dashboard is the unlock. Non-technical admins can manage the bot without touching code. That's what turns a Discord bot from a developer side project into a real community tool. For more complex internal tooling on the same Blink stack, see[how to build an internal tool](https://blink.new/blog/how-to-build-internal-tool) .


## Frequently Asked Questions


Yes — Discord bots need a persistent process to stay online. If the process stops, the bot stops responding. Blink handles this automatically: your bot runs on Blink's infrastructure, stays online around the clock, and restarts if it crashes. You don't pay for a separate server or VPS.


Yes. From the Blink project, connect to any AI API — OpenAI, Anthropic, or Google — to respond to natural language queries in specific channels. Configure the system prompt and which channels get AI responses from the web dashboard. Your API keys live in Blink's encrypted environment variables.


Generate an OAuth2 invite link in the Discord Developer Portal with the` bot` scope and the permissions your bot requires. Any server admin uses that link to approve it in one click. Your Blink deployment serves every server automatically, storing per-server config in the shared database.


Yes. Enable the "Message Content Intent" in the Discord Developer Portal under Privileged Gateway Intents. This lets your bot read message content, detect trigger words, fire auto-responses, and flag spam. Without this intent, the bot only responds to slash commands — not to regular messages.


Store the token in Blink's environment variables panel — never in source code or public configs. Your bot token grants full API access to your Discord application. If it leaks, regenerate it immediately in the Developer Portal and update your Blink env vars. Blink encrypts all secrets at rest.
