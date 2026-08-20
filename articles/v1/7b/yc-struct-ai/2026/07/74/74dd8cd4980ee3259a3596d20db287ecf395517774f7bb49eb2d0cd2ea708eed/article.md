---
schema_version: "1.0.0"
document_id: "74dd8cd4980ee3259a3596d20db287ecf395517774f7bb49eb2d0cd2ea708eed"
company_key: "yc-struct-ai"
company: "Struct"
source_id: "yc-struct-ai-rss-e2c56fab48fe"
canonical_url: "https://struct.ai/articles/key-sre-principles-on-call/"
published_at: "2026-07-12T05:11:02+00:00"
first_seen_at: "2026-07-27T05:30:38.706787+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:0291bac87be6a2dc4d618ba5b93b35f206c1e577a9d39c8b53b2be40742d6311"
---

# Key Principles of Site Reliability Engineering for On-Call

*Written by: Nimesh Chakravarthi, Co-founder & CTO, Struct*


## Key Takeaways


-


SRE on-call acts as a reliability feedback loop that improves software quality when it runs smoothly, but it quickly burns out engineers when it depends on manual log hunting across scattered tools.


-


Seven core principles create a measurable framework for sustainable on-call: actionable alerting, defined error budgets, toil reduction, bounded pager load, blameless postmortems, runbook-driven response, and continuous feedback loops.


-


Healthy targets include limiting pages to two per shift, keeping MTTD under five minutes, and holding toil below 50% of on-call time so teams avoid overload and protect engineer health.


-


Runbooks and automated investigation tools transfer tribal knowledge, cut escalation rates, and let junior engineers handle more incidents on their own.


-


