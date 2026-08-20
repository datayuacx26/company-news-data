---
schema_version: "1.0.0"
document_id: "59d0159e677cbca4e8da90a62b0181ac786b306a3db0af27d6e5ba51d4be8e80"
company_key: "yc-cubic"
company: "cubic"
source_id: "yc-cubic-news-import-612274238019"
canonical_url: "https://www.cubic.dev/blog/codebase-scans-launch-week-03"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-23T06:53:38.364034+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:fd59abbf00f5ec67960fb680845b9b347ab88d167423edb291539bacbb81e8d9"
---

# cubic blog: Codebase Scans | Launch Week 03, Day 1

For the past few months, we have been piloting a new class of autonomous agents with select customers.


They found critical vulnerabilities for Cloudflare and caught an RCE for Openclaw within hours that had gone undetected for months.


These were real bugs in production systems maintained by strong engineering teams.


Today we are making this available to everyone. Day 1: Codebase Scans.


Our review agents are already state-of-the-art. They rank #1 on[Martian's Code Review benchmark](https://codereview.withmartian.com/?model=anthropic_claude-opus-4-5-20251101&mode=offline) (read our[deep dive on how we achieved this](https://www.cubic.dev/blog/benchmark-number-one) ). But we started wondering what would happen if we gave those same agents unlimited time and tokens, with no pressure to be fast.


But PR reviews are optimized for speed. You want feedback in minutes so you can keep shipping. That is the right tradeoff for most changes. But some bugs only surface after hours of tracing call chains across dozens of files. And some risks, like compromised third-party dependencies, never show up in a PR diff at all.


So we removed the time constraint and let our agents run for 12+ hours. That became Codebase Scans.


**How it works**


You point cubic at a repository. It clones it into a sandbox, maps the structure, and deploys agents to explore the codebase in parallel. Each agent traces data flows, follows call chains across files, and checks external documentation to verify whether an issue is reachable and exploitable. Findings are deduplicated and scored by risk and confidence.


Scans run in two modes:


-


**Historical:** A deep scan of your entire existing codebase for accumulated bugs and vulnerabilities.


-


**Continuous:** Agents run on a nightly or weekly schedule, catching systemic issues as your codebase evolves.


These agents share the same memory layer as cubic's GitHub and CLI review agents. They already know your team's coding patterns, conventions, and past decisions. The more you use cubic, the sharper Codebase Scans get.


**You do not need to manage this**


Most tools that find bugs create a new backlog you have to maintain. Codebase Scans handle the entire lifecycle:


-


cubic notifies the right code owners and creates tickets in Linear or Jira.


-


Background agents write fixes and open PRs for you to review.


-


When a merged PR resolves an open finding, cubic detects it and closes the ticket.


You set it up once. It runs in the background. The backlog cleans itself.


Available today on the cubic Pro plan.


cubic.dev/codebase-scans


docs.cubic.dev/codebase-scans
