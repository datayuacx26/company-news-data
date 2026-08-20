---
schema_version: "1.0.0"
document_id: "fa3e71bfa8e28a57f1d081d8cc1c99a92411372e637a294010730328357b204c"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/how-to-set-up-openclaw-on-discord"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:44:23.277081+00:00"
content_hash: "sha256:10ef26e92369d7568b56186c1321a73c1fafefd7359c0c700d6b6a1f4a5238d5"
---

# How to set up OpenClaw on Discord, step by step

Discord is one of the steadier channels for an OpenClaw agent because it runs on a real bot API backed by[Discord's gateway](https://discord.com/developers/docs/events/gateway) , but the first run stalls in two predictable places. The bot connects, shows green, and then sits there silent because one privileged toggle is off, or the gateway refuses to start because the application ID and the bot token got crossed.


## Step 1: Create the application in the Developer Portal


A Discord bot is an application you register in the[Discord Developer Portal](https://discord.com/developers/applications) . Sign in with your Discord account, click New Application in the top right, give it a name, accept the developer terms, and create it. The name here is the app, and you can rename the bot user later.


Open the Bot page from the left sidebar. Older accounts had an Add Bot button; on current Discord the bot user is created with the application, so you land straight on its settings. This is also where you set the bot's display name and avatar, and where you decide whether it is public. A public bot can be added to any server by anyone with the invite link, so for a personal agent flip Public Bot off and keep the invite to yourself.


If you have not opened the Developer Portal before, Tech With Tim's tutorial covers the same ground in its opening section, creating the application, switching on the intents, and copying the token, before it carries on into writing a bot in Python that you can skip for this setup.


## Step 2: Turn on the Message Content Intent and copy your two secrets


On the Bot page, scroll to Privileged Gateway Intents and switch on Message Content Intent. Discord classifies message text as[privileged data](https://discord.com/developers/docs/events/gateway#privileged-intents) , so until you enable it the bot receives a message event with an empty` content` field and has nothing to read. If you also plan to scope the agent to specific people or roles, turn on Server Members Intent too, since OpenClaw's user allowlists rely on it. Save your changes.


Discord lets any bot in fewer than 100 servers use these privileged intents with no review, which covers a personal agent in your own server. The formal[intent request and verification](https://support-dev.discord.com/hc/en-us/articles/6205754771351-How-do-I-get-Privileged-Intents-for-my-bot) only enter the picture once a bot spreads past that threshold.


Now collect the two values OpenClaw needs. The first is the Application ID, sometimes called the client ID, on the General Information page. It identifies your app and is what lets OpenClaw register slash commands; it is not secret. The second is the bot token, back on the Bot page under Reset Token. Click Reset Token, confirm, and copy the string immediately, because Discord shows it once. That token is the bot's password, so treat it the way you would any credential. Anyone who has it can log in and act as your bot, and if it leaks you reset it from the same place, which instantly kills the old one.


Crossing these two is the single most common first run failure. The application ID and the token are different strings with different jobs, and pasting the token into the application ID slot produces` Failed to resolve Discord application id` when the gateway starts.


## Step 3: Invite the bot to your server


Creating the bot does not put it anywhere. You invite it with an OAuth2 URL that names the scopes and permissions you want. In the Developer Portal, open OAuth2, then URL Generator. Under scopes, select` bot` so it can join, and` applications.commands` so OpenClaw's slash commands appear in the message box. If you skip` applications.commands` , the bot still works but the` /` menu never shows up.


A scopes-only invite joins the bot with no abilities, so pick its baseline permissions in the same generator. For an agent that reads and replies, the useful ones are View Channels, Send Messages, Read Message History, Embed Links, Attach Files, and Send Messages in Threads if you lean on threads. Discord's[permissions reference](https://discord.com/developers/docs/topics/permissions) explains how a channel can override these, which is the usual cause when the bot looks fine at the server level but cannot post in one room. Copy the generated URL, open it, choose your server, and authorize. You need Manage Server on that server to add a bot.


## Step 4: Put the token and application ID in your config


OpenClaw reads channel settings from` ~/.openclaw/openclaw.json` . Add a Discord block with the two values from step 2 and leave the DM policy on its default of pairing, which is what stops strangers from talking to your agent:


```text
{
channels: {
discord: {
enabled: true,
applicationId: "123456789012345678",
token: "YOUR_BOT_TOKEN",
dmPolicy: "pairing",
},
},
}


```


If you would rather keep the token out of the file, OpenClaw also reads` DISCORD_BOT_TOKEN` from the environment, and config values win when both are set. Either way the token sits somewhere readable, so on a machine other people can log into, lock the state directory to your own user or hand the token through the environment instead of writing it into the file. The full set of options, including multi account setups under` channels.discord.accounts` , lives in the[Discord channel docs](https://docs.openclaw.ai/channels/discord) .


## Step 5: Start the gateway and pair your first DM


Start the gateway so the bot logs in to Discord:


```text
openclaw gateway


```


Watch for it to reach a logged in state rather than hang at` awaiting gateway readiness` . Now open a direct message with the bot. For Discord to deliver that DM at all, your server has to allow it, which you enable by right clicking the server icon, opening Privacy Settings, and turning on Direct Messages. Send any message. Because the DM policy is pairing, the bot replies with a short code instead of answering, and you approve that code from the machine running the gateway:


```text
openclaw pairing list discord
openclaw pairing approve discord <CODE>


```


Message the bot again and it answers. The first sender you approve becomes the command owner, so owner only commands work for you without extra setup. Confirm the channel is healthy before moving on:


```text
openclaw channels status --probe


```


A clean result shows the bot enabled, configured, and logged in. If the gateway hangs at readiness instead, that points at a known client lifecycle bug rather than your config, and the[Discord troubleshooting guide](https://operator.io/blog/debugging-openclaw-discord) walks through it along with the duplicate reply and` 4014` intent errors.


## Step 6: Let it speak in a server channel


A DM works after pairing, but a server channel has two more defaults to handle. OpenClaw uses a guild allowlist, so a server you have not listed stays blocked even though the bot is sitting in it. Add your server's ID under` channels.discord.guilds` , and inside the channel the agent is mention gated, meaning it stays quiet until you` @mention` it or reply to one of its messages:


```text
{
channels: {
discord: {
groupPolicy: "allowlist",
guilds: {
YOUR_SERVER_ID: {
requireMention: true,
users: ["YOUR_USER_ID"],
},
},
},
},
}


```


To get your server ID, enable Developer Mode in Discord under Settings, Advanced, then right click the server icon and choose Copy Server ID. The same menu copies your own user ID from your avatar, which is what goes in the` users` list.


Leave the mention gate on in any room with other people. Setting` requireMention: false` makes the agent read and act on every message posted in that channel, and any member can then point it at the integrations you connected, so the open setting fits a server that is only you or a small group that all expects the agent to be listening. For a shared channel, leave the gate on and widen the` users` list to name who is allowed to drive it. Once the basics work, the[usage guide](https://operator.io/blog/talk-to-your-agent-on-discord) covers slash commands, threads, and how sessions stay separate per channel.


## Or skip the setup entirely


None of these steps are hard on their own, but together they are a detour: register an app, flip a privileged intent, copy two different secrets without crossing them, invite with the right scopes, list your guild, and keep the plugin installing cleanly through every update.[Operator.io](https://operator.io/) runs OpenClaw with Discord wired up as a managed channel, so there is no application ID to resolve, no intent to toggle, and no pairing code to chase. You sign in and the agent is reachable in your server from the first message, so[a free trial](https://operator.io/pricing) gets it answering in under a minute. Work through the steps above instead if you would rather hold the token and run the bot on your own machine.
