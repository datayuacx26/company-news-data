---
schema_version: "1.0.0"
document_id: "f21b7baa5164af4c84c5acb9f4ed4454bed4763437f05888c751ebd5cf80ff22"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/introducing-kestrel-workflows"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:96a9f245d6a7684132fcf3fbf19883faa3f70fdf35d3e180f91fbabc067fd821"
---

# Introducing Kestrel Workflows: From Plain English to Production-Ready Automation

Every platform engineering team carries the same quiet backlog. Incident runbooks that live in one person's head. Cloud provisioning requests that pile up in Slack. CI/CD glue scripts that only the engineer who wrote them can run. And a never-ending tail of "can you just" developer asks. Automating any of it usually means writing brittle scripts, wiring up webhooks, and gluing a dozen APIs together by hand — then maintaining all of it forever.


Today we're launching **Kestrel Workflows** , and the **Workflow Agent** that builds them. You automate incident response, cloud provisioning, CI/CD, developer requests, and security with AI agents that build deterministic workflows across your entire stack. Describe what you want in plain English, and Kestrel assembles the whole thing for you. The demo video above shows the entire loop end to end — going from a sentence to a running, branching, multi-integration workflow in under two minutes.


## From one prompt to a complete workflow


Here's the workflow from the demo, described entirely in one prompt:


"When a pod is in CrashLoopBackOff in the production namespace, trigger an RCA and generate a fix. If it's a YAML fix, post the RCA to the #incidents channel in Slack, wait for approval in the #platform-ops channel, then apply the fix to the cluster. If it's a code fix, create a pull request in GitHub, notify the #incidents channel, and create a Jira ticket in the INFRA project."


One Cmd+Enter later, the agent has built the entire graph: a Pod CrashLoopBackOff trigger filtered to the` production` namespace, a Trigger RCA & Generate Fix action, a condition that checks whether the fix is application-level, and two branches — one that applies a YAML fix behind a Slack approval, the other that opens a GitHub PR and files a Jira ticket. Every node lands on the canvas already wired together, with the right template variables threaded between steps.


From there it's yours to refine. In the demo we open the trigger and add` staging` alongside` production` , inspect the Slack message template and its RCA variables, then drag a PagerDuty *Create Alert* action straight from the sidebar and drop it in after the Slack notification — set to **critical** , using the incident title as the alert title. The toolbar shows a 24-hour cooldown, so the workflow won't fire repeatedly for the same pod.


## From words to production-ready workflows


Use your words to build automations for incident response, cloud provisioning, CI/CD pipelines, developer requests, and more — with 140+ pre-built actions across 30+ integrations, plus any number of custom actions. Open the workflow builder and the canvas starts empty, with a single input pinned to the bottom: **Describe workflow in natural language** . Click it to summon the Workflow Agent, type what you want, and hit **Generate** . The agent returns a complete, production-ready workflow graph — a name, a description, a configured trigger, and a directed graph of actions, conditions, and approval gates with the edges already drawn. It drops onto the canvas fully editable, never a black box.


Click to expand


## A canvas for everything you automate


Under the hood, every workflow is a visual DAG. A searchable sidebar lists every signal and action, grouped by integration; drag blocks onto the canvas, connect them with edges, and click any node to configure its parameters. There are four kinds of blocks:


- Triggers


— the signal that starts a run: a Kubernetes pod crash, an AWS cost anomaly, a PagerDuty page, a PostHog exception, a Vercel deploy failure, a GitHub event, a custom webhook, or a developer request from Slack.
- Actions


— the work the workflow does: run RCA, generate and apply a fix, open a PR, file a Jira ticket, sync ArgoCD, run a Helm upgrade, post to Slack, and much more.
- Conditions


— an **If / Else** block that branches on the value of a field from an earlier step.
- Approval gates


— pause the workflow until someone signs off, in the platform, in Slack, or by merging a pull request.


## Refine with words, drag-and-drop, or code


However you start, you can keep editing the same workflow three ways. Re-open the Workflow Agent and describe a change in **natural language** . Edit visually on the **canvas** — add or remove steps, draw new edges, tweak parameters and approvals. Or open the **Code** panel to see the equivalent **Python SDK** code, generated live as you build. Every trigger, action, condition, and approval gate maps to a typed method, so you can copy it, version-control it, drop it into a CI pipeline, or open a PR to manage the workflow as code in your repo.


The visual canvas and the Python SDK are two views of the same workflow — the same triggers, the same branching, the same gates — just a different interface. Switch between them however your team prefers to work.


