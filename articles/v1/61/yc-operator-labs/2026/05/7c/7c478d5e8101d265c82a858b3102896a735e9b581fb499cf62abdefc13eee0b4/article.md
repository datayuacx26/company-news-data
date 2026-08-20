---
schema_version: "1.0.0"
document_id: "7c478d5e8101d265c82a858b3102896a735e9b581fb499cf62abdefc13eee0b4"
company_key: "yc-operator-labs"
company: "Operator Labs"
source_id: "yc-operator-labs-news-import-4e5219b8eb88"
canonical_url: "https://operator.io/blog/automate-anything-with-composio"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T07:49:23.882568+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:a8b642d251421ade312696fbe1836c52c68988c41a4f5a393f6f91407aa30fd9"
---

# How to use Composio with OpenClaw to automate anything

Most of the work you would hand an agent lives inside other people's apps. Your ad spend is in Google Ads, your inbox is in Gmail, your team talks in Slack, your tasks sit in Linear or Notion. Wiring an agent into each of those one at a time is the slow part.


Composio collapses that into a single connection. You link your own Composio account to[Operator.io](https://operator.io/) once, and from then on the agent can reach the apps Composio supports without you setting up another integration.


## What Composio gives your agent


Composio publishes its whole catalog as one[Model Context Protocol](https://modelcontextprotocol.io/) server called the Tool Router.


Rather than adding a separate server for Google Ads, another for Slack, and another for every tool after that, your agent points at a single endpoint and Composio decides which tools to surface based on what you asked for. Their[toolkit directory](https://composio.dev/toolkits) lists ad platforms, CRMs, email, calendars, project trackers, databases, and the long tail of SaaS you actually use.


The part that saves you the most time is authentication. Composio holds the OAuth connection for each app you link and refreshes the tokens on its own, so you are not pasting API keys or babysitting expiry.


The[Tool Router docs](https://github.com/ComposioHQ/composio/blob/next/ts/docs/api/tool-router.md) cover how the session and tool discovery work if you want the mechanics. Because the router only loads the tools that match the task at hand, your agent's context stays lean even though the catalog behind it is large.


For a look at what the platform feels like in practice, Composio's own demo walks through the dashboard, the actions an agent can call inside each app, and the connected accounts that hold the managed authentication.


Composio is one of a few aggregators that expose many apps through MCP, alongside[Zapier's MCP](https://zapier.com/mcp) and[Pipedream Connect MCP](https://pipedream.com/docs/connect/mcp) . They differ in catalog shape and billing.


Composio's draw for personal agents is breadth across everyday SaaS plus a usable free tier on[Composio pricing](https://composio.dev/pricing) : twenty thousand standard tool calls per month on the "Totally Free" plan, with premium tools like search sandboxes metered separately on the[premium tools page](https://docs.composio.dev/toolkits/premium-tools) .


## Your own Composio account


This setup uses your own Composio account. When you connect, you sign in to Composio yourself, so the apps you link, the OAuth tokens behind them, and the usage that runs through them all sit under your account. Operator is not handing you a managed key to a central Composio project, and nothing you connect is pooled with other people.


You start on Composio's free tier, and the paid tiers are public:


Tier Standard tool calls a month Price


Free 20,000 no card on file


Paid 200,000 $29 a month


Paid 2,000,000 $229 a month


If you blow past the included calls, Composio bills per thousand at the rates on its[pricing page](https://composio.dev/pricing) .


## Connect Composio in Operator


Open the MCPs page in your Operator dashboard. Composio is the first tile at the top.


1. Click Add on the Composio tile. A window opens at Composio's sign in page.
2. Sign in or create your Composio account, then approve the access.
3. The window closes and Composio shows up as connected. Operator stores the token and keeps it refreshed, so this is a one time step.


There is nothing to restart and no config file to edit. The connection registers with your agent the moment it is approved.


## Link the apps you want it to use


Connecting Composio does not switch on any apps by itself. Each app gets linked the first time you ask the agent to use it.


Tell the agent to do something in Google Ads, and Composio hands back a link to authorize your Google Ads account. You click it, approve the access, and the agent runs the action.


Gmail, Slack, Notion, and the rest follow the same pattern: the first request for a new app prompts a quick authorization, and after that the agent just uses it.


If you want tighter control over what the agent can touch, the[Composio dashboard](https://composio.dev/) lets you bring your own OAuth credentials for an app and choose the scopes it is granted, instead of using Composio's shared credentials. That is the route to take for anything you run in production.


## Put it to work


You talk to OpenClaw on Telegram, so connect it on the channels page if you have not already. Then start with a request that reads rather than writes, where you can sanity check the result:


```text
Pull yesterday's Google Ads spend by campaign and list the five that spent the most.


```


Once you trust how it reads an account, let it act, and chain a couple of apps together:


```text
Take the top five campaigns by spend yesterday and post them to my #marketing Slack channel as a short summary.


```


```text
Create a Linear issue from the last email in this Gmail thread and reply to let them know it is logged.


```


Because the agent holds context across the conversation, you can follow one request with another and it carries the thread through. The same requests become standing automation once you put them on a schedule.


Ask the agent to send that ad spend summary every weekday morning, and it runs on its own from then on. The[prompts library](https://operator.io/prompts) has ready made starting points you can send to your agent and adapt.


## Good to know


Everything the agent runs counts against your own Composio plan, and the free tier's twenty thousand standard calls a month go a long way for personal and small team use. Premium tools burn a separate premium quota at roughly three times the cost of a standard call, which matters if you lean on search or sandbox actions heavily.


To stop the agent from reaching a single app, disconnect that app in your Composio dashboard. To remove Composio entirely, delete it from the MCPs page in Operator, which drops the connection for that instance.


Composio is open source under the MIT license, so if you want to read how the Tool Router works or run it yourself, the code is on[GitHub](https://github.com/ComposioHQ/composio) . For most people the hosted connection is the faster path.
