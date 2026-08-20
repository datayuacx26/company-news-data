---
schema_version: "1.0.0"
document_id: "bf4caeea41a452c6123b5f61bd68c09c8e15f3a58fb6637a3a784c64492466f2"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/one-agent-per-customer"
published_at: "2026-07-07T17:51:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:20ee327f1e562d3c5cbb6de935e8302b404209e81c9da2bef316eeb2cf67fbd7"
---

# One Agent Per Customer

A typical morning in my life as a Solutions Architect used to be quite hectic. While I was pouring my first coffee, customer requests and escalations would pile up in real time, short and critical-sounding, all saying things needed to be taken care of immediately. I manage about 20 to 30 accounts, and the context about each customer is spread across Salesforce, email, Slack, Zendesk, GitHub, Gong, and other internal systems. So before responding or taking a call, half the battle was a marathon of opening tabs and gathering the information.


One customer sent a Slack over the weekend: one of their data channels needed a quick configuration change before Monday reporting, and they flagged it as urgent. I almost missed it while skimming the dozens of messages that poured in that morning. And then it dawned on me. The part eating up my time was not the actions of thinking and resolving. It was reconstructing the context from various systems and keeping track of many channels at once.


## So I gave every account its own agent


Instead of building another dashboard or another workflow to support the meticulous information gathering and stitching, I decided to give every customer their own dedicated agent. The agent never sleeps, watches the customer’s incoming channels, and keeps track of the current context scoped to that customer. The main difference here, though: the agent decides the next best step and performs the action, gated on a final review before the email or Slack message hits the customer’s inbox. The agent runs the extra mile and presents a set of ranked options, cutting down not just the time to research the solution but also thinking multiple steps ahead, similar to how my own thinking would work when I draft the technical account plan for the customer. One plan per customer, multiple options ranked on impact, reliability, and cost, all presented for review by the customer’s agent as soon as there is more context available.


The next learning had to come fast. Agentic runtimes are nondeterministic and inherently unpredictable. When you point a long-running agent towards multiple production APIs and evergreen context, the number of possibilities to calculate the next move is of exponential nature, like in a chess game. My first naive approach was to build better prompts, stricter instructions, rules in capital letters that say “Do not ever send a Slack DM without asking me first.” Prompts got longer, rules became skills, and the next model generations became smarter, but none of it made the agent deterministic in production. It turned out to be impossible to prompt your way to reliability. So the next insight was to keep thinking separate from context. The agent loop is nondeterministic and applies intelligence. Everything else, like retrieving information, building the context, and combining different outputs to ground the model, those are deterministic actions: schedules, procedures, access, and approvals, which need to happen reliably, every single time, with the same outcome.


## The context layer


The boring, but critical part first: data plumbing. The agent is only useful with the data and context it can access to ground the model in truth. That data lives, for my accounts, in six to seven distinct systems which don’t usually talk to each other. The Airbyte Agents platform provides one single access point through the agent connectors. It’s the same connector foundation that used to move data into warehouses; now it’s used by agents instead. The agent can access the data without accessing credentials, so the auth lives in the connector layer, where it should be.


Every connector provides a typed set of tools exposing each system over MCP, and the agent talks to the CRM with the same standardized language as to the support case data or the data lake. That wiring makes the system boring, or more accurately, predictable, which is the target for production systems that need to be as unexciting as possible when it comes to reliability.


The hidden cost behind dozens of MCPs, always wired up, comes to realization later. Each connector has a few tools, and the number of tools compounds quickly. I stopped managing tool selection manually somewhere north of thirty MCP servers, so loading a few hundred tool schemas into the context window became quite expensive. That’s where deferred tool loading came in handy. The agent is smart enough to fetch the full schema only when the tools are needed; otherwise the tool names are enough to keep a reference to the capabilities of the MCP connector. Large surface area, small working set.


## The Context Store


My first versions kept context the dumb way. Every run re-read the same threads, the same tickets, the same call notes, straight from the source APIs. That works right up until it does not: context windows balloon, paginated list calls drag on, and rate limits stop being theoretical when twenty agents wake up in the same hour.


Airbyte Agents solves this with the Context Store, a managed and searchable replica of the useful entities from every connected source. Instead of crawling the support desk ticket by ticket to answer "what is open for this account," the agent runs one structured search against the store, with filters, field selection, sorting, and pagination, translated from a plain-language ask. Fewer tokens, faster answers, and the source APIs stay unbothered.


The read and write paths split automatically. Reads that can tolerate a slightly stale copy go to the store. Anything real-time, and every write, goes through the connector to the live API. The store is scoped to the organization, so an agent only ever searches data its own org owns. This is also the piece that makes one agent per customer honest: their context stays current not because the agents never sleep, but because the replica is maintained for them while they do.


## Skills


