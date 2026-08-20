---
schema_version: "1.0.0"
document_id: "14a247d192ed843c33e7d394b1fe512e819aac333cf1f4ecb07e60718da58c3a"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-9cd8203e3449"
canonical_url: "https://www.elastic.co/security-labs/elastic-workflows-ga-9-4"
published_at: "2026-05-05T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:09.053610+00:00"
fetched_at: "2026-07-28T22:12:48.521638+00:00"
content_hash: "sha256:d7e27bf974e744d93abe310b847d4c99dec53f0a732560f9a9e6e2b90e43b477"
---

# Elastic Workflows GA: automation where your security data already lives

5 May 2026 •


[Tinsae Erkailo](https://www.elastic.co/security-labs/author/tinsae-erkailo)


# Elastic Workflows GA: automation where your security data already lives


Elastic Workflows is generally available in 9.4, bringing production-ready security automation with deeper case management integration, human-in-the-loop support, natural language authoring, and more.


8 min read


[Product Updates](https://www.elastic.co/security-labs/category/product-updates)


Elastic Workflows is generally available in 9.4. It is the automation layer built directly into Elastic, running where your data lives across Security, Observability, and Search. While this post focuses on a security deep dive, the same workflow capabilities apply across solutions, with no separate platform to deploy and no data to move. When an alert fires or a schedule triggers, a Workflow executes: querying Elasticsearch, enriching with threat intel, creating cases, calling external APIs, and notifying your team. Define it in YAML or describe it in natural language and let AI generate the workflow.


The[9.3 Tech Preview](https://www.elastic.co/security-labs/security-automation-with-elastic-workflows) introduced the foundation for native automation in Elastic. 9.4 brings it to general availability with production stability and significantly expanded capabilities. Case management gets 25 dedicated automation steps covering the full lifecycle. Human-in-the-loop becomes a first-class Workflow primitive. Natural language authoring moves to Tech Preview. The platform gains more flow-control primitives (` while` ,` switch` , iteration control), data-transformation steps for working with collections, deeper AI integration with[Agent Builder](https://www.elastic.co/elasticsearch/agent-builder) , and broader event-driven triggers. Production-ready automation across Elastic.


##


Case automation at scale


The biggest addition for security teams is case management. In the Tech Preview, working with cases involved four generic steps: creating, retrieving, updating, and commenting. Anything beyond that required raw API calls.


Now there are 25 dedicated` cases.*` steps covering the full lifecycle: create, find, find similar, update, close, assign, unassign, add alerts, add observables, add comments, add tags, set severity, set status, and more. Each step is typed, validated, and appears in the YAML editor's autocomplete, which means natural-language authoring can generate them accurately, too.


Here's what a realistic triage workflow looks like. An alert fires. The Workflow checks whether a case already exists for this alert. If not, it creates one, attaches the alert and observables, assigns the on-call analyst, and routes severity based on risk score:


The code below shows an example of a workflow described using YAML:


```text
name: Triage Workflow
enabled: true
triggers:
- type: alert


steps:
- name: check_existing
type: cases.getCasesByAlertId
with:
alert_id: "{{ event.alerts[0]._id }}"


- name: route
type: if
condition: "steps.check_existing.output : ''"
steps:
- name: update_existing
type: cases.addComment
with:
case_id: "{{ steps.check_existing.output[0].id }}"
comment: |
Correlated alert: {{ event.rule.name }}
Source: {{ event.alerts[0].source.ip | default: "unknown" }}
Risk score: {{ event.alerts[0].kibana.alert.risk_score }}
else:
- name: create_case
type: cases.createCase
with:
owner: securitySolution
title: "{{ event.rule.name }} - {{ event.alerts[0].host.name }}"
description: |
Auto-created from detection rule: {{ event.rule.name }}
Host: {{ event.alerts[0].host.name }}
Source IP: {{ event.alerts[0].source.ip | default: "N/A" }}
severity: high
tags:
- auto-triage
- "{{ event.rule.name }}"


- name: attach_evidence
type: cases.addAlerts
with:
case_id: "{{steps.create_case.output.case.id}}"
alerts:
- alertId: "{{ event.alerts[0]._id }}"
index: "{{ event.alerts[0]._index }}"


- name: add_observables
type: cases.addObservables
with:
case_id: "{{steps.create_case.output.case.id}}"
observables:
- typeKey: observable-type-ipv4
value: "{{ event.alerts[0].source.ip }}"
- typeKey: observable-type-file-hash
value: "{{ event.alerts[0].file.hash.sha256 }}"
on-failure:
continue: true
```


The Workflow automatically handles deduplication, evidence attachment, observable enrichment, graceful handling of missing fields, and analyst assignment. The analyst opens Kibana and sees a case with all the context already there.


The 25 new` cases.*` steps include create, find, find similar, update, close, delete, assign, unassign, add/remove alerts, add/remove observables, add/update/delete comments, add/remove tags, set severity, set status, get by ID, get by alert ID, and more for custom fields, user actions, and metrics. Each step is typed and validated. If you're using natural language authoring, the AI can generate them accurately because they're part of the schema.


As Workflows mature, you'll see more domain-specific steps for detection rule management, endpoint response actions, and threat intelligence operations.


##


Human checkpoints in automated Workflows


Case automation handles the mechanical work, but not every decision should be fully automated. AI can classify an alert and gather context, but the analyst should decide whether to escalate. The question is how much mechanical work happens before they make that call.


` waitForInput` pauses a Workflow for human judgment. The Workflow runs the investigation, gathers evidence, classifies the alert with AI, and stops. It presents structured findings and waits. The analyst reviews, approves or redirects, and adds notes, and then the Workflow resumes based on their input.


As the following image shows, the automation handles the investigation, but the analyst makes the decision before the automation executes it.


*Classifies incoming alerts with AI, pauses for analyst review and approval, and only escalates approved cases by creating a high-severity incident with context from both the model and the analyst.*


```text
name: HITL Example
enabled: true
triggers:
- type: alert


steps:
- name: classify
type: ai.classify
connector-id: Anthropic-Claude-Sonnet-4-6
with:
includeRationale: true
input: ${{ event.alerts[0] }}
categories:
- true_positive
- false_positive
- needs_investigation


- name: approval_gate
type: waitForInput
with:
message: |
Alert: {{ event.rule.name }}
Classification: {{ steps.classify.output.category }}
Rationale: {{ steps.classify.output.rationale }}
Review the classification and approve to escalate.
schema:
type: object
properties:
approved:
type: boolean
title: Approve escalation
notes:
type: string
title: Analyst notes
required:
- approved


- name: escalate
type: if
condition: "steps.approval_gate.output.approved : true"
steps:
- name: create_escalated_case
type: cases.createCase
with:
owner: securitySolution
title: "[Escalated] {{ event.rule.name }}"
description: |
Escalated by analyst after AI classification.
Classification: {{ steps.classify.output.category }}
Notes: {{ steps.approval_gate.output.notes }}
severity: high
tags:
- escalated
- analyst-reviewed
```


Today,` waitForInput` works through the Kibana execution view and the REST API. Slack, Teams, and email delivery channels are coming so analysts can review and approve without switching context. This is especially important for workflows defined as tools in Agent Builder, where agent-driven investigations benefit from human checkpoints before taking action.


##


Building Workflows from natural language


With the automation patterns in place, the next challenge is making them accessible. We chose YAML as the Workflow language because it's declarative, reviewable, and portable. But it was also a strategic choice: YAML is structured text, and large language models are very good at generating structured text. A well-typed workflow schema is an ideal target for AI-powered authoring. In 9.4, that bet pays off. Natural language authoring is available in Tech Preview.


Inside the Workflow editor, describe what should happen: "When a malware alert fires, check the file hash against VirusTotal, create a high-severity case, attach the alert and observables, and notify the SOC channel." The AI assistant generates the YAML. You review, refine, and deploy.


The YAML is fully inspectable and editable. If you know what should happen but aren't a YAML expert, this gets you from intent to a working workflow. If you are a YAML expert, you can start in natural language and refine in the editor.


The authoring experience will keep evolving. Visual editing is a complementary mode. Authoring that extends into Slack and other tools your team uses. The goal is to meet security teams wherever they work.


##


Composable Workflows


As your automation library grows, organization becomes critical. Security automation gets complex fast. You start with a single triage workflow, then realize different alert types need different investigation steps. You add conditionals, then more conditionals, and suddenly your workflow is hundreds of lines with nested logic that's hard to follow and harder to change.


` workflow.execute` lets you build reusable workflows with typed inputs and outputs. Instead of embedding all your investigation logic in one place, you create focused workflows for specific scenarios and compose them together. Update your malware investigation workflow once, and every workflow that calls it benefits. The logic stays organized, changes stay contained, and your automation scales without becoming brittle.


Here's what this looks like in practice. Classify the alert, then route to the specialized workflow based on the threat type:


```text
steps:
- name: dispatch
type: switch
expression: "{{ steps.classify.output.category }}"
cases:
- match: malware
steps:
- name: run_malware
type: workflow.execute
with:
workflow-id: ${{ consts.malware_workflow_id }}
inputs:
alert: ${{ event.alerts[0] }}
- match: phishing
steps:
- name: run_phishing
type: workflow.execute
with:
workflow-id: ${{ consts.phishing_workflow_id }}
inputs:
alert: ${{ event.alerts[0] }}
default:
- name: run_generic
type: workflow.execute
with:
workflow-id: ${{ consts.generic_triage_id }}
inputs:
alert: ${{ event.alerts[0] }}
```


Each workflow is independently testable. Most teams start with a single workflow and extract sub-workflows as patterns emerge. Composition is something you grow into.


The template library will eventually move into the product so you can discover and install pre-built workflows directly in Kibana.


##


Where analysts already work


Workflows are most useful when they're available where decisions get made. The primitives and patterns are in place, but automation only delivers value if analysts can reach it without switching tools. We're investing in making Workflows accessible throughout the security analyst experience, starting with the alerts table and Attack Discovery. Analysts can send alerts or entire attacks directly to a Workflow, triggering the investigation logic they've built without leaving their current context. This integration will continue expanding so that Workflow automation becomes a seamless part of the analyst's daily work, not a separate destination.


"Run workflow" in the alerts table lets you right-click an alert (or select multiple) and trigger a Workflow directly. The alert context passes automatically.


This is the beginning. Workflows will continue expanding into more parts of the security experience, making automation accessible where analysts need it.


##


More control, more flexibility


Beyond the major features, the Workflow language itself has become more capable. New flow control primitives give you cleaner ways to handle complex logic.` while` loops let you poll for conditions or wait for external state changes.` switch` provides multi-way branching so you can route alerts by category without nested conditionals.` loop.break` and` loop.continue` give you standard iteration control.


Step-level data transformation steps handle filtering, finding, aggregating, and JSON operations on collections, so you can process alert lists, IOC lookups, and enrichment responses without custom scripting.


The AI steps (` ai.prompt` ,` ai.classify` ,` ai.summarize` ,` ai.agent` ) are more stable and now support structured outputs, making it easier to use AI-generated classifications and summaries in downstream workflow logic.


LiquidJS templating expanded too. You can now set variables and access them throughout the workflow, making it easier to reference computed values across multiple steps.


##


Reliability and production controls


All of this is useful only if it's reliable. Every step supports` on-failure` configuration: retry for transient failures, continue when a step isn't critical, and abort when downstream steps depend on the result. Error details are available at` steps.<step-id>.error` to help route around failures.


Concurrency controls prevent alert storms from spawning hundreds of parallel executions. Loop guardrails prevent runaway execution. Composition depth limits prevent infinite recursion.


Elastic 9.4 introduces event-driven triggers, starting with` workflows.failed` . When a workflow execution fails, it can trigger a separate handler workflow. Build a notification workflow that alerts your team when automation goes down. More trigger types are coming: case status changes, alert state transitions, and detection rule updates, making Workflows reactive to what's happening across your security environment.


Every execution is recorded in execution history with step-by-step results, including analyst decisions from` waitForInput` steps. This is your debugging tool and your audit trail.


Workflows is enabled by default in 9.4 with an Enterprise license. Granular RBAC controls who creates, edits, executes, and views workflows. Every management operation writes to the security audit log. Import/export moves Workflows between environments with connector references intact.


##


Licensing and pricing


Workflows is available with an Enterprise license on Elastic Cloud Hosted and self-managed deployments, and with the Complete tier on Elastic Cloud Serverless for Security projects.


Version 9.4 introduces a unified execution-based pricing model across all deployment types.


Across Serverless, Elastic Cloud Hosted, and self-managed, pricing is based on workflow executions, with volume-based discounts at scale. **Each month includes a baseline allocation of workflow executions that are not billed** . Usage beyond this allocation is billed per execution, with volume-based discounts applied as usage increases.


**Elastic Cloud Serverless** begins applying this model on May 1, 2026. Usage beyond the monthly non-billed executions is charged per execution, with discounts at higher volumes.[See full pricing details](https://www.elastic.co/pricing/serverless-security) .


**Elastic Cloud Hosted and self-managed** deployments follow the same pricing model, **but are currently in a promotional period** with execution charges not yet applied. This allows teams to build and run Workflows, establish usage patterns, and prepare for the transition to execution-based billing in a future release.


Start building now. The Workflows you create today will continue to run as pricing transitions to the unified model.


##


Getting started


Workflows are enabled by default in 9.4. Open Kibana, navigate to Workflows, and start building.


If you're new to Workflows, the[Tech Preview post](https://www.elastic.co/security-labs/security-automation-with-elastic-workflows) walks through building your first triage Workflow. The[Chrysalis APT workflow](https://www.elastic.co/security-labs/speeding-apt-attack-discovery-confirmation-with-attack-discovery-workflows-and-agent-builder) shows Agent Builder integration in action. The[documentation](https://www.elastic.co/docs/explore-analyze/workflows) has the complete step type reference. The[Elastic Workflow Library](https://github.com/elastic/workflows) has ready-to-use templates.


Start with the Workflow that would save your team the most time this week. If you can describe it, you can build it.


---


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


#### Jump to section


- [Case automation at scale](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#case-automation-at-scale)
- [Human checkpoints in automated Workflows](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#human-checkpoints-in-automated-workflows)
- [Building Workflows from natural language](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#building-workflows-from-natural-language)
- [Composable Workflows](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#composable-workflows)
- [Where analysts already work](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#where-analysts-already-work)
- [More control, more flexibility](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#more-control-more-flexibility)
- [Reliability and production controls](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#reliability-and-production-controls)
- [Licensing and pricing](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#licensing-and-pricing)
- [Getting started](https://www.elastic.co/security-labs/elastic-workflows-ga-9-4#getting-started)


#### Elastic Security Labs Newsletter


[Sign Up](https://www.elastic.co/elastic-security-labs/newsletter?utm_source=security-labs)


#### Share this article


[X](https://twitter.com/intent/tweet?text=Elastic%20Workflows%20GA:%20automation%20where%20your%20security%20data%20already%20lives&url=https://www.elastic.co/security-labs/elastic-workflows-ga-9-4)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.elastic.co/security-labs/elastic-workflows-ga-9-4)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://www.elastic.co/security-labs/elastic-workflows-ga-9-4&title=Elastic%20Workflows%20GA:%20automation%20where%20your%20security%20data%20already%20lives)[Reddit](https://reddit.com/submit?url=https://www.elastic.co/security-labs/elastic-workflows-ga-9-4&title=Elastic%20Workflows%20GA:%20automation%20where%20your%20security%20data%20already%20lives)
