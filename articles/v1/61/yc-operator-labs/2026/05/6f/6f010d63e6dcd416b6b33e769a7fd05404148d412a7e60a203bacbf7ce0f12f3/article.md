---
schema_version: "1.0.0"
document_id: "6f010d63e6dcd416b6b33e769a7fd05404148d412a7e60a203bacbf7ce0f12f3"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/connect-pipedream-to-your-agent"
published_at: "2026-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:14f3f7e26668662413320c9d6ad046e1e3740ee986020e896cfa958aeb5430fe"
---

# Connect Pipedream to your agent and reach the apps you use

Most of what you would hand an agent lives inside apps you already pay for. Your mail is in Gmail, your team talks in Slack, your notes and tasks sit in Notion or Linear. Wiring an agent into each of those by hand, one OAuth client and token store at a time, is the part that eats an afternoon.


Pipedream collapses that into a single connection. You link your own Pipedream account to[Operator.io](https://operator.io/) once, turn on the apps you want it to reach, and from then on the agent calls them without you registering anything else.


## What Pipedream gives your agent


[Pipedream Connect](https://pipedream.com/docs/connect) exposes the apps you authorize as a[Model Context Protocol](https://modelcontextprotocol.io/) server. Instead of adding a separate integration for Gmail, another for Slack, and another for every tool after that, your agent points at one Pipedream endpoint and calls the tools for the apps you have switched on. Pipedream's[app directory](https://pipedream.com/apps) spans email, calendars, CRMs, project trackers, databases, and the long tail of SaaS people run.


What saves the most time is the auth. Pipedream holds the OAuth connection for each app you link, keeps the credentials in its own vault, and refreshes the tokens for you, so the agent keeps working instead of failing the morning a refresh token expires. The[MCP docs](https://pipedream.com/docs/connect/mcp) describe how the server resolves tools per app if you want the mechanics. Because the credential never enters the model's prompt, a stray instruction in a conversation cannot read it back out.


Pipedream is one of a handful of brokers that surface many apps through MCP, alongside[Composio](https://operator.io/blog/automate-anything-with-composio) and[Zapier's MCP](https://zapier.com/mcp) . They overlap heavily and differ in catalog shape, billing, and how tools are loaded. Pipedream's draw for a personal agent is the breadth of its app registry and a free plan you can start on without a card, with usage metered in credits on the[pricing page](https://pipedream.com/docs/pricing) .


## Your own Pipedream account


This setup runs on your own Pipedream account. When you connect, you sign in to Pipedream yourself, so the apps you link, the OAuth grants behind them, and the usage that runs through them all sit under your account. Operator is not handing you a key to a central project, and nothing you connect is pooled with other people.


That ownership is also where control lives. To cut the agent off from one app, disconnect that app in your Pipedream account. To remove the whole connection, delete Pipedream from the Integrations page in Operator. For anything you run in production, Pipedream lets you bring your own OAuth client per app and pin the scopes it is granted, instead of leaning on the broker's shared credentials.


## Connect Pipedream in Operator


Open the Integrations page in your Operator dashboard. Pipedream sits near the top with Composio under Recommended.


1. Click Add on the Pipedream tile. A window opens at Pipedream's sign in page.
2. Sign in or create your Pipedream account, then approve the access.
3. The window closes and Pipedream shows up as connected. Operator stores the token and keeps it refreshed, so this is a one time step.


There is no config file to edit and nothing to restart. The connection registers with your agent the moment it is approved.


## Turn on the apps you want, then reconnect


Connecting Pipedream to Operator does not switch on any apps by itself; you enable each one separately. From the Pipedream server in your Integrations list, the Connect your apps button takes you to[mcp.pipedream.com](https://mcp.pipedream.com/) under your account, where you enable Gmail, Slack, Notion, or whatever you need and complete each app's sign in once.


After you enable a new app in Pipedream, come back to Operator and press Reconnect on the Pipedream server. That is what pulls the new app's tools into the connection so the agent can see them. Skip it and the app is linked on Pipedream's side while the agent still shows the old tool list, which reads like the connection failed when it just has not refreshed yet. Enabling Gmail and then asking the agent to read your inbox without reconnecting is the most common version of that confusion.


## Put it to work


You reach OpenClaw on Telegram, so connect the channel first if you have not. Start with a request that reads rather than writes, where the output is easy to check:


```text
Read my unread Gmail from the last day and tell me which three need a reply.


```


Once you trust how it reads an account, let it act and chain a couple of apps:


```text
Take that email from Priya about Thursday and draft a reply confirming 2pm, but show it to me before sending.


```


```text
Summarize the last 20 messages in my #launch Slack channel and save the summary as a new page in Notion.


```


Because the agent carries context across the conversation, you can follow one request with the next and it holds the thread. The same requests turn into standing automation once you put them on a schedule, so a "summarize my unread mail and post it to Slack" becomes a thing that runs every weekday morning on its own. The[prompts library](https://operator.io/prompts) has starting points you can send to your agent and adapt.


## Good to know


Everything the agent runs counts against your own Pipedream plan, and the free plan covers a lot of personal use before you look at a paid tier. Pipedream meters in credits where one credit is roughly thirty seconds of compute, so a chatty agent that makes many small calls behaves differently from one running a few heavy actions; the current limits are on the[pricing page](https://pipedream.com/docs/pricing) .


Pipedream's integration registry is open source on[GitHub](https://github.com/PipedreamHQ/pipedream) , so you can read how an app's actions and triggers are defined or contribute one. If you would rather keep Google access entirely on your own hardware instead of through a broker, the[Gmail without a Google Cloud project](https://operator.io/blog/connect-gmail-to-your-agent-without-google-cloud) guide weighs the managed path against running the tokens yourself. For most people the hosted connection is the faster route.
