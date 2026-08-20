---
schema_version: "1.0.0"
document_id: "7706e87f0f4e33ae3b39ed2ed23d8c3307a08a76f1d3ec722c9a015eb71770c9"
company_key: "yc-struct-ai"
company: "Struct"
source_id: "yc-struct-ai-rss-e2c56fab48fe"
canonical_url: "https://struct.ai/articles/agentic-on-call-knowledge-sharing/"
published_at: "2026-08-13T05:03:22+00:00"
first_seen_at: "2026-08-13T05:37:38.849890+00:00"
fetched_at: "2026-08-13T05:37:39.516911+00:00"
content_hash: "sha256:96ece50455c344c1085dd06d085720ae98b260bbea3a43ab86ccedc36ca5ee87"
---

# Agentic On-Call Knowledge Sharing with AI Agents

*Written by: Nimesh Chakravarthi, Co-founder & CTO, Struct*


## Key takeaways for agentic on-call with Struct


-


Agentic on-call systems use autonomous AI agents that investigate alerts, capture resolution steps, and verify outcomes against observability data before storing results in version-controlled runbooks.


-


Reasoning lets agents break alerts into investigation steps, execute across tools, evaluate results, and update plans while keeping human-approval gates for production changes.


-


Structured, version-controlled runbooks stored in service repositories ensure every investigation writes to a consistent schema that future engineers can retrieve in under two minutes.


-


Incident resolution verification confirms that error rates, latency, and resource utilization have returned to normal, which prevents repeated firefighting and cuts investigation time from 30 minutes to 2 minutes.


-


