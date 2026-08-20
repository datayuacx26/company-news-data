---
schema_version: "1.0.0"
document_id: "c8621b86af903c12709b6aa6d335ff115bffb199e096a2dd392a9ce99c92513d"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/introducing-raindrop-2/"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T10:48:02.200992+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:0d281c2e8d5076413b48c7a7d0c49d0c2154296edac50cea7f4c74fe7962912c"
---

# Introducing Raindrop 2.0: Self-Healing Agents

Today we're launching Raindrop 2.0: self-healing agents.


Agents are too complex to debug by hand. One response fans out into tool calls, web searches, sandboxes, and code execution. Failures are hard to predict, hard to reproduce, and hard to keep from recurring. Most surface only when a user complains, and then an engineer has to dig through traces to find what broke.


Raindrop 2.0 closes that loop. It detects the failure and finds the root cause, your coding agent fixes it, and the failure becomes an eval to prevent regressions. Here's how each step works.


## Detect: Issue Detection v2


It starts with detection, which we rebuilt from the ground up. Legacy eval platforms were built for the chatbot era. Agents are different: a single run can span dozens of tool calls over minutes or hours, and failures slip by silently in production.


Issue Detection v2 works in three layers:


-


**Stumbles** : a single failure in one run, the smallest unit Raindrop tracks, including ones the agent flags itself mid-run (self-diagnostics).


-


**Issues** : the same stumble recurring across many runs and users. Ranked by severity, surfacing how many users are affected and examples. Like Sentry issues, for agents.


-


**Signals** : classifiers that score every trace to track a behavior over time, like tool errors, refusals, context loss, and user frustration.


Severe issues land in Slack with how many users are affected, when the spike started, and clear examples, before users report them. It has already caught critical errors for customers.


> "If we're having an issue like a build failure or agents stuck in a loop, we see that issue in Slack. We see context like how many people are experiencing it and chats we can dig into. More than anything, Raindrop brought more visibility into the most severe issues. Raindrop is our ultimate situation report."
>
>
> Bani Singh
>
>
> AI Engineer, v0 (Vercel)


## Fix: root cause, then fix, then eval


Detection gets you to the issue. Fixing it starts with the triage agent, which investigates and finds the root cause. In Raindrop, agents are a first-class citizen: anything a person can do in the UI, a coding agent can do over MCP, so you can hand the issue straight to your coding agent.


In Claude Code, your coding agent pulls the issue from Raindrop with everything it needs: the failing trace, the root cause, and the affected runs. It makes the fix, then writes the eval with Workshop, our open-source local debugger. Workshop reads the spans, generates a code-aware eval from the real failure, and runs your agent against it until it passes. The eval asserts on what actually broke (output, tool-call sequence, state changes, files), so the bug becomes a test.


> "The place you don't want to end up is on a manual treadmill, fielding complaints about quality and fixing them yourself. There's this self-healing loop where your coding agents use Raindrop MCP, understand all the context, suggest improvements to the prompt, and open the PR."
>
>
> Andrew Hsu
>
>
> CTO, Speak


## Verify: Experiments


A fix isn't done until you know it worked in production. That's what experiments are for.


Experiments is an A/B suite for production agents: compare a model, prompt, tool, or pipeline change across millions of real interactions, broken down by metric (tool usage, error rates, conversation duration, response length) and by signal. Roll it out behind a flag and watch whether the issue dropped and whether anything else regressed. It also runs in reverse: start from a problem like an agent stuck in a loop and trace back to the model, tool, or flag driving it.


> "If you don't know whether your AI agent is getting better or worse for real users, you need Raindrop today."
>
>
> Koen Bok
>
>
> CEO, Framer


## Announcing VPC for Enterprise


The whole loop can run in your own cloud. Raindrop 2.0 now deploys directly in your VPC, so traces and agent data never leave your infrastructure.


It comes with the controls enterprise teams expect: SOC 2 Type II compliance, PII redaction, SSO, and role-based access. We're rolling VPC out with a select group of initial partners. If you're interested,


[reach out here](https://cal.com/team/raindrop-ai/chat-15-min) .


## Get started


Raindrop 2.0 is live, and already running at some of the fastest-growing AI companies, including Vercel, Speak, Clay, and Framer, and at Fortune 100 enterprises.


Try it at


[raindrop.ai](https://raindrop.ai/) .
