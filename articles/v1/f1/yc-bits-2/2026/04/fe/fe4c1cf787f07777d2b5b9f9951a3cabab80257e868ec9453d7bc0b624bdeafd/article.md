---
schema_version: "1.0.0"
document_id: "fe4c1cf787f07777d2b5b9f9951a3cabab80257e868ec9453d7bc0b624bdeafd"
company_key: "yc-bits-2"
company: "Bits"
source_id: "yc-bits-2-news-import-8ae56e6aae1c"
canonical_url: "https://klausai.com/blog/how-to-connect-openclaw-to-whatsapp-slack-telegram-and-discord/"
published_at: "2026-04-03T00:00:00+00:00"
first_seen_at: "2026-07-24T21:07:46.493176+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:f71355508afa76125ea101d9715e8f049d69fdd9c2df86b2b51c8a3953d27f2c"
---

# How to Connect OpenClaw to WhatsApp, Slack, Telegram, and Discord

We configure messaging integrations for every Klaus customer. Slack and Telegram are done in minutes. WhatsApp works well until it disconnects, and then you re-scan a QR code and move on with your life. Discord takes about ten minutes and is rock-solid once connected.


This guide covers self-hosted setup for all four platforms. If you want someone else to handle the configuration, that’s what[managed hosting](https://klausai.com/blog/openclaw-hosting-managed-vs-self-hosted) is for.


## Which Platform Should You Start With?


OpenClaw supports[over 20 messaging channels](https://docs.openclaw.ai/channels/channel-routing) simultaneously, but start with one. The right choice depends on how you plan to use your agent.


Platform Setup Time API Type Reliability Best For


Telegram ~5 min Official Bot API High (no disconnects) Personal use, mobile-first


Slack ~15 min OAuth + Socket Mode High Teams, workplace automation


Discord ~10 min Official Bot API High Communities, developer teams


WhatsApp ~5 min Unofficial (Baileys) Medium (periodic re-auth) Client-facing, existing WhatsApp users


> **Note:** The Baileys library has no affiliation with Bailey Wickham, Klaus Founder and CTO.


**Personal use or mobile-first?** Start with Telegram. The[official Bot API](https://docs.openclaw.ai/channels/telegram) is stable, well-documented, and has no terms-of-service grey area.


**Team or workplace?** Slack.[Socket Mode](https://docs.slack.dev/apis/events-api/using-socket-mode/) means no public URL needed. Channel-based access control lets you restrict which channels the bot responds in.


**Community or developer team?** Discord. The[official Bot API](https://docs.openclaw.ai/channels/discord) is stable, and Discord’s guild/channel structure gives you fine-grained control over where the bot operates. Voice channel support is a bonus if your community uses voice.


**Client-facing or already on WhatsApp?** WhatsApp. But read the caveats section before you commit. The[integration uses an unofficial API](https://docs.openclaw.ai/channels/whatsapp) and requires periodic re-authentication.


## How to Connect OpenClaw to Telegram


To connect OpenClaw to Telegram, create a bot via @BotFather, add the token to your OpenClaw config, and start the gateway. Total setup time is about five minutes.


Telegram is the recommended starting point. The[Bot API](https://core.telegram.org/bots/api) (currently v9.5 as of March 2026) is official, stable, and built specifically for bots. No unofficial libraries, no ToS concerns.


### Step 1: Create a Bot via @BotFather


Open Telegram and search for` @BotFather` . Start a chat and send` /newbot` . BotFather asks for a display name and a username (must end in “bot”). Once created, you get a bot token that looks like this:


```text
7104583291:AAH8kLp_Rq3vZ0xNmT4fW2yUj6bKcDe5sXo
```


Copy that token. You need it in the next step.


### Step 2: Configure OpenClaw


Add the token to your[OpenClaw config](https://docs.openclaw.ai/channels/telegram) :


```text
{
channels  : {
telegram  : {
enabled  :   true  ,
botToken  :   "YOUR_TOKEN_HERE"  ,
dmPolicy  :   "pairing"
}
}
}
```


The` dmPolicy` setting controls who can talk to your bot.` pairing` is the default and requires you to approve each user before they can send messages. Other options:` allowlist` (pre-configured user IDs only),` open` (anyone can message), or` disabled` .


To find your numeric Telegram user ID, send a message to your bot and check the logs with` openclaw logs --follow` . Your ID appears in the` from.id` field.


### Step 3: Start the Gateway and Approve Pairing


```text
openclaw   gateway
```


Then approve your own pairing request:


```text
openclaw   pairing   list   telegram
openclaw   pairing   approve   telegram   <  COD  E  >
```


Pairing requests[expire after one hour](https://docs.openclaw.ai/channels/telegram) , and pending requests are capped at three per channel.


Send any message to your bot in Telegram. OpenClaw should respond within a few seconds.


### Group Chats


Add your bot to any Telegram group. By default, the bot only responds when someone @mentions it. If you want the bot to see all group messages, either disable privacy mode via` /setprivacy` in BotFather or make the bot a group admin.


One important security detail: group sender authorization is separate from DM pairing approvals (this boundary was[enforced as of the 2026.2.25 release](https://docs.openclaw.ai/channels/telegram) ). Approving someone for DMs does not give them access in groups.


### What to Watch


[Bot API rate limits](https://core.telegram.org/bots/api) sit at roughly 30 messages per second for broadcasts, with token-bucket throttling since Bot API v7.0. Not an issue for personal use, but it matters if you’re broadcasting updates to multiple groups.


## How to Connect OpenClaw to Slack


To connect OpenClaw to Slack, create a Slack app with Socket Mode enabled, configure bot token scopes, and subscribe to message events. Setup takes about 15 minutes.


If your team already lives in Slack, the agent should too. Slack has[over 42 million daily active users](https://chatmaxima.com/blog/business-messaging-statistics-2026/) and channel-based access control that lets you control where the bot operates.


### Step 1: Create a Slack App and Enable Socket Mode


Head to[api.slack.com/apps](https://api.slack.com/apps) and create a new app from scratch. Select your workspace.


In the app settings, enable[Socket Mode](https://docs.slack.dev/apis/events-api/using-socket-mode/) . This generates an App Token (starts with` xapp-` ) with the` connections:write` scope. Socket Mode uses a WebSocket connection where your OpenClaw gateway connects outbound to Slack’s servers. That means no public URL, no SSL certificates, and no firewall changes on your end.


### Step 2: Configure Bot Token and Scopes


Under OAuth & Permissions, add the[required bot token scopes](https://docs.openclaw.ai/channels/slack) :


- ` chat:write` (send messages)
- ` channels:history` and` groups:history` (read channel messages)
- ` im:history` and` im:read` (read direct messages)
- ` app_mentions:read` (detect @mentions)
- ` reactions:read` and` reactions:write` (acknowledgment reactions)
- ` commands` (slash commands)


Click “Install to Workspace” and copy the Bot User OAuth Token (starts with` xoxb-` ).


### Step 3: Subscribe to Events and Launch


Under Event Subscriptions, subscribe to these bot events:


- ` app_mention`
- ` message.channels` ,` message.groups` ,` message.im` ,` message.mpim`
- ` reaction_added` ,` reaction_removed`


Enable the App Home “Messages Tab” for direct message support.


Add both tokens to your[OpenClaw config](https://docs.openclaw.ai/channels/slack) :


```text
{
channels  : {
slack  : {
enabled  :   true  ,
mode  :   "socket"  ,
appToken  :   "xapp-..."  ,
botToken  :   "xoxb-..."
}
}
}
```


Run` openclaw gateway` and send a DM to your bot in Slack.


### Channel Access Control


The` groupPolicy` setting controls which channels the bot responds in. Options:` open` (all channels),` allowlist` (specific channels only), or` disabled` . Use stable channel IDs in the allowlist, not channel names.


Messages in channels are mention-gated by default. The bot only responds when someone @mentions it or replies to one of its messages. You can override this per channel.


### What to Watch


Slack has the most configuration surface of the four platforms. The common failure mode is “bot connected but not responding.” That’s usually a missing event subscription or a` groupPolicy` misconfiguration. Run this to diagnose:


```text
openclaw   channels   status   --probe
```


If events still aren’t coming through, verify that Socket Mode is enabled in the Slack app settings and that your App Token has the` connections:write` scope.


## How to Connect OpenClaw to Discord


To connect OpenClaw to Discord, create a bot in the Discord Developer Portal, invite it to your server, add the token to your OpenClaw config, and start the gateway. Setup takes about ten minutes.


Discord is a natural fit if your community, team, or project already lives there. The[official Bot API](https://docs.openclaw.ai/channels/discord) is stable and well-supported. OpenClaw’s Discord integration includes guild/channel-level access control, thread bindings, and voice channel support.


### Step 1: Create a Discord Bot


Go to the[Discord Developer Portal](https://discord.com/developers/applications) and create a new application. Under the Bot tab, click “Reset Token” to generate a bot token. Copy it — you’ll need it in the next step.


Under Privileged Gateway Intents, enable **Message Content Intent** . Without this, the bot can’t read message text. Optionally enable **Server Members Intent** if you want the bot to resolve member info.


### Step 2: Invite the Bot to Your Server


Under the OAuth2 tab, select the` bot` scope. Under Bot Permissions, select at minimum:


- Send Messages
- Read Message History
- Add Reactions
- Manage Messages (optional, for thread management)
- Use Slash Commands (optional)


Copy the generated invite URL and open it in your browser to add the bot to your server.


### Step 3: Configure OpenClaw


Add the token to your[OpenClaw config](https://docs.openclaw.ai/channels/discord) :


```text
{
channels  : {
discord  : {
enabled  :   true  ,
token  :   "YOUR_BOT_TOKEN"  ,
dmPolicy  :   "pairing"  ,
guilds  : {
"YOUR_GUILD_ID"  : {
channels  : {
general  : {   allow  :   true  ,   requireMention  :   true   }
}
}
}
}
}
}
```


Alternatively, set the token via environment variable:` DISCORD_BOT_TOKEN=YOUR_BOT_TOKEN` in your` .env` file.


To find your guild (server) ID, enable Developer Mode in Discord settings, then right-click your server name and select “Copy Server ID.”


### Step 4: Start the Gateway and Approve Pairing


```text
openclaw   gateway
```


Send a DM to your bot in Discord. With` dmPolicy: "pairing"` , you’ll need to approve the pairing request:


```text
openclaw   pairing   list   discord
openclaw   pairing   approve   discord   <  COD  E  >
```


In guild channels, the bot responds to @mentions by default (controlled by the` requireMention` setting per channel).


### Guild and Channel Access Control


The` guilds` config gives you per-server, per-channel control. Each guild entry can specify:


- ` channels` — which channels the bot is allowed to respond in
- ` requireMention` — whether the bot only responds to @mentions (default: true in guilds)
- ` users` and` roles` — allowlists for who can interact with the bot in that guild


If you want the bot active in all channels of a guild, set the guild-level config without specifying individual channels.


### Voice Channels


OpenClaw supports joining Discord voice channels for voice conversations:


```text
{
channels  : {
discord  : {
voice  : {
enabled  :   true  ,
autoJoin  : [
{   guildId  :   "YOUR_GUILD_ID"  ,   channelId  :   "VOICE_CHANNEL_ID"   }
]
}
}
}
}
```


### What to Watch


Discord rate limits are per-bot and per-channel. For typical use, they’re not an issue. If the bot connects but doesn’t respond, check that Message Content Intent is enabled in the Developer Portal — this is the most common setup mistake.


## How to Connect OpenClaw to WhatsApp


To connect OpenClaw to WhatsApp, run the login command, scan a QR code with your phone, and start the gateway. The initial setup takes about five minutes, but read the caveats before committing.


WhatsApp has[over 2 billion active users and 500 million businesses](https://chatmaxima.com/blog/business-messaging-statistics-2026/) using it as a daily communication tool. If your clients or team are already on WhatsApp, meeting them there has clear value. But WhatsApp integration is the most fragile of the four.


### Step 1: Run the Login Command


```text
openclaw   channels   login   --channel   whatsapp
```


This generates a QR code in your terminal.


### Step 2: Link Your Phone


On your phone, open WhatsApp and navigate to Settings > Linked Devices > Link a Device. Point your camera at the QR code. OpenClaw registers as a linked device on your WhatsApp account, the same way you’d link a laptop.


### Step 3: Configure Access and Start


Set your access policy in the[OpenClaw config](https://docs.openclaw.ai/channels/whatsapp) :


```text
{
channels  : {
whatsapp  : {
dmPolicy  :   "pairing"  ,
allowFrom  : [  "+15551234567"  ],    // these numbers bypass pairing
groupPolicy  :   "allowlist"
}
}
}
```


With` dmPolicy: "pairing"` , unknown senders receive a pairing challenge code and are blocked until approved. Numbers in` allowFrom` skip the pairing step and are allowed immediately. If you want to restrict access to only the numbers in` allowFrom` with no pairing flow at all, use` dmPolicy: "allowlist"` instead.


Run` openclaw gateway` and send a test message. The agent should respond automatically.


### The WhatsApp Caveats


This is where honesty matters more than enthusiasm.


**Unofficial API.** OpenClaw uses[Baileys](https://github.com/WhiskeySockets/Baileys) , an open-source library that mimics WhatsApp Web. There is no official bot API for personal WhatsApp accounts. From Meta’s perspective, there is no distinction between legitimate automation and a spam bot.[Reports of account bans](https://github.com/WhiskeySockets/Baileys/issues/1869) among long-time Baileys users have increased, with some accounts that operated for three or more years now being flagged.


**Session disconnects.** WhatsApp may disconnect the linked device after roughly 14 days. When that happens, re-run` openclaw channels login --channel whatsapp` and scan again. This is a WhatsApp limitation, not an OpenClaw bug.


**Meta’s policy direction.** Meta[banned general-purpose AI chatbots](https://respond.io/blog/whatsapp-general-purpose-chatbots-ban) on the WhatsApp Business API effective January 15, 2026. Baileys operates outside the Business API, so this ban doesn’t directly apply. But it signals where Meta is headed with AI automation on WhatsApp.


**Use a separate phone number.** The[OpenClaw docs recommend this](https://docs.openclaw.ai/channels/whatsapp) , and so do I. If your automation gets flagged, losing a dedicated number is inconvenient. Losing your personal WhatsApp is worse.


## Running Multiple Channels at Once


OpenClaw runs all channels simultaneously from a single gateway process. Configuration is additive: each channel gets its own section in the config with its own access policies. Adding Slack doesn’t affect your Telegram setup.


The gateway shares a session store across channels. If you bind multiple channel identities to the same user, a conversation started on Telegram can continue on Slack. The agent retains the context regardless of which app you’re messaging from.


A practical pattern that works well for Klaus customers: Telegram for personal and mobile use, Slack for team and workplace tasks, Discord for community engagement, WhatsApp for clients who already prefer it. One agent, multiple interfaces, shared memory.


## What Klaus Handles for You


For[managed hosting](https://klausai.com/pricing) customers, messaging setup is pre-configured. Pick your channels during onboarding and start talking. No tokens to generate, no OAuth apps to create, no gateway to manage.


Klaus runs Slack, Telegram, WhatsApp, and Discord for customers and handles reconnections automatically. Clawbert, our automated monitoring agent, watches instance uptime and re-authenticates WhatsApp sessions when they disconnect. You don’t have to re-scan a QR code at 2 AM.


If the configuration described in this guide sounds like more work than you want, that’s what managed hosting exists for. If you prefer to self-host, the[complete setup guide](https://klausai.com/blog/how-to-set-up-openclaw-complete-getting-started-guide) covers everything from installation to your first conversation.


## Frequently Asked Questions


### Can I use OpenClaw with iMessage?


Yes, but it’s harder to set up. iMessage works natively on macOS through the local Messages database, or cross-platform via BlueBubbles. Either way, it takes more configuration than the four platforms covered here. Klaus is rolling out iMessage support that provides an external number you can text to talk with Klaus. You won’t be able to use Klaus to send messages to others from your phone.


### What happens if WhatsApp bans my number?


You lose access to that WhatsApp account. This is why using a separate, dedicated number is essential. Baileys operates outside WhatsApp’s official API, so there’s inherent risk. If reliability matters more than reaching WhatsApp contacts, Telegram is the safer choice.


### Does OpenClaw work with Microsoft Teams?


Yes, via the Bot Framework with Azure AD app registration. It’s a more enterprise-heavy setup than the four platforms covered here. Microsoft Teams integration is best suited for organizations that already manage Azure AD.


### Can my agent respond in group chats?


Yes, on all four platforms. Telegram, Slack, and Discord require @mentions by default, which prevents the agent from responding to every message in a busy channel. WhatsApp group support is available, but access control is more limited since WhatsApp lacks the granular bot permissions of the other platforms.


### Which messaging channel is the most reliable for OpenClaw?


Telegram is the most reliable. It uses an official Bot API with no session disconnects and no terms-of-service grey area. Slack is a close second, with stable OAuth-based authentication. WhatsApp requires periodic re-authentication every couple of weeks and carries the risk of account restrictions.


## Key Takeaways


- Telegram is the easiest and most stable messaging integration for OpenClaw. Start there if you’re setting up for the first time.
- Slack is the best choice for team and workplace use. Socket Mode eliminates the need for public URLs or SSL certificates.
- Discord is ideal for communities and developer teams, with guild/channel-level access control and voice support.
- WhatsApp works but uses Baileys, an unofficial API. Use a dedicated phone number and expect to re-scan a QR code every couple of weeks.
- All four platforms can run simultaneously from one OpenClaw gateway with shared agent context.
- Access control (who can talk to your bot) is configured per channel via` dmPolicy` and` groupPolicy` settings.
- If setup friction is the problem, Klaus pre-configures messaging integrations for managed hosting customers.


---


Want messaging set up without the configuration?[Sign up at klausai.com](https://klausai.com/) .


Need to install OpenClaw first? Read our[complete setup guide](https://klausai.com/blog/how-to-set-up-openclaw-complete-getting-started-guide) . For ideas on what to do once messaging is connected, see[10 ways our customers actually use OpenClaw](https://klausai.com/blog/openclaw-use-cases-how-our-customers-actually-use-it) .


## Sources


### Primary Sources


- OpenClaw. “Telegram Channel Documentation.” 2026.[https://docs.openclaw.ai/channels/telegram](https://docs.openclaw.ai/channels/telegram)
- OpenClaw. “WhatsApp Channel Documentation.” 2026.[https://docs.openclaw.ai/channels/whatsapp](https://docs.openclaw.ai/channels/whatsapp)
- OpenClaw. “Slack Channel Documentation.” 2026.[https://docs.openclaw.ai/channels/slack](https://docs.openclaw.ai/channels/slack)
- OpenClaw. “Discord Channel Documentation.” 2026.[https://docs.openclaw.ai/channels/discord](https://docs.openclaw.ai/channels/discord)
- Telegram. “Bot API v9.5.” March 2026.[https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)
- Slack. “Using Socket Mode.” 2026.[https://docs.slack.dev/apis/events-api/using-socket-mode/](https://docs.slack.dev/apis/events-api/using-socket-mode/)


### Secondary Sources


- respond.io. “Not All Chatbots Are Banned: WhatsApp’s 2026 AI Policy Explained.” 2026.[https://respond.io/blog/whatsapp-general-purpose-chatbots-ban](https://respond.io/blog/whatsapp-general-purpose-chatbots-ban)
- WhiskeySockets/Baileys. “High number of bans on WhatsApp! (Issue #1869).” 2025.[https://github.com/WhiskeySockets/Baileys/issues/1869](https://github.com/WhiskeySockets/Baileys/issues/1869)
- ChatMaxima. “Business Messaging Statistics 2026.” 2026.[https://chatmaxima.com/blog/business-messaging-statistics-2026/](https://chatmaxima.com/blog/business-messaging-statistics-2026/)
