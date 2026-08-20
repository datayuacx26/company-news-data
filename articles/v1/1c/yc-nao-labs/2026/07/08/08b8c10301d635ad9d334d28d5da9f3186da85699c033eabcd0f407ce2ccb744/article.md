---
schema_version: "1.0.0"
document_id: "08b8c10301d635ad9d334d28d5da9f3186da85699c033eabcd0f407ce2ccb744"
company_key: "yc-nao-labs"
company: "nao Labs"
source_id: "yc-nao-labs-news-import-1dd8f9256e0a"
canonical_url: "https://getnao.io/blog/nao-redesign/"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-22T05:14:42.862420+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:9956b3faa342b0fc5e054d3b3896fa2b290045e32173691170568c62b2edef75"
---

# nao has a new look

nao has a new look.


We rebuilt the whole interface. Same product, same[governed context](https://getnao.io/blog/how-to-do-context-engineering-for-data-teams) underneath, but every screen a business user touches has been redesigned for clarity.


Two people use nao. A business user asks a question in plain English. A data team owns the context that makes the answer reliable. The old UI served neither well enough. This redesign is for both.


## A home screen that starts from your data


Open nao and it greets you by name, then asks one thing: what do you want to analyze?


One input. Pick your model inline (Claude, GPT, Gemini, whatever you connected). Below it, **suggested ideas** generated from your actual data set, and a grid of your **latest stories** so you can pick up where the team left off. No empty state, no blank prompt box.


## Queue your prompts


You rarely ask one question. You ask, then ask a follow-up, then ask for a chart.


The new composer lets you **queue prompts** . Line up "as a follow up" and "do something next" while the agent is still working, drag to reorder, and it runs them in sequence. Voice input and the model picker sit right in the bar.


## The agent shows its work


Trust comes from seeing how the answer was built. The agent now explains itself as it goes.


It navigates your[context layer](https://getnao.io/blog/how-to-build-context-stack-for-agentic-analytics) like a file system: it searches, lists folders, opens the files it needs, and shows you each step. You can watch it read` nao_config.yaml` , check` RULES.md` , and pull the right tables before it writes a line of SQL. When the answer lands, you already know why to trust it.


## Charts and SQL, cleaned up


Every result renders in the new visual system: sharper charts, readable tables, and a SQL view when you want to check the query.


The agent can also turn a result into plain English. Instead of handing a table to a stakeholder, nao writes the insight for you.


This is the same rendering you get through the[nao MCP app](https://getnao.io/blog/launching-nao-mcp-app) , so a chart looks identical whether you see it in nao, ChatGPT, or Cursor.


## Stories, redesigned


[Stories](https://getnao.io/blog/how-to-turn-chat-with-data-into-shareable-stories) turn a chat into a shareable, versioned report. The workspace around them got the same treatment.


Pin the reports your team checks every week, organize them into folders, and set any story to go **live** so it refreshes on a schedule. This is what[headless analytics](https://getnao.io/blog/introducing-headless-analytics-agent) looks like in practice: one place the whole company trusts, one context the data team controls.


## One design system


Under all of it is a single design system: new color tokens, one type scale, consistent components.


Same system powers the web app, the[Slack and Teams bots](https://getnao.io/blog/setup-ai-analytics-slack-bot-open-source) , and every chart the[MCP](https://getnao.io/blog/launching-nao-mcp) renders into another agent. Change it once, it changes everywhere.


## Try it


The redesign is live in nao today.


1. Update to the latest version:[github.com/getnao/nao](https://github.com/getnao/nao)
2. Run` nao chat` or open your deployed instance
3. Ask a question and watch the new flow


Full docs:[docs.getnao.io](https://docs.getnao.io/)


Star us on GitHub:[github.com/getnao/nao](https://github.com/getnao/nao)
