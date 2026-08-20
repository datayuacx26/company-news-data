---
schema_version: "1.0.0"
document_id: "6dadf05c9e7b3f416ab626d05bf28922dadafb60d9ca706ba53d0b536022cb21"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/automate-terraform-operations"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:b2e976c757325d57b2b0cf8fbcb4290185a15ae5e5210b52105f89e47819d100"
---

# How to Automate Terraform Operations with Kestrel Workflows: Incident Response, Provisioning, CI/CD, Drift Detection, and Security

Every team running Terraform Cloud carries the same operational load. A run errors at 2am and the failure sits unexamined until standup. A plan finishes and waits hours for someone to notice it needs confirmation. Real infrastructure drifts from state and nobody finds out until the next apply produces a diff nobody expected. A crashed run leaves a workspace locked, and the one person who knows it's safe to force-unlock is on vacation. None of this is a Terraform problem — each response is a known, repeatable sequence of steps that happens to be executed by a human with the Terraform Cloud UI open. The fix is codifying those sequences as workflows that run the same way every time, with a human approving anything that changes real infrastructure.


This post shows how to automate the seven domains of Terraform operations with[Kestrel Workflows](https://usekestrel.ai/blog/introducing-kestrel-workflows) : **incident response** , **infrastructure provisioning** , **developer requests** , **CI/CD** , **security** , **drift detection and remediation** , and **workspace and environment lifecycle** . Each section includes prompts written the way you'd hand them to the Workflow Agent — paste one in, review the workflow it builds, and activate it.


## How it works: plain English in, deterministic workflows out


You describe the automation in plain English and the Workflow Agent assembles it from real building blocks —[Terraform Cloud](https://docs.usekestrel.ai/integrations/terraform) 's run lifecycle (queue plan and destroy runs, apply, discard, or cancel them, and wait for completion), workspace locking and force-unlock, variables, state outputs, drift assessments, and a read-only AI investigation block — plus the stack around it:[GitHub](https://docs.usekestrel.ai/integrations/github) pull requests, Slack, PagerDuty, and Jira. The AI builds the workflow once; **the workflow itself runs deterministically** — the exact same steps on every execution, not an agent improvising at runtime against your production infrastructure.


Terraform Cloud pushes events to Kestrel through per-workspace notification webhooks, each delivery signed and verified — no polling delay. Run created, planning, needs-attention, applying, completed, and errored events, plus drift detections and failed health assessments, all become triggers that fire the moment Terraform Cloud emits them. Workflows start from the trigger that fits the job: Run Errored, Run Needs Attention, Drift Detected, CI events, schedules, or plain-English developer requests. And every workflow runs inside Kestrel's[access control and approval](https://usekestrel.ai/changelog/workflow-access-control-approvals) model — restrict who can invoke it and drop approval gates anywhere in the graph so no run is applied, no workspace destroyed, and no variable changed until the right person signs off in the dashboard, in Slack, or by merging a PR.


The Workflow Agent building a production-ready workflow from a plain-English description.


## How to automate incident response in Terraform


Terraform incidents announce themselves through the run lifecycle: the run that errors halfway through an apply, the failure at 2am that nobody sees until standup. Because run events arrive as signed webhooks, the workflow fires the moment the run fails — and the split is always the same: investigating and notifying is safe to automate fully, and anything that blocks or changes infrastructure gets an approval gate. Pair the freeze workflow below with an unlock companion (“when the on-call engineer requests unfreezing a workspace, unlock it and confirm in **#incidents** ”) so the lock never outlives the incident.


/kestrel-workflow


Diagnose


When a Terraform Cloud run errors, run a read-only investigation into what failed and why — the run's error, the workspace's recent run history, and what changed — then post the findings and the run URL to **#incidents** in Slack. If the workspace is a **production** workspace, also page the on-call engineer through PagerDuty with the summary attached.


/kestrel-workflow


Contain


When a Terraform Cloud run errors during an apply on a **production** workspace, send locking the workspace to a Slack approval gate for the on-call engineer — so no further runs can pile onto a half-applied state — lock it on approval with the failure reason recorded in the lock message, and post the freeze and the errored run's URL to **#incidents** in Slack.


/kestrel-workflow


Correlate


When a PagerDuty incident fires for one of our infrastructure-backed services, run a read-only investigation into the related Terraform Cloud workspace — recent runs, what they changed, and whether a change lines up with the incident window — and post the change-correlation summary to the incident's Slack channel so responders know immediately whether an infrastructure change is the likely cause.


## How to automate infrastructure provisioning in Terraform


Provisioning through Terraform is the platform-owned work of turning “we need an environment” into applied infrastructure — with the plan reviewed before anything is created. Done by hand it's a plan someone queues, forgets, and re-queues; done as workflows it's one governed pipeline: plan, show the diff, gate, apply, confirm. And where the infrastructure doesn't exist in code yet, the workflow generates the Terraform and opens a pull request, so provisioning never routes around code review.


/kestrel-workflow


Provision


When a team requests a new environment, queue a plan run on the environment's Terraform Cloud workspace, fetch the planned resource additions, changes, and destructions, and send them to a Slack approval gate for the platform team with the requesting team and their stated reason in the approval message. On approval, apply the run, wait for it to finish, and confirm the result in **#platform** in Slack.


/kestrel-workflow


Configure


When the platform team requests a configuration rollout — a new instance size, a feature flag, a region setting — send updating the workspace's Terraform variable to a Slack approval gate for the platform lead, set it on approval, then queue a plan run and post the planned changes to **#platform** in Slack so the rollout's impact is visible before anyone applies it.


/kestrel-workflow


Create


When a team requests a new standardized cloud resource — a bucket, a queue, a database — generate the Terraform for it with our tagging and encryption standards applied, and open a pull request against our infrastructure repository with the requesting team and the request in the PR description, so the new resource lands through code review and the normal plan-and-apply pipeline instead of ClickOps.


## How to automate developer requests in Terraform


Most of what developers need from Terraform isn't an apply — it's an answer. What's the database endpoint? What would this change do? Why is the workspace locked? Each of those is a request to whoever holds Terraform Cloud access, and each blocks a developer until someone context-switches. Handled by request-triggered workflows, these become self-service: the request is matched to a pre-built workflow, the parameters are extracted from the phrasing, and team-scoped permissions decide who can ask for what — no Terraform Cloud seats or shared API tokens required.


/kestrel-workflow


Read


When a developer requests a workspace's state outputs — the database endpoint, the load balancer DNS name, the bucket ARN — fetch the current state outputs from the requested Terraform Cloud workspace and reply in Slack with the values. Sensitive outputs are redacted automatically, so this is safe to run ungated for any developer.


/kestrel-workflow


Preview


When a developer requests a plan preview on their team's workspace, queue a plan run, wait for it to finish, and reply in Slack with the planned additions, changes, and destructions and the run URL — never apply. Scope this to the requester's team workspaces so developers can see what their changes would do without touching production.


/kestrel-workflow


Unblock


When a developer reports a workspace stuck by a crashed run's lock, fetch the workspace's lock state and recent runs for context, send force-unlocking it to a Slack approval gate for the platform team with that context attached, force-unlock on approval, and confirm to the requester in Slack — so a stuck lock is a five-minute request instead of a half-day hunt for an admin.


## How to automate CI/CD with Terraform


The heart of Terraform CI/CD is the confirmation step: a plan finishes, and someone has to look at it before it applies. That's exactly what Terraform Cloud's *needs attention* event exists for — and exactly the shape of an approval gate. Wire it to Slack and the review happens where your team already is, with the change counts in the approval message; wire the merge event to a plan and the completed event to a release note, and the whole pipeline reports itself.


/kestrel-workflow


Gate


When a Terraform Cloud run finishes planning and awaits confirmation on a **production** workspace, fetch the run's planned resource additions, changes, and destructions and send them to a Slack approval gate for the release owner with the run URL. Apply the run on approval, discard it on rejection, and post the decision to **#releases** in Slack either way.


/kestrel-workflow


Deploy


When a pull request merges to **main** in our infrastructure repository, queue a plan run on the matching Terraform Cloud workspace with the PR title as the run message, wait for the plan to finish, and post the result — the planned change counts and the run URL — to **#releases** in Slack so the merge and its infrastructure impact land in the channel together.


/kestrel-workflow


Report


When a Terraform Cloud run completes on a **production** workspace, fetch the run's applied additions, changes, and destructions and post a release summary with the run URL to **#releases** in Slack — so every production infrastructure change is announced with its actual diff, not discovered in next week's plan.


## Terraform security automation


Terraform security has a quiet failure mode: the risky configuration nobody looks at. The workspace someone flipped to auto-apply during a crunch, the credential that's been in a variable for a year, the run queue that keeps executing during an active incident. Workflows make the risky states visible on a schedule and put the sensitive changes — rotations, freezes — behind gates with an audit trail.


/kestrel-workflow


Audit


Every Monday at 9am, run a read-only investigation across our Terraform Cloud organization — which workspaces have auto-apply enabled, which have errored or stale runs sitting in the queue, and which have failing health assessments — and post the audit report to **#security** in Slack so risky configurations get reviewed weekly instead of discovered during an incident.


/kestrel-workflow


Rotate


When the security team requests a credential rotation, send updating the workspace's variable — marked sensitive and write-only — to a Slack approval gate for the security lead, set it on approval, queue a plan run to roll the new credential out, and confirm the rotation and the run URL in **#security** in Slack.


/kestrel-workflow


Contain


When a PagerDuty security incident fires, send locking our **production** Terraform Cloud workspaces to a Slack approval gate for the security lead — with the incident in the lock reason — lock them on approval so no infrastructure changes can land mid-incident, and confirm the freeze in **#security** in Slack.


## How to automate Terraform drift detection and remediation


Drift is the gap between what your state file says and what your cloud actually runs — and it widens silently until an apply produces a diff nobody expected. Terraform Cloud's health assessments detect it (on workspaces with assessments enabled — an HCP Terraform Plus feature); the workflows below decide what happens next: an investigation with findings attached, a gated remediation run, and a tracked ticket when the assessment itself stops working.


/kestrel-workflow


Diagnose


When Terraform Cloud detects drift on a workspace, run a read-only investigation into which resources drifted and what the likely cause is — a manual console change, an external automation, a deleted resource — then post the findings to **#platform** in Slack and create a Jira ticket for the owning team so the drift is tracked work with an owner.


/kestrel-workflow


Remediate


When Terraform Cloud detects drift on a **production** workspace, queue a plan run to preview the remediation, fetch the planned changes, and send applying them to a Slack approval gate for the platform team with the change counts in the approval message. Apply on approval and confirm in **#platform** that state and reality have reconverged.


/kestrel-workflow


Alert


When a Terraform Cloud health assessment fails to run on a workspace, fetch the workspace's status and recent run history for context, post the failure to **#platform** in Slack, and create a Jira ticket for the platform team — a workspace whose drift detection is broken is a blind spot, and blind spots get tracked.


## Terraform workspace and environment lifecycle automation


Workspaces accumulate: the ephemeral environment nobody tore down, the lock nobody released, the inventory nobody has looked at since Q1. Lifecycle workflows keep the estate tidy on a schedule and make teardown a governed request instead of a dreaded manual destroy. Pair the change-freeze workflow with its Monday companion (“every Monday at 8am, unlock the production workspaces and confirm in **#platform** ”) so the freeze lifts itself.


/kestrel-workflow


Freeze


Every Friday at 6pm, lock our **production** Terraform Cloud workspaces with the reason “weekend change freeze” and post the list of locked workspaces to **#platform** in Slack — so weekend applies require deliberately breaking the freeze, not just clicking Confirm.


/kestrel-workflow


Teardown


When a team requests tearing down an ephemeral environment, send queueing a destroy run on the environment's Terraform Cloud workspace to a Slack approval gate for the platform team with the requesting team and reason in the approval message. On approval, create the destroy run, wait for it to finish, and confirm the teardown result in **#platform** in Slack.


/kestrel-workflow


Inventory


Every Monday at 9am, list our Terraform Cloud workspaces with their lock state and resource counts and post the inventory report to **#platform** in Slack — so forgotten environments, long-held locks, and workspaces that quietly doubled in size get spotted in review instead of on the invoice.


## Making Terraform easier to manage


Twenty-one workflows, seven domains, one pattern: describe the operation in plain English, let the Workflow Agent assemble it from real Terraform Cloud blocks and the stack around them, review the graph, and put approval gates on anything that applies, destroys, or unlocks. Terraform keeps doing what it's best at — declaring your infrastructure as code — and Kestrel turns its run lifecycle into governed action: investigations with findings attached, gated applies and destroys, drift remediations, credential rotations, change freezes, pages, and tickets. The recurring 80% runs itself, and your team's time goes to the judgment calls.


This isn't Terraform-only, either. The same workflow engine spans 30+ integrations across infrastructure, PaaS, observability, comms, databases, CI/CD, and IaC — so the workflow that gates a production apply lives next to the one that restarts a Kubernetes deployment or drains a Cloudflare load balancer pool. For deeper dives, see the[internal developer platform guide](https://usekestrel.ai/blog/internal-developer-platform) and the companion posts on[automating Kubernetes operations](https://usekestrel.ai/blog/automate-kubernetes-operations) ,[automating ArgoCD operations](https://usekestrel.ai/blog/automate-argocd-operations) ,[automating Pulumi operations](https://usekestrel.ai/blog/automate-pulumi-operations) ,[automating AWS cloud operations](https://usekestrel.ai/blog/automate-aws-cloud-operations) ,[automating Cloudflare operations](https://usekestrel.ai/blog/automate-cloudflare-operations) ,[automating Vercel operations](https://usekestrel.ai/blog/automate-vercel-operations) ,[automating Neon operations](https://usekestrel.ai/blog/automate-neon-operations) ,[automating ClickHouse operations](https://usekestrel.ai/blog/automate-clickhouse-operations) ,[automating Supabase operations](https://usekestrel.ai/blog/automate-supabase-operations) , and[automating PlanetScale operations](https://usekestrel.ai/blog/automate-planetscale-operations) .


## FAQ: automating Terraform operations


### How do you automate Terraform operations?


Turn each recurring run-lifecycle event — a failed run, a plan awaiting confirmation, detected drift, a stuck lock — into a deterministic, approval-gated workflow instead of a human watching the Terraform Cloud UI. With Kestrel, you describe the automation in plain English, the Workflow Agent assembles it from real Terraform Cloud blocks plus the stack around them (GitHub, Slack, PagerDuty, Jira), and it runs the exact same steps every time, pausing at approval gates before anything is applied.


### What is the best automation tool for Terraform?


It depends on scope. Terraform itself — with` terraform plan` and` apply` in a CI pipeline — is the right primitive for declaring and shipping infrastructure. For turning Terraform Cloud events into governed action *across* your whole stack — run investigations, gated applies and destroys, drift remediation, credential rotations, change freezes, workspace inventories, pages, and tickets in one system — Kestrel Workflows builds deterministic workflows from plain English, triggered by run lifecycle events, drift detections, failed assessments, schedules, CI events, and developer requests.


### Is it safe to automate destructive operations in Terraform?


Yes — because Terraform's own model separates preview from execution, and Kestrel enforces that separation. Queueing a plan changes nothing; it produces a diff. The steps that change real infrastructure — applying a run, queueing a destroy, force-unlocking a workspace, setting a variable — always sit behind an approval gate in these workflows, so nothing executes until the right person signs off in Slack, with the requester, the plan's change counts, and every step recorded in the run's audit trail. Read-only workflows (investigations, state outputs, inventories, drift checks) are always safe to run ungated.


### How do you make Terraform easier to manage?


Reduce the number of run-lifecycle events a human has to react to. Automate the recurring responses — errored runs, plans waiting for confirmation, drifted workspaces, stuck locks — give developers self-service state outputs, plan previews, and unlock requests without Terraform Cloud access, and keep humans in the loop only where judgment matters: approving applies, destroys, variable changes, and force-unlocks, not babysitting the run queue.


## Getting started


Connect[Terraform Cloud](https://docs.usekestrel.ai/integrations/terraform) — an API token plus a notification webhook on the workspaces you care about, about five minutes end to end — then pick the domain that hurts most. For most teams that's CI/CD: the plan that sits awaiting confirmation for hours should be a Slack approval with the change counts attached, not a browser tab someone forgot. Paste one of the prompts above into the Workflow Agent, review the workflow it builds, tighten the scoping and approvals, activate it, and add the next one. The[Workflows Quickstart](https://docs.usekestrel.ai/quickstart/workflows) walks through your first workflow end to end, and the[Create Workflows guide](https://docs.usekestrel.ai/workflows/create-workflows) and[Integrations Setup](https://docs.usekestrel.ai/workflows/setup-integrations) cover connecting[GitHub](https://docs.usekestrel.ai/integrations/github) and the rest of your stack. Prefer the terminal or an AI coding agent? Workflows can also be created and managed through the[CLI](https://docs.usekestrel.ai/workflows/cli) ,[Python SDK](https://docs.usekestrel.ai/workflows/sdk) , and[MCP](https://docs.usekestrel.ai/workflows/mcp) . Within a few weeks every workspace, run, and drift assessment that matters will have a workflow behind it — and an audit trail to prove it.


### Automate your Terraform operations with $1,000 in credits


Incident response, infrastructure provisioning, developer requests, CI/CD, security, drift detection, and workspace lifecycle — describe each workflow in plain English and Kestrel builds it. New accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
