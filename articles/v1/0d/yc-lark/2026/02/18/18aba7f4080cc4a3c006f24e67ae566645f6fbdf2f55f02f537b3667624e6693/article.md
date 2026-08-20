---
schema_version: "1.0.0"
document_id: "18aba7f4080cc4a3c006f24e67ae566645f6fbdf2f55f02f537b3667624e6693"
company_key: "yc-lark"
company: "Lark"
source_id: "yc-lark-news-import-b4dd00669ba5"
canonical_url: "https://getlark.ai/blog/our-engineers-each-ship-8-features-per-week-heres-how-we-keep-up-with-testing"
published_at: "2026-02-19T00:00:00+00:00"
first_seen_at: "2026-07-24T03:28:52.485440+00:00"
fetched_at: "2026-07-28T21:57:40.692150+00:00"
content_hash: "sha256:d0c226838cb2a68744655fb8114ae110607aa8304579df4cb95d15d6b356aa82"
---

# We Ship 8 Features a Week Per Engineer. Here's How We Keep Up With Testing.

At most companies, velocity and confidence trade off against each other. Ship faster and you break things. Test more carefully and you slow down. We didn't want to accept that tradeoff.


We're a small team. Each engineer is running 8+ features in parallel at any given time, most of them kicked off by a prompt to a coding agent. Cursor or Claude Code does the implementation, opens a PR, and we're already onto the next thing. The bottleneck wasn't writing code anymore, it was verifying that the code actually worked.


### The old way was brutal


To test a new feature, you had to stop what you were doing, check out the branch, spin up the local environment, poke around, find the bugs, write them up, and hand them back to the agent. By the time you'd done that for three PRs, the day was gone.


We knew we needed two things: live preview environments for every branch, and automated tests that could run against them without anyone babysitting the process.


### Preview environments: the easy part


We built a system where every feature branch gets its own live environment at` <branch-slug>.preview.getlark.ai` . It's a t3.medium running the full stack — frontend, API, Postgres, cache — spun up automatically when a PR is labeled` deploy-preview` . The URL gets posted as a comment on the PR. When the PR closes, the instance is terminated. The whole thing costs about $1.13 per branch per day.


The infrastructure is simple by design: EC2, S3, Route53, Docker Compose, and Caddy for TLS. About 400 lines of shell. No Kubernetes, no Terraform state, no new abstractions to maintain.


But a preview environment without automated verification is just staging with a different URL.


### The harder part: actually testing them


This is where Lark comes in — and honestly, it's what made the whole system worthwhile.


Instead of writing Playwright scripts that break every time the UI shifts, our coding agents write Lark tests in plain English alongside the feature implementation. They describe what the feature should do the way you'd explain it to a teammate. Lark's AI agents run those tests against the preview environment automatically on every push.


The result is a fully autonomous loop: the agent implements the feature, the preview environment spins up, Lark runs the tests, and the PR only merges when everything passes. We don't touch it until it's green.


### Why it's different from writing tests yourself


Lark tests don't break when the UI evolves — the agents adapt instead of relying on hardcoded selectors. They cover the frontend, backend APIs, and async workflows without requiring you to stitch together multiple tools. And because they're written in plain English, a coding agent can author them end-to-end with no hand-holding.


When something does fail — in a preview environment, in staging, or in production — we get an alert in Slack with logs and screenshots attached. Not a vague CI red. An actual explanation of what broke.


### The outcome


We ship faster than we did when we were writing tests by hand, and we do it with more confidence. The coding agents handle implementation. Lark handles verification. We handle decisions.


If you're building a preview environment setup and want the testing side handled for you,[book a demo](https://cal.com/founders-lark/lark-chat) .
