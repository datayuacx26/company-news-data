---
schema_version: "1.0.0"
document_id: "bf32fb23a4f7a94aced9f9241a4c145a6c9815bfce012d95407bc60f526a21c1"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/we-built-an-sre-bot-on-aura-heres-what-we-learned"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:9aaa86832df72c239d9bf38de3d0eecf43881f638f0680752c4e2b08adca6685"
---

# We built an SRE bot on AURA. Here's what we learned.

PagerDuty fires. You open the incident. Title, timestamp, nothing else. Whatever context exists is in someone's head, in a Slack thread from two weeks ago, or in a runbook nobody has touched since the last reorg.


We got tired of that. So we put an AURA agent behind a Slack bot and pointed it at our own production environment.[AURA is our open-source production-operations agent](https://www.mezmo.com/aura) : it connects to the tools SREs already use, gathers the context they would otherwise chase manually, and keeps the human in control.


We've been using it for a few months. It's not done, but it's already useful enough that I'd notice if we turned it off.


Here's a walkthrough of what we built. Read on for the details.


## Why AURA


We weren't looking for a reason to dogfood. We had the same problem AURA is meant to solve: on-call engineers burning five to ten minutes per incident just getting oriented. That first pass of context-gathering is routine production work, and it should not depend on whoever happens to be awake and what they remember at 2 a.m.


AURA connects to systems via MCP, which gives agents a standard way to reach tools and context.


We wired up four MCPs:[Mezmo for pipeline and log data](https://www.mezmo.com/platform/active-telemetry) , an internal MCP for infrastructure tooling, PagerDuty, and Prometheus for metrics. That covers most of what you need to figure out what's actually happening.


## The setup


We run one AURA agent per environment, each fronted by its own Slack bot: dev talks to dev, prod talks to prod. Each integration uses environment-scoped credentials, so there are no shared keys and no access matrix to rebuild after the next reorg.


Slack is the interface because that's where incidents already happen. DM it for quick questions. @-mention it in a shared channel and it answers in a thread, showing the tool calls it makes while it works so responders can audit how it got there. It also reads every message humans have posted since it last spoke before it responds. That last part matters more than it sounds. If your team has been going back and forth on something for 20 minutes, the bot reads that first, summarizes it, and adds that information to the context.


## The thing that actually made it useful


For the first few weeks, it was useful in the way a search tool is useful: it helped when you remembered to ask. PagerDuty webhooks changed the shape of the workflow.


When PagerDuty opens an incident, a webhook fires, the bot passes the payload to the AURA agent, the agent pulls logs and metrics, and posts an analysis note to the incident. All before anyone touches it. When you open PagerDuty at 2 a.m., there's already something there.


When a dedicated incident channel is created, the bot joins automatically, posts a deeper analysis, and stays available for follow-up questions as the team investigates.


## What it doesn't do


The agent is read-only today by design. It investigates, explains its work, and reports, but it does not remediate on its own. That is the right trust ladder for production: first prove the agent can diagnose real incidents, then graduate well-understood paths to scoped, human-approved actions. As those patterns prove themselves, routine actions can move closer to AURA; judgment stays with the SRE.


## If you want to build this


The pattern is simple: one AURA agent per environment, MCPs for the systems it needs to understand, Slack where responders already work, and PagerDuty webhooks so the first pass of investigation starts automatically. Add an MCP and the agent can reach it—no application code changes.


AURA is Apache 2.0. You can inspect it, run it in your own environment, and decide what work it is allowed to do.


If this kind of open, inspectable operational AI resonates with you, give the repo a star or come build with us:[github.com/mezmo/aura](https://github.com/mezmo/aura) .


‍