[Struct connects to your observability stack](https://cal.com/deepanm/struct-demo) in under 10 minutes, runs verified investigations, and stores every resolution in a queryable knowledge base so your on-call runbook stays current.


## How reasoning powers agentic AI for on-call


Reasoning is the core capability that separates agentic on-call systems from simple alert routers. An agentic system interprets an alert, breaks it into investigation steps, executes those steps across connected tools, evaluates whether the results confirm a root cause, and updates its plan if they do not, all before a human opens a laptop.


Research shows that repeated incident-management cycles can produce a reusable knowledge base from agent failures. The[plan-act-reflect loop](https://dataiku.com/blog/agentic-workflows) formalizes this pattern. The agent interprets intent and breaks the task into steps (plan). It then executes through controlled tool calls (act). Finally, it evaluates whether results meet the goal before updating the plan (reflect).


The[AWS agentic lens](https://aws.amazon.com/what-is/agentic-ai/) treats memory persistence as a first-class requirement. Short-term memory holds task context during a single investigation. Long-term memory carries verified resolution patterns across future incidents so the system improves over time.


While memory lets agents learn from past incidents, human-approval gates define the boundary between autonomous action and irreversible change.[GS Consulting’s agentic workflow framework](https://gsconsultingllc.com/insights/agentic-workflows) scores incident containment at the maximum control load. Disabling a service or isolating a system therefore requires a strict approval gate unless a narrowly defined emergency playbook has already authorized it.


Struct implements this as a human-on-the-loop model. The agent triages, correlates, and suggests. The engineer approves before any remediation action executes.


## Runbook schema that keeps on-call knowledge usable


Every investigation an agentic system runs should write to a structured, version-controlled record that any future on-call engineer can retrieve in under two minutes. A consistent schema prevents captured knowledge from decaying into unstructured postmortem prose that neither agents nor humans can query reliably.


[Runbooks stored in the same repository as the service they describe](https://em-tools.io/frameworks/runbook-framework) , in a dedicated` docs/runbooks` directory, are version-controlled, reviewed alongside code changes, and discoverable by the owning team. Manual runbook maintenance often fails because engineers rarely update documentation after resolving an incident. Struct extends this pattern by automatically populating the record fields below after every investigation, then flagging entries whose resolution verification status has not been revalidated within the review window. This keeps the knowledge base current without relying on manual edits.


The table shows how each field maps to a specific observability source and defines the exact trigger that should prompt a human review. This structure keeps runbooks aligned with your architecture as it evolves.


Field


Description


Source


Review Trigger


Alert ID


Unique identifier from PagerDuty or Slack alert


PagerDuty / Slack


On every recurrence


Service


Affected service or component boundary


Service catalog


On architecture change


Trigger Condition


Exact threshold or anomaly that fired the alert


Datadog / Prometheus


On threshold update


Investigation Trace


Correlated log, metric, and trace timeline generated by Struct


Struct dashboard


After every incident


Resolution Verification Status


Pass / Fail / Pending, confirmed against observability data


Struct Incident Tracker


~1-minute automated loop


Last Validated


Timestamp of most recent successful verification


Struct


Quarterly minimum


Owner


Named service team responsible for runbook accuracy


On-call rotation


On team change


[Each runbook entry should answer three questions in its first ten lines](https://sync-o.io/blog/runbook-documentation-best-practices) : what the procedure accomplishes, the specific alert or condition that triggers it, and the blast radius if executed incorrectly. Struct’s dynamically generated dashboards surface all three automatically, so the knowledge record is populated before a human writes a single line of documentation.


## How incident resolution verification stops repeat incidents


Incident resolution verification confirms that an incident is actually resolved, not just acknowledged, by checking that error rates, latency, and resource utilization have returned to normal in observability data. Without this closed loop, engineers mark incidents resolved based on gut feel, and the same failure mode resurfaces weeks later.


Struct’s Incident Tracker runs a **~1-minute automated verification loop** against Datadog, Grafana, Sentry, and connected observability sources. The verification loop checks that the observable success signals defined in the runbook, such as error rate below threshold for N minutes, latency within the SLO band, and no downstream regressions, are all satisfied before the incident status moves to resolved.[Rootly’s AI SRE guide](https://rootly.com/ai-sre-guide/metrics-and-roi) defines this verification pass rate as a Layer A workflow quality metric that must be defined before execution using observable signals, not after the fact.


The production impact of closing this loop is measurable and concrete.[Arcana reduced average investigation time from 30 minutes to 2 minutes, reclaimed 56 engineer-hours per month, and runs 2,100+ automated investigations monthly](https://struct.ai/case-study/arcana) ,[scaling investigation coverage 17x while maintaining an over-80% helpful investigation rate](https://struct.ai/case-study/arcana) . The[2019 Catchpoint SRE Report](https://cdn2.hubspot.net/hubfs/5595333/Research%20Papers/2019%20SRE%20Report.pdf) discusses SRE stress from incidents, while operational toil consumes a substantial portion of developer time. Incident resolution verification directly addresses these challenges by ensuring each incident is resolved once, correctly, with a verified record that prevents recurrence.


## On-call agent capabilities and approval gates


The table below maps each function Struct performs during an on-call investigation to its corresponding human-approval gate. Use this table to see which agent actions run without intervention, as read-only operations, and which require your explicit sign-off as state-modifying operations. The approval gate column defines your control boundary.


Agent Function


What Struct Does


Human-Approval Gate


Output Stored in Knowledge Base


Triage


Correlates alert with blast radius, dedupes related alerts, classifies severity


None, fully autonomous


Alert ID, service, trigger condition


Root-cause hypothesis


Generates ranked hypotheses from log, metric, and trace correlation


None, engineer reviews output


Investigation trace with evidence links


Log correlation


Queries Datadog, CloudWatch, GCP Logs, Sentry across the incident window


None, read-only data access


Correlated timeline appended to trace


Suggested fix


Proposes remediation steps or generates a PR via code agent handoff


Required before any code merge or deploy


Suggested fix linked to runbook entry


Resolution verification


Runs ~1-minute loop against observability data to confirm incident is closed


None, automated, escalates if signals fail


Resolution verification status plus last validated timestamp


[See these approval gates in action — Book a 20-minute demo and watch Struct run a live investigation on your stack, showing exactly where human oversight is required and where the agent runs autonomously.](https://cal.com/deepanm/struct-demo)


## Step-by-step setup for incident resolution verification


Struct connects to your existing observability stack in under 10 minutes and begins running verified investigations immediately. You keep Datadog, Grafana, and Sentry in place, and Struct sits on top as an investigation and verification layer.


The setup sequence builds a complete investigation path from alert to verified resolution.


1.


Authenticate your alert source to give Struct a starting signal. Connect Slack, PagerDuty, or Linear so Struct listens to your designated on-call channels.


2.


Connect your observability context so each alert gains rich telemetry. Authenticate Datadog, AWS CloudWatch, GCP Logs, Sentry, Grafana, or Prometheus so Struct can query logs, metrics, and traces.


3.


Link your code repository to correlate changes with incidents. Connect GitHub so Struct can cross-reference recent PRs and deploys against the incident timeline.


4.


Import your runbooks so the agent follows your existing practices. Paste your current on-call runbooks directly into Struct, and the agent follows those operational procedures when an alert fires.


5.


Enable auto-investigations so the full triage-to-verification loop runs continuously. Struct begins intercepting alerts and executing investigations without additional configuration.


Struct is[SOC 2 Type II and HIPAA compliant](https://trust.struct.ai/) (documented at trust.struct.ai), which makes it suitable for fintech and health-tech teams operating under strict SLA and data-handling requirements. Logs are accessed and processed ephemerally. The composable widget architecture lets your team encode service-specific investigation logic, including custom correlation ID formats, specific dashboard panels, and escalation paths, so every investigation reflects how your senior engineers actually debug your system.


## Frequently asked questions


### How does Struct keep tribal knowledge version-controlled?


Every investigation Struct runs writes a structured record, including alert ID, service, trigger condition, investigation trace, resolution verification status, last validated timestamp, and owner, to a persistent knowledge base. That record is updated after every recurrence and tied to the originating alert, so the knowledge base grows with your system rather than decaying alongside it.


Engineers can import existing runbooks directly into Struct, and the agent follows those procedures automatically when a matching alert fires. Because the record schema is consistent across every incident, new engineers can query past investigations by service or failure mode without needing to ask a senior engineer for context.


### What happens when an investigation fails verification?


When Struct’s automated verification loop detects that observability signals have not returned to normal, such as error rate still above threshold, latency outside the SLO band, or downstream regressions present, the incident status remains open and Struct escalates with a specific signal report. The failed verification attempt is logged in the knowledge base entry so the next engineer inherits the full context of what was tried, what the signals showed, and why the incident was not closed.


This prevents the common failure mode where an incident is marked resolved based on a subjective judgment call and then resurfaces hours later.


### Can new engineers safely take on-call with Struct?


New engineers can safely take on-call with Struct because the platform performs the first-pass investigation automatically. By the time a new engineer opens their laptop, the blast radius, root-cause hypothesis, correlated timeline, and suggested fix are already in the Slack thread.


New engineers do not need deep systemic context to begin triaging. They review Struct’s output, ask follow-up questions via the Slack-native conversational interface, and approve or escalate based on verified evidence rather than tribal knowledge. A Series A fintech with over 40 engineers used this capability to put new hires on call safely after integrating Struct, cutting triage time by 80% and protecting strict SLA windows that previously required senior engineer involvement on every alert.


### How does incident resolution verification differ from simple alert deduping?


Alert deduping suppresses duplicate notifications for the same underlying event. It reduces noise but does not confirm that the underlying condition has actually cleared. Incident resolution verification is a separate, downstream step.


After a fix is applied, Struct queries your observability data on a ~1-minute loop and checks that the specific signals that triggered the alert, such as error rate, latency, and saturation, have returned to normal with no downstream regressions. A deduplicated alert can still represent an unresolved incident. A verified incident has observable proof that the system is healthy.


Struct performs both functions. It auto-dedupes related alerts with no configuration and runs one investigation per real incident, then closes the loop with resolution verification before marking the incident resolved.


## Conclusion: Give your engineers their nights and focus back


Manual on-call investigation is a compounding tax on engineering velocity. The[2019 Catchpoint SRE Report](https://cdn2.hubspot.net/hubfs/5595333/Research%20Papers/2019%20SRE%20Report.pdf) discusses SRE stress from incidents, and operational toil continues to consume a substantial portion of developer time. Every incident that is resolved without a verified, version-controlled record is an incident that will be investigated again from scratch by the next engineer who draws the short straw at 3 AM.


Agentic on-call knowledge sharing with Struct breaks that cycle. The plan-act-reflect loop investigates alerts before you wake up. The structured knowledge base captures every resolution in a queryable record. Incident resolution verification then confirms against real observability data that the system is actually healthy, not just acknowledged. The Arcana results mentioned earlier, with 56 engineer-hours reclaimed monthly and investigation time down to 2 minutes, show what verified, reusable team memory looks like in production.


Struct is the only platform that owns incident resolution verification as a closed-loop category. It automatically confirms resolution against Datadog, Grafana, Sentry, and your full observability stack, then stores the verified record so your next on-call engineer inherits the answer instead of repeating the search.


[Stop burning your best engineers on 3 AM log-hunting expeditions — Reduce the time your team spends triaging issues by 80% and give them their product velocity back. Set up Struct in under 10 minutes and let AI handle your next on-call investigation.](https://cal.com/deepanm/struct-demo)
