---
schema_version: "1.0.0"
document_id: "70b0a96102e9c9dc7f9c056522e567299964fe90e0c34eb80bc9325a6401f376"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/setup-ai-analytics-slack-bot-open-source/"
published_at: "2026-03-12T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:8c068891f5210a56b7c675477d1763c0e4edb3026c5442a19ba15d1a18aca04d"
---

# How to Set Up an AI Analytics Slack Bot with an Open Source Framework

If your team already works in Slack, that is where your **analytics agent** should live too.


The fastest path is not building a custom bot from scratch. It is using an **open source** framework that already handles the analytics layer, the chat interface, and the context stack behind it.


With **nao** , you can connect the same agent you use in the browser to Slack, so people can **chat with data** in channels and direct messages while still using the same **context engineering** , **dbt** , and **data stack** setup behind the scenes.


This guide follows the official Slack bot flow in nao and turns it into a practical rollout sequence for data teams.


## What the Slack bot does


According to the official nao Slack docs, the bot:


- listens for mentions in Slack channels,
- answers direct messages,
- uses the same agent as the web chat interface,
- returns SQL queries, tables, and visualizations,
- keeps context inside Slack threads,
- streams progress updates and can upload CSV results,


That matters because you do not need to maintain a separate bot brain for Slack. Any improvement you make to the browser agent also improves the Slack experience.


If you are still building the core agent, start with[How to Build Your In-House Analytics Agent Fully with Open Source](https://getnao.io/blog/build-open-source-analytics-agent-text-to-sql) . If your stack already uses MCPs,[5 Steps to Deploy an Analytics Agent on dbt MCP to Your Whole Company](https://getnao.io/blog/deploy-analytics-agent-dbt-mcp-steps) is the right companion.


## Step 1. Open the Slack integration in nao


Inside nao, go to your profile, then open` Project > Slack` .


The first thing to check is the deployment URL. nao uses that URL to prefill the Slack app manifest and connect Slack events back to your deployed analytics agent.


This is the operational requirement behind the setup: Slack is only the interface. The real work still happens in your deployed **analytics agent** , which is why this setup fits teams already investing in **agentic analytics** and long-term **context engineering** .


## Step 2. Create the Slack app from nao


From the Slack integration page in nao, click` Create Slack App` .


The nao docs state that this opens Slack with a pre-filled app manifest, including:


- scopes,
- event subscriptions,
- interactivity,


That is important because it removes most of the tedious manual app configuration work.


Instead of building a bot integration field by field in Slack, you start from a manifest already designed for the nao chat workflow.


## Step 3. Install the Slack app to your workspace


In Slack app settings:


1. Go to` Install App` ,
2. Click` Install to Workspace` ,
3. Approve the requested permissions,


At this point, the app exists in Slack, but it is not yet connected to your nao deployment. Installation gives the app presence in your workspace. The next step gives it credentials to talk to the deployed agent.


## Step 4. Connect the bot credentials back to nao


The Slack docs call for two values:


1. In` OAuth & Permissions` , copy the Bot User OAuth Token, which starts with` xoxb-` ,
2. In` Basic Information` , copy the Signing Secret,


Paste both values into nao:


- Bot Token,
- Signing Secret,


Then click` Save` .


This is the point where the Slack app becomes a real **AI analytics Slack bot** . Slack can now authenticate requests, and nao can map those requests to the same agent logic your team already uses in the browser.


## Step 5. Add the bot to the right channels


Go to each Slack channel where you want the bot to work and run:


text


/invite @nao


Or invite the bot using the custom bot name you configured.


After that, the bot listens for mentions in those channels. This is the right time to start small:


- one finance channel,
- one growth or product analytics channel,
- one internal data help channel,


That rollout pattern is safer than enabling the bot everywhere on day one. It lets you tune the agent against real questions before wider adoption.


## Step 6. Test the user experience in-thread


The official docs show the expected usage pattern:


text


@nao What were our top 5 products by revenue last month?


The bot then:


1. processes the question with the same agent as the web UI,
2. generates SQL from your context,
3. executes queries against connected databases,
4. returns a formatted Slack answer,
5. sends a link to the full browser conversation,


Follow-up questions should stay in the same thread. The bot keeps thread context, so a second question like "Can you break that down by region?" builds on the earlier result instead of starting over.


That thread behavior is what makes the bot useful for real **chat with data** workflows rather than one-off slash-command demos.


## Step 7. Roll out Slack only after the agent is reliable


The Slack integration is easy. The hard part is whether the agent behind it is trustworthy.


Before you roll this out broadly, make sure your underlying setup is solid:


- trusted data sources,
- explicit metric definitions,
- clean warehouse access,
- clear **dbt** and business context,
- repeatable evaluation,


Slack adoption rises fast when people get good answers in the first week. It dies just as fast when the first few answers are wrong.


That is why the correct sequence is:


1. build the agent,
2. test the agent,
3. add the Slack surface,


If you need that evaluation layer,[How to Evaluate an Analytics Agent: A Practical Guide with nao test](https://getnao.io/blog/analytics-agent-benchmark-context-stack) is the next step.


## When Slack is the right choice


Slack is usually the best bot surface when:


- your company already runs operational workflows in Slack,
- analysts answer repeated ad hoc questions there,
- you want lightweight distribution without another dashboard,
- your team wants a fast way to embed **agentic analytics** into the daily workflow,


In practice, it is one of the easiest ways to turn an internal **open source** analytics setup into a visible product for the company.


## Final takeaway


If you want to set up an **AI analytics Slack bot** with an **open source** framework, the shortest path is to keep Slack as the interface and let nao run the actual analytics system underneath.


That gives your team one shared agent across browser and Slack, one shared **context engineering** layer, and one shared path for improving reliability across the full **data stack** .
