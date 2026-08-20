---
schema_version: "1.0.0"
document_id: "6764b2141d665b3b989924d43e769bb40521b533033659e420e9afaa76b5edeb"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/automate-nebius-operations"
published_at: "2026-06-13T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:2ece5ff20addf7cd5ca3a04e5902b1fa07f38c7439e538cfcfda1140cafe80e8"
---

# How to Automate Nebius Operations with Kestrel Workflows: Incident Response, GPU Provisioning, CI/CD, and Security

Every team training or serving models on Nebius carries the same operational load. A node reports a GPU error mid-training-run and someone spends an hour working out whether to restart, replace, or wait. A researcher needs a node group scaled up *now* and the one person with project admin access is in a meeting. The nightly training job starts before anyone remembered to add capacity. And the development instances that nobody stopped are quietly burning GPU-hours all weekend.


None of this is a Nebius problem — it's an automation problem. Each of those tasks is a known, repeatable sequence of steps that happens to be executed by a human with console access. The way to make Nebius easier to manage is not more admin access; it's codifying those sequences as workflows that run the same way every time, with a human approving anything that creates or destroys billable GPU capacity.


This post shows how to automate the five domains of Nebius operations with[Kestrel Workflows](https://usekestrel.ai/blog/introducing-kestrel-workflows) : **incident response** , **GPU provisioning** , **developer requests** , **CI/CD** , and **security** . Each section includes three prompts written the way you'd hand them to the Workflow Agent — paste one in, review the workflow it builds, and activate it.


## How it works: plain English in, deterministic workflows out


You describe the automation in plain English and the Workflow Agent assembles it from real building blocks —[Nebius](https://docs.usekestrel.ai/integrations/nebius) 's GPU and cluster operations (Create, Delete, Start, Stop, and Restart Instance, Get Instance Status, List Instances, Clusters, and Node Groups, Scale, Create, and Delete Node Group, and AI-powered Investigate Nebius) plus the stack around it:[GitHub](https://docs.usekestrel.ai/integrations/github) pull requests and Actions, Slack, PagerDuty, and Jira. The AI builds the workflow once; **the workflow itself runs deterministically** — the exact same steps on every execution, not an agent improvising at runtime against your GPU fleet.


Nebius has no outbound webhooks, so Kestrel detects GPU and node lifecycle events by polling the Nebius API on an interval you configure — there is nothing to set up inside Nebius itself. Workflows start from the trigger that fits the job: GPU Error, Node Not Ready, Instance Stopped, Maintenance Scheduled, CI events, schedules, or plain-English developer requests. And every workflow runs inside Kestrel's[access control and approval](https://usekestrel.ai/changelog/workflow-access-control-approvals) model — scope it to specific Nebius projects, restrict who can invoke it, and drop approval gates anywhere in the graph so nothing creates or destroys billable capacity until the right person signs off in the dashboard, in Slack, or by merging a PR.


The Workflow Agent building a production-ready workflow from a plain-English description.


## How to automate incident response in Nebius


Nebius incidents come in three shapes: the node that reports a GPU error mid-run, the managed-Kubernetes node that goes NotReady and takes training capacity with it, and the instance that stops when nobody asked it to. All three follow the same split — the investigation is safe to automate fully, and the remediation gets an approval gate.


/kestrel-workflow


Investigate & Fix


When a Nebius node reports a GPU error, run an investigation across the affected instance, its cluster, and its node group to determine whether the error is a transient driver fault or a failing GPU, and post the analysis to **#ml-infra** in Slack. Send an instance restart to a Slack approval gate for the on-call engineer; on approval, restart the instance, fetch its status to confirm it is back to running, and if the GPU error persists after the restart, page on-call through PagerDuty with the investigation attached.


/kestrel-workflow


Respond


When a node in our Nebius managed Kubernetes cluster goes NotReady, list the cluster's node groups to find the group the node belongs to, and send a scale-up of that node group by one node to a Slack approval gate so training capacity is restored while we debug. On approval, scale the node group, confirm the new node count, create a Jira ticket for the bad node, and post the summary to **#ml-infra** in Slack.


/kestrel-workflow


Triage


When a Nebius instance stops unexpectedly, fetch the instance's status and classify whether the stop was intentional. If it wasn't, start the instance, confirm it comes back to running, and post what happened to **#ml-infra** in Slack. If the instance fails to start, page the on-call engineer through PagerDuty with the instance details attached.


## How to automate GPU instance provisioning in Nebius


GPU provisioning is the highest-stakes automation in this post because every action has a price tag attached. The pattern that works: creation and teardown go behind approval gates, and cost control goes on a schedule so idle capacity stops burning money without anyone having to remember.


/kestrel-workflow


Provision


When a team requests a new GPU instance, send the request — platform, resource preset, and boot disk size — to a Slack approval gate for the platform team. On approval, create the instance in the team's Nebius project with a CUDA image and their SSH key, wait for it to reach running, and reply to the requester in Slack with the instance ID and connection details.


/kestrel-workflow


Schedule


Every weekday at 8pm, list the running Nebius instances in our development and experiment projects and stop every one of them. Every weekday morning at 8am, post a summary to **#ml-infra** in Slack — how many instances were stopped and the estimated GPU-hours saved overnight. No approval gate; these are non-production projects.


/kestrel-workflow


Teardown


When a team requests teardown of a finished project's GPU capacity, send the plan to a Slack approval gate for the platform lead. On approval, delete the project's instances including their boot disks so no orphaned disk keeps billing, delete the project's node groups from the managed Kubernetes cluster, post the completed teardown summary to **#ml-infra** in Slack, and close the tracking ticket in Jira.


## The best way to automate developer requests in Nebius


For teams training and serving models, GPU compute is the request that carries the most cost, so it's the one you most want to make self-service *and* governed. Developers and researchers ask in Slack with` /kestrel-workflow` , from the[CLI](https://docs.usekestrel.ai/workflows/cli) , or on the Developer Requests page; Kestrel matches the request to an active request-triggered workflow, extracts the parameters from the phrasing, and runs it behind the approval gates, budget caps, and project scoping you defined.


/kestrel-workflow


Provision


When a researcher asks to scale up a GPU node group for a training run, scale the named Nebius managed-Kubernetes node group to the requested size, wait for the new nodes to become ready, and reply in Slack with the node count and the estimated hourly cost. Require platform-admin approval before scaling, and cap the node group at the size the requester's team is budgeted for.


/kestrel-workflow


Scale


When a researcher asks to stop or start a GPU instance, stop or start the named Nebius instance and confirm the new state in Slack — stopping idle instances to save cost, starting them when a run begins. Allow start for the team's instances without approval; require approval to stop anything tagged **shared** or **production** .


/kestrel-workflow


Investigate


When a researcher asks why an instance or cluster is unhealthy or idle, run a read-only Nebius investigation across their instances, clusters, and node groups, and post a summary of what it found — including idle GPUs that could be stopped to save cost — back to the requester. Allow it for everyone with no approval gate.


This is the same pattern our[internal developer platform guide](https://usekestrel.ai/blog/internal-developer-platform) builds across the whole stack —[Kubernetes](https://docs.usekestrel.ai/integrations/kubernetes) ,[AWS](https://docs.usekestrel.ai/integrations/aws) ,[PlanetScale](https://docs.usekestrel.ai/integrations/planetscale) , and more. Here it's scoped to Nebius, but the request workflows compose: the same front door serves every integration you connect.


## How to automate CI/CD with Nebius GPU infrastructure


On Nebius, CI/CD isn't just deploying code — it's coordinating GPU capacity with the pipeline. Training runs need nodes before they start and don't need them after; model-serving rollouts need to restart inference instances without dropping traffic; and GPU test jobs shouldn't keep an instance alive a minute longer than the job runs. These three workflows wire capacity into the pipeline itself.


/kestrel-workflow


Schedule


Every night at 1am, scale our Nebius training node group up to the size the nightly run needs, then trigger the GitHub Actions workflow that launches the training job and wait for it to complete. When the run finishes — success or failure — scale the Nebius node group back down to its daytime size and post the run result and the GPU-hours consumed to **#ml-infra** in Slack.


/kestrel-workflow


Release


When a PR is merged on our inference-service repo, wait for the GitHub Actions build to complete, then roll the new model server out across our Nebius inference instances one at a time: restart the first instance, fetch its status to confirm it is back to running, and move to the next. Post the rollout progress and final result to **#deploys** in Slack. Require a Slack approval from the release owner before the first restart in production.


/kestrel-workflow


Provision


When our GPU integration-test workflow starts in GitHub Actions, create a single-GPU Nebius instance with a CUDA image for the job to run against, and post the instance ID to the workflow run. When the workflow completes, delete the instance including its boot disk and post the job result and the instance's total runtime to **#ml-infra** in Slack, so every CI run's GPU cost is visible.


## How to automate Nebius security and compliance


GPU fleets have a security problem most stacks don't: the capacity itself is the prize. A quietly created instance in a forgotten project is either shadow spend or someone else's cryptominer, and either way nobody notices until the invoice. These three workflows turn fleet hygiene into a schedule.


/kestrel-workflow


Audit


Every Monday at 9am, list the instances, Kubernetes clusters, and node groups in each of our Nebius projects, then run an investigation that compares the inventory against our approved capacity baseline — flagging instances that shouldn't exist, node groups above their budgeted size, and anything running in a project that should be empty. Post the findings to **#security** in Slack and create a Jira ticket for each new finding. Read-only.


/kestrel-workflow


Audit


Every day at 7am, list the running instances across all our Nebius projects and flag any instance in a project where no capacity is expected, or with a name that doesn't match our naming convention — the classic signs of shadow GPU spend or a compromised credential spinning up miners. Post anything suspicious to **#security** in Slack with the project, instance name, and state, and create a Jira ticket to track each one. Read-only.


/kestrel-workflow


Comply


When Nebius schedules maintenance on a node or GPU host, notify the owning team's Slack channel with the affected node and the maintenance window, and create a Jira ticket to migrate workloads off the node before the window starts. If the ticket is still open as the window approaches, page the on-call engineer through PagerDuty so the migration doesn't get missed.


## Making Nebius easier to manage


Fifteen workflows, five domains, one pattern: describe the operation in plain English, let the Workflow Agent assemble it from real Nebius blocks and the stack around them, review the graph, and put approval gates on anything that creates or destroys billable capacity. The recurring 80% of Nebius operations — the GPU error triage, the node replacements, the overnight instance stops, the capacity audits — runs itself, and your team's time goes to training and shipping models.


This isn't Nebius-only, either. The same workflow engine spans 30+ integrations across infrastructure, PaaS, observability, comms, databases, CI/CD, and IaC — so the workflow that scales a Nebius node group lives next to the one that restarts a Kubernetes deployment or resizes an RDS instance. For deeper dives, see the[internal developer platform guide](https://usekestrel.ai/blog/internal-developer-platform) and the companion posts on[automating Kubernetes operations](https://usekestrel.ai/blog/automate-kubernetes-operations) ,[automating AWS cloud operations](https://usekestrel.ai/blog/automate-aws-cloud-operations) ,[automating Vercel operations](https://usekestrel.ai/blog/automate-vercel-operations) ,[automating Railway operations](https://usekestrel.ai/blog/automate-railway-operations) , and[automating Fly.io operations](https://usekestrel.ai/blog/automate-flyio-operations) .


## FAQ: automating Nebius operations


### How do you automate Nebius operations?


Codify each recurring task — incident response, GPU provisioning, developer requests, CI/CD, and security — as a deterministic, approval-gated workflow. With Kestrel, you describe the automation in plain English, the Workflow Agent assembles it from real Nebius blocks plus the stack around them (GitHub, Slack, PagerDuty, Jira), and it runs the exact same steps every time, pausing at approval gates before anything touches billable GPU capacity.


### What is the best automation tool for Nebius?


It depends on scope. The Nebius CLI and Terraform automate provisioning as code; managed Kubernetes autoscaling handles basic capacity. For automating operations *across* the whole lifecycle in one governed system, Kestrel Workflows builds deterministic workflows from plain English, triggered by GPU errors, NotReady nodes, instance stops, and scheduled maintenance — detected by polling the Nebius API, so there's no webhook setup — with native blocks for the Nebius instance and node-group lifecycle plus GitHub, Slack, PagerDuty, and Jira.


### Is it safe to automate GPU instance creation and deletion?


Safer than doing them by hand under deadline pressure, if the automation is deterministic and gated. Kestrel workflows run fixed step sequences — no runtime improvisation — scoped to specific Nebius projects, and approval gates pause execution before any instance creation, node-group scale, or teardown until a human signs off in Slack or the dashboard. Deletion blocks remove boot disks by default so teardowns don't leave orphaned disks billing. Read-only workflows (investigations, inventory audits) can run ungated.


### How do you make Nebius easier to manage?


Reduce how much of Nebius your team touches by hand. Automate the recurring operations with workflows, give researchers plain-English self-service for scaling, restarts, and investigations, and keep humans in the loop only where judgment matters — approvals and budget caps, not execution.


## Getting started


Connect your[Nebius projects](https://docs.usekestrel.ai/integrations/nebius) — a service-account key and a poll interval is all it takes — then pick the domain that hurts most. For most ML platform teams that's either the GPU error channel or the idle-instance bill. Paste one of the prompts above into the Workflow Agent, review the workflow it builds, tighten the scoping and approvals, activate it, and add the next one. The[Workflows Quickstart](https://docs.usekestrel.ai/quickstart/workflows) walks through your first workflow end to end, and the[Create Workflows guide](https://docs.usekestrel.ai/workflows/create-workflows) and[Integrations Setup](https://docs.usekestrel.ai/workflows/setup-integrations) cover connecting[GitHub](https://docs.usekestrel.ai/integrations/github) and the rest of your stack. Prefer the terminal or an AI coding agent? Workflows can also be created and managed through the[CLI](https://docs.usekestrel.ai/workflows/cli) ,[Python SDK](https://docs.usekestrel.ai/workflows/sdk) , and[MCP](https://docs.usekestrel.ai/workflows/mcp) . Within a few weeks you'll have a governed automation layer over your GPU fleet that no amount of console access can match.


### Automate your Nebius operations with $1,000 in credits


GPU incident response, provisioning, developer requests, CI/CD, and security — describe each workflow in plain English and Kestrel builds it. New accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