Click to expand


## Deterministic by design


This is the part that matters most: the agent that *builds* your workflow is not the thing that *runs* it. Once a workflow is configured, execution is fully deterministic. It runs the exact steps you defined, in the exact order, with no model in the runtime loop deciding what to do next. No hallucinations, no surprises, no drift between runs.


That distinction is what makes Kestrel Workflows safe to put in front of production. AI gives you the speed of authoring in plain English; deterministic execution gives you the reliability your runbooks actually need.


## Access control and approval gates


Automation is only as safe as its guardrails. The moment a workflow can restart a deployment, open a pull request, or provision cloud infrastructure, two questions matter more than anything else: *who is allowed to trigger it* , and *who has to sign off before it runs* . Kestrel Workflows answers both. You can scope who is allowed to start a workflow, and drop **approval gates** anywhere in the graph that pause execution until the right people approve — in the platform, in Slack, or by merging a pull request.


For on-demand **developer request** workflows — the ones your team kicks off themselves rather than ones that fire on an infrastructure signal — you can restrict exactly who is allowed to run them with **Allowed Users** and **Allowed Groups** on the request trigger. Layer these with the request's own scoping — allowed clusters, namespaces, AWS accounts, zones, and more — so a self-service workflow can be opened up to a whole team while staying fenced into the exact resources they're permitted to touch.


Click to expand


### Approval gates, three ways


Drag an **Approval Gate** block onto the canvas anywhere you want a human in the loop. The block pauses the workflow and branches into two paths — **Approved** and **Rejected** — so you decide what happens next in either case. Pick one of three approval types: a **manual** in-app approval from the Kestrel dashboard, an interactive **Slack** request your team approves right where they already work, or a **PR / MR approval** that ties the gate to a pull request — the workflow continues when the PR is approved or merged, and stops if it's closed, so your existing code-review process becomes the approval step.


Click to expand


For manual and Slack gates, the **Require Approval From** editor lets you build approval rules out of individual users and groups. By default anyone in the org can approve; add people or groups to a rule and *all* of them must approve, and add a second rule so *either* rule can satisfy the gate. That covers everything from "any platform admin" to "both the on-call lead and a security reviewer."


Click to expand


Every execution waiting on a human lands in one place. The **Pending Approvals** page collects every blocked run across your whole organization — each row shows the workflow, the approval type, the required approvers, and how long it's been waiting. Approve or reject manual and Slack gates directly, jump to the full execution for context, or open the pull request for PR gates, with a **Recent Approvals** history that keeps an auditable record of who approved or rejected what, and when.


Click to expand


It's role-based by default. Building, activating, and approving workflows is scoped to your platform administrators, while app developers get a focused view for submitting developer requests — governed by the Allowed Users and Allowed Groups rules above. So you can safely open self-serve automation to your whole engineering org without handing everyone the keys to production.


## Connected to your entire stack


Workflows ship with a large library of pre-built actions spanning your infrastructure, deployment platforms, source control, CI/CD, observability, project management, networking, IaC, and communication tools — 30+ integrations in all. For anything not yet built in, custom HTTP action blocks and custom webhook triggers let you wire up any service you run. You connect each integration once, and every workflow in your org shares the same connections.


### Infrastructure


