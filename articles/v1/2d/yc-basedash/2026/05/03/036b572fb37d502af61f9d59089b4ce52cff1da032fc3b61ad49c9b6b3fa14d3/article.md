---
schema_version: "1.0.0"
document_id: "036b572fb37d502af61f9d59089b4ce52cff1da032fc3b61ad49c9b6b3fa14d3"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-triage-data-requests-an-intake-framework-for-analytics-teams/"
published_at: "2026-05-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:d6d151783afb6d41df221da476c25f93d6811ee601a2460f2d2190efb428f7f0"
---

# How to triage data requests: an intake framework for analytics teams

Most analytics teams do not have a capacity problem. They have an intake problem. A senior analyst can answer a well-scoped question in twenty minutes, but the same question takes half a day when it arrives in three different Slack channels with no context, no deadline, and no clear owner. Multiply that by twenty requests a week and the team is permanently behind.


This guide is for analytics leads, founders, and operators who own the data request queue at a startup or growth-stage company. It defines the request types you will see, gives a triage framework that takes about two minutes per request, sets a default SLA model, and explains which requests should never become tickets in the first place. The goal is judgment that a small team can run consistently, not a heavy ticketing process.


## The short answer


A working intake system has four moving parts. Most teams are missing two or three of them.


1. A single front door for requests, with a structured intake form.
2. A taxonomy that sorts each request into one of five types.
3. A triage rule that scores priority and effort, and decides what to do today.
4. A clear list of requests the team will not take, with a self-serve path instead.


If you put those four in place, the queue stops being a source of constant context-switching and starts being a planning tool. The rest of this post is the longer version, with the specific fields, scoring, and routing rules that make it work.


## Why data requests pile up


Before fixing intake, it helps to understand why requests grow faster than the team that answers them.


- **Asking is free, answering is not.** A two-line Slack message can take an hour of warehouse exploration to answer well. The asymmetry is huge, and it is invisible to the person asking.
- **Every dashboard creates new questions.** A new revenue dashboard surfaces three previously hidden inconsistencies, and each becomes its own follow-up request.
- **There is rarely a clear definition of done.** “Can you pull churn for me?” has no acceptance criteria, so the analyst over-delivers, and the next person asks for the same thing.
- **The data team is treated as a help desk.** With no triage, every request is implicitly P1, and the team works on the loudest request rather than the most important one.
- **Self-serve is half-built.** The BI tool exists, but the dashboard the requester actually needs is buried in a folder named` archive` .


These pressures do not get better with hiring. They get better with structure.


## A taxonomy of data requests


Almost every request a small data team receives falls into one of five buckets. Naming them speeds up triage and makes routing obvious.


### 1. Lookup


A specific number against a known data source. “How many sign-ups did we have last week from paid channels?” “What is our active customer count in EMEA?”


These are the cheapest requests if self-serve works, and the most expensive if it does not. They should almost always be answered by the requester, an existing dashboard, or an AI BI tool against a governed connection.


### 2. Investigation


A “why” question. “Why did MRR drop in week 19?” “Why are conversion rates lower for users from our new onboarding flow?”


Investigations are open-ended. They benefit most from an analyst’s judgment and the least from hand-coded SQL by the requester. They are also the requests most likely to expand if not scoped tightly.


### 3. New artifact


A dashboard, report, alert, or recurring delivery the team does not have yet. “Can we set up a weekly support metrics email?” “Build a customer health score dashboard.”


These have the highest cost per request because they create ongoing maintenance. Treat them as small product launches.


### 4. Modification


A change to an existing artifact. “Can you add a region filter to the marketing funnel dashboard?” “Change the revenue dashboard to use net revenue.”


Most analytics queues are dominated by modifications, not new artifacts. They are usually quick to do and easy to forget, which is how dashboards drift over time.


### 5. Data engineering


A pipeline, model, or schema change. “Can we backfill paid status?” “Stripe webhooks stopped writing to the warehouse.” “We need a new dim table for products.”


These are not analytics requests, but they enter the same intake door. Routing them to the right place quickly is half the value of a triage system.


