---
schema_version: "1.0.0"
document_id: "e47819f92e9d87b8aef512d1d54a7a358a380107c086011950cb7474a1364cf8"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-mcp-connectors/"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T22:13:07.113521+00:00"
content_hash: "sha256:985a05eca59f3f9aa36b7ccccc15254520865114d8874a35a188e6faa674cada"
---

# Introducing Basedash MCP connectors

Today we’re launching **MCP connectors** — a way to plug any external Model Context Protocol server into Basedash so the agent can take action on the data it already reads.


Basedash already pulls from your databases, warehouses, and 750+ SaaS tools. Connect an MCP server — Linear, HubSpot, Slack, Resend, Notion, GitHub, your own internal one — and every tool that server exposes becomes available to the Basedash agent inside chat and[Automations](https://www.basedash.com/features/automations) .


Ask it to email this week’s signups a personalized welcome based on the features they set up. File a Linear bug from a support ticket and link the user record. Update HubSpot leads for everyone who hit the paywall yesterday. Connect any app. Act anywhere.


## Why we built MCP connectors


BI tools have always been read-only. They can tell you a signup cohort is stuck, a support issue is recurring, or a customer record needs attention — but the moment you want to do something about it, you leave the dashboard and open another tab.


That hand-off is where most “data-driven” workflows quietly break. The analysis happens in one place, the action happens in another, and the loop never fully closes. The answer to “what should we do about this?” gets lost on the way to the inbox or the CRM or the issue tracker.


MCP connectors close the loop. Once an external MCP server is connected, its tools sit right next to your data sources. The agent can read from your database and act in Linear in the same turn. It can compare cohorts and update HubSpot from the same prompt. The follow-through happens where the analysis happens.


This is the natural next step for the agent. We’ve spent the last year making it a great analyst. Now it can also be a great operator — with all the same governance.


## How MCP connectors work


Adding a connector takes about a minute:


1. **Open the command menu** in Basedash, go to Data sources, and choose **Add MCP server** .
2. **Enter a name and the remote MCP server URL.** Streamable HTTP and SSE are both supported. Add headers if the server needs them.
3. **Complete OAuth** if the server prompts for it. Basedash syncs the available tools.
4. **Use it.** The connector now appears alongside your other data sources in the command menu, and every synced tool is available to the agent inside chat and Automations.


Behind the scenes, Basedash speaks remote MCP over streamable HTTP or SSE, handles OAuth refresh, and re-syncs tools whenever the upstream server changes.


## If it speaks MCP, it works


Out of the box, our customers are connecting the apps they already live in.


- **Linear** — file bugs, update issues, link the right user record
- **HubSpot** — update leads, sync deals, fire workflows from product signal
- **Slack** — post into the right channel based on what just happened in the data
- **Resend** — send personalized email to a list the agent just generated
- **Notion** — drop a structured brief into the right doc or database
- **GitHub** — open an investigation issue with the chart and the SQL attached
- **Stripe, Intercom, your own internal MCP server, anything that speaks remote MCP** — same flow, same governance


If a service exposes a remote MCP server, Basedash can use it. If you have an internal MCP server in front of an internal API, point Basedash at that too.


## What you can ask for


The shape we’ve seen most often: read your governed data, then act in another app.


- **“Email this week’s signups a personalized welcome based on the features they actually set up.”** Reads your production DB and product events; sends through` resend.send_email` .
- **“File a Linear bug from this support ticket and link the user record.”** Reads Intercom and your production DB; creates the issue through` linear.create_issue` .
- **“Update the HubSpot lead for everyone who hit the paywall this week.”** Reads your product events; updates contacts through` hubspot.update_lead` .
- **“Post a Slack thread in #wins for any 100+ seat company that signed up today, with the account context.”** Reads your CRM and product DB; posts via` slack.post_message` .
- **“Open a GitHub investigation issue with this dashboard attached when error rate doubles.”** Reads your logs and alerts; opens the issue via` github.create_issue` .


Same pattern every time: governed read, governed action, in one prompt.


## Trust the tools you’ve vetted. Gate the rest.


Read-only BI is safe by default. Action-taking isn’t — so we built MCP connectors around explicit human review.


Every synced tool gets one of three access modes:


- **Always allow.** Trusted tools run without interruption — read-only lookups, well-scoped helpers, anything you’ve vetted.
- **Needs approval.** Higher-stakes actions pause for a human review of the full payload before they run. New tools start here by default.
- **Blocked.** Anything you don’t want the agent to touch — flip a tool off and it disappears from the available toolset.


When the agent calls a` Needs approval` tool, a review card appears in the chat with the exact payload — recipients, subject, body, every field — so you can approve or reject before anything fires. The same approval flow works in Slack, so the team can keep moving without jumping back into Basedash.


## Scope each connector to the right people


Connectors also carry their own audience. You can:


- Open a connector up to **everyone in the org**
- Restrict it to **specific groups** — only Support sees Intercom, only Growth sees HubSpot
- Hand it to **specific members** when the action is sensitive or scoped to a role


Pair connector-level scoping with tool-level approval modes and you get fine-grained control over both who can use a connector and which actions inside it run automatically.


## Run the same flow on a schedule


Every tool a connector exposes is also available to[Automations](https://www.basedash.com/features/automations) . The one-off agent run that emails this week’s signups becomes a weekday-9am workflow that emails this week’s signups, every week, forever — same prompt, same governance, no extra setup.


That’s where MCP connectors get really interesting. A one-prompt experiment becomes a recurring operation the moment it works. Daily ICP welcome. Weekly HubSpot refresh. Real-time bug-filing from support escalations. Sales pipeline updates the moment a deal hits a stage.


## How we use MCP connectors at Basedash


We’ve been dogfooding connectors internally. A few favorites:


- **Welcome emails** — every weekday morning, an automation reads new signups from production, joins them with what they did in product, and sends a personalized welcome through Resend. No human in the loop after the initial approval.
- **Bug intake** — when a support ticket comes in that looks like a bug, the agent files a Linear issue with the customer context attached, then posts a link back in Intercom.
- **Customer-health flags** — every Monday, accounts trending down get pushed into HubSpot as tasks for the right owner, alongside the chart that flagged them.


These used to be the slow seams between teams. Now they’re loops the agent runs, with the team reviewing the actions that matter most.


## Getting started


MCP connectors are available today for all Basedash workspaces.


1. [Sign up for Basedash](https://charts.basedash.com/signup) (or log in)
2. Open the command menu and go to **Data sources** → **Add MCP server**
3. Paste a remote MCP server URL, complete OAuth if prompted, and start using its tools in chat
4. Pair with[Automations](https://www.basedash.com/features/automations) when you’re ready to run the same flow on a schedule


For more, see the[MCP connectors feature page](https://www.basedash.com/features/mcp-connectors) or the[docs](https://www.basedash.com/docs/data-sources/mcp-connectors) .


## What’s next


MCP connectors complete the picture. The[Basedash MCP server](https://www.basedash.com/features/mcp-server) lets any AI client read your governed data. MCP connectors let the Basedash agent take action in any tool your team already uses. Together — alongside[AI chat](https://www.basedash.com/features/ai-data-analyst) ,[Insights](https://www.basedash.com/features/insights) ,[Automations](https://www.basedash.com/features/automations) , and the[Dashboard Agent](https://www.basedash.com/features/dashboards) — Basedash is now the place where governed analytics and AI-driven action live in the same workspace.


**Try MCP connectors today and turn the data you already trust into actions you can ship.**
