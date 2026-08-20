---
schema_version: "1.0.0"
document_id: "7f07bbb675ba84f9434f74796030bb1e70c2d004d6f1db80aecde4ba52140742"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/internal-developer-platform"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T10:40:48.612135+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:f48ab6744bc241d49d16d6c8b9dfd2a610b2e52ce6648b98f7869a2d635a350d"
---

# Build an Internal Developer Platform with Kestrel Workflows

Every platform team has the same quiet backlog. A developer needs a new namespace, a bigger RDS instance, an ephemeral database branch for a pull request, or a production rollback — and the request lands in a Slack thread, a ticket, or a tap on the shoulder. Multiply that across an engineering org and your platform team spends its week as a human API in front of` kubectl` , the AWS console, and a dozen dashboards.


The usual fix is to build an **internal developer platform** (IDP): a self-service layer where developers get golden paths and the platform team keeps the guardrails. But building a real IDP is normally a quarters-long project — you're writing a portal, wiring it to every backend, bolting on RBAC and approvals, and then maintaining all of it forever.


With[Kestrel Workflows](https://usekestrel.ai/blog/introducing-kestrel-workflows) and[Developer Request Workflows](https://usekestrel.ai/changelog/developer-request-workflows) , you already have one. Developers ask for what they need in plain English — from Slack, the CLI, or the in-platform Developer Request Agent — and Kestrel routes each request to a governed, approval-gated workflow you defined. The rest of this post shows how that spans your *whole* stack, with copy-paste prompts you can hand straight to the Workflow Agent to build the workflows behind it.


## How it works: requests, routed to workflows


Under the hood it's the same deterministic workflow engine that powers Kestrel's incident response and provisioning automations. To make a workflow requestable, you give it a **developer request** trigger. When a request comes in, Kestrel matches it to your active request-triggered workflows by intent, extracts the parameters from the phrasing, and runs it — pausing for any approval gates you've defined. The AI builds and matches; the workflow itself runs deterministically, the exact steps every time.


Requests meet developers where they already work, through any of three channels:


- Slack


— type` /kestrel-workflow` followed by your request in any channel, or just mention the app. Kestrel replies in-thread, asks for anything missing, and surfaces approvals right there.
- CLI


—` kestrel workflows request "scale the payments deployment to 5 replicas"` , the terminal equivalent, ideal for scripts and CI/CD.
- In-platform Developer Request Agent


— a conversational chat in the Kestrel dashboard for developers who'd rather click than type a slash command.


Click to expand


In the dashboard, the **Developer Requests** page gives every developer a focused, conversational way to ask for what they need. Answers stream back conversationally, each request keeps its own history in a sidebar, and once a request is matched you get a status badge — **Matched** , **Executing** , **Completed** — and a link to the full execution.


## Governed by default


This is what makes it safe to open up self-service to the whole org. Every request workflow runs inside Kestrel's[access control and approval](https://usekestrel.ai/changelog/workflow-access-control-approvals) model. On the request trigger you scope a workflow with **Allowed Users** and **Allowed Groups** , fence it to specific **clusters, namespaces, cloud accounts, zones, or repositories** , and drop **approval gates** anywhere in the graph so nothing touches production until the right person signs off — in the dashboard, in Slack, or by merging a pull request.


Building, activating, and approving these workflows stays with your platform administrators, while developers get a focused view for submitting requests. So a self-service workflow can be opened up to a whole team while staying fenced into the exact resources they're permitted to touch. Developers get a golden path; you keep the guardrails.


## A golden path across the whole stack


The point of building your IDP on workflows is that a single request interface reaches every layer of your platform — the same` /kestrel-workflow` command provisions compute, resizes cloud infrastructure, spins up a database branch, controls a production deploy, and manages your edge, monitoring, and AI compute. Below are ready-to-use prompts across representative layers — **Kubernetes** (compute), **AWS** (cloud infrastructure), **Cloudflare** (edge & DNS), **PlanetScale** (databases), **Vercel** (deployments), **GitHub** (CI/CD), **Datadog** (observability), **Nebius** (AI compute), and **Daytona** (sandboxes). Each prompt is written the way you'd type it into the Workflow Agent to build the workflow — trigger, steps, scoping, and approval gate included. Paste one in, review the graph it generates, add a developer request trigger, and activate it.


## Kubernetes: self-service compute


Kubernetes is where most "can you just" requests land — a namespace, a scale-up, a ConfigMap, or a quick answer about why a pod is unhealthy. These map cleanly onto the request types Kestrel supports: **Create / Provision** , **Edit / Update / Scale** , **Delete / Remove** , and **Get / Investigate / Debug** . Build each as a request workflow scoped to the namespaces a team is allowed to touch, gated on production.


/kestrel-workflow


Provision


When a developer requests a new namespace, create the namespace in the requested cluster with the standard baseline — a ResourceQuota, a LimitRange, and the team's RBAC RoleBinding — label it with the requesting team and cost center, then post a confirmation with the namespace name to the requester in Slack. Only allow the **dev** and **staging** clusters; require platform-admin approval for any request that targets **production** .


/kestrel-workflow


Scale


When a developer asks to scale a deployment, set the replica count on the named deployment in the namespace they specify, wait for the rollout to finish, and reply in Slack with the new ready replica count. Restrict this to the namespaces owned by the requester's team, cap the replica count at 10, and require approval before scaling anything in the **production** namespace.


/kestrel-workflow


Investigate


When a developer asks why a workload is unhealthy, run a root cause analysis on the named deployment or pod in the given namespace — pull recent events, container restart reasons, and logs — and post a plain-English summary with the likely cause and a suggested fix back to the requester. This is read-only, so allow it for all developers across every non-production namespace without an approval gate.


Click to expand


## AWS: self-service cloud infrastructure


Cloud infrastructure requests are the ones that most often get stuck waiting on the platform team, because they carry real cost and blast radius. That's exactly what approval gates and account scoping are for: let developers ask for what they need, run it deterministically, and require a sign-off before anything expensive or production-facing is created.


/kestrel-workflow


Provision


When a developer requests an S3 bucket, create it in the requested AWS account and region with encryption at rest, versioning, and public access fully blocked by default, apply the team's standard tags, and return the bucket name and ARN to the requester in Slack. Restrict this to the **dev** and **sandbox** accounts; require platform-admin approval for the **production** account.


/kestrel-workflow


Scale


When a developer asks to resize an RDS instance, look up the current instance class and storage for the named database, then post the requested change to a Slack approval gate showing the before and after. On approval, apply the modification during the maintenance window and reply with the new configuration. Fence this to the accounts the requester's team owns and always require approval.


/kestrel-workflow


Investigate


When a developer asks why their AWS costs spiked, investigate cost and usage for the requested account over the last 30 days, break down the increase by service and tag, identify the top drivers, and post a summary with the likely cause and cost-saving recommendations back to the requester. Read-only, so allow it for all developers on the accounts they own with no approval gate.


## Cloudflare: self-service edge & DNS


"Add a DNS record in Cloudflare" is the canonical developer request — small, frequent, and almost always blocked on whoever holds the Cloudflare login. The Slack demo below is exactly that flow. Kestrel's Cloudflare blocks (Create / Update / Delete DNS Record, Block IP Address, Deploy / Rollback Worker, Purge Cache, and WAF and rate-limit rules) turn edge and DNS changes into governed, self-service requests, with the riskier security actions gated behind approvals.


/kestrel-workflow


Create


When a developer requests a DNS record, create the record in the requested Cloudflare zone with the type, name, and target they specify, set proxying to their preference, and reply in Slack with the created record and its ID. Allow this for the team's non-production zones; require platform-admin approval for records in a **production** zone.


/kestrel-workflow


Update


When a developer asks to block an IP address, add the requested IP to the Cloudflare zone firewall access rules with a note describing why, then confirm the block in Slack. Require approval from the on-call security reviewer before the block is applied, and scope it to the zones the requester's team owns.


/kestrel-workflow


Deploy


When a developer requests a Worker deploy, deploy or update the named Cloudflare Worker script, confirm it went live, and post the result to the requester in Slack. If the deploy causes errors, roll the Worker back to the previous version. Require approval before deploying to a **production** Worker.


Click to expand


## PlanetScale: self-service databases


Databases are where self-service pays off the most, because the alternative — a developer with production database credentials, or a ticket that blocks a PR for a day — is exactly what you want to avoid. Kestrel's PlanetScale blocks (Create Branch, Create Backup, Create/Deploy Deploy Request, Promote Branch to Production, and a read-only Investigate) turn common database asks into governed, one-line requests.


/kestrel-workflow


Create


When a developer requests a database branch for a pull request, create a new PlanetScale branch off the main branch of the named database, associate it with their git branch, and reply in Slack with the branch name and connection details so they can point their preview environment at it. Allow this for all developers, no approval needed — these are ephemeral preview branches.


/kestrel-workflow


Provision


When a developer asks for a backup before a risky migration, create a backup of the named PlanetScale branch, wait until it reports complete, and post the backup name and timestamp back to the requester so they have a known-good restore point. Allow it for all developers with no approval gate.


/kestrel-workflow


Deploy


When a developer requests a schema change, open a deploy request from their branch into the target branch of the named database, then send the deploy request's diff and state to a Slack approval gate for the on-call DBA. On approval, deploy the deploy request and reply with the result; on rejection, close it. Always require approval, and only allow deploys into **production** databases for the platform-admin group.


/kestrel-workflow


Investigate


When a developer asks about the state of a database, branch, deploy request, or recent backups, run a read-only PlanetScale investigation to answer the question and post the findings back to the requester. Allow it for everyone with no approval gate.


## Vercel: self-service deployments


Frontend and full-stack teams live in their deployment platform, and the requests there are time-sensitive — a bad deploy needs a rollback *now* , and promoting a preview to production shouldn't require pinging the one person with dashboard access. Kestrel's Vercel blocks (Rollback Production, Promote to Production, Get Deployment, Get Build Logs, and Investigate) make those safe to hand to the whole team behind an approval gate.


/kestrel-workflow


Rollback


When a developer requests a production rollback, roll production back to the previous deployment for the named Vercel project, confirm the rollback succeeded, and post the now-live deployment URL to the **#deploys** channel and the requester in Slack. Require approval from the on-call lead before rolling back, and restrict this to the projects the requester's team owns.


/kestrel-workflow


Deploy


When a developer asks to promote a deployment to production, look up the requested preview deployment for the named project, send its URL and git metadata to a Slack approval gate, and on approval promote it to production and reply with the result. Always require approval, scoped to the team's projects.


/kestrel-workflow


Investigate


When a developer asks why a deploy failed, fetch the deployment details and build logs for the named Vercel project, analyze the failure, and post a plain-English explanation of the root cause with a suggested fix back to the requester. Read-only, so allow it for all developers with no approval gate.


## GitHub: self-service CI/CD


CI/CD is the request the whole org makes — "run the deploy pipeline," "re-run the failed job," "cut a release" — and it usually routes through whoever has the right Actions permissions. Kestrel's GitHub blocks (Trigger GitHub Action, Wait for Action Run, Get Run Status, Create Pull Request, Create Issue, and Investigate Action Failure) let developers drive pipelines themselves without handing out repo write access, with the deploy paths gated behind approvals.


/kestrel-workflow


Deploy


When a developer asks to run a deploy, dispatch the named GitHub Actions workflow on the branch they specify with any inputs they provide, wait for the run to finish, and reply in Slack with the conclusion and a link to the run. Restrict this to the repositories the requester's team owns, and require platform-admin approval before dispatching any workflow that targets **production** .


/kestrel-workflow


Update


When a developer asks to re-run a failed CI job, look up the most recent run of the named workflow for their branch, re-dispatch it, wait for it to complete, and post the new conclusion back to the requester. Allow this for all developers on their team's repositories with no approval gate — it's re-running their own pipeline.


/kestrel-workflow


Create


When a developer requests a release, open a pull request from the release branch into` main` for the named repository with a generated changelog in the description, then send it to a Slack approval gate for the release manager. On approval, trigger the release GitHub Action and reply with the run link; on rejection, close the PR. Always require approval.


/kestrel-workflow


Investigate


When a developer asks why a CI run failed, investigate the failed GitHub Actions run for the named workflow — read the logs, identify the failing step and likely cause — and post a plain-English explanation with a suggested fix back to the requester. Read-only, so allow it for everyone with no approval gate.


## Datadog: self-service observability


Observability requests pile up too — a developer wants a monitor on their new service, or a noisy alert muted during a deploy, and they shouldn't need Datadog admin access to get either. Kestrel's Datadog blocks (Query Metrics, Create Monitor, Mute Monitor, and Send Event) make monitoring self-service without opening up your Datadog org.


/kestrel-workflow


Create


When a developer requests a monitor for their service, create a Datadog monitor on the metric and threshold they describe, tag it with the requesting team and service, route its notifications to the team's Slack channel, and reply with a link to the new monitor. Allow this for all developers on the services their team owns.


/kestrel-workflow


Update


When a developer asks to mute a monitor during a deploy or maintenance window, mute the named Datadog monitor for the duration they specify, send a Datadog event recording who muted it and why, and confirm in Slack — noting the automatic un-mute time. Cap the mute duration at 4 hours and require approval to mute a **production** monitor.


/kestrel-workflow


Investigate


When a developer asks about a metric or a spike, query the relevant Datadog metrics over the window they describe, summarize the trend and any threshold breaches, and post the findings back to the requester. Read-only, so allow it for everyone with no approval gate.


## Nebius: self-service AI compute


For teams training and serving models, GPU compute is the request that carries the most cost, so it's the one you most want to make self-service *and* governed. Kestrel's Nebius blocks (Start / Stop / Restart Instance, List Instances / Clusters / Node Groups, Scale Node Group, and Investigate) let researchers manage their own GPU capacity behind approval gates and cost guardrails.


/kestrel-workflow


Provision


When a researcher asks to scale up a GPU node group for a training run, scale the named Nebius managed-Kubernetes node group to the requested size, wait for the new nodes to become ready, and reply in Slack with the node count and the estimated hourly cost. Require platform-admin approval before scaling, and cap the node group at the size the requester's team is budgeted for.


/kestrel-workflow


Scale


When a researcher asks to stop or start a GPU instance, stop or start the named Nebius instance and confirm the new state in Slack — stopping idle instances to save cost, starting them when a run begins. Allow start for the team's instances without approval; require approval to stop anything tagged **shared** or **production** .


/kestrel-workflow


Investigate


When a researcher asks why an instance or cluster is unhealthy or idle, run a read-only Nebius investigation across their instances, clusters, and node groups, and post a summary of what it found — including idle GPUs that could be stopped to save cost — back to the requester. Allow it for everyone with no approval gate.


## Daytona: self-service sandboxes


Development sandboxes are the perfect self-service primitive — short-lived, isolated, and exactly the kind of thing a developer shouldn't have to file a ticket for. Kestrel's Daytona blocks (Create Snapshot, Start / Stop / Archive / Delete Sandbox, Run Command in Sandbox, Set Auto-Stop Interval, and Investigate) make spinning up and tearing down sandboxes a one-line request.


/kestrel-workflow


Provision


When a developer requests a sandbox, start a Daytona sandbox from the requested snapshot, set an auto-stop interval of 60 minutes of inactivity so it doesn't run up cost, and reply in Slack with the sandbox ID and how to connect. Allow this for all developers with no approval gate.


/kestrel-workflow


Delete


When a developer is done with a sandbox, stop it to preserve its state, or delete it entirely if they ask to tear it down, and confirm in Slack. Allow stop and archive for everyone; require approval before permanently deleting a sandbox so nobody loses work by accident.


/kestrel-workflow


Investigate


When a developer asks about their sandboxes, snapshots, or volumes — what's running, what's costing money, or why one won't start — run a read-only Daytona investigation and post the answer back to the requester. Allow it for everyone with no approval gate.


That spans compute, cloud, databases, deploys, CI/CD, the edge, observability, AI compute, and sandboxes — but the same pattern applies to every integration Kestrel connects. **Supabase** gives you Create Branch, Create Restore Point, and Restore Backup for self-service Postgres; **Railway** gives you Redeploy, Restart, Rollback, and Set Variables for self-service services; and **ArgoCD** , **Helm** , **GitLab** , and **PagerDuty** round out the rest of the stack. If a service isn't built in yet, a[custom action block](https://usekestrel.ai/changelog/custom-integrations) makes it requestable too.


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


## Nothing falls through the cracks


An IDP is only as good as its coverage, and no catalog is ever complete. When a request doesn't match any existing workflow, the developer is told their platform team has been notified — and the request shows up on the admin **Workflow Requests** page, complete with an AI-suggested workflow to fulfill it. From there your team can **Review & Deploy** the suggestion, **Create** a new workflow, or **Reject** the request. Every unmet request becomes a prompt to expand your platform's golden paths, so your IDP grows to fit how your org actually works instead of what you guessed up front.


## Getting started


Pick one of the prompts above, open the Workflow Agent, and paste it in. Review the graph it builds, add a **developer request** trigger, scope it with **Allowed Users** and **Allowed Groups** , and activate it. Your team can then make requests from Slack with` /kestrel-workflow` , from the terminal with` kestrel workflows request` , or from the **Developer Requests** page in the dashboard. Start with a couple of read-only investigate workflows to build trust, then add the create, scale, and deploy paths behind approval gates. See the[Create Workflows guide](https://docs.usekestrel.ai/workflows/create-workflows) and[Integrations Setup](https://docs.usekestrel.ai/workflows/setup-integrations) to connect your stack.


### Start building your IDP with $1,000 in credits


Turn your platform into a self-service golden path across Kubernetes, AWS, databases, and deployments. Describe each request workflow in plain English and Kestrel builds it — new accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
