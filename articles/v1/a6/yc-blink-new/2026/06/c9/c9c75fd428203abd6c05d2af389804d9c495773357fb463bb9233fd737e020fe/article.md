---
schema_version: "1.0.0"
document_id: "c9c75fd428203abd6c05d2af389804d9c495773357fb463bb9233fd737e020fe"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/claude-code-dynamic-workflows"
published_at: "2026-06-12T12:57:01+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:b2c2dd066385f36bd56f5fc9d8b49bcf0fd43ef44271ca8ca9f09546b8474ce6"
---

# Claude Code Dynamic Workflows: Run Hundreds of Parallel Subagents on a Single Task

## How to start a workflow


Dynamic workflows are available on Pro, Max, Team, and Enterprise plans. They're also available via the Claude API, including Amazon Bedrock, Vertex AI, and Microsoft Foundry.


Turn on auto mode first — it's required for the best experience. Then you have two paths.


1


#### Ask Claude directly


Type "Create a workflow" followed by your task description. Claude reads your prompt, plans the subtask breakdown, and starts the parallel run immediately. Use this when you know the task warrants full orchestration.


2


#### Enable ultracode


Open the effort menu in Claude Code. Enable the` ultracode` setting. This sets effort to` xhigh` and lets Claude decide automatically when your task needs a workflow. You don't have to ask — Claude reads the scope and triggers orchestration when it fits.


` Ultracode` is the hands-off path. Set it once, and Claude makes the judgment call for every task going forward. That decision — "does this warrant a full workflow or a standard session?" — is exactly what ultracode offloads.


How Claude Code dynamic workflows orchestrate parallel subagents — Claude plans, fans out, verifies, and returns a single coordinated result


Blink


*How Claude Code dynamic workflows orchestrate parallel subagents — Claude plans, fans out, verifies, and returns a single coordinated result*


## What tasks trigger a workflow


Three categories show the strongest results in early access.


**Codebase-wide audits.** Bug hunts, security hardening passes, profiler-guided optimization reviews, dead code identification. Claude searches your entire service in parallel, then runs independent verification on every finding. The report surfaces real issues — not false positives from a single pass.


**Large-scale migrations.** Framework swaps, API deprecations, language ports that span thousands of files. This is where dynamic workflows have the most dramatic compression effect on timeline.


**High-stakes decisions.** When the cost of a wrong answer is high, Claude spins up adversarial agents to try to break the result before you see it. Independent attempts run in parallel. The run iterates until answers converge and you get a result worth trusting.


These aren't the only use cases. But they're the patterns where parallel verification produces the biggest difference versus a standard session.


## Usage and access


Dynamic workflows consume significantly more tokens than a typical Claude Code session.


Start scoped. Run one workflow on a contained task before pointing it at your most complex migration. That gives you a real calibration point for what this costs in your work specifically.


**Plan access:**


- **Max, Team, Enterprise** : workflows on by default
- **Claude API** (Bedrock, Vertex AI, Microsoft Foundry): workflows on by default
- **Pro** : enable in` /config`


The first time a workflow triggers in any session, Claude Code shows you exactly what's about to run and asks for confirmation. You see the full plan before execution starts.


Organization admins can disable workflows entirely through managed settings. If your company has compliance requirements around automated agent runs, or cost controls you need to enforce, that setting lives in your admin panel.


## What else Anthropic shipped this sprint


Dynamic workflows aren't the only new capability. Two related announcements shipped alongside the GA.


**Claude Managed Agents — scheduled deployments and environment variable vaults (public beta, June 9, 2026):** You can now run Claude Code agents on a cron schedule. Define the schedule, point it at a task, and the agent runs without you present. The same update added environment variable vaults — secure storage for the secrets your agents need at runtime, without hardcoding credentials in prompts.


**Routines in Claude Code (GA, April 14, 2026):** Routines let you define repeatable multi-step sequences that Claude Code executes on command. Less about massive parallel scale, more about encoding your team's standard procedures so agents run them consistently every sprint.


The three features cover different shapes of work. Dynamic workflows handle the large, one-off complex task. Routines handle the repeatable procedure your team runs every two weeks. Managed Agents handle anything that needs to run on a schedule, unattended.


If you're deciding between Claude Code and Cursor for your daily workflow, see our[Claude Code vs Cursor comparison](https://blink.new/blog/cursor-vs-claude-code) — dynamic workflows are now a significant differentiator in that comparison.


## Build Dynamic Workflows Into Your App With Claude Code or Cursor


Add Blink as your full-stack infrastructure layer — install[14 skills](https://blink.new/docs/cloud/tools/skills) in one command:


```text
npx   skills   add   blink-new/blink-plugin
blink   login
```


Then ask your agent:


> "Build me a multi-agent orchestration app that fans research tasks out in parallel and host it on Blink."


Your agent provisions database, auth, backend, and hosting automatically — no Vercel config, no Supabase account.[Learn more about Blink Cloud →](https://blink.new/cloud)


Getting started with Claude Code dynamic workflows — one command triggers parallel subagents on your most complex engineering tasks


Blink


*Getting started with Claude Code dynamic workflows — one command triggers parallel subagents on your most complex engineering tasks*


Both start a dynamic workflow.` Ultracode` is the hands-off path — you enable the setting once and Claude decides automatically when your task warrants orchestration. Asking "Create a workflow" directly starts one immediately on that specific prompt. Use` ultracode` when you want Claude to judge the scope. Use the direct ask when you've already made that decision yourself.


Anthropic describes the scale as "tens to hundreds" of parallel subagents per session. The Bun port example ran hundreds of agents in parallel — two reviewers per file. The actual count depends on how Claude plans your specific task, not a hard cap you configure.


Max, Team, and Enterprise plans have workflows on by default. Pro plan users can enable them in` /config` . Dynamic workflows are also available via the Claude API — including Amazon Bedrock, Vertex AI, and Microsoft Foundry, on by default for API access.


Anthropic describes usage as "substantially more tokens than a typical Claude Code session." The exact multiple depends on task scope. Start with a contained test task to calibrate costs before running your most complex migration — that gives you a real number to plan around.


Yes. Organization admins can disable workflows through managed settings. Every user also sees a confirmation prompt the first time a workflow triggers — Claude shows you the full plan before execution starts.


Three tools for three different shapes of work. **Dynamic workflows** : massive one-off parallel tasks — a migration, a codebase audit, a rewrite. **Routines** : repeatable multi-step procedures your team runs on command. **Claude Managed Agents** : running any Claude Code task on a cron schedule, with environment variable vaults for runtime secrets.


Yes. Dynamic workflows are available on the Claude API and all three major cloud integrations: Amazon Bedrock, Vertex AI, and Microsoft Foundry. They're on by default for API access — no additional configuration required.
