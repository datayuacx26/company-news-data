---
schema_version: "1.0.0"
document_id: "742d8a74ad24b9d4261f31fc454e9cca5592168995a22dcf2fad190227d26850"
company_key: "yc-stably-ai"
company: "Stably AI"
source_id: "yc-stably-ai-news-import-4d9fcc9f146f"
canonical_url: "https://www.stably.ai/blog/how-we-use-a-slack-cli-to-10x-our-internal-ops"
published_at: "2026-03-08T00:00:00+00:00"
first_seen_at: "2026-07-24T02:44:25.851186+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:1bd67a264046ed19f6e27aaf8e3b791078b5b72b1aaeadad6d7a77084db8f63f"
---

# How We Use a Slack CLI to 10x Our Internal Ops

[A Slack CLI for agent](https://github.com/stablyai/agent-slack)


We've been building agent skills that automate our Slack-based workflows. We use a mixture of agents for our internal ops, and the key enabler is **agent-slack** , a CLI we open-sourced that lets them read, write, and search Slack natively.


Sharing what's worked for us and how this integration has 10x'd our internal operations.


## What We Use It For


We run several agent skills powered by` agent-slack` that have fundamentally changed how our engineering and ops teams interact with Slack.


### 1. Auto File GitHub Issues From Customer Slack Messages


We maintain shared Slack Connect channels with many of our enterprise customers. To streamline issue tracking, we run an agent skill that scans these channels and reads full thread contexts—including canvases, screenshots, and forwarded messages.


The agent automatically classifies each message by type, priority, and size, assigns it based on team expertise, and creates GitHub issues directly on our project board. No more copying and pasting Slack messages into GitHub.


### 2. Daily Standup Generation


Writing daily standup updates used to be a chore. Now, an agent skill pulls our recent work directly from our git commits and PRs (via the` gh` CLI), our recent Slack activity (via` agent-slack` ), and our calendar events.


It synthesizes this context into a neat standup draft and opens a browser-based draft editor. The human stays in the loop to review and tweak the update before posting it, ensuring high accuracy with almost zero effort.


### 3. Slack Search That Actually Works


Slack's built-in search is notoriously difficult to navigate when looking for deeply nested technical context.


With` agent-slack` , search returns structured JSON that our AI agents can reason over. Instead of hunting for keywords, we simply ask: *"Find all messages about the latency issue in the last week."* The agent finds the relevant threads, reads the full context, and synthesizes exactly what happened. It is vastly superior to scrolling through endless Slack search results.


```text
# Example: Searching Slack natively via agent-slack
agent-slack search "latency issue" --days 7 --json


```


### 4. Read Slack Canvases


A lot of tribal knowledge, runbooks, architecture decisions, and onboarding documents end up living in Slack canvases.


The` agent-slack canvas get` command pulls these documents as clean markdown so our agents can reference them instantly. There is no more hunting for "that one doc someone wrote in a canvas somewhere."


### 5. Closing the Loop on Fixes


When an engineer fixes a bug and creates a PR, the agent automatically replies in the original Slack thread where the issue was reported and reacts with a ✅ checkmark.


The stakeholders get notified immediately, and the full feedback loop is closed without the engineer needing to context-switch back to Slack to update the customer.


## The Setup: Zero Infrastructure Required


One of the things that made` agent-slack` so practical for our team is that it **doesn't require any infrastructure** .


There is no Slack App to register, no bot tokens to manage, and no MCP (Model Context Protocol) server to run. It simply reads your local Slack Desktop session on macOS, meaning it authenticates as *you* with your existing permissions.


We had previously tried the MCP Slack integration, but the OAuth and bot setup friction meant only one person ever configured it. With` agent-slack` , everyone on the team was up and running and using it within one minute.


## Get Started


We open-sourced` agent-slack` because we believe native integrations are the future of CLI-based AI agents.


Check out the repository, try it out, and let us know what workflows you automate.


**[Explore agent-slack on GitHub](https://github.com/stablyai/agent-slack)**