A simple test: if a senior analyst can answer the request with a query against the existing warehouse, it is type 1, 2, or 4. If it requires a new dashboard, it is type 3. If it requires a change to data plumbing, it is type 5.


## The intake form


Almost every problem with a data request can be traced back to a missing piece of context. A short, structured intake form gets that context up front and prevents the back-and-forth that wastes everyone’s time.


A useful form has six fields and no more.


1. **What question are you trying to answer?** Plain English. One sentence.
2. **What decision will this change?** If the answer is “nothing specific,” the request should probably wait.
3. **When do you need it by?** Real deadline, not a wish. Tied to a meeting, a board prep date, a launch, or a customer commitment.
4. **What have you already tried?** Existing dashboard, AI tool, asked someone else. This catches duplicates and reveals gaps in self-serve.
5. **How will the answer be used?** Slack post, slide, dashboard, internal doc, customer-facing report. Determines fidelity.
6. **Who is the audience?** You, your team, the exec team, an external party. Determines review effort.


Two of these matter more than the others. The decision the request will change, and the deadline tied to a real event. Together they tell you whether to do the work today, this week, or never.


## The triage matrix


Every request gets two scores during triage. Both can be assigned in under a minute.


### Priority


A function of how important the decision is and when it has to be made.


- **P1, blocks a decision today.** Board meeting tomorrow, a launch decision this week, a support escalation right now. Drop other work.
- **P2, important and time-sensitive.** Tied to a clear deliverable in the next week or two.
- **P3, useful but not urgent.** A new dashboard, a useful investigation, a “nice to have” alert.
- **P4, would be interesting.** No deadline, no decision, no clear owner of the answer.


### Effort


A T-shirt size estimate of analyst time, not calendar time.


- **S, under 30 minutes.** Single query, existing data, no new artifact.
- **M, half a day.** Multiple queries, light investigation, simple chart.
- **L, one to three days.** New dashboard, multi-source investigation, modeling work.
- **XL, week or more.** New data model, complex investigation, multi-table backfill.


### The triage rule


Map the two scores to a default action.


S (≤30m) M (half day) L (1–3 days) XL (week+)


**P1** Do now Do today Do today Escalate


**P2** Do today Schedule Schedule Plan


**P3** Self-serve Schedule Plan Defer


**P4** Self-serve Defer Defer Decline


Two principles drive the table. First, P1-XL is almost always misclassified. If the work is genuinely a week and the decision is genuinely tomorrow, something has gone wrong upstream and the right answer is to escalate, not heroics. Second, P3-S and P4-S are the requests where a self-serve push has the highest leverage, because the cost of teaching someone is similar to the cost of answering.


## An SLA model that works


Most teams do not need formal SLAs. They need predictable defaults that everyone understands.


A reasonable starting point for a small team:


- **P1:** acknowledged within an hour during business hours. First answer or status update within four hours.
- **P2:** acknowledged same day. Delivered within five business days.
- **P3:** acknowledged within two business days. Scheduled into the next planning cycle.
- **P4:** acknowledged within a week. May be declined or rolled into a larger project.


The point of the SLA is not to be ironclad. It is to make the team’s response time predictable so requesters can plan around it. A predictable two-day turnaround beats an unpredictable two-hour turnaround in almost every case.


Two practical tweaks:


- **Reserve capacity, do not commit it all.** Aim to keep 30 to 40 percent of the team’s week unscheduled at the start of the week, so P1 work has somewhere to land without blowing up the rest of the plan.
- **Track delivery against the SLA, not against ad hoc Slack expectations.** If the team consistently misses P3, the answer is more capacity, fewer P3 commitments, or a stronger self-serve path. It is not “try harder.”


## When to deflect to self-serve


The single biggest source of leverage in any intake system is the requests that never become tickets, because the requester can answer them on their own.


A clean deflection rule: if the question is a lookup against well-modeled data, point the requester at the BI tool and an existing dashboard or AI workflow. Do not write the SQL.


Specifically, deflect when:


