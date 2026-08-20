---
schema_version: "1.0.0"
document_id: "5ba584983d258677469d3adea911ad439cb7838f587b0594440489d5b39c110a"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/automate-daytona-operations"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:9de121040f271c00100e73691498fedbccf67e63594499d05cc0d6f38cd7b6c3"
---

# How to Automate Daytona Operations with Kestrel Workflows: Incident Response, Sandbox Provisioning, CI/CD, and Security

Every team running sandboxes at scale on Daytona carries the same operational load. A sandbox enters an error state and someone spends an hour working out whether it's the image, the volume, or the code that ran in it. A snapshot build fails and the team that depends on it finds out at standup. Stopped sandboxes pile up holding compute. And the sandbox sprawl from three sprints of AI-agent experiments is now a line item nobody can explain.


None of this is a Daytona problem — it's an automation problem. Each of those tasks is a known, repeatable sequence of steps that happens to be executed by a human with dashboard access. The way to make Daytona easier to manage is not more dashboard access; it's codifying those sequences as workflows that run the same way every time, with a human approving anything destructive.


This post shows how to automate the five domains of Daytona operations with[Kestrel Workflows](https://usekestrel.ai/blog/introducing-kestrel-workflows) : **incident response** , **sandbox provisioning** , **developer requests** , **CI/CD** , and **security** . Each section includes three prompts written the way you'd hand them to the Workflow Agent — paste one in, review the workflow it builds, and activate it.


## How it works: plain English in, deterministic workflows out


You describe the automation in plain English and the Workflow Agent assembles it from real building blocks —[Daytona](https://docs.usekestrel.ai/integrations/daytona) 's sandbox operations (Create, Start, Stop, Archive, and Delete Sandbox, Run Command in Sandbox, Set Auto-Stop Interval, snapshot and volume lifecycle, and AI-powered Investigate Daytona) plus the stack around it:[GitHub](https://docs.usekestrel.ai/integrations/github) pull requests and Actions, Slack, PagerDuty, and Jira. The AI builds the workflow once; **the workflow itself runs deterministically** — the exact same steps on every execution, not an agent improvising at runtime against your sandbox fleet.


Daytona delivers sandbox, snapshot, and volume lifecycle events to Kestrel through signed webhooks — every delivery is verified against the webhook's signing secret before it can fire a workflow, and triggers fire the moment something changes. Workflows start from the trigger that fits the job: Sandbox Created, Sandbox Stopped, Sandbox Error, Sandbox Execution Failed, Snapshot Build Failed, Volume Error, CI events, schedules, or plain-English developer requests. And every workflow runs inside Kestrel's[access control and approval](https://usekestrel.ai/changelog/workflow-access-control-approvals) model — restrict who can invoke it and drop approval gates anywhere in the graph so nothing destructive happens until the right person signs off in the dashboard, in Slack, or by merging a PR.


The Workflow Agent building a production-ready workflow from a plain-English description.


## How to automate incident response in Daytona


Daytona incidents come in three shapes: the sandbox that enters an error state, the snapshot build that fails and blocks everyone provisioning from it, and the volume error that quietly breaks every sandbox mounting shared data. All three follow the same split — the investigation is safe to automate fully, and the remediation gets an approval gate.


/kestrel-workflow


Investigate & Fix


When a Daytona sandbox enters an error state, fetch the sandbox's state and error reason, run an investigation to determine the root cause, and post the analysis to **#platform** in Slack. Send a sandbox restart to a Slack approval gate; on approval, start the sandbox and fetch its state to confirm it is back to started. If it enters an error state again, create a Jira ticket with the investigation attached and page the on-call engineer through PagerDuty.


/kestrel-workflow


Triage


When a Daytona snapshot build fails, investigate the failure — which Docker image the snapshot was building from and what went wrong — and post a plain-English explanation to **#platform** in Slack with the snapshot name and state. Create a Jira ticket for the team that owns the image so the broken build doesn't block sandbox provisioning for everyone else. Read-only, no approval gate.


/kestrel-workflow


Respond


When a Daytona volume enters an error state, fetch the volume's state, run an investigation to work out which sandboxes depend on it, and post the findings to **#platform** in Slack. If the volume backs shared team datasets, page the on-call engineer through PagerDuty with the investigation attached; otherwise create a Jira ticket for the owning team to recreate it.


## How to automate sandbox provisioning in Daytona


Provisioning on Daytona is really three jobs: publishing the golden snapshots teams provision from, creating the shared volumes that carry datasets across sandboxes, and reclaiming the compute that stopped sandboxes are still holding. Publishing and volume creation go behind approval gates; reclaim goes on a schedule.


/kestrel-workflow


Publish


When a team requests a new golden snapshot, send the request — snapshot name and the pinned Docker image it builds from — to a Slack approval gate for the platform team. On approval, create the snapshot, verify it by creating a test sandbox from it and running a smoke-test command inside, then delete the test sandbox and announce the new snapshot in **#platform** in Slack. If the smoke test fails, post the output instead and don't announce.


/kestrel-workflow


Provision


When a team requests a shared volume for datasets or build caches, send the request to a Slack approval gate for the platform team. On approval, create the Daytona volume, confirm it reaches the ready state, and reply to the requester with the volume ID and how to mount it in their sandboxes.


/kestrel-workflow


Schedule


Every night at 2am, list the Daytona sandboxes that have been stopped and archive them so their compute is freed while their disk state is retained. Post a morning summary to **#platform** in Slack — how many sandboxes were archived and roughly how much compute was reclaimed. No approval gate; archiving is reversible.


## The best way to automate developer requests in Daytona


Development sandboxes are the perfect self-service primitive — short-lived, isolated, and exactly the kind of thing a developer shouldn't have to file a ticket for. Developers ask in Slack with` /kestrel-workflow` , from the[CLI](https://docs.usekestrel.ai/workflows/cli) , or on the Developer Requests page; Kestrel matches the request to an active request-triggered workflow, extracts the parameters from the phrasing, and runs it with cost controls applied automatically and approval gates on anything permanent.


/kestrel-workflow


Provision


When a developer requests a sandbox, start a Daytona sandbox from the requested snapshot, set an auto-stop interval of 60 minutes of inactivity so it doesn't run up cost, and reply in Slack with the sandbox ID and how to connect. Allow this for all developers with no approval gate.


/kestrel-workflow


Delete


When a developer is done with a sandbox, stop it to preserve its state, or delete it entirely if they ask to tear it down, and confirm in Slack. Allow stop and archive for everyone; require approval before permanently deleting a sandbox so nobody loses work by accident.


/kestrel-workflow


Investigate


When a developer asks about their sandboxes, snapshots, or volumes — what's running, what's costing money, or why one won't start — run a read-only Daytona investigation and post the answer back to the requester. Allow it for everyone with no approval gate.


This is the same pattern our[internal developer platform guide](https://usekestrel.ai/blog/internal-developer-platform) builds across the whole stack —[Kubernetes](https://docs.usekestrel.ai/integrations/kubernetes) ,[AWS](https://docs.usekestrel.ai/integrations/aws) ,[PlanetScale](https://docs.usekestrel.ai/integrations/planetscale) , and more. Here it's scoped to Daytona, but the request workflows compose: the same front door serves every integration you connect.


## How to automate CI/CD with Daytona sandboxes


Daytona's best CI/CD trick is the ephemeral sandbox: a clean-room runner that exists for exactly one job and deletes itself when it stops. That turns three pipeline problems — polluted test environments, post-deploy verification, and diagnosing failed commands — into three workflows.


/kestrel-workflow


Verify


When a pull request is opened on our main repo, create an ephemeral Daytona sandbox from our CI snapshot, run the test suite inside it, and post the results — pass or fail with the failing output — as a comment on the pull request and to **#deploys** in Slack. The sandbox is ephemeral, so it deletes itself once stopped. No approval gate.


/kestrel-workflow


Verify


When our deploy workflow completes in GitHub Actions, create an ephemeral Daytona sandbox, run our smoke-test script against the staging URL inside it, and post the pass/fail result to **#deploys** in Slack. If the smoke tests fail, page the on-call engineer through PagerDuty with the test output attached.


/kestrel-workflow


Diagnose


When a command run in a Daytona sandbox by one of our workflows exits non-zero, fetch the sandbox's state, investigate the failure using the command output and exit code, and post the command, the exit code, and a plain-English analysis to the owning team's Slack channel. Create a Jira ticket if the same command has failed more than once this week. Read-only, no approval gate.


## How to automate Daytona security and compliance


Sandbox fleets accumulate risk quietly: the sandbox running with no auto-stop that nobody remembers starting, the snapshot from an image nobody maintains, and the archived resources from experiments long finished. These three workflows turn fleet hygiene into a schedule.


/kestrel-workflow


Enforce


Every day at 9am, list the running Daytona sandboxes and set a 60-minute auto-stop interval on any sandbox that has auto-stop disabled, so no sandbox can idle indefinitely. Post a summary to **#security** in Slack listing the sandboxes that were brought into compliance. No approval gate; this only tightens limits.


/kestrel-workflow


Audit


Every Monday at 9am, list our Daytona sandboxes, snapshots, and volumes, then run an investigation that flags anomalies — sandboxes with names that don't match our naming convention, sandboxes that have been running continuously for more than a week, snapshots or volumes in an error state, and anything that looks like it doesn't belong. Post the findings to **#security** in Slack and create a Jira ticket for each new finding. Read-only.


/kestrel-workflow


Clean up


Every Sunday at 8pm, list our archived Daytona sandboxes and our snapshots, flag the archived sandboxes and unused snapshots older than our retention policy, and send the teardown list to a Slack approval gate for the platform lead. On approval, delete the flagged sandboxes and snapshots, post the completed cleanup summary to **#security** in Slack, and log the deletions in Jira.


## Making Daytona easier to manage


Fifteen workflows, five domains, one pattern: describe the operation in plain English, let the Workflow Agent assemble it from real Daytona blocks and the stack around them, review the graph, and put approval gates on anything destructive. The recurring 80% of Daytona operations — the error triage, the snapshot publishing, the compute reclaim, the fleet audits — runs itself, and your team's time goes to shipping.


This isn't Daytona-only, either. The same workflow engine spans 30+ integrations across infrastructure, PaaS, observability, comms, databases, CI/CD, and IaC — so the workflow that archives idle sandboxes lives next to the one that scales a Nebius GPU node group or restarts a Kubernetes deployment. For deeper dives, see the[internal developer platform guide](https://usekestrel.ai/blog/internal-developer-platform) and the companion posts on[automating Kubernetes operations](https://usekestrel.ai/blog/automate-kubernetes-operations) ,[automating AWS cloud operations](https://usekestrel.ai/blog/automate-aws-cloud-operations) ,[automating Vercel operations](https://usekestrel.ai/blog/automate-vercel-operations) ,[automating Railway operations](https://usekestrel.ai/blog/automate-railway-operations) ,[automating Fly.io operations](https://usekestrel.ai/blog/automate-flyio-operations) , and[automating Nebius operations](https://usekestrel.ai/blog/automate-nebius-operations) .


## FAQ: automating Daytona operations


### How do you automate Daytona operations?


Codify each recurring task — incident response, sandbox provisioning, developer requests, CI/CD, and security — as a deterministic, approval-gated workflow. With Kestrel, you describe the automation in plain English, the Workflow Agent assembles it from real Daytona blocks plus the stack around them (GitHub, Slack, PagerDuty, Jira), and it runs the exact same steps every time, pausing at approval gates before anything destructive.


### What is the best automation tool for Daytona?


It depends on scope. The Daytona SDK and CLI automate sandbox operations from code; auto-stop intervals handle basic cost control. For automating operations *across* the whole lifecycle in one governed system, Kestrel Workflows builds deterministic workflows from plain English, triggered by sandbox errors, failed snapshot builds, volume errors, and failed commands — delivered as signed webhooks from Daytona, so triggers fire the moment something changes — with native blocks for the full sandbox, snapshot, and volume lifecycle plus GitHub, Slack, PagerDuty, and Jira.


### Is it safe to automate sandbox deletion and cleanup?


Safer than letting stale resources pile up, if the automation is deterministic and gated. Kestrel workflows run fixed step sequences — no runtime improvisation — and approval gates pause execution before any permanent deletion until a human signs off in Slack or the dashboard. Reversible operations (stop, archive, tightening auto-stop) can run ungated, and read-only workflows (error triage, fleet audits) always can.


### How do you make Daytona easier to manage?


Reduce how much of Daytona your team touches by hand. Automate the recurring operations with workflows, give developers plain-English self-service for sandboxes with cost controls applied automatically, and keep humans in the loop only where judgment matters — approvals on deletion, not execution.


## Getting started


Connect your[Daytona organization](https://docs.usekestrel.ai/integrations/daytona) — an API key and a webhook signing secret is all it takes — then pick the domain that hurts most. For most teams that's either the sandbox error channel or the idle-compute bill. Paste one of the prompts above into the Workflow Agent, review the workflow it builds, tighten the scoping and approvals, activate it, and add the next one. The[Workflows Quickstart](https://docs.usekestrel.ai/quickstart/workflows) walks through your first workflow end to end, and the[Create Workflows guide](https://docs.usekestrel.ai/workflows/create-workflows) and[Integrations Setup](https://docs.usekestrel.ai/workflows/setup-integrations) cover connecting[GitHub](https://docs.usekestrel.ai/integrations/github) and the rest of your stack. Prefer the terminal or an AI coding agent? Workflows can also be created and managed through the[CLI](https://docs.usekestrel.ai/workflows/cli) ,[Python SDK](https://docs.usekestrel.ai/workflows/sdk) , and[MCP](https://docs.usekestrel.ai/workflows/mcp) . Within a few weeks you'll have a governed automation layer over your sandbox fleet that no amount of dashboard access can match.


### Automate your Daytona operations with $1,000 in credits


Incident response, sandbox provisioning, developer requests, CI/CD, and security — describe each workflow in plain English and Kestrel builds it. New accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
