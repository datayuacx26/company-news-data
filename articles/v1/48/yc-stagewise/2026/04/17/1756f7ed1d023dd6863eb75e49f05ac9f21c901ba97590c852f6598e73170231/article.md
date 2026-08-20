---
schema_version: "1.0.0"
document_id: "1756f7ed1d023dd6863eb75e49f05ac9f21c901ba97590c852f6598e73170231"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-april-13-19"
published_at: "2026-04-20T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:3264b7f9ede3418a87afb1a04c67d3dad01cf0ec39cd7ed00c7e80c8ced77b49"
---

# Release Week: April 13–19 · stagewise Newsroom

## What Shipped Last Week


During the last week, we shipped **Alpha 55** through **Alpha 63** . Let's talk about the biggest changes.


### First-Day Support for Claude Opus 4.7


We shipped support for Claude Opus 4.7 on it's release day, giving users access to Anthropic's latest and greatest (publicly available) model. Opus 4.6 continues to be included as a model, but get's disabled by default (re-enable in Settings if needed).


### Support for Globally Installed Skills


Release **Alpha 55** included support for globally installed **Agent Skills** . This allows you to give the agent simple access to all skills on your machine without having to load dedicated workspaces. The supported path for globally installed skills correlates to the standard **Agent Skill** specification.


### ` /learn` command


Release **Alpha 57** added a **learning skill** for creating and updating other skills. This marks a first step into the direction of continuous learning, giving users the possibility to manually decide when an agent should learn a certain workflow, fact or behavioral pattern to reuse at a later time by simply using the **` /learn`** command in chat.


### ` /debug` command


Release **Alpha 60** included a new **debug skill** , enabling the agent to automatically instrument any codebase with tracing, collect data and analyze it for potential bugs. The debug skill can be used by simply invoking the **` /debug`** command in chat.


## What the Week Adds Up To


stagewise is getting more powerful in typical development workflows - and more intelligent in the work you do again and again.