- The metric exists on a certified dashboard. Link to the dashboard, not the number.
- The data is well-modeled and the team uses an AI BI tool with a governed connection that can answer the question safely. Tools like[Basedash](https://www.basedash.com/) make this easier because the AI generates SQL against a read-only role and exposes the query, so non-technical users can verify the answer themselves.
- The question is exploratory and the requester is comfortable with SQL. Give them a starter query and a link to the relevant tables.
- The metric is genuinely undefined. Do not produce a number. Get the metric defined and documented first, then build a single source of truth. See[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) for the tradeoffs of metric definition layers.


Do not deflect when:


- The question requires interpretation, not just retrieval.
- The data model is fragile and the requester is likely to misjoin tables.
- The answer will be quoted to an executive or customer.
- The requester has tried self-serve once and got the wrong number.


A rough heuristic: if a wrong answer would be cheap to discover and fix, deflection is the right call. If a wrong answer would be embarrassing or expensive, the analyst should run the query.


## Routing rules


For teams of more than two analysts, routing matters as much as triage.


A working pattern uses three roles, rotating weekly:


- **On-call analyst.** Owns the queue. First response on every new request, triages, runs anything S or M, and forwards anything L or XL into the planning queue.
- **Project analyst.** Works heads-down on the L and XL items planned in advance. Not interrupted by the queue except for genuine P1 work.
- **Reviewer.** Quick second pair of eyes on anything going to executives, customers, or the certified dashboards. Often the team lead.


Rotating the on-call role does two useful things. It distributes the context-switching cost so it does not fall on one person, and it forces every analyst to see what self-serve gaps look like in practice. Most “we should build that dashboard” insights come from a week on the queue.


For solo data teams, the same shape applies in miniature: protect mornings for project work, batch queue work into one or two windows in the afternoon, and write down anything that might wait for the next week. The biggest leverage is in not letting the queue interrupt deep work continuously.


## What not to do


A few patterns that look like good intake hygiene but make the queue worse.


- **Building a heavyweight ticket system before there is a queue.** Jira, ServiceNow, or a custom intake portal adds friction without changing the underlying triage problem. Start with a shared form, a shared spreadsheet, or a single Slack channel.
- **Promising a turnaround you cannot keep.** A team that says “we’ll get to it this week” and ships in three is worse than a team that says “this is P3” and ships in five days as scheduled.
- **Letting the loudest request win.** Without explicit prioritization, urgency becomes a function of seniority and proximity. Triage prevents this without confrontation.
- **Refusing requests without offering a path.** “We can’t take this” is not a useful answer. Either deflect to a self-serve resource, schedule it explicitly, or defer it with a clear reason and a date to revisit.
- **Building a new dashboard for every modification request.** Modifications to existing certified dashboards are usually the right call. Keep dashboard counts down. See[dashboard sprawl: how to audit, certify, and retire dashboards](https://www.basedash.com/blog/dashboard-sprawl-how-to-audit-certify-and-retire-dashboards) for the broader argument.


## A worked example


Suppose three requests come in on a Monday morning.


**Request A.** Head of sales: “Can you tell me revenue by region for Q4? Need it for the board deck Wednesday.”


This is a lookup tied to a real decision and a real deadline. Priority is P1, effort is S if a certified revenue dashboard exists with a region breakdown. The right action is to link the dashboard, not run a one-off query, and confirm the requester can self-serve next time. If the dashboard does not exist, the right action is to run the query today and add the breakdown to the certified dashboard this week.


**Request B.** Product manager: “Can we figure out why feature X has lower engagement than we expected?”


This is an investigation. Priority is P2, useful but not blocking anything before next sprint planning. Effort is L because it involves multi-table joins and judgment. The right action is to scope it: agree on the specific question (engagement compared to what baseline), the data needed, and the format of the answer. Plan it for the project analyst this week, not the on-call analyst.


**Request C.** Marketing analyst: “Can you build a paid channel performance dashboard?”


This is a new artifact. Priority is P3, important but no specific deadline. Effort is L. The right action is to schedule a 30-minute scoping conversation, agree on the metrics and tier (probably tier 2 team-owned), and put it on next week’s plan. Do not start it Monday.


Three requests, three different tracks, all triaged in under five minutes total. No back-and-forth, no context loss, no missed board deck.


## Common mistakes


A short list of patterns that quietly degrade an intake system over time.


- **Treating priority as the requester’s preference.** Priority is a function of impact and deadline, not the requester’s tone. Push back politely on P1 labels that do not survive the “what decision does this change today?” test.
- **Skipping the deflection conversation.** Every deflected request that succeeds builds the requester’s self-serve confidence. Every deflected request that fails surfaces a gap to fix. Both are wins.
- **Closing requests without acceptance.** A delivered query is not a closed ticket until the requester confirms it answered the question. Otherwise the same request reopens in a different channel a week later.
- **Letting the queue grow indefinitely.** A queue with two hundred items is the same as no queue. Set a cap (often 30 to 50 active items), and either close, schedule, or decline the rest.
- **Hiding the queue from the rest of the company.** A visible queue, even just a shared spreadsheet, makes priorities legible and removes the pressure to over-promise.


## How AI tools change the intake math


Modern AI BI tools shift the intake economics in a real way for type 1 and type 4 requests. A non-technical operator with access to a governed warehouse and a tool that translates English to SQL can resolve their own lookup or modification in minutes, with the analyst reviewing only the SQL the model produced.


Two practical implications for an intake system:


- **More requests should be deflectable.** If the team has invested in metric definitions, a semantic layer, or a governed AI connection, the share of P3-S and P4-S requests that can be self-served grows. The intake form should ask “have you tried the AI tool?” the way it once asked “have you checked the dashboard?”
- **The review burden does not go away, it moves.** A team that ships AI BI to non-technical users still owns the SQL the model emits, the metric definitions it pulls from, and the dashboards the answers are quoted on. See[how to review AI-generated SQL](https://www.basedash.com/blog/how-to-review-ai-generated-sql-a-checklist-for-analysts-and-operators) for the checklist that goes with this shift.


The triage framework above does not change. The mix of requests in each bucket does, and the team’s leverage goes up.


## FAQ


### Is a Slack channel enough for intake, or do I need a real form?


A dedicated channel with a pinned message containing the six intake questions works for most teams under twenty people. Once requests stop fitting in a single channel, move to a shared form (Notion, Airtable, Linear, a Slack form workflow). The form does not need to be fancy. It needs to ask the same six questions every time.


### How long should I keep a request open before declining it?


If a P3 or P4 request has not been picked up after a month and the requester has not followed up, decline it explicitly with a short note. Most of those requests were exploratory and are no longer relevant. The few that still matter will come back, and you can re-prioritize them with current context.


### Should every request go through triage, including ones from the CEO?


Yes, including ones from the CEO. Triage is not a gatekeeping mechanism, it is a planning mechanism. The CEO’s request will usually be P1 or P2 with a clear decision attached, and the framework just confirms what the team would have done anyway. The benefit is that requests without that profile, including from execs, get scheduled rather than interrupting other work.


### How do I prevent dashboards from accumulating from every “new artifact” request?


Default to modifying an existing certified dashboard rather than creating a new one. When a new dashboard is genuinely required, assign a tier and an owner at the time it is built, not retroactively. The cost of cleanup later is much higher than the cost of a five-minute conversation now.


### What does a healthy data team queue look like?


A rough sanity check: at any point, the on-call analyst has fewer than ten open S or M items in their personal queue. The team has fewer than fifty open requests across all priorities. SLA hit rate for P1 and P2 is above 90 percent. P3 cycle time is steady, not growing. P4 is small and aging out into either decline or schedule. If any of those drift, that is the signal that intake or capacity needs adjustment.


### How does this change for teams without a dedicated analyst?


For teams where engineering or operations runs the queue, the framework is identical, but the deflection bar is much higher. Anything that can be answered with self-serve, an AI BI tool, or a saved query template should be deflected by default. The intake form is even more important, because the person triaging is also doing other work and cannot afford the back-and-forth.


A working intake system is not heavy. It is a single front door, six form fields, a five-bucket taxonomy, a four-by-four matrix, and a default SLA. Most of the value comes from running it consistently, not from the policy itself. Once the team can predict their own response times, the queue stops being a source of stress and starts being a planning input. That is the moment a small data team starts producing more useful work without working more hours.
