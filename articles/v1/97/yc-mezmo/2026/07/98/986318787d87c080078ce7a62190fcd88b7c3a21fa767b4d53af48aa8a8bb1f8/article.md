---
schema_version: "1.0.0"
document_id: "986318787d87c080078ce7a62190fcd88b7c3a21fa767b4d53af48aa8a8bb1f8"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/an-sre-agent-for-production"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:726f103ba6c452fdd95f5b09b3995c4a7aaf4195b228bdcd07339f0bedb2ac83"
---

# An SRE agent for production

AI has changed how software gets built. It hasn't changed how software gets run.


Most of the AI money in software has gone into the IDE: code generation, copilots, developer assistants, faster pull requests. That work matters. But writing software is one slice of the lifecycle. The harder problem, and the more expensive one, is running that software in production.


Production is where systems fail in ways nobody predicted. Incidents don't stay inside one service. They jump across deploys, dashboards, logs, traces, runbooks, and the on-call engineer's own judgment. When the answer is wrong in production, you don't get a failed test. You get downtime, a customer escalation, lost revenue, or a 2 a.m. page.


That's the environment[Mezmo](https://www.mezmo.com/) built AURA for.


[AURA](https://www.mezmo.com/aura) is Mezmo's SRE agent for production. It runs on your infrastructure, under your rules and your trust model, and helps SRE and platform teams investigate incidents, coordinate workflows, and move toward automation they can actually rely on, without handing over control of their data or their operational calls.


Your infrastructure. Your rules. Your production SRE agent.


## A demo is not production


It's easy to build an AI demo that looks useful. It's much harder to build an agent that holds up in production.


A demo answers a clean question against a curated dataset. A production agent works inside a live system: noisy telemetry, missing context, half a dozen overlapping tools, ambiguous signals, and real risk if it gets things wrong.


That raises the bar. An agent working near production can be helpful. An agent working in production has to be trusted. It has to know what it can see and what it's allowed to do. It has to leave an audit trail, behave the same way twice, and support the human on call instead of stepping around them. And it has to live inside the tools SRE teams already run.


So AURA isn't a generic chatbot, a dashboard overlay, or a sealed automation layer bolted onto your stack. It's an open-source agent built for production work.


## The hard parts are built in


Connecting an LLM to a few observability tools is the simple part. The hard parts are everything that makes an agent useful and safe once it's touching a live environment.


### Specialized agent workers


Production incidents are rarely one step. They need investigation, a check on the findings, and a synthesis of what actually happened, sometimes a remediation plan on top of that. AURA splits an incident across specialized workers so each one reasons over the right slice of context instead of asking a single general-purpose agent to "figure it out."


### Grounded in real evidence


A production agent can't run on vibes. AURA works from your telemetry, runbooks, service context, and incident history, not raw MCP output or whatever the model assumes is probably true. Every investigation is anchored to the evidence your team already trusts, so you get more than a plausible-sounding story.


### Output that checks itself


AURA runs a self-checking loop: it inspects the evidence, retries when something doesn't hold up, and evaluates its findings before handing them off. Production teams don't just need an answer. They need to know the answer is supported. That step is what cuts down on shallow conclusions and confident guesses.


### Human-in-the-loop control


Autonomy in production gets earned, not switched on. Some actions should stay advisory. Some should require a review. Some can go fully automated once your team trusts the workflow and understands how it fails. AURA keeps risky actions reviewable before they touch production, and you decide which workflows need sign-off and how far the agent can go. Earn autonomy. Don't grant it.


### Observable and auditable


If an agent is helping run production, you need to see what it did. AURA makes the whole workflow inspectable: prompts, tool calls, decisions, the evidence it followed, and the results. Your team can trace how it reached a conclusion and what it recommended or acted on. That trail isn't there for a compliance checkbox. It's how you debug the agent and decide whether to trust it with more.


## SRE teams want control, not another black box


SRE teams own reliability. They don't hand production access to systems they can't inspect or change, and that goes double for AI.


If an agent is going to help run production, the team needs to see how it reasons, what it calls, what context it pulls, and where a human has to approve. "Turn it on and hope" isn't a plan. Trust gets built one workflow at a time.


That's the whole design. AURA runs in your environment. It's open source and inspectable. You define how it works, wire it to the tools you already depend on, and set the limits on how far it's allowed to act.


The point isn't to replace SRE judgment. It's to take the repetitive, reactive work off people's plates so they can spend more time designing systems that hold up on their own.


## From firefighter to reliability architect


Good SRE teams do more than answer pages. They build the guardrails, policies, and patterns that make incidents rarer and less painful when they do happen. AI should push that shift further.


AURA takes the reactive work so your team can own the proactive side: defining safe workflows, improving reliability patterns, governing what's automated, and building systems that get better over time. The role doesn't shrink. It gets more leverage. The expert doesn't disappear; the expert designs the system the agent runs inside.


## Why open source matters


A closed SRE agent is quick to try, but it asks you to trust a black box with some of your most sensitive operational work. That's a bad trade for a lot of teams, especially the ones that want to see and change what they run.


[AURA is open source](https://www.mezmo.com/blog/why-we-open-sourced-aura-infrastructure-for-production-ai) because a production agent should be inspectable and extensible. You can read how it works, run it in your own environment, connect it to your own tools, and adapt it to your own workflows, all while keeping control of the rules that govern how it behaves in production.


## The path forward


Most teams are being told to "use AI." For SRE and platform teams, poking at a demo isn't the goal. You need an agent you can run on your own infrastructure, govern by your own rules, and hand real production work to.


Your infrastructure. Your rules. Your production SRE agent.


Learn more about AURA at[mezmo.com](http://mezmo.com/) , star the repo athttps://github.com/mezmo/aura


‍