[Kubernetes](https://github.com/KestrelAI/Kestrel-Operator)


AWS


OCI


### PaaS


Vercel


Railway


Fly.io


### AI Compute


Nebius


Daytona


Beam


### Observability


OTel


PostHog


Datadog


### On-Call


Slack


PagerDuty


incident.io


### Project Mgmt


Jira


Linear


Confluence


Notion


### Networking


Cloudflare


Cilium


Envoy


Istio


### Databases


Supabase


PlanetScale


Neon


ClickHouse


### CI/CD


GitHub


GitLab


ArgoCD


Helm


### IaC & GitOps


Terraform


Pulumi


OpenTofu


CFN


+ custom webhook triggers and HTTP API action blocks


That breadth is what lets a single workflow span an entire incident: detect a Kubernetes failure, run RCA, generate a fix, open a GitHub PR, wait for Slack approval, sync ArgoCD, and post the result back to the team — all in one deterministic run.


## What can you automate with workflows?


Kestrel Workflows is a general-purpose automation engine for platform engineering teams — across incident response, cloud provisioning, CI/CD, cost optimization, security, and more. Here are a few examples, each built from a single plain-English prompt:


### Incident Response


Detect, investigate, and remediate in seconds


When a PagerDuty alert fires or a Kubernetes or cloud incident is detected, trigger a workflow that runs root cause analysis, posts findings to Slack, creates a Jira ticket, and generates a fix.


Workflow Agent prompt


When a PagerDuty alert fires for a production Kubernetes cluster, automatically run root cause analysis, post the findings to #incidents in Slack, create a Jira ticket with severity P1, and apply the recommended fix if confidence is above 90%.


Workflow Agent


✕


Describe your workflow in plain English. The AI agent will generate a workflow graph you can edit.


Cmd+Enter to generate


Generate


PagerDuty K8s Incident Response


Automated incident detection, investigation, and remediation pipeline


TRIGGER


PagerDuty Alert


KESTREL


Run Root Cause Analysis


CONDITION


Is Severity Critical?


rca_result.severity


True →


False →


SLACK


Post RCA to #incidents


JIRA


Create P1 Jira Ticket


APPROVAL


Approve Fix in Slack


Approved →


Rejected →


KESTREL


Apply YAML Fix


SLACK


Notify #incidents


Describe workflow in natural language...


</>


Python SDK Code


```text


```


pip install kestrel-sdk


✦ Create PR


### Cloud Provisioning


Self-serve infrastructure for developers


Let developers request cloud resources through Slack or the Kestrel platform. Workflows handle approval routing, IaC provisioning, access configuration, and notifications — with guardrails enforced at every step.


Workflow Agent prompt


When a developer submits a cloud provisioning request via Slack, route it to the team lead for approval, provision the requested AWS resources using Terraform, configure IAM permissions, and notify the developer with access details.


Workflow Agent


✕


Cmd+Enter to generate


Generate


Cloud Provisioning Request


Self-serve infrastructure provisioning with approval gates


TRIGGER


Dev Request via Slack


KESTREL


Generate Terraform IaC


APPROVAL


Team Lead Approval


Approved →


Rejected →


KESTREL


Open IaC Pull Request


GITHUB


Wait for PR Merge


SLACK


Notify Developer


Describe workflow in natural language...


</>


Python SDK Code


```text


```


pip install kestrel-sdk


✦ Create PR


### Developer Requests


Self-serve with justification and approval gates


Turn ad-hoc developer asks into governed, auditable workflows. When a developer requests a resource through the platform, Kestrel collects justification, routes it to the right team for approval, fulfills the request automatically when approved, and notifies the requester either way.


Workflow Agent prompt


When a developer requests a new Cloudflare domain through the platform Developer Request chat, ask them for a justification, then route it to the platform team for approval. If approved, create the Cloudflare DNS record and confirm in the Developer Request chat. If rejected, notify the requester that the request was denied.


Workflow Agent


✕


Cmd+Enter to generate


Generate


Cloudflare Domain Request


Developer self-serve with justification and platform approval


TRIGGER


Developer Request: New Domain


KESTREL


Request Justification


APPROVAL


Platform Team Approval


Approved →


Rejected →


CLOUDFLARE


Create DNS Record


KESTREL


Confirm in Developer Request Chat


KESTREL


Notify Requester of Denial


Describe workflow in natural language...


</>


Python SDK Code


```text


```


pip install kestrel-sdk


✦ Create PR


### CI/CD Pipelines


Ship with confidence, automate the glue


Orchestrate the steps between your CI/CD system and the rest of your stack. Trigger deployments on merge, run post-deploy health checks, roll back on failure, and notify the team — all as a single workflow.


Workflow Agent prompt


When a PR is merged to main in GitHub, trigger a deployment to the staging cluster, run health checks for 5 minutes, promote to production if healthy, and send a deployment summary to #releases in Slack. If health checks fail, roll back and alert the on-call engineer.


Workflow Agent


✕


Cmd+Enter to generate


Generate


CI/CD Deploy & Promote


Automated deployment pipeline with health checks and rollback


TRIGGER


PR Merged to Main


GITHUB


Deploy to Staging


GITHUB


Run Health Checks


CONDITION


Deploy Healthy?


conclusion


True →


False →


ARGOCD


Promote to Prod


ARGOCD


Wait for Sync


SLACK


Post to #releases


GITHUB


Trigger Rollback


PAGERDUTY


Alert On-Call


Describe workflow in natural language...


</>


Python SDK Code


```text


```


pip install kestrel-sdk


✦ Create PR


### Cost Optimization


Monitor spend, cut waste, enforce budgets


Monitor cloud spend, detect anomalies, identify idle resources, and automatically enforce budgets — turning cost management from a monthly fire drill into a continuous, automated process.


Workflow Agent prompt


Every 24 hours, scan all AWS accounts for unused EBS volumes, idle load balancers, and oversized instances. Send a cost summary to #cloud-costs in Slack and create rightsizing tickets in Jira.


Workflow Agent


✕


Cmd+Enter to generate


Generate


Daily Cloud Cost Audit


Automated cost scanning, analysis, and ticketing


TRIGGER


Every 24 Hours


AWS


Query Cost Explorer


AWS


Detect Anomalies


KESTREL


AI Cost Analysis


CONDITION


Anomalies Found?


anomaly_count


True →


False →


SLACK


Post to #cloud-costs


JIRA


Create Rightsizing Ticket


SLACK


Post Summary


Describe workflow in natural language...


</>


Python SDK Code


```text


```


pip install kestrel-sdk


✦ Create PR


### Security


Detect threats and respond instantly


Automate security response across your entire stack. When usage anomalies, firewall attacks, IAM security events, or SecurityHub findings are detected, trigger workflows that investigate, contain, and remediate — before damage spreads.


Workflow Agent prompt


When Vercel detects a firewall attack or usage anomaly on a production project, immediately block the suspicious IP range, capture request logs for forensic analysis, notify the security team in Slack with a summary, and create a P1 incident in PagerDuty.


Workflow Agent


✕


Cmd+Enter to generate


Generate


Vercel Security Response


Automated threat detection, containment, and incident creation


TRIGGER


Firewall Attack Detected


VERCEL


Investigate Attack


KESTREL


Generate WAF Block Rule


APPROVAL


Security Team Approval


Approved →


Rejected →


KESTREL


Block IP Range


SLACK


Notify #security


PAGERDUTY


Create P1 Incident


Describe workflow in natural language...


</>


Python SDK Code


```text


```


pip install kestrel-sdk


✦ Create PR


## Custom integrations for anything you run


The built-in catalog covers the most common tools in the platform engineering stack, but every team runs something it doesn't cover yet — an internal API, a niche SaaS product, a homegrown deployment service. None of that is a dead end. With **Custom Integrations** , you can build your own action blocks and webhook triggers for anything that speaks HTTP or sends webhooks, right from the workflow builder sidebar. Once saved, they sit alongside the built-in blocks and drop into any workflow.


### Custom action blocks — paste a cURL command


A custom HTTP action block lets you call any API endpoint as a workflow step. The fastest way to build one is to **paste a cURL command** from the API you want to call. Kestrel's AI agent reads it and automatically extracts the method and URL, the headers, the request body, the auth type, response mappings, and which values should become dynamic parameters. Review the auto-detected configuration, adjust names and defaults, and click **Create** — or skip the agent and build every field from scratch.


Click to expand


Action blocks support **API key** , **bearer token** , and **basic auth** , and credentials are stored encrypted — never exposed in workflow logs or execution history. URLs and bodies accept template variables like` {{rca_result.root_cause}}` , and the values you map from the JSON response become available downstream as` {{step_outputs.<action-id>.<variable>}}` .


### Custom webhook triggers — paste a payload


A custom webhook trigger lets you start a workflow from any external service that can send an HTTP POST. The fastest way to build one is to **paste an example JSON payload** . Kestrel detects the event-type field, extracts the useful fields as template variables, and generates a field mapping with sensible names — available as` {{signal.<field_name>}}` in your workflow. Each trigger gets a unique webhook URL, can filter on specific event types so one webhook only fires the workflows it should, and supports **HMAC signature verification** so Kestrel rejects any payload that isn't authentic.


Click to expand


Because custom blocks compose with the built-in ones, a single workflow can span your homegrown tooling and your whole stack — fire a workflow off a Prometheus Alertmanager webhook to triage a firing alert, investigate a` payment_intent.payment_failed` event from Stripe, or kick off a GitOps PR and ArgoCD sync from your own deployment service's deploy event.


## Getting started


Open **Workflows** in your Kestrel dashboard, click **New Workflow** , and describe what you want to automate. Kestrel uses your connected integrations to wire the workflow up correctly — so the more you've connected, the more it can do out of the box. See the[Create Workflows guide](https://docs.usekestrel.ai/workflows/create-workflows) to learn every way to build — natural language, drag-and-drop, CLI, SDK, and MCP — and[Integrations Setup](https://docs.usekestrel.ai/workflows/setup-integrations) to connect your stack.


### Start building with $1,000 in credits


Automate incident response, cloud provisioning, CI/CD, developer requests, security, and more. Describe it in plain English and Kestrel builds the workflow — new accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
