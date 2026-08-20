---
schema_version: "1.0.0"
document_id: "ceb6b9df949956e5fbc44e726965f04138010f1df3e24dbdd6d0e55e0b46d1a7"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/best-openclaw-skills"
published_at: "2026-06-05T12:57:29+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:4ce5772e2e3f51d965acf3efa4f4a79d669a31e2b9889a9228929da85d87cb96"
---

# 15 Best OpenClaw Skills in 2026 (Tested and Ranked)

## Tier 2: High Value (Top Productivity Multipliers)


### 6. github-actions


**What it does:** Trigger GitHub Actions workflows, read workflow status, and get notified on failures.


**Why developers love it:** Your agent monitors CI/CD without you checking GitHub. Failed tests send a Telegram alert. Long-running pipelines notify on completion.


**Use cases:** Deploy monitoring, test failure alerts, automated PR status updates.


### 7. slack-notify


**What it does:** Post messages to Slack channels or direct messages.


**When to use over telegram-notify:** When your team uses Slack and you want briefings or alerts in team channels, not personal Telegram.


**Note:** Requires a Slack app configuration (20-minute setup). More complex than Telegram but necessary for team use cases.


### 8. news-aggregator


**What it does:** Pulls headlines from configurable RSS feeds and news APIs. Returns summaries by topic.


**Best sources to configure:** TechCrunch, The Verge, Hacker News, specific industry publications via RSS.


**Use cases:** Daily news section in morning briefings, competitive monitoring, trend detection.


### 9. linear-tasks


**What it does:** Read and write Linear issues. Can create tasks, update status, and pull daily issue lists.


**Why it matters:** Linear is the task system of choice for many dev teams in 2026. This skill connects your agent to your real work queue, enabling automations like "daily standup summary from Linear issues."


### 10. notion-read


**What it does:** Read Notion pages, databases, and blocks. Returns structured content.


**Use cases:** Pull meeting notes into your briefing, read project wikis before starting work, reference documentation during tasks.


**Note:** Notion's API limits writes more than reads. This skill is read-optimized; use a different approach for heavy writes.


---


## Tier 3: Specialized (Install When You Need Them)


### 11. code-reviewer


**What it does:** Reviews code diffs against style guides and security patterns. Returns structured feedback.


**Who it's for:** Developers who want a second pass on PRs before human review.


**Best use:** Combine with github-actions to auto-trigger a review on every PR and post feedback as a comment.


### 12. twitter-search


**What it does:** Searches Twitter/X for terms, pulls recent tweets with engagement metrics.


**Use cases:** Competitor monitoring, finding customer conversations about your product, trend detection in your market.


**Note:** Requires Twitter API v2 credentials (free tier limited to 500K tweets/month).


### 13. pdf-reader


**What it does:** Extracts text from PDF files and returns structured content.


**Use cases:** Reading contracts, research papers, invoices, reports. Essential if you deal with PDFs regularly.


**Warning:** OCR accuracy depends on PDF quality. Scanned documents are less reliable than text-based PDFs.


### 14. stripe-read


**What it does:** Pull Stripe data — recent charges, subscription status, customer counts, MRR.


**Use cases:** Daily revenue briefings ("yesterday's MRR: $8,420"), failed payment alerts, churn detection.


**Note:** Read-only by default (safer). A separate write skill exists for charge creation, but most users don't need it.


### 15. weather-current


**What it does:** Current conditions and forecast for a configured location via OpenWeatherMap.


**Why it's worth installing:** Every morning briefing is better with weather context. 1,000 API calls/day on the free tier — more than enough.


---


## Skills to Avoid (And Why)


**auto-reply-email:** Sends emails automatically without your review. High risk of embarrassing responses. Use email-summary instead and review before sending.


**browser-automation:** Takes control of browsers to fill forms and click buttons. Works sometimes, breaks others. Use dedicated APIs when they exist.


**crypto-trader:** Executes trades autonomously. Never give your agent autonomous control over financial transactions.


**social-auto-poster:** Posts to social media automatically without review. Social media requires human tone judgment that current agents don't have reliably.


---


## Installing a Custom Skill Stack


For the most common use case (power user + morning briefings), install this set:


```text
openclaw   skills   install   telegram-notify   web-search   email-summary   memory-wiki   google-calendar   news-aggregator
```


This gives you a fully functional daily briefing agent with notification, research, email triage, memory, scheduling, and news — all in one install command.


Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo →[blink.new/claw](https://blink.new/claw)


Verified skills (badge on ClaWHub) have been reviewed by the OpenClaw team. Community skills haven't. Before installing a community skill, check: is the code public? When was it last updated? Does it request permissions it doesn't need? For skills that access email or messages, stick to verified skills. For read-only tools like weather, community skills are lower risk.


OpenClaw loads all installed skills into context at the start of each session. More skills = more context consumed. Blink Claw recommends keeping your active skill set under 20 for optimal performance. Install the skills you actually use, remove ones you don't.


Yes. OpenClaw skills are TypeScript files following a standard interface. The full development guide is in the OpenClaw docs. Blink's own plugin (14 skills for full-stack infrastructure) follows the same pattern — you can use it as a reference implementation.


Mostly yes. Some skills require credentials you configure yourself. On Blink Claw, common credentials (LLM APIs, web search) are pre-configured — you only need to add service-specific credentials (your Gmail, your Stripe key). Self-hosted requires all credentials set manually.


memory-wiki. Most users focus on notification and search skills because the payoff is obvious. But without persistent memory, your agent resets to zero every session. memory-wiki is the infrastructure that makes everything else compound — your agent gets smarter about your preferences, work context, and history over time.
