---
schema_version: "1.0.0"
document_id: "c2a60e5dbefb335967679f59e3ae34eafd14719978a1f31e5c7c29b78735d9ee"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/introducing-basedash-actions/"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:2834ecd4d818a9f23d85f14f21da236d5fd931f929ee9732fb7caa2cf3c4f919"
---

# Introducing Basedash Actions

Today we’re launching **Actions** — the Basedash agent can now change things, not just report on them.


Two new capabilities land together. **Data editing** lets the agent write and run SQL against any database connection an admin has enabled for edits — extend a trial, fix a bad record, update the state of a hundred items at once. And **MCP actions** let the same agent do things in the rest of your stack: update a subscription in Stripe, create a lead in HubSpot, send an email, file an issue — anything a connected MCP server exposes.


Both run on the same rule: nothing consequential happens without a human saying yes. The agent shows you the exact SQL or tool payload, and you approve or deny before it runs.


## Why we built Actions


BI tools have always stopped at the answer. They can tell you a customer’s trial expired yesterday, that a record is wrong, or that a subscription is out of sync — and then hand the actual work back to you. The analysis lives in one tab; the follow-through lives in five others.


That last step is where data-driven workflows quietly die. You leave the dashboard, open a SQL client, double-check the WHERE clause, switch to Stripe, then to email. Every hop loses context the agent already had.


Actions close that gap. The same agent that found the answer — with the full context of your schema, your definitions, and your governance rules — can now make the change, in the database and in the tools around it, from the same conversation.


## Data editing: the agent can write SQL, not just read it


Admins can now enable **Allow edits** on any SQL database connection. Once it’s on, users with access to that data source can ask the agent to change things, and it writes the SQL itself.


This isn’t limited to flipping single values. Because the agent writes real SQL with the full intelligence of your connected data, it handles the messy, multi-row work too:


- **“Extend the trial for every org that signed up during the outage.”** One UPDATE, scoped exactly to the affected accounts.
- **“Mark these 200 stale tasks as archived.”** A bulk state change you’d otherwise script by hand.
- **“Set up a demo org with realistic sample data.”** INSERTs across the right tables, respecting your schema.


Basedash started as a read-only BI tool. With data editing, it becomes a tool that can also configure and operate the systems your data lives in — using everything it already knows about them.


## MCP actions: do things in the rest of your stack


We[launched MCP connectors](https://www.basedash.com/blog/introducing-basedash-mcp-connectors) recently, and reading data through them was only half the story. The other half is tools that *do* things — and that’s where connectors get interesting.


Connect any MCP server and the agent can act through every tool it exposes:


- **Stripe** — update subscriptions, apply credits, extend trials on the billing side
- **HubSpot** — create leads, update contacts from product signal
- **Resend** — send personalized email to a list the agent just built
- **Linear, Slack, GitHub, Notion** — file issues, post updates, write docs
- **Anything that speaks MCP** — including your own internal servers


Ask once, and the agent chains the read and the action together: it looks up the account in your database, then makes the matching change in Stripe, in one governed conversation.


## Approval is the interface


Giving an agent write access only works if the human stays in control. So every consequential action pauses for review.


When the agent wants to edit your database, you see an approval card with the data source, a plain-language description of the change, and the exact SQL it wants to run. Nothing executes until you hit **Approve and run** . Deny it, and nothing happens — the agent moves on.


MCP tools go further with per-tool permissions. Every tool on a connector gets one of three modes:


- **Always allow** — trusted tools run without interruption
- **Needs approval** — the agent pauses and shows you the full payload before the tool fires (the default for new tools)
- **Blocked** — the agent can’t use the tool at all


That means you can let` send_email` run automatically while` cancel_subscription` always waits for a human — or stays blocked entirely. Combined with connector-level audience scoping and the[governance systems](https://www.basedash.com/features/mcp-connectors) Basedash already has, you decide exactly what the agent can do, for whom, and with how much supervision.


## Skills turn actions into workflows


Actions get most powerful when you compose them.[Skills](https://www.basedash.com/features/skills) — reusable instructions every Basedash agent can read — can now describe multi-step workflows that mix reads, edits, and external actions.


Take extending a customer’s trial. Internally, ours is a skill that tells the agent to: look up the account in Postgres, update the trial end date, sync the subscription in Stripe, email the customer a confirmation, and report back in a specific format. What used to be a database edit, a billing change, and a follow-up email across three tabs is now one request — with one approval on the step that needs it.


Any workflow that touches your data and your tools can live in a skill: offboarding a customer, provisioning a demo environment, escalating an at-risk account. Complex, multi-step operations that run inside Basedash, with the context of all of your company’s data.


## How we use Actions at Basedash


We’ve been running on Actions internally for the past few weeks:


- **Trial extensions** — the “extend trial” skill above. What used to take a support engineer ten minutes across three tools is one approved request.
- **Demo environments** — sales asks the agent to seed a fresh demo org with realistic data before a call. INSERTs into production-shaped tables, reviewed once.
- **Data cleanup** — when a bug leaves records in a bad state, the fix is a prompt and an approval instead of a migration script and a deploy.


The pattern is always the same: the agent does the work, a human reviews the part that matters.


## Getting started


Actions are available today for all Basedash workspaces.


1. [Sign up for Basedash](https://charts.basedash.com/signup) (or log in)
2. As an admin, open a SQL data source in the command menu and turn on **Allow edits**
3. Connect an MCP server under **Data sources** → **Add MCP server** , and set each tool to always allow, needs approval, or blocked
4. Ask the agent to change something — and approve it when the card appears


For more on connectors and tool permissions, see the[MCP connectors feature page](https://www.basedash.com/features/mcp-connectors) or the[docs](https://www.basedash.com/docs/data-sources/mcp-connectors) .


## What’s next


Actions complete a progression we’ve been building toward all year:[AI chat](https://www.basedash.com/features/ai-data-analyst) made your data conversational,[Insights](https://www.basedash.com/features/insights) made it proactive,[Automations](https://www.basedash.com/features/automations) made it scheduled, and[MCP connectors](https://www.basedash.com/features/mcp-connectors) plugged in the rest of your stack. Now the agent can act on all of it — governed, approved, and audited.


**Turn on Allow edits today and let your agent close the loop.**
