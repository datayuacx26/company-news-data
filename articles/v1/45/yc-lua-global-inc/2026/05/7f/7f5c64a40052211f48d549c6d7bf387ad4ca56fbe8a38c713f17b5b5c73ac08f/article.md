---
schema_version: "1.0.0"
document_id: "7f5c64a40052211f48d549c6d7bf387ad4ca56fbe8a38c713f17b5b5c73ac08f"
company_key: "yc-lua-global-inc"
company: "Lua Global Inc"
source_id: "yc-lua-global-inc-rss-87d3f9cc68e5"
canonical_url: "https://blog.heylua.ai/250-services-two-clicks-one-agent/"
published_at: "2026-05-04T08:00:59+00:00"
first_seen_at: "2026-07-24T10:03:36.530538+00:00"
fetched_at: "2026-07-28T21:56:02.721351+00:00"
content_hash: "sha256:ab12ab531be9375fd048db3a5dd12adcd211bf7ca12a5c9640aa4fdc30a5e87f"
---

# 250+ services. Two clicks. One agent.

Today every Lua agent can plug into **250+ third-party services** — Linear, Gmail, HubSpot, Stripe, Notion, GitHub, Slack, Zendesk, and the long tail of CRM, accounting, calendar, ticketing, and HR tools your team already uses.


Click **Connect** , approve the OAuth popup, you're done. Your agent now has access to your data and can act on it — answering customer tickets, updating deals, summarizing meetings, scheduling follow-ups, whatever its job is.


No code. No tool definitions. No "first, install our SDK." Just connect.


## What it looks like


Open any agent in the[Lua admin dashboard](https://admin.heylua.ai/?ref=blog.heylua.ai) and click into the visual builder. You'll see a curated catalog grouped into **Power Packs** — Productivity, Communication, Customer, Developer — and a search across the full 250+ list when you need something specific.


Pick the integration. The OAuth popup opens. Approve the scopes. The integration is live, and your agent immediately gains the ability to read from and write to that service.


**The visual builder — channels feed into the agent on top, integrations attach as nodes, triggers branch off as wake-up events on the right.**


Developers can do the same thing from the terminal — same flow, same outcome:


```text
lua integrations connect
```


**The CLI walks you through the same flow — pick the integration, the auth method, the scopes, the triggers.**


Every step has a flag for non-interactive use, so the same flow runs in CI.


## Triggers — your agent wakes up when something happens


Connecting a service gives your agent the ability to act *on demand* . Triggers are how it acts *automatically* .


When you connect Linear, you can opt into events like "a new task was created," "a status changed," or "someone added a comment." Each one wakes the agent the moment it happens. Same idea for HubSpot deal updates, Stripe payment failures, Zendesk ticket replies, GitHub PRs.


You don't poll. You don't write a webhook handler. The agent runs when something real happens in your business.


Triggers can be paused and resumed anytime without disconnecting the integration — useful when you're testing, deploying a change, or just don't want a noisy CRM pinging the agent at 3am.


## Under the hood — every integration becomes a toolset


Here's the part that makes it work without code: when you connect, say, Linear, Lua automatically generates a complete set of tools for your agent — *create issue, update status, assign user, query projects, comment* , every action Linear's API exposes. Your agent picks the right tool for the job, the same way it picks any other tool.


Technically, each connection becomes an **MCP (Model Context Protocol) server** dedicated to that integration. If you've worked with MCP before, that one sentence tells you everything you need to know. If you haven't, it doesn't matter — the upshot is your agent gets the right toolset for every service, automatically, with zero work on your end.


## The catalog


The full catalog spans **24 categories** : CRM, accounting, calendar, commerce, payments, ticketing, messaging, marketing, ATS, HRIS, knowledge management, storage, shipping, task management, dev repos, GenAI, and more. We work with[Unified.to](https://unified.to/?ref=blog.heylua.ai) as the integration backbone, which means coverage of essentially every major B2B SaaS.


Eight integrations are turnkey OAuth right now — click and you're connected: **Linear, Gmail, Google Calendar, Google Drive, Google Docs, Discord, HubSpot** , plus a generic Google auth flow.


For the other 245+, we set up the OAuth credentials on request — usually within a day. Ping the Lua support agent, drop into our Discord, or email us. We'll have it live for you fast, and the catalog grows as customers ask for more.


The most-connected integrations on Lua today: **Linear, Gmail, Google Calendar, Google Drive, Discord, Notion, GitHub, OpenAI, ActiveCampaign, Stripe** . Notion's worth calling out — that one isn't on the OAuth-ready list yet, customers are connecting it via API token, which proves the on-request path actually works in practice.


## A few patterns we've seen


- **Linear triage agent** — connect Linear, switch on the "task created" trigger, and the agent drafts a triage comment plus assigns labels the moment a ticket lands.
- **Stripe ops agent** — listen for failed payments, agent runs the dunning flow, optionally pings the customer over WhatsApp through Lua's voice and messaging stack.
- **HubSpot pipeline assistant** — listen for deal-stage changes, agent enriches the deal with context and posts a summary into Slack so the AE walks in prepared.
- **GitHub PR reviewer** — listen for PR opened, agent runs a first-pass review and leaves comments before the human team gets to it.
- **Customer support agent** — connect Zendesk and Gmail, agent reads inbound tickets, drafts replies, escalates the hard ones.


None of those required writing tools. The integrations brought their own.


## Get started


Open any agent in the[Lua admin dashboard](https://admin.heylua.ai/?ref=blog.heylua.ai) and click into the visual builder.


Or, if you live in the terminal:


```text
lua integrations connect
```


Don't see the integration you need on the OAuth-ready list? Tell us — support agent, Discord, email — and we'll wire up the credentials. The catalog grows on request.


Connect once. Your agent gets to work.
