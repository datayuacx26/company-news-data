---
schema_version: "1.0.0"
document_id: "1e5b1ac03981283c0e100da4d3b55962f994b9bd59f55c12876d9706c23b7297"
company_key: "yc-struct-ai"
company: "Struct"
source_id: "yc-struct-ai-rss-e2c56fab48fe"
canonical_url: "https://struct.ai/articles/corrective-action-vs-preventive-action/"
published_at: "2026-08-12T05:04:38+00:00"
first_seen_at: "2026-08-12T05:12:47.909279+00:00"
fetched_at: "2026-08-12T05:12:50.269053+00:00"
content_hash: "sha256:4e5f08417349d73770e4a1ef34338f0119446c5d24a755d500caf9a9b470e1b0"
---

# Corrective vs Preventive Action in Software Engineering

*Written by: Nimesh Chakravarthi, Co-founder & CTO, Struct*


## Key Takeaways for Engineering Leaders


- Correction stops immediate harm during an incident, corrective action removes the root cause after a confirmed failure, and preventive action removes risk before any failure occurs.
- Corrective action is reactive and begins after a production failure. Preventive action is proactive and starts when risk signals or trends appear.
- A complete CAPA flow for software teams uses seven steps and ends with automated incident resolution verification that confirms the failure mode is gone through observability data.
- Real-world incident pairs show how a single root cause can trigger corrective actions on the affected system and preventive actions that protect similar systems through CI gates and policy changes.
- Struct automates root-cause analysis and incident resolution verification for both corrective and preventive loops.[Automate your on-call runbook](https://cal.com/deepanm/struct-demo) to reclaim engineer-hours lost to manual triage.


## Timing Difference Between Corrective and Preventive Actions


Corrective action is reactive and starts after a production failure is confirmed. Preventive action is proactive and starts when a risk signal appears before any failure occurs.


[ISO 9001:2015 Clause 10.2](https://brightercompliance.co.uk/iso-9001corrective-and-preventive-action) requires organizations to react to nonconformities, evaluate their causes, implement appropriate actions, and review effectiveness. Preventive thinking is no longer a standalone clause.[ISO 9001:2015 eliminated Preventive Action as a standalone requirement](https://sgrii.com/2026/04/11/iso-9001-clause-10-improvement) and distributed it across Clauses 4, 6, and 10 as risk-based thinking. For engineering teams, your on-call runbook must handle both loops: a post-incident corrective loop triggered by alert data and a proactive preventive loop triggered by trend analysis, error-rate drift, or near-miss signals from observability dashboards.


## Seven-Step CAPA Flow with Resolution Verification


A complete CAPA flow for software teams closes only when observability data confirms the failure mode is gone, not when the ticket is marked done.[There is no single common verification window for CAPA effectiveness](https://connect981.com/faqs/how-long-should-we-monitor-a-process-to-verify-capa-effectiveness) . Teams choose periods based on risk and process frequency, often from weeks to 12 months, and confirm via objective evidence such as trends or repeat nonconformances.


The following seven-step flow applies to every production incident your team investigates:


1. **Detection:** An alert fires in Slack or PagerDuty, and the system records timestamp, impacted services, and blast radius.
2. **Containment (Correction):** Roll back, turn off a feature flag, or reroute traffic to stop user impact immediately.
3. **Root-Cause Analysis:** Use 5 Whys or fishbone analysis to trace the causal chain from symptom to systemic cause.
4. **Action Definition:** Classify each output as corrective action that stops recurrence of this failure or preventive action that stops first occurrence on similar systems.
5. **Implementation:** Ship code changes, update runbooks, adjust alert thresholds, or modify deploy gates with named owners and due dates.
6. **Incident Resolution Verification:** Automatically check observability data such as error rates, latency percentiles, and saturation metrics to confirm the failure mode is absent, not just that the ticket is closed.
7. **Closure and Continuous Improvement:** Archive the RCA, update the runbook, and feed risk signals into the preventive action backlog per[ISO 9001:2015 Clause 10.3](https://sgrii.com/2026/04/11/iso-9001-clause-10-improvement) .


Step 6, incident resolution verification, is the most commonly skipped step.[Verified closure treats “action complete” and “action effective” as two separate events](https://certaintysoftware.com/guides/capa-software) . Teams must gather independent evidence that the deviation has not recurred at the same control point.


The following three incident pairs show how this seven-step CAPA flow works in practice and how one root cause can drive both corrective and preventive actions.


## Three Paired Production-Incident Examples in Practice


Each pair below shows one corrective and one preventive action drawn from real-style software incidents, structured by trigger, root cause, action taken, and verification outcome.


**Pair 1 — Database Connection Pool Exhaustion**


A silent config-library default change reduced the database connection pool from 100 to 10, causing checkout p99 latency to exceed 30 seconds within three minutes of deploy. The corrective action reverted the config default and added a saturation alert on pool utilization. Verification confirmed p99 returned below 500 ms within one deploy cycle. The preventive action added a CI gate that fails any config change touching connection pool defaults without an explicit reviewer sign-off, preventing first occurrence on all other services sharing the library.


**Pair 2 — Kubernetes Pod OOM Storm**


[A Kubernetes pod OOM storm caused cascading restarts across a payment namespace.](https://sreschool.com/blog/capa) The corrective action patched the memory leak and adjusted pod memory limits. Verification showed zero OOM events over a 30-day monitoring window. The preventive action added a memory-based autoscaler rule and a CI memory regression test, blocking the same class of change from reaching production on any namespace.


**Pair 3 — Serverless Cold-Start Latency Breach**


[Lazy-loading failures in a serverless function caused p99 invocation latency to breach SLA thresholds.](https://sreschool.com/blog/capa) The corrective action performed lazy-loading of libraries and reduced initialization work. Synthetic canaries validated improvement over 14 days. The preventive action enforced image size budgets and CI checks across all Lambda functions, stopping the same latency pattern from appearing in functions not yet affected.


## How ISO 9001:2015 Handles Prevention Today


[ISO 9001:2015 replaced the formal preventive action clause from the 2008 version with risk-based thinking](https://iso9001expert.com/blog/risk-based-thinking-iso-9001-practical-implementation-guide) because the committee recognized that good preventive thinking should sit inside every process, not in a separate procedure. Prevention now appears across Clauses 4.1 (context), 6.1 (planning actions to address risks), and 10.2 (corrective action feeding back into risk registers.


For engineering teams, this shift has a direct operational implication.[Under the 2008 version, the preventive action clause often resulted in empty folders at audit](https://tryharmony.ai/risk-based-thinking-iso-9001) because it required documenting actions against problems that had not yet occurred. The 2015 model instead requires your team to show that risk signals such as error-rate trends, near-miss alerts, and dependency health scores actively feed your planning and deploy processes, not a backlog ticket labeled “someday.”


## Using 5 Whys and Fishbone for Root Cause


The 5 Whys traces a single causal chain from symptom to systemic cause. The fishbone diagram maps contributing factors across multiple categories at the same time. Most software teams combine a timeline with the 5 Whys and then use a fishbone when the incident is multi-factor.


Using the connection pool incident from Pair 1, a 5 Whys analysis runs as follows:


1. Why did checkout latency spike? The database rejected new connections.
2. Why did the database reject connections? The pool was exhausted at 10 connections.
3. Why was the pool limited to 10? A config library update changed the default silently.
4. Why did the update ship without review? No CI gate checked connection pool config diffs.
5. Why did no gate exist? The team had never instrumented pool saturation as a deploy signal.


A fishbone analysis of the same incident maps contributing factors across four categories:


- **Process:** No config-diff review step in the deploy checklist.
- **Tooling:** No saturation alert on connection pool utilization.
- **People:** Library update authored by a team without database context.
- **Environment:** Staging pool size masked the default change under lower load.


The corrective action addresses the process and tooling categories. The preventive action, a CI gate blocking pool-config changes, addresses the systemic gap that would affect every service using the library.


## How Struct Automates Investigation and Verification


The root-cause analysis step in the CAPA flow is where most engineering teams lose 30–45 minutes per incident. Struct removes that manual investigation window.


When an alert fires in your Slack or PagerDuty channel, Struct immediately correlates logs from Datadog, AWS CloudWatch, Sentry, and GitHub to produce a root-cause output in under 10 minutes, before your on-call engineer opens their laptop. This 80%+ reduction in triage time, from 30 minutes to 2 minutes per incident, reclaims 56 engineer-hours per month across 2,100+ automated investigations. The root-cause step feeding both your corrective and preventive action workflows no longer acts as a bottleneck.


Struct’s Incident Tracker, launched August 3, 2026, extends this automation to the verification step by running an approximately one-minute loop against your observability data to confirm an incident is actually resolved, not just acknowledged. The system checks error rates, latency percentiles, and saturation metrics continuously until the failure mode is absent, then closes the loop automatically. For the preventive side of the CAPA loop, Struct’s Deploy Guard operates at the PR level, running instrumentation review and post-deploy health checks before a bad change reaches production.


Struct sits on top of your existing observability stack such as Datadog, Grafana, and Sentry as an investigation layer. It does not replace those tools. It automates the manual triage work that happens between alert and resolution.


[See how Struct eliminates manual triage](https://cal.com/deepanm/struct-demo) and stop burning engineer-hours on root-cause steps that a purpose-built system can complete in minutes.


For more on how engineering teams structure incident workflows, see the Incident Tracking & Resolution Verification hub and the[Incident Response & Management hub](https://struct.ai/incident-response) .


## Frequently Asked Questions


### What is the simplest way to distinguish corrective action from preventive action?


The trigger provides the simplest distinction. If a failure has already occurred and you are eliminating its root cause to stop recurrence, that is corrective action. If a risk signal, trend, or near-miss has appeared and you are acting before any failure occurs, that is preventive action. The timing of the trigger, after failure versus before failure, separates the two.


### Who owns corrective and preventive actions on an engineering team?


Corrective actions should be owned by the engineer or team closest to the system that failed, with a named individual, a due date, and a SMART definition of done. Preventive actions often require cross-functional ownership because they address systemic gaps such as deploy pipelines, alert thresholds, or CI gates that span multiple teams. Engineering leadership is responsible for ensuring both action types have owners and are tracked to verified closure, not just ticket completion.


### How do you verify that a corrective action actually worked?


Verification requires objective observability evidence, not a date-based review. Check that the specific failure mode, such as the error rate, latency breach, or saturation event, has not recurred over a defined window, typically 7–30 days of production traffic. A corrective action is not closed until that evidence exists. Struct’s Incident Tracker automates this verification loop by continuously checking your observability data against the incident’s failure signature and confirming resolution without manual follow-up.


### Can the same root cause produce both a corrective and a preventive action?


Yes, and it should. The corrective action addresses the system that already failed. The preventive action extends the same fix, or a related one, to similar systems, services, or configurations that share the root cause but have not yet failed. A connection pool fix on one service becomes a CI gate that protects all services. A memory limit adjustment on one Kubernetes namespace becomes an autoscaler rule applied cluster-wide.


### How does Struct fit into an existing CAPA workflow?


Struct automates Step 3 of the CAPA flow, root-cause analysis, by correlating logs, traces, metrics, and code context the moment an alert fires. It outputs a root-cause summary and suggested fixes within minutes, giving your team a verified starting point for defining corrective and preventive actions. Struct also handles incident resolution verification automatically by confirming through live observability data that the failure mode is gone before the incident is closed. Setup takes under 10 minutes and connects to Datadog, Sentry, AWS CloudWatch, GCP Logs, GitHub, and your existing Slack alerting channels.


### What metrics indicate that corrective and preventive actions are working?


Track repeat incident rate, the number of incidents sharing the same root cause within a 90-day window divided by total incidents. A rate below 5% indicates strong follow-through on corrective actions. Also track MTTR trend over 30, 60, and 90 days. If corrective actions are effective, average investigation time should decrease. Struct’s 80%+ triage-time reduction, from 30-minute to 2-minute investigations, provides a concrete benchmark for what automated root-cause analysis can deliver against these metrics.


### What is incident resolution verification and why does it matter?


Incident resolution verification is the automated process of confirming that an incident is genuinely resolved by checking live observability data, not by marking a ticket done. Without it, teams close incidents based on the absence of new alerts rather than evidence that the failure mode is gone. This pattern causes recurring incidents and inflated MTTR. Struct’s Incident Tracker runs a continuous verification loop against your observability stack and closes incidents only when the data confirms the failure signature has cleared. This closed-loop step separates a complete CAPA workflow from one that produces documentation without preventing recurrence.


Every manual root-cause investigation your team runs is time not spent shipping product. Struct removes the 30–45 minute manual investigation window, automates incident resolution verification, and feeds both your corrective and preventive action workflows with evidence-backed root-cause outputs.[Book a demo to reclaim your team’s triage hours](https://cal.com/deepanm/struct-demo) and stop losing engineer-hours to manual investigation today.
