---
schema_version: "1.0.0"
document_id: "f99498b11ea2fc6163c5acd956b96f0d63772ef5e5becf8900e04c45adaf62b4"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/the-insights-factory-how-we-run-deep-data-investigations-with-llm-agents"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:5acecda506923f72cd0c673d18735e7858d37ab232b67e171476a619535bac6e"
---

# The Insights Factory: how we run deep data investigations with LLM agents

*How Photoroom's data team turns one hard question into hundreds of small, auditable queries, run by agents and kept honest by a human.*


## **In one sentence**


The Insights Factory is a system that turns one hard question into hundreds of small, auditable queries. They are run by agents and kept honest by a human. Three properties hold the whole thing up:


1.


the investigation lives in one file instead of a context window


2.


every agent gets one small job with a fresh context


3.


and a human frames the question and validates the answer.


The machinery that wires this together is a small set of skills, reusable playbooks that dispatch, log, and sequence the work. There are only five of them. We will come back to what each one does, but keep that word in mind as you read: the system is a handful of skills orchestrating a lot of small agents. They share a common memory through a file.


## **This is a playbook to steal**


Someone on your team asks: "Can you help me understand the evolution of Max ARR retention?”


It sounds like a single question. But to answer it properly you have to pull a retention baseline, split renewals by cohort, decompose churn by type, cut it by platform and country, check for a pricing or product change in the window, and reconcile the numbers against the source of truth. That is six to ten tasks and, in our experience, fifty to over a hundred separate SQL queries. It is a small project wearing the costume of a Slack message.


We built a system to run these questions end to end. It is reliable, auditable, and cheap (usually around $5. We could also run it from our Claude Max subscription). It is part of our normal weekly workflow now. This article explains how it works in enough detail that you can build your own version.