Skills are how I claw determinism back from the model. A skill is a procedure in a Markdown file: a name, a description, trigger phrases, then numbered steps. My morning triage is one. It pulls every ticket that changed since the last successful run, cross-references the support desk against Slack and the engineering tracker, computes SLA state deterministically instead of trusting anyone's dashboard, and writes me a brief sorted by who, let’s just say, needs attention most urgently.


The point is not that the steps are clever. The point is that they are the same steps, in the same order, with the same output shape, every single run. When I improve the procedure I edit the file, and every future run inherits the change. The behavior lives in git, not in my head. This mattered the week I was traveling and slammed: the briefs kept coming out identical, because nothing about them depended on me remembering anything.


## Loops


Skills say what. Loops say when. A loop is a scheduled tick registered with the operating system scheduler. It wakes, does one unit of work, and goes back to sleep. No open session. No human present.


The design detail that matters is that a tick runs in one of three modes, and the mode is a cost decision. A deterministic tick is pure shell and spends zero model tokens. An agentic tick invokes the model. The third mode is the one I use most: a gated tick, where a near-free shell check runs first and the model is invoked only if the check finds something. Most ticks find nothing and cost nothing.


My busy accounts tick hourly through the workday, weekdays only. Quiet accounts get one tick a day, in the early afternoon, when my laptop is reliably open. That asymmetry is the entire affordability story. The model is the exception, not the rule, which is what lets the whole fleet fire all day without the bill becoming a second mortgage.


## Watchers


Loops poll on a schedule. Watchers wait for an event, and the reason they exist is a weekend I am still slightly embarrassed about. I once left an agent looping to see whether a customer had replied on a thread. Every few minutes it woke, re-read the whole context, concluded nothing had changed, and went back to sleep. It did this for two days. I paid for a full model invocation, dozens of times, to learn "no news" over and over.


A watcher is the grown-up version. It polls the API directly from a shell script, holds no model context, and burns nothing while idle. When the reply actually lands, it wakes the agent, which spends tokens exactly once, at the moment there is finally something to do. Right now watchers are sitting on a handful of customer threads and one email chain for me. Idle is free. You pay only for the event.


## The orchestrator


One agent per customer still leaves jobs that span all of them, like the weekly sweep across the whole book of accounts. For those there is an orchestrator, and the important thing about it is that it is dumb on purpose. It does no reasoning of its own and spends no tokens. It fans the job out, one scoped child agent per account, each in its own isolated context, then collects the results.


Isolation is the payoff I did not appreciate until it saved me. One child hit a permissions error mid-sweep and died. The other nineteen finished fine, and the report simply noted the gap. The old serial version of that job would have dragged one bloated context through every account and taken the failure down with it.


## The one rule: it drafts, I send


Nothing customer-facing sends automatically. Not email, not Slack, not a support ticket reply. Every outbound thing the system produces becomes a draft, and then a small bot DMs me out of band so the ping reaches my phone even when no session is open. I read it, sometimes I edit it, and then I send it. Every time.


The line I hold is reversibility. Reading is reversible, so the agent does it alone. Drafting is reversible, same. Sending is not, so a human owns it, with no exceptions for confidence. The nightmare this prevents is specific: a fluent, plausible, wrong message going to my biggest account at the worst possible moment. And the rule is enforced in the plumbing, not in the prompt. The send path does not exist without me.


## The shape, assembled


One picture instead of a thousand words of narration: a deterministic control plane drives the agent runtime, the runtime reasons over a governed context layer, and every outbound action is forced through the human gate before it touches the outside world.


## Why this shape


Ranked, because the three properties are not equal. Safe to trust comes first. Access is governed at the connector layer and nothing irreversible happens without me, and without that property none of the rest would be allowed anywhere near my customers. Cheap to leave on comes second, since always-on is only real if idle costs nothing. Reliability I rank last, oddly. Not because it does not matter, but because it falls out of the other two: when procedures, schedules, and approvals are deterministic and the context lives in a managed replica, a failure degrades into a slightly stale read instead of a lost signal.


## Build your own


Nothing here is specific to solutions architecture. If your work is spread across many tools, and you can name the moment each week where the gathering eats the thinking, the pattern transfers. A governed context layer through Airbyte connectors, with the Context Store keeping reads cheap and searchable. Procedures as version-controlled skills. Loops with cheap gates, and watchers so waiting is free. An orchestrator when the work is parallel. A human gate, in the plumbing, on every irreversible action.


The models will keep getting better without any effort from you. The architecture is the part you build, and it is the part that decides whether you have a production system or a demo. If you want to give an agent governed access to your own systems, that is what Airbyte Agents is for. Start with the context layer, give the agent something real to read, and keep a human on the send button.


*Ready to build context-aware AI on your own data?[\[Get started with Airbyte Agents.\]](https://app.airbyte.ai/)*
