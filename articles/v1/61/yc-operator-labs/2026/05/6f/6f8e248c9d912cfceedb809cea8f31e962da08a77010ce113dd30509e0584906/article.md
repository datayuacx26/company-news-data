---
schema_version: "1.0.0"
document_id: "6f8e248c9d912cfceedb809cea8f31e962da08a77010ce113dd30509e0584906"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/set-up-your-telegram-channel-on-operator"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:44:23.277081+00:00"
content_hash: "sha256:c6141d209a5110aee1e8b94cf49d9f40333906cafc77307601bbf2d829f63e01"
---

# Set up your Telegram channel on Operator

[Operator.io](https://operator.io/) is managed OpenClaw, so connecting Telegram happens on the dashboard. You make your own bot with BotFather, because every Telegram bot is its own token, but Operator takes care of the rest: it stores the token, restarts your agent, and pairs your account by listening for your first message. No config file, no pairing code typed into a terminal.


## Step 1: Create a bot with BotFather


Every Telegram bot starts with a token from Telegram's own bot,[BotFather](https://core.telegram.org/bots#botfather) . In the Telegram app, search for` @BotFather` , confirm the blue verification check so you have the real one, and open the chat. Send` /newbot` , then answer the two prompts: a display name, which can have spaces, and a username that is unique across Telegram and ends in` bot` , for example` myagent_helper_bot` .


BotFather replies with a token that looks like` 123456789:AAExampLEtoKEN` . That string is the bot's credential, so keep it to yourself. If it ever leaks, open BotFather, pick your bot under` /mybots` , and use Revoke access token to mint a new one and kill the old.


## Step 2: Paste the token into Operator


There are two places to connect, and both want the same token. New accounts land in the guided setup, which walks you straight through Telegram: paste the token, click Connect, and Operator saves it. If you are already in the dashboard, open the Channels page, click Add, pick Telegram, and paste the token there. On the Channels page you also paste your numeric Telegram user ID, which you get by messaging[@userinfobot](https://t.me/userinfobot) ; the guided setup gets that ID for you in the next step instead.


Either way, saving the token restarts your agent so it picks up the bot. The dashboard shows the restart and settles on its own in a short while.


## Step 3: Say hello and get paired


Operator handles the pairing here. Open your bot in Telegram, tap Start if the app shows it, and send any message. The guided setup is listening for that message, and the account that sends it gets paired and allowlisted, so the next thing you say gets a real reply. The first account paired becomes the one allowed to drive the agent, which is what keeps a stranger who stumbles on the bot from talking to it.


If you connected from the Channels page with your user ID already pasted, you are paired the moment the token saves, and you can just message the bot. Should the listen step ever miss your message, the dashboard lets you paste your numeric user ID from @userinfobot to finish by hand.


## Step 4: Add it to a group, if you want


The Telegram panel on the Channels page has a group chat policy. Disabled keeps the bot to direct messages, open lets it respond in any group it joins, and allowlist limits it to the groups you name. To run it in a specific group, add the bot to that group, get the group's ID from a helper like[@RawDataBot](https://t.me/RawDataBot) , and paste the ID under allowlist. Group IDs are negative numbers that start with` -100` .


Leave the require mention toggle on for any group with other people in it, so the bot only answers when it is tagged rather than reading every message in the room. For a group that is just your own workspace, turning it off lets the agent follow the whole conversation.


## Talk to it, and where to go deeper


In a direct message you just type and the agent answers; the streaming previews, command menu, and per chat memory are covered in[talking to your agent on Telegram](https://operator.io/blog/talk-to-your-agent-on-telegram) . To give it work that reaches your other apps, connect Composio or Pipedream on the Integrations page so it can read your inbox, post to Slack, or update Notion.


If you would rather run the bot on your own machine, the[self hosted Telegram setup](https://operator.io/blog/how-to-set-up-openclaw-on-telegram) covers the config file and the` openclaw pairing approve` flow, and[Telegram not working](https://operator.io/blog/openclaw-telegram-not-working) collects the fixes for tokens, pairing, and group permissions that come up either way.