We did not invent the core idea. We adapted it from[Rondo](https://github.com/eltonio450/rondo) , an open-source project from[Antoine Sauvage](https://www.linkedin.com/in/antoine-sauvage-0679b761/) , CTO of Ovrsea and my former boss there 🥹, built for autonomous coding. I discovered it at a conference talk by[Marie-Sophie Richard](https://www.linkedin.com/in/mariesophie-richard/) (Head of Data at Ovrsea), saw that it would transfer cleanly to data analytics, and ported it from code delivery to data analytics. Credit belongs there. Everything below is the honest version, including the parts that are still unsolved. If you take the pattern and improve it, we would love to hear how.


## **First, the part you cannot skip: foundations**


Before any of the agent machinery matters, one thing has to be true. Your data foundations have to be excellent.


I don’t say this as a tiny caveat, it is the load-bearing wall. Plugging agents onto bad data is useless at best and dangerous at worst, because an agent will not tell you the data is bad. It will produce a confident, well-written, completely wrong answer, and it will produce it faster than any human could. A person who runs into a broken metric usually notices something is off. An agent runs the query, reads the result at face value, writes it into the synthesis, and moves on. Bad foundations do not slow the factory down. They industrialise the mistake.


So the real prerequisite is a data stack you already trust by hand.
Two things in particular have to be solid. Metrics have to be defined once and defended. Every metric lives as version-controlled code, surfaced through our BI tool, so there is one definition of ARR, one of retention, one of activation, with no spreadsheet drift. And quality has to be enforced rather than hoped for, through pre-push and CI checks, production monitoring, and automated plus human review of the models.


This is the quiet prerequisite behind everything else. An agent can only be trusted with a query because the layer underneath it is already trustworthy. If your metrics are ambiguous or scattered across spreadsheets, stop here and fix that first. **The rest of this article assumes it is done.**


## **Why LLMs struggle with this exact kind of work**


With good foundations in place, why not just ask one model the whole question? Because the interesting questions do not fit in one query. The ones worth asking are open-ended meta-analyses: why did something move, across platforms, cohorts, and time. Answering means decomposing, exploring, and following the evidence.


Here is the shape of that, from real investigations we have run. Six to ten tasks each, five to fifteen queries per task, hundreds in total.


Investigation


Tasks


Queries


ARR snapshot drift over time


10


~65


Self-serve Pro new-ARR spike since May 6th


11


~78


Early signals for high-potential users


6


~59


Max ARR movements, board readout


9


~123


Pro to Max upgraders and gap-to-budget


7


~62


Three structural limits of LLMs make this hard, and they bite hardest exactly here.


1.


**Performance degrades with context.** The more you stuff into the window, the worse the reasoning gets. Even the best models drop off: they lose earlier facts and blur the thread. There is a clear demonstration in[this paper on long-context degradation](https://arxiv.org/abs/2502.05167) .


2.


**Cost rises with context.** Providers charge per token, and every token re-read at each step is billed again. A ten-task investigation that drags its whole history along pays for that history ten times over.


3.


**One-shot models get lost in multi-step work.** Ask an LLM to "pull Max retention by weekly cohort" and you get one clear job with one clear answer. LLMs are excellent at that. Ask it to "figure out what changed and why" and it wanders, forgets steps, and contradicts itself.


## **Design principles**


An LLM is excellent on an atomic task and poor on a six-step investigation. Four rules turn that insight into a working system. We took the shape from Rondo and tailored it to our stack, our metrics, and our review culture.


1.


Memory lives in a file, not in a context window, and that file is the single source of truth.


2.


Work is split into atomic tasks, each handed to an agent with a fresh context.


3.


Skills orchestrate the flow, dispatching, logging, and sequencing the work.


4.


And a human bookends the process, framing the question at the start and validating the answer at the end.


Each principle solves one of the three limits directly.


-


Fresh context per task means no agent carries a long history, so performance never degrades.


-


External memory in the file means tokens are not re-billed at every step, so cost stays flat.


-


Atomic tasks mean every agent only does the one thing LLMs are good at, so multi-step work stops going off the rails.


The three problems fall to the same architecture at once.


Concretely, the system has three moving parts: a markdown file that holds the long-term memory, the skills and agents that do the work, and the tools they reach through, the same Snowflake, Omni, and Amplitude we query by hand.


## **How to build it, part 1: the[investigation-plan.md](http://investigation-plan.md/)**


The heart of the system is a single markdown file per investigation. If you build one thing from this article, build this. It has three parts. This structure (Frontmatter / Brief / Logbook) is directly copied from Ovrsea’s Rondo. I really like it.


### The frontmatter


The frontmatter is the control panel.


A short metadata block declares how the machine runs:


-


a` status` that moves through` draft` ,` ready` ,` in_progress` ,` awaiting_review` ,` done` ;


-


a` mode` that sets the shape of the final synthesis;


-


the model the sub-agents run on;


-


a token budget per task and per investigation;


-


and a flag for whether a shareable HTML report is expected.


### The brief


The brief frames the question before any query runs, and a human writes it, with the help of a skill.


-


It names an owner accountable for the answer,


-


states the context and objective,


-


sets a decision frame: what decision this feeds, the action threshold, and a kill criterion so the investigation knows when to stop.


-


It lists watch-outs, the known traps and biases and which sources to prefer or avoid.


-


It holds the ordered list of atomic tasks.


-


And it records the deliverable, what "done" looks like.


### The logbook


The logbook records everything as it happens, and the AI writes it.


-


Every task gets a findings entry that links to the chart or query, summarises the data, names the insight, and notes how it changes the next steps.


-


The agent may add, skip, or reorder tasks as it learns, and every adjustment is logged.


-


A run log table tracks each task with its duration and tokens.


-


When the work is done, an agent writes the synthesis into the same file: in exploration mode, a landscape map and a list of decisions; in decision mode, a verdict, the evidence, and a recommendation.


Two guardrails come for free.


1.


Atomic tasks beat hallucinations, because a small bounded task has little room to invent and its answer is easy to verify.


2.


And the plan is fully auditable, because findings, decisions, durations, and tokens all sit in one file. No black box, and you can retrace every step.


## **How to build it, part 2: the skills, the folder, and the loop**


Around that file sit the five skills, a folder, and an orchestration loop.


The skills are the reusable playbooks:


1.


**insights-factory-brief-builder** : the planning partner.
It co-writes the investigation plan with the human through a structured conversation: the question, the stakes, which data to use (and avoid), and an ordered list of analysis tasks. It outputs` investigation-plan.md` with status` ready` .


2.


**insights-factory-run-investigation** : the autonomous orchestrator.
It loops *insights-factory-run-next-step* as sub-agents (one per task, each with its own fresh context), re-reads the plan file between steps, respects a two-failure circuit breaker, and can adapt the plan as findings come in — skipping tasks that became irrelevant or adding new ones. Sets the plan status to` awaiting_review` when done.


3.


**insights-factory-run-next-step** : the analyst.
It executes exactly *one* task from the plan: read the plan, do the analysis, write the findings into the logbook, update progress, exit. Designed to run in a fresh sub-agent context window so every step gets full attention, with zero drift from earlier work.


4.


**insights-factory-synthesize** : the decision writer.
It reads the completed logbook and writes the final deliverable: a verdict, evidence, and recommendation in *decision* mode, or a landscape map plus decisions-to-take in *exploration* mode. Saves a dated, immutable snapshot in` results/` and marks the investigation` done` .


5.


**insights-factory-report** : the publisher (optional).
It turns the finished investigation into two self-contained, shareable HTML documents: a long-form bundle (brief, synthesis, glossary, findings, SQL appendix) and an executive slide deck with one slide per finding.


They are what "skills orchestrate the flow" means in practice.


Each investigation gets its own directory. Alongside` investigation-plan.md` you keep an immutable dated snapshot of the results once the synthesis is approved, and an` artifacts/` folder holding one` .sql` file per task (every query each agent ran, with a header and expected values) plus a` glossary.md` defining every term. The SQL log is the key trick: it sits outside the main agent's context, so it is auditable but never re-read. That is what keeps both quality and cost flat as an investigation grows.


The loop is a dispatcher. While the plan's status is ready or in progress, it keeps spawning sub-agents, one per atomic task. Each sub-agent gets a short context summary plus its single task, runs its queries, logs its SQL, writes its findings, and leaves. When every task is done, the dispatcher stops and hands the raw findings log to a human. Nothing gets synthesised into an answer until a person has looked at the evidence.


If you know Rondo, the mapping from coding to data is clean: feature plan becomes investigation plan, a code change becomes a data pull or SQL query, CI after each commit becomes the watch-outs and kill criterion checked between tasks, and the final PR review becomes the synthesis review.


Here is a detailed drawing of the system


## **See it in action: Max ARR retention**


Here is that first question running through the system.


1.


**A question arrives** :
"Can you help me understand the evolution of Max ARR retention?".


2.


**Human and Claude build the brief together.**
A short back-and-forth sharpens the objective, the watch-outs, and the decision frame, and fills in the brief.
In practice: I ask Claude Code to run an investigation, and basically copy paste the question. The brief builder skill is triggered on “run an investigation”. It asks all the questions necessary to fill in the brief, as described in part 1.


3.


**Claude proposes a list of atomic tasks.**
Retention curve baseline, month-one renewal trend, churn-type decomposition, and so on. The human reviews and edits the list before anything runs.


4.


**The human gives the go.** "Ok, run the investigation."
An explicit gate: the factory only starts when a person says so.


5.


**The dispatcher spawns sub-agents.**
While the plan is ready or in progress, it keeps spawning agents, one per task.


6.


**One sub-agent, one task.**
Each agent gets a context summary plus a single task, runs its queries, logs its SQL to a` .sql` file, writes its findings, and leaves. Its context is then gone, which is what keeps quality and cost flat.


Each agent logs their tasks, how long it took to run, and how many tokens were used.


*For this investigation, we spent **560,377 tokens** across the six task sub-agents, about $3 for Sonnet. You have to add the cost of building the brief, running the synthesis and generating the HTML reports, and the cost of running queries, which adds up to around $5.*


7.


**A human reviews the raw findings.** When all tasks are done the dispatcher stops and hands the findings log to a person. Nothing is synthesized into an answer until a human has looked at the evidence.


8.


**An agent summarizes.** It reads the full findings log, not a stale context, and writes the synthesis and recommendation into the plan.


9.


**A human validates the synthesis.** The second bookend.


10.


**The human asks for a report, if needed.** Only if the answer needs to travel.


11.


**An agent creates a shareable HTML report.** Findings adapted to whoever is reading, from analyst to board.


*dummy HTML report for the executive team.*
*(the numbers and recommendations have been changed so it is ok to share).*


The human time is front-loaded into framing and review, roughly fifteen minutes at the start and a few minutes at the end. The agent work in the middle is fast and cheap.


*Human time is front-loaded into framing and review; the AI work is fast and cheap. Illustrative timings.*


Everything lands in the codebase. Plans, results, reports, and SQL are versioned, reviewed, and reused like any other code we ship, rather than trapped in a chat window.


## **The impact**


Days become hours. A deep investigation that used to take days end to end now takes one to a few hours, and the human is only in the loop for framing and review. It is also cheap. A typical investigation runs on roughly 600k to 700k tokens, report included, which is a few dollars on Sonnet. It stays that low by design: every sub-agent works in a fresh context, so the investigation's history is never dragged along and re-billed. We run at least once a week, and it is a normal part of how we work now, used on real questions every week..


## **Next steps**


We run this on our own investigations today, and it works because the data team already has what it needs: direct access to Snowflake, Omni, Amplitude, Notion…; a codebase where creating and editing files is natural; and the habit of running the extra checks. The next step is to open it to anyone in the company through an internal skills marketplace (see[this article](https://www.photoroom.com/how-we-built-a-skills-marketplace-for-claude) ), and this surfaces real problems we have not fully solved yet.


-


Data access: what does a non-analyst connect through, and with what permissions?


-


Where do the files live? An analyst works in the repo but a marketer does not.


-


Cost at scale: a few dollars per run is fine for a small team but needs watching company-wide.


-


And verification is the hardest part. When an analyst doubts a result they can read the SQL. How does someone who cannot read SQL check the answer when it matters? We do not have clean answers yet, and if you do, we are listening.


## **Take it and build your own**


If there is one thing to carry away, it is the insight the whole system rests on: an LLM is excellent on an atomic task and poor on a long investigation, so keep the memory in a file and the tasks small, and let a human own the two ends. And build it only on foundations you already trust ([see this article](https://www.photoroom.com/inside-photoroom/ai-for-bi-write-definitions-as-code) ), because an agent on bad data just makes wrong answers faster.


You don’t need our code to do this. The blueprint above is the whole system: one markdown file for memory, a brief a human writes, atomic tasks an agent runs with fresh context each time, a synthesis a human validates, and a few small skills to sequence it. Build it against the tools you already query by hand. If you want to see where the pattern came from,[Rondo](https://github.com/eltonio450/rondo) is open source, though it is built for shipping code, so treat it as inspiration rather than a starting point: trade its feature plan for an investigation plan, its code change for a data pull, its CI for the watch-outs and kill criterion you set up front, and its PR review for a synthesis review.


Then tell us what you learned. We shared this because good ideas should travel, and we would rather see ten teams improve on it than keep it to ourselves.
