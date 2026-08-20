---
schema_version: "1.0.0"
document_id: "2f79270d37c53a9a4527d67c5f1927ac3fe603b747d9d3da7ffe6840a56ed265"
company_key: "yc-promptless"
company: "Promptless"
source_id: "yc-promptless-news-import-48c94b307195"
canonical_url: "https://promptless.ai/blog/product-updates/introducing-promptless-for-agent-instructions/"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-08-02T05:51:56.655888+00:00"
fetched_at: "2026-08-05T03:48:36.198781+00:00"
content_hash: "sha256:047017682a2480ba3eef29c88e3f12583ac01f10a4296d9d0620b70e4246f2d7"
---

# Fix skill slop before it makes your AI workforce worse

# Fix skill slop before it makes your AI workforce worse


[← Back to Blog](https://promptless.ai/blog)


Great skills and agent instructions are like giving superpowers to AI, but unmaintained skills *actually make your agents worse* . OpenAI has a Leverage Engineering team to orchestrate and maintain internal agent instructions. Promptless is your Leverage Engineering team.


Promptless ingests your agent session traces and continuously improves your skills, subagents, hooks, and other agent instructions, without your traces ever leaving your system.


Even I was shocked by how much of an impact it had across our own engineering, GTM, and ops teams. Reach out tofounders@gopromptless.ai to see what it’ll look like for you.


## Why we built this


Section titled “Why we built this”


We launched our first product last year—an AI teammate that automatically updates customer-facing docs. What we saw:


- **Customers pulled us to agent instructions:** Users literally jerry-rigged our product to start updating agent instructions, not just public docs.
- **The best tech writers were already solving agent performance issues that stumped engineering teams.** Turns out that tech writing principles like progressive disclosure and information architecture matter as much for agents as for humans.
- **Agent instructions have something ordinary docs don’t: the reader leaves traces.** Those traces reveal where instructions are missing, stale, contradictory, or unclear. We built an internal loop that uses that evidence to continuously improve our own agents, and then used it for our own internal agent instructions.
- **Our own engineers kept wasting time hand-holding our agents through mistake-laden sessions.** This only got worse as eng velocity increased, since agent dev skills would increasingly fall out of sync with the rest of our codebase.


## The impact we saw


Section titled “The impact we saw”


Yes, the numbers from our dogfooding are impressive. But here are some examples that make it real:


- **Superpowered dev environments.** It detected that one of our engineers had an insanely streamlined local dev environment, and imported it as a skill for immediate use across the team.
- **No leaked sales-deck placeholders.** It detected my repeated scolding at Claude Tag and improved our customize-sales-deck skill. No more anxiety around placeholders making their way to prospects.
- **Eliminated auth dead ends.** It saw Claude and Codex churn after universal auth tokens expire, and created a hook to prompt the user to re-login. No more long-running sessions that are just stuck/degraded by missing auth.
- **Better incident investigation.** It saw Codex ignoring our new Datadog LLMObs tools, then updated AGENTS.md and created a specialized subagent. No more coaching my team to steer agents to use tools the right way.


## How it works


Section titled “How it works”


1. Promptless organizes and versions all your team’s skills, subagents, hooks, and other agent instructions into your **Instruction Hub** .
2. The Instruction Hub is distributed as native plugins to your **AI Workforce** , refreshed on every agent startup. Each agent only gets access to what it needs.
3. A **Trace Analyzer** , deployed on your systems, ingests all agent traces, and catches churn, hallucinations, leaks, tool/MCP errors, interruptions, and user interruptions/rage.
4. The **Governance Platform** turns those findings into new/updated/deleted agent instructions, which get merged to Instruction Hub after passing an eval gate.


## See what Promptless finds in your agent sessions


Section titled “See what Promptless finds in your agent sessions”


Start with a local diagnostic to see how much instruction debt your agents have already accumulated and where it’s affecting their work.


## More from the blog


- [Slack Notifications for Suggestion Outcomes](https://promptless.ai/blog/product-updates/suggestion-lifecycle-notifications) Product Updates


- [Connect Multiple GitHub Organizations to One Promptless Account](https://promptless.ai/blog/product-updates/multi-org-github-connect) Product Updates


- [Edit Doc Collection Settings Without Contacting Support](https://promptless.ai/blog/product-updates/self-serve-doc-collection-editing) Product Updates


[← Back to Blog](https://promptless.ai/blog)