[Automating your on-call runbook](https://cal.com/deepanm/struct-demo) with Struct removes the first 30–45 minutes of manual triage so engineers can focus on fixing the problem instead of repeating the same investigation steps.


## How SRE On Call Keeps Systems Reliable


[Google’s SRE book](https://sre.google/sre-book/table-of-contents/) defines site reliability engineering as applying software engineering practices to operations problems. On-call sits at the center of that work. Engineers carry a pager, respond to alerts, and own production reliability during their shift. The goal extends beyond fixing incidents. Teams use incident data and postmortems to make future incidents less likely and less severe.


The seven principles that make on-call sustainable:


1.


Actionable alerting only


2.


Defined error budgets


3.


Toil reduction as a first-class goal


4.


Bounded pager load


5.


Blameless postmortems


6.


Runbook-driven response


7.


Continuous feedback loops


## Principle 1: Actionable Alerting Only


Every alert that fires must require a human decision. Informational, self-resolving, or consistently ignored alerts create noise, and noise destroys the signal-to-noise ratio that keeps on-call manageable.


Start by auditing every alert rule quarterly and delete or demote any that fired without requiring action in the last 30 days. For alerts that remain, write descriptions that explain the user impact instead of only listing a metric threshold. Route low-severity alerts into a ticket queue instead of a pager to preserve focus for true emergencies. Finally, require a linked runbook for every paging alert before it reaches production so responders know exactly what to do next.


**Target metric:**[Google’s SRE Workbook](https://liuning0820.github.io/2019/03/30/the-SRE-workbook-reading-notes.html) recommends targeting a maximum of two incidents per on-call shift.


## Principle 2: Defined Error Budgets


An[error budget](https://sre.google/sre-book/embracing-risk/) is the acceptable amount of unreliability derived from a service-level objective, or SLO. It turns an abstract reliability target into a concrete operational constraint that guides when to ship features and when to invest in stability.


Define SLOs for every user-facing service before placing it on call. Calculate the error budget as 100% minus the SLO target, expressed in minutes of downtime per month. Freeze non-critical deployments automatically when the error budget drops below 10%. Review budget burn rate weekly in engineering stand-ups, not only after incidents.


**Target metric:** Error budget burn rate above 2× the expected rate for more than one hour reliably signals an impending SLO breach.


## Principle 3: Toil Reduction as a First-Class Goal


[Toil](https://sre.google/sre-book/eliminating-toil/) is manual, repetitive, automatable work that scales with service growth and produces no lasting value. On-call toil such as acknowledging duplicate alerts, copy-pasting log queries, and manually correlating trace IDs is a primary driver of engineer burnout.


Track toil hours per rotation explicitly and keep less than 50% of on-call time spent on toil. Automate any investigation step performed identically more than three times. Deduplicate alert streams before they reach the pager. Encode repeated diagnostic queries into runbooks or automated tooling so humans do not repeat the same steps by hand.


**Target metric:**[Google SREs aim for less than half of their time to be spent on toil.](https://cloud.google.com/blog/products/management-tools/identifying-and-tracking-toil-using-sre-principles)


[Eliminate repetitive investigation toil](https://cal.com/deepanm/struct-demo) — Struct encodes your existing runbooks and executes the first-pass investigation automatically the moment an alert fires.


## Principle 4: Bounded Pager Load


Bounded pager load protects engineers from unsustainable expectations. Without explicit limits, on-call engineers absorb every reliability debt the team has accumulated, regardless of whether that workload is safe.


Set a hard cap of no more than two pages per shift as the team’s operational target. Track pages per shift as a team health metric in every sprint retrospective. Escalate to engineering leadership when the rolling four-week average exceeds the cap. Rotate on-call schedules to distribute load evenly across the team, including senior engineers.


**Target metric:** Mean time to detect under five minutes combined with fewer than two pages per shift indicates a well-tuned alerting system.


## Principle 5: Blameless Postmortems


A[blameless postmortem](https://sre.google/sre-book/postmortem-culture/) treats every incident as a systems failure, not a human failure. The output is a set of action items that make the system more resilient, not a record of who made a mistake.


Complete a postmortem for every incident that breaches an SLO within 48 hours of resolution. Structure postmortems around a five-why root cause analysis instead of a timeline of blame. Assign every action item an owner and a due date before the postmortem document is closed. Publish postmortems internally so the entire engineering organization learns from each incident.


**Target metric:** Postmortem action item completion rate above 80% within 30 days shows that the process is driving real reliability improvements.


## Principle 6: Runbook-Driven Response


A runbook is a documented, step-by-step procedure for diagnosing and resolving a known class of incident. Runbooks transfer tribal knowledge from senior engineers to the entire team. This makes it safe to put junior engineers on call without creating unnecessary escalation risk.


Require a runbook for every paging alert before it reaches production. Version-control runbooks alongside the code they describe. Review and update runbooks after every incident where the documented steps were insufficient. Test runbooks in staging environments before relying on them in production incidents.


**Target metric:** Escalation rate below 15% per shift indicates runbooks are comprehensive enough for on-call engineers to resolve most incidents independently.


## Principle 7: Continuous Feedback Loops


On-call data such as pages, MTTD, MTTR, and error budget consumption directly measures system reliability. Teams that feed this data back into sprint planning and architecture decisions improve reliability systematically instead of reacting only after major failures.


Review on-call metrics in every sprint retrospective alongside feature velocity metrics. Treat recurring pages for the same root cause as a P1 engineering priority. Use MTTR trends to identify which services need architectural investment. Share on-call health dashboards with product and leadership so reliability stays visible.


**Target metric:** Mean time to resolution trending downward quarter-over-quarter confirms the feedback loop is working.


## Making On-Call Sustainable in Practice


On-call sustainability depends on three layers working together. Alert quality must be high enough that every page is worth waking up for. Investigation tooling must be fast enough that resolution does not require 45 minutes of manual log hunting. Organizational norms must treat pager load as a team metric rather than an individual burden.


The seven principles above address each of these layers. The most common failure mode comes from treating them as aspirational instead of operational. Sustainability requires measuring each principle with a concrete metric and escalating when targets are missed.


## On-Call Health Benchmarks


The following benchmarks translate these principles into concrete targets. They highlight the difference between healthy on-call operations and teams that are approaching burnout.


Metric


Healthy Target


Warning Threshold


Pages per on-call shift


[≤ 2 actionable pages](https://sre.google/workbook/alerting-on-slos/)


> 5 pages per shift


Mean Time to Detect (MTTD)


< 5 minutes


> 15 minutes


Mean Time to Resolve (MTTR)


< 30 minutes


> 60 minutes


Error budget consumption


< 50% consumed mid-month


> 90% consumed before month-end


Toil as % of on-call hours


[< 50%](https://sre.google/sre-book/eliminating-toil/)


> 70%


[See how Struct hits these MTTR targets](https://cal.com/deepanm/struct-demo) — Move your team’s resolution time from 45 minutes to under 5 in a 10-minute live demo.


## Where Automated Investigation Fits


Most incidents begin with 30 to 45 minutes spent on tasks that do not require human judgment. Engineers acknowledge the alert, pull logs from CloudWatch or Datadog, find the relevant exception in Sentry, cross-reference the recent deploy in GitHub, and assemble a timeline. This work is pure toil and keeps MTTR high even on teams that follow the seven principles above.


Struct removes this phase entirely. When an alert fires in a designated Slack channel or PagerDuty queue, Struct automatically queries every connected observability source such as Datadog, AWS CloudWatch, GCP Logs, Azure Traces, Sentry, Grafana, and GitHub. It correlates the signals into a unified timeline, identifies the likely root cause, and delivers a dynamically generated dashboard to the alert thread before the on-call engineer opens their laptop. The investigation finishes in under five minutes. The engineer reviews the evidence, confirms the root cause, and moves directly to resolution.


Teams with custom runbooks can encode those procedures directly so every automated investigation follows the same diagnostic steps a senior engineer would take. Junior engineers on call receive a fully contextualized starting point for every alert, which reduces escalation rates and makes it safe to distribute on-call load across the full team. The result is an 80% reduction in triage time and MTTR that trends toward the healthy benchmark in the table above from the first shift.


## Conclusion


Automated investigation closes the execution gap that prevents teams from reaching these benchmarks manually. These principles form a complete framework for turning on-call from a burnout rotation into a reliability engine when teams apply them together. Each principle is measurable, and the benchmarks table above provides concrete targets. Struct removes the triage phase described earlier so engineers can apply this framework at scale instead of only in theory.


[Start automating your incident response](https://cal.com/deepanm/struct-demo) — Set up Struct in under 10 minutes and let AI handle your next on-call investigation from alert to root cause before you finish your coffee.


## Frequently Asked Questions


### What is the difference between MTTD and MTTR in SRE on-call?


Mean Time to Detect measures how long it takes from when a problem begins to when an alert fires and reaches the on-call engineer. Mean Time to Resolve measures how long it takes from detection to full resolution. MTTD primarily depends on alerting quality, including thresholds, coverage, and routing. MTTR primarily depends on investigation speed and runbook quality. Teams track both metrics separately because they have different root causes and different remediation strategies. A team can have excellent MTTD but poor MTTR when alerting is well tuned but the investigation process remains manual and slow.


### How many pages per shift is considered healthy for an SRE on-call rotation?


Google’s SRE practice focuses on keeping on-call pager load at a sustainable level so engineers have time to investigate, resolve, and document each incident. When page volume stays high over time, teams should audit their alert rules, deduplicate noisy sources, and automate the investigation of recurring alert classes before adding headcount to absorb the load.


### How does an error budget connect to on-call decision-making?


An error budget is the quantified tolerance for unreliability derived from a service-level objective. In on-call practice, it functions as a real-time decision framework. When the error budget is healthy, the team has operational runway to ship features and accept deployment risk. When the budget is burning faster than expected or is nearly exhausted, the team should freeze non-critical changes, prioritize reliability work, and increase scrutiny on deployments. Tracking error budget burn rate during on-call shifts gives engineers an objective basis for escalating reliability concerns to product and leadership rather than absorbing the risk silently.


### Can junior engineers safely take on-call shifts without deep system knowledge?


Junior engineers can take on-call shifts safely when the right tooling and runbooks are in place. The primary barrier for junior engineers on call is the lack of tribal knowledge required to navigate complex, multi-service architectures during an incident. Comprehensive runbooks that document diagnostic steps for every paging alert class significantly lower this barrier. Automated investigation tools that perform the first-pass triage by correlating logs, identifying the blast radius, and surfacing a root cause hypothesis lower it further by giving junior engineers a fully contextualized starting point instead of a blank screen. Teams that combine detailed runbooks with automated investigation tooling report materially lower escalation rates and faster onboarding of new on-call engineers.


### What is toil in SRE and why does it matter for on-call sustainability?


Toil is manual, repetitive, automatable work that scales linearly with service growth and produces no lasting improvement to system reliability. In on-call rotations, toil includes tasks such as acknowledging duplicate alerts, manually pulling the same log queries for recurring incidents, copy-pasting correlation IDs across observability tools, and re-running the same diagnostic steps documented in a runbook. Toil matters because it consumes engineering capacity without generating reliability improvements. Google’s SRE practice sets a target of less than 50% of on-call hours spent on toil, and teams above that threshold are effectively paying senior engineers to perform work that automation can handle, which reduces product velocity and accelerates burnout.
