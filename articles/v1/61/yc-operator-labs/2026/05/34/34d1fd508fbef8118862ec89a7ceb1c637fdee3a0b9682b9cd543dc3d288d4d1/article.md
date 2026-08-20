---
schema_version: "1.0.0"
document_id: "34d1fd508fbef8118862ec89a7ceb1c637fdee3a0b9682b9cd543dc3d288d4d1"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/set-up-your-discord-channel-on-operator"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:44:23.277081+00:00"
content_hash: "sha256:28843fdbe48fc781a61047d326f2a51fdf556d584aabe820f21941aae1082278"
---

# Set up your Discord channel on Operator

[Operator.io](https://operator.io/) is managed OpenClaw, so connecting Discord happens on the dashboard rather than in a config file. You still create your own bot in Discord's Developer Portal, because Discord only ever talks to a registered application, but Operator handles the parts that make self hosting fiddly: storing the token, restarting your agent, pairing your account, and setting the server rules.


## Step 1: Create the bot in the Developer Portal


Open the[Discord Developer Portal](https://discord.com/developers/applications) , sign in, and click New Application. Give it a name and create it. Open the Bot page from the left sidebar, where the bot user already exists, and set its display name and avatar. For a personal agent, switch Public Bot off so only you can add it to a server.


While you are on the Bot page, click Reset Token, confirm, and copy the token that appears. Discord shows it once, so paste it somewhere safe for a moment. That token is what Operator uses to log your bot in, and anyone who has it can act as your bot, so treat it like a password and reset it from this same page if it ever leaks.


## Step 2: Turn on the privileged intents


Still on the Bot page, scroll to Privileged Gateway Intents and enable all three: Presence Intent, Server Members Intent, and Message Content Intent. The one that matters most is Message Content, because[Discord treats message text as privileged data](https://discord.com/developers/docs/events/gateway#privileged-intents) and the bot reads empty messages without it. A bot missing these will connect and then drop with gateway error 4014. Save your changes. Discord lets any bot in fewer than 100 servers use these intents with no review, which covers a personal agent in your own server.


MrJAwesome's guide walks the same Privileged Gateway Intents screen and shows why Message Content is the toggle that keeps a bot from connecting but reading nothing, the error 4014 case above.


## Step 3: Invite the bot to your server


Open the Installation tab. Under Guild Install, add the scopes` bot` and` applications.commands` , the second of which is what makes Operator's slash commands show up when you type` /` . Then enable the permissions the agent needs to read and reply: View Channels, Send Messages, Send Messages in Threads, Create Public Threads, and Read Message History. Discord's[permissions reference](https://discord.com/developers/docs/topics/permissions) covers how a single channel can override these if the bot looks fine at the server level but cannot post in one room.


Copy the install link from that same page, open it, choose your server, and authorize. Adding a bot needs the Manage Server permission on that server.


## Step 4: Paste the token into Operator


Open the Channels page in your Operator dashboard, click Add, and pick Discord for your agent. Paste the bot token from step 1, and enter your Discord username in the field marked for auto pairing. Click Connect.


Operator stores the token, restarts your agent to pick it up, and allowlists your username so you are paired the moment you message the bot. The restart takes a short while, and the page reflects it. There is no` openclaw.json` to edit and no` openclaw pairing approve` to run from a terminal, which is the difference from running the bot yourself.


## Step 5: Set the server policy


Once the token is saved, the Discord panel shows a server configuration section. The server policy decides where the bot answers:


- Disabled keeps it to direct messages only.
- Open lets it respond in any server it is added to, when mentioned.
- Allowlist limits it to the servers you name.


Pick allowlist for most setups and paste your server ID. To get that ID, enable Developer Mode in Discord under Settings, Advanced, then right click the server icon and choose Copy Server ID. Leave the require mention toggle on in any channel with other people, since with mentions off the bot reads and acts on every message posted and any member can steer it toward the apps you connected. For a server that is only you, turning it off makes the agent respond to everything.


## Talk to it, and where to go deeper


Open a direct message with the bot or mention it in an allowed channel, and it answers like any other assistant. The day to day commands, threads, and how each channel keeps its own session are covered in[talking to your agent on Discord](https://operator.io/blog/talk-to-your-agent-on-discord) . To give it real work to do, connect Composio or Pipedream on the Integrations page so it can reach Gmail, Slack, Notion, and your other apps.


If you would rather run the bot on your own machine instead of through the dashboard, the[self hosted Discord setup](https://operator.io/blog/how-to-set-up-openclaw-on-discord) walks through the config file and CLI version of all this, and the[Discord troubleshooting guide](https://operator.io/blog/debugging-openclaw-discord) covers the application ID, intent, and duplicate reply errors that come up either way.
