---
schema_version: "1.0.0"
document_id: "9d3cfc03b3ec0b92318e4ab84605a24d904f855f952171bd3a4cbee6e1e95f79"
company_key: "yc-worklytics"
company: "Worklytics"
source_id: "yc-worklytics-news-import-9ab18239f248"
canonical_url: "https://www.worklytics.co/blog/tracking-claude-code-usage"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T07:25:13.507600+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:dbb89de7c5fc589aa9908262050e199e2f75ba7a1a00ac2930dc6d5d10d0aa33"
---

# Tracking Claude Code Usage Across Your Engineering Team

Claude Code has gone from terminal curiosity to a primary way software gets written in many organizations. That shift raises a question engineering leaders are now expected to answer: how much of our shipped code involves Claude Code, who has adopted it, and is it worth what we're paying for it? Unlike chat-based AI tools, agentic coding assistants do real, attributable work - which means the measurement story is both richer and easier to get wrong.


Anthropic now ships substantial analytics for Claude Code. This guide covers what the native dashboards measure, how PR attribution actually works, where the costs show up, what admins can and can't see, and where native analytics stop being enough.


## Claude Code's built-in analytics dashboard


Where you look depends on how your organization buys Claude:


- **Claude for Teams / Enterprise** : the dashboard lives at claude.ai/analytics/claude-code, visible to Admins and Owners
- **API customers (Claude Console)** : platform.claude.com/claude-code, visible to anyone with the UsageView permission (Developer, Billing, Admin, Owner, Primary Owner)


The Team/Enterprise dashboard tracks four things:


1. **Usage metrics** - daily active users, sessions, lines of code accepted, and suggestion accept rate (how often developers accept Claude Code's edits)
2. **Contribution metrics** - merged PRs and lines of shipped code that involved Claude Code assistance (more on this below)
3. **Leaderboard** - top contributors by Claude Code usage, with a full-org CSV export
4. **Charts** - adoption trends, PRs per user over time, and a daily breakdown of PRs with vs. without Claude Code involvement


The Console version for API customers adds **spend tracking** : daily API costs alongside user counts, plus a per-user table of spend and accepted lines for the current month. (Anthropic notes these spend figures are estimates - reconcile against your billing page.)


‍


‍


## Contribution metrics: measuring code that actually ships


This is the most interesting part of Claude Code's analytics, because it answers the question usage metrics can't: did any of this make it to production?


With the GitHub integration enabled (a GitHub admin installs the Claude GitHub app, then a Claude Owner enables analytics under admin settings), Claude Code matches session activity against merged pull requests. PRs containing code written during a Claude Code session get counted as "PRs with Claude Code" and labeled` claude-code-assisted` in GitHub - which means you can query them directly in your own tooling.


The attribution methodology is deliberately conservative, and that's worth understanding before you present these numbers to a board:


- Only lines with high-confidence matches to Claude Code session output are counted
- Code developers substantially rewrote (more than 20% changed) is **not** attributed to Claude Code
- Lock files, generated code, build directories, and test fixtures are excluded
- Sessions from 21 days before to 2 days after the PR merge are considered


In other words, the "lines of code with Claude Code" number is a floor, not a ceiling. If your dashboard says 30% of shipped code involved Claude Code, the true figure is higher.


Two caveats: contribution metrics are in public beta on Teams and Enterprise plans, and they are **not available for organizations with Zero Data Retention enabled** - ZDR orgs see usage metrics only. They also only cover your claude.ai organization; usage through the Console API isn't included.


‍


## Tracking spend and token costs


Claude Code sessions consume dramatically more tokens than chat usage - a single agentic session can run through more tokens than a user's entire week of conversations. Three ways to keep visibility:


- The **Console dashboard** shows daily spend and per-user monthly costs for API customers
- The **Claude Code Analytics API** provides programmatic access to usage and productivity metrics for custom reporting and cost allocation by team
- **OpenTelemetry export** gives you real-time per-user token counts and cost estimates in your own observability stack


If you're managing AI spend across multiple tools, see our guide to[tracking LLM token usage and cost](https://www.worklytics.co/blog/how-to-track-llm-token-usage-and-cost) for the attribution architecture that makes these numbers actionable.


## What admins can see and what they can't


A question every engineering leader gets from their team: *is this surveillance?*


Claude Code's analytics show **usage and contribution metadata** - sessions, accepted lines, PR involvement - not the prompts developers write or the conversations they have. The leaderboard shows who uses Claude Code most, not what they asked it.


Conversation-level access exists only through Claude Enterprise's Compliance API, which requires a Primary Owner to create a dedicated access key and is designed for security and legal teams running DLP and audit workflows - not for managers reading prompts. We cover that boundary in detail in our guide to[tracking Claude Enterprise usage](https://www.worklytics.co/blog/track-if-employees-are-using-claude-enterprise) .


This distinction matters beyond ethics: teams that believe they're being watched use AI tools less, and visibly metadata-only measurement is one of the[compliance-safer approaches to workplace analytics](https://www.worklytics.co/blog/key-compliance-laws-for-remote-employee-monitoring-data-protection) under GDPR and similar regimes.


## The limits of Claude Code's native analytics


Three gaps show up once you operationalize this:


1. **It's a single-tool view.** Most engineering orgs in 2026 run Claude Code alongside Cursor, GitHub Copilot, or both - and developers move between them. Claude Code's dashboard can't tell you whether overall AI-assisted output is growing or just shifting between tools. (Our guides to[Cursor usage analytics](https://www.worklytics.co/blog/tracking-how-employees-utilize-cursor-ai) and[Copilot utilization](https://www.worklytics.co/blog/tracking-copilot-utilization-in-your-organization) cover the same problem from the other side.)
2. **Usage isn't outcomes.** Accept rates and PR counts say nothing about cycle time, review load, defect rates, or whether AI-heavy teams actually ship faster. Anthropic itself recommends pairing contribution metrics with DORA metrics or sprint velocity.
3. **No organizational context.** The native dashboard can't segment by role, tenure, or team structure, can't benchmark you against peer organizations, and can't flag the department where adoption quietly stalled.


## From tool metrics to engineering intelligence


This is the gap Worklytics closes. We combine Claude Code usage signals with data from your other AI tools (Cursor, Copilot, ChatGPT Enterprise, Gemini) and your collaboration stack to show AI adoption by team and role, benchmark it against[2025–26 software engineering productivity baselines](https://www.worklytics.co/resources/software-engineering-productivity-benchmarks-2025-good-scores) , and connect adoption to outcome metrics - all privacy-first, with no prompt content collected.[Explore the AI adoption dashboard](https://www.worklytics.co/measureai) to see what that looks like in practice.


Running OpenAI's agent too? See our guide to[tracking Codex usage](https://www.worklytics.co/blog/tracking-codex-usage) across your org.


## Frequently asked questions


**Can admins see Claude Code prompts?**
No. Claude Code's analytics dashboards show usage and contribution metrics - sessions, lines of code accepted, PRs shipped with assistance - not prompt or conversation content. Conversation-level access exists only through Claude Enterprise's Compliance API, which is restricted to keys created by a Primary Owner for compliance workflows.


**How does Claude Code decide a PR was "AI-assisted"?**
When contribution metrics are enabled, merged PR diffs are matched against Claude Code session output from a window of 21 days before to 2 days after the merge. Only high-confidence matches count, code rewritten more than 20% by a developer is excluded, and auto-generated files are filtered out. Matching PRs are labeled` claude-code-assisted` in GitHub.


**Does Claude Code analytics work with Zero Data Retention?**
Partially. Organizations with ZDR enabled get usage metrics (active users, sessions, accept rates) but not contribution metrics, which require session data that ZDR prevents retaining.


**Can individual developers on Pro or Max plans see this?**
No - the analytics dashboards are for Team, Enterprise, and Console (API) organizations. Individual plans don't include usage analytics.


**How do I export Claude Code usage data?**
Three routes: the leaderboard's "Export all users" CSV (full contribution data, not just the top 10), the Claude Code Analytics API for programmatic access, and OpenTelemetry export for real-time token and cost metrics in your own stack.


‍
