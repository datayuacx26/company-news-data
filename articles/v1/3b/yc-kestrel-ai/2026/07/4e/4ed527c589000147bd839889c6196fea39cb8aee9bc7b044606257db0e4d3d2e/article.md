---
schema_version: "1.0.0"
document_id: "4ed527c589000147bd839889c6196fea39cb8aee9bc7b044606257db0e4d3d2e"
company_key: "yc-kestrel-ai"
company: "Kestrel AI"
source_id: "yc-kestrel-ai-news-import-7c48f7782d06"
canonical_url: "https://usekestrel.ai/blog/build-an-ai-dba"
published_at: "2026-07-11T00:00:00+00:00"
first_seen_at: "2026-08-08T06:43:08.152106+00:00"
fetched_at: "2026-08-08T06:43:09.174743+00:00"
content_hash: "sha256:ed46e856507fa9453d00bb67a448302c43b59e6668d92e094ba29b1043595dca"
---

# How to Build an AI DBA with Kestrel Workflows

"AI DBA" has become a crowded label. Oracle and IBM are shipping database engines that patch, tune, and increasingly heal themselves. A generation of advisors will tell you which index to add and which autovacuum setting is wrong. Cloud providers apply machine learning to their own telemetry and hand you a recommendation. All of it is genuinely useful, and none of it is what most teams actually need — because the database work that consumes a team's time isn't usually query tuning. It's the failed backup nobody noticed for three days. The preview branch still billing two weeks after its pull request merged. The schema migration waiting on someone to be awake. The staging database open to the internet since the prototype phase. The credential that should have been rotated last quarter.


That work is repetitive, it spans several databases and several vendors, and almost all of it already has a runbook. Which makes it a workflow problem more than a modeling problem. This post shows how to build an AI DBA out of[Kestrel Workflows](https://usekestrel.ai/blog/introducing-kestrel-workflows) — what the job actually covers, why the existing products stop short of it, and how to assemble the parts you want automated while keeping everything else firmly in human hands.


## What an AI DBA actually has to do


Before evaluating any tool, it's worth writing down the job. Across teams running Postgres, MySQL, ClickHouse, and a lakehouse, the recurring database work falls into seven areas:


- Incident response.


A service degrades, a replica falls behind, a job run fails, a query error rate spikes. Someone has to find out why and decide what to do.
- Provisioning.


New databases, branches, read replicas, and analytics services — each of which should start life with the right network posture and the right owner, not inherit a default.
- Developer self-service.


Preview databases, restore points, credentials, backfills, ad-hoc compute. The alternative is either a ticket that blocks a pull request for a day or a developer with the production dashboard password.
- Schema migrations and CI/CD.


The change that has to run against the parent database, the failed migration that has to be rolled back, the preview database that should exist for the life of a pull request and no longer.
- Security and access.


Credential rotation, network restriction audits, access reviews, offboarding, and the break-glass lockdown you hope never to use.
- Backups and disaster recovery.


Verifying that backups actually ran, that point-in-time recovery is enabled where it matters, and executing the restore when it's needed.
- Cost and lifecycle.


Idle compute, forgotten branches, oversized services, quota thresholds — the slow leak that shows up on an invoice rather than a dashboard.


Notice how little of that is query optimization, and how much of it is coordination: gather context, decide, act, tell someone, leave a record. That shape is what makes it automatable — and what determines which kind of AI DBA is actually useful to you.


## Why existing AI DBA products stop short


The market splits three ways, and each group hits a different ceiling. **Self-driving database engines** — Oracle Autonomous AI Database, IBM Db2 Genius Hub — are the most genuinely autonomous things in the category, handling patching, indexing, plan management, and increasingly approval-gated remediation. Their ceiling is scope: they automate their own engine and nothing else. If your data lives in Supabase, PlanetScale, ClickHouse, and Databricks, a self-driving Db2 does not help.


**Tuning and observability advisors** — pganalyze, DBtune, Metis, EverSQL, Datadog Database Monitoring — are excellent at the diagnosis half and honest about stopping there. Their ceiling is the last mile: they identify the missing index, the misconfigured autovacuum, the regressed plan, and then a human applies it. That is the right boundary for a tuning tool, but it means your team's time is still spent executing rather than deciding.


**Cloud-native ML monitors** — AWS DevOps Guru for RDS and its equivalents — sit on top of a provider's own telemetry and cost almost nothing to turn on. Their ceiling is both scope and action: single-cloud, single-service, advisory. We compare all of them in detail in[the 10 best AI DBA tools in 2026](https://usekestrel.ai/blog/best-ai-dba-tools) .


The pattern across all three is the same one we described in[AIOps vs AI agents vs workflow automation](https://usekestrel.ai/blog/aiops-vs-ai-agents-vs-workflow-automation) : the diagnosis has gotten dramatically better and the execution hasn't moved. And there's a second, subtler problem. Each of these products decides for you which operations are automated and how much autonomy the AI gets. You pick an autonomy mode from a list. What you cannot do is say “automate our backup verification completely, gate every restore behind two approvals from the data team, never let anything touch the payments database without a ticket, and leave query tuning entirely to us.” That sentence describes most teams' actual policy, and it's the thing a fixed product can't express.


## How it works: plain English in, deterministic workflows out


Kestrel inverts the model. Instead of shipping an AI DBA with fixed behavior, it gives you the pieces and lets the AI assemble them from a description. You write what you want in plain English — “when a Supabase project's backup fails, investigate the project's backup history and health, page the on-call engineer through PagerDuty with the findings attached, and post the failure and the investigation summary to **#databases** ” — and the Workflow Agent builds a reviewable graph out of real database blocks and the stack around them: Slack, PagerDuty, Jira, GitHub pull requests and Actions.


The critical property is where the AI sits. **The AI builds the workflow once; the workflow itself runs deterministically** — the exact same steps on every execution, not an agent improvising at runtime against your production databases. At runtime the AI does exactly one job: read-only investigation. Every database integration ships an Investigate block that queries the provider's API to answer a question or perform root-cause analysis, and in the Databricks case that read-only guarantee is enforced by the client rather than by a prompt — it rejects any request that isn't a GET, and every mutation is a POST. Everything that writes is an explicit, pre-reviewed step in a graph your team approved.


The Workflow Agent building a production-ready workflow from a plain-English description.


## The part that matters: you decide what gets automated


This is the real argument for building an AI DBA rather than buying one. A DBA's job is not one thing with one risk profile — it's dozens of operations spanning completely different blast radii. Listing branches is harmless. Restoring a production database to a point in time is not. Any product that applies a single autonomy setting across both is either too cautious to be useful or too aggressive to trust.


In Kestrel, autonomy is a per-workflow property, and it moves independently. Teams climb a trust ladder one workflow at a time:


- Rung 1 — read-only.


The workflow gathers context and reports. Investigations, backup verification sweeps, branch inventories, network restriction audits, freshness checks. These are safe to run ungated from day one, and they're where every team should start, because they also let you evaluate the diagnosis quality before anything writes.
- Rung 2 — gated writes.


The workflow investigates automatically and pauses at an approval gate before acting. Schema migration merges, point-in-time restores, replica replacement, network lockdowns, stale-branch deletion. The context arrives with the request, so the human is making a decision rather than doing an investigation.
- Rung 3 — full automation for reversible actions.


Remove the gate for operations whose blast radius you understand and whose effect you can undo: resetting a preview branch from its parent, stopping idle compute, reverting a failed schema deployment, creating a restore point, tearing down a merged pull request's database. The workflow stays deterministic either way — removing a gate doesn't make it improvise.


Two things follow from this that a fixed product can't offer. First, the ladder is per workflow, so your backup verification can be fully automatic while your restores require two approvals — and moving one doesn't move the other. Second, you can simply *not build* the workflows you don't want. If your team's view is that no automation should ever touch the payments database, you don't configure an exception — that workflow doesn't exist, and access control makes sure no other workflow can reach it either.


## Your approval flows, not ours


An approval gate in Kestrel is a node in the graph, which means it can go anywhere and there can be more than one. You decide who approves — a user, a group, the on-call engineer, a security lead — and where they do it: in the dashboard, in Slack, or by merging a pull request. The gate carries the context with it, so the approval message contains the investigation summary, the requesting team, their stated reason, and the before-and-after of whatever is about to change.


Click to expand


The split that recurs across every database integration we've built is worth stating plainly, because it's the design rule: **the investigation is safe to automate fully, and the remediation gets an approval gate** . A failed backup investigates itself and pages someone; the restore waits for a human. A degraded project investigates itself and posts to Slack; replacing the replica waits. A failed branch migration investigates itself; the reset waits.


But the rule has deliberate exceptions, and those exceptions are the proof that gates are a choice rather than a ceiling. In[our PlanetScale workflows](https://usekestrel.ai/blog/automate-planetscale-operations) , reverting a failed schema deployment runs with *no* approval gate — because when a deploy request has already failed, getting back to the last known-good schema is the safe default, and making a human confirm it just adds minutes of broken production. The same logic applies to stopping idle compute and resetting a preview branch. Whether an action needs a gate depends on whether the action or the inaction is riskier, and only your team can answer that for your databases.


Alongside gates, every workflow is fenced by[access control](https://usekestrel.ai/changelog/workflow-access-control-approvals) . On the trigger you scope a workflow with **Allowed Users** and **Allowed Groups** , and you fence it to the specific projects, branches, services, clusters, and cloud accounts it may touch. Building, activating, and approving workflows stays with your platform or data administrators, while developers get a request interface. So a self-service database workflow can be opened to a whole team while staying fenced into exactly the databases they're permitted to reach.


Click to expand


## Auditability: every database interaction on the record


The reason most organizations won't let an AI near a production database isn't capability — it's accountability. If something changes, someone has to be able to explain what changed, why, on whose authority, and whether it was supposed to happen. An agent that reasons its way to a different sequence of API calls each time it runs cannot answer those questions, which is precisely why so many teams cap their AI tooling at read-only.


Determinism is what fixes this. Because the graph is fixed before it runs, every execution of a workflow produces the same shape of record — and[every run is observable step by step](https://usekestrel.ai/blog/workflow-observability) : which trigger fired and with what payload, what each block did, what the investigation concluded, who approved which gate and when, what the database returned, and where it failed if it failed. That's a change record you can hand to an auditor, not a transcript you have to interpret.


Click to expand


That record works at two zoom levels, and both matter for a database estate. The **Observability** dashboard is the bird's-eye view across every workflow you've built — total executions, success rate, average duration, average approval wait time, and which workflows run most and fail most. For an AI DBA that's the answer to “which of our database automations are actually carrying load, and which one has been quietly failing?”, plus the approval wait times that tell you whether a gate is a real control or just a bottleneck nobody staffs.


Then you click into a single execution and see the workflow as a DAG with real-time status on every step: duration per step, a green check when it completes, the current step highlighted, and any failure surfaced right on the node. Click the trigger to see the original signal — what happened, which database, and the exact timestamp — or click any action block to see precisely what it sent and what came back.


Click to expand


There's a second, less obvious benefit. Access control scoping means the audit trail also documents what a workflow *could not* have done. “Could this automation have dropped a table in production?” is answerable by reading the workflow definition, not by reasoning about what a model might have decided. For teams with data residency or air-gap requirements, the same workflows run[fully self-hosted](https://docs.usekestrel.ai/on-premise/overview) , so neither the database credentials nor the investigation data leaves your environment.


## The AI DBA workflow library


Here is the job description, expressed as workflows. Each of the seven areas below has two prompts written the way you'd hand them to the Workflow Agent, drawn from the database integrations they exercise. Paste one in, review the graph, adjust the scoping and the gate to match your policy, and activate it. Every prompt names a specific database because the blocks are specific — but the pattern transfers, and the deeper per-integration walkthroughs are linked in each section.


### 1. Incident response


The first thing to automate, because it's the work that arrives at 3 a.m. and the investigation half is completely safe. See[automating Supabase operations](https://usekestrel.ai/blog/automate-supabase-operations) and[automating ClickHouse operations](https://usekestrel.ai/blog/automate-clickhouse-operations) .


/kestrel-workflow


Investigate & Escalate


When a Supabase project's health degrades, check the per-service health of the project, run a read-only investigation into what's unhealthy and why, and post the analysis to **#databases** in Slack. If the **db** service is among the unhealthy ones, page the on-call engineer through PagerDuty with the investigation attached; otherwise create a Jira ticket for the owning team.


/kestrel-workflow


Investigate & Fix


When a Supabase read replica goes unhealthy, fetch the project's health, run an investigation into the replica's state, and post the findings to **#databases** in Slack. Send replacing the replica to a Slack approval gate for the platform team; on approval, remove the unhealthy read replica and set up a new one in the same region, then confirm the new replica is healthy.


### 2. Provisioning


Provisioning is where an AI DBA earns trust with the security team, because a governed workflow can guarantee something a human checklist can't: that every new environment starts with the right network posture. See[automating Neon operations](https://usekestrel.ai/blog/automate-neon-operations) .


/kestrel-workflow


Harden


When a new Supabase environment is being set up, read the project's current network restrictions, then set the allowed CIDRs to our standard VPN and office allowlist and post the before-and-after to **#platform** in Slack. Require approval from the platform team before the restrictions are applied — a new environment should never start life open to the internet.


/kestrel-workflow


Provision


When a service needs read capacity in a new region, set up a Supabase read replica for the named project in the requested region and confirm the replica's status in **#platform** in Slack. Require approval from the platform team before the replica is created — replicas are a cost and capacity decision, not a self-service one.


### 3. Developer self-service


The highest-leverage area, because it removes the DBA from the critical path of every pull request without handing out production credentials. Developers ask in Slack with` /kestrel-workflow` , from the[CLI](https://docs.usekestrel.ai/workflows/cli) , or on the Developer Requests page. See the[internal developer platform guide](https://usekestrel.ai/blog/internal-developer-platform) .


/kestrel-workflow


Create


When a developer requests a preview database for their work in progress, create an ephemeral Supabase branch off the named project, fetch its connection host, and reply in Slack with the branch name and connection details so they can point their local environment at it. Allow creation for all developers with no approval gate, but require approval before any delete.


/kestrel-workflow


Investigate


When a developer asks about the state of a database — service health, branch status, recent backups, read replicas, or why last night's job run failed — run a read-only investigation to answer the question and post the findings back to the requester. Allow it for everyone with no approval gate; the investigation cannot change anything.


### 4. Schema migrations and CI/CD


The area with the sharpest risk gradient — and the clearest illustration of why gates belong on some steps and not others. See[automating PlanetScale operations](https://usekestrel.ai/blog/automate-planetscale-operations) .


/kestrel-workflow


Gate


When a pull request with migrations is merged in GitHub, send merging the corresponding Supabase branch to a Slack approval gate for the on-call engineer, with the branch name and PR link attached. On approval, merge the branch to run its migrations against the parent project, verify the project's health afterward, and post the result to **#releases** in Slack.


/kestrel-workflow


Roll back


When a PlanetScale schema deployment fails, run a read-only investigation into what the migration was doing and why it failed, revert the deploy request so the schema returns to its last known-good state, and post the failure and the analysis to **#releases** in Slack. No approval gate — with a deployment already failing, reverting is the safe default and every minute of hesitation is a minute of broken production.


### 5. Security and access


Continuous audit with gated remediation. The most common database security finding is also the most boring — a database reachable from anywhere because nobody set network restrictions after the prototype phase — which makes it perfect for automation. Database credential rotation is available through the[HashiCorp Vault integration](https://usekestrel.ai/blog/automate-vault-operations) as well as natively on Neon.


/kestrel-workflow


Audit


Every Monday at 9am, list our Supabase projects and read each project's network restrictions. Flag any project whose database is open to **0.0.0.0/0** and post the findings to **#security** in Slack. For each flagged project, send locking it down to our standard CIDR allowlist to a Slack approval gate, and apply the restrictions on approval.


/kestrel-workflow


Rotate


Every quarter, rotate the database credentials for our Neon production branch, update the connection string in our secret store, and post a confirmation with the rotation timestamp to **#security** in Slack. Require approval from a security lead before the rotation runs, and never include the credential values in any message.


### 6. Backups and disaster recovery


Backups fail silently — that's what makes them dangerous — so verification is the single highest-value read-only workflow you can build. Note that not every engine frames DR the same way: Neon's answer is copy-on-write branch reset rather than point-in-time restore, so the workflow that protects a Neon database looks different from the one that protects a Supabase project. See[automating ClickHouse operations](https://usekestrel.ai/blog/automate-clickhouse-operations) .


/kestrel-workflow


Verify


Every morning at 8am, list the backups of each of our production Supabase projects and check the status of the most recent one and whether point-in-time recovery is enabled. If the latest backup isn't healthy or PITR is disabled on a production project, post a warning to **#databases** in Slack and create a Jira ticket for the platform team. Read-only, no approval gate.


/kestrel-workflow


Restore


When the platform team requests a point-in-time restore of a Supabase project, send the request — project, target timestamp, and stated reason — to a Slack approval gate for the database owner. On approval, restore the project to the requested point in time, check the project's health once the restore completes, and post the outcome to **#databases** in Slack. Restores are destructive and incur downtime — always require approval.


### 7. Cost and lifecycle


The area where full automation is safest, because stopping compute and deleting merged preview branches are reversible and cheap to redo. This is usually the first place teams remove a gate. AWS databases are covered here too — RDS, ElastiCache, OpenSearch, and Redshift spend is queryable through the Cost Explorer blocks. See[automating Databricks operations](https://usekestrel.ai/blog/automate-databricks-operations) .


/kestrel-workflow


Clean up


Every Sunday at 10am, list the database branches of each Supabase project and run a read-only investigation to identify preview branches that look stale — branches in a failed migration state or older than 30 days. Post the candidates to a Slack approval gate for the platform team; on approval, delete the approved branches and post a summary of what was reclaimed to **#platform** in Slack.


/kestrel-workflow


Reclaim


Every weekday at 8pm, list our Databricks SQL warehouses and clusters and run a read-only investigation to identify which non-production compute is still running. Stop the idle non-production warehouses and terminate the idle non-production clusters, then post a summary of what was reclaimed to **#data-eng** in Slack. No approval gate — the configuration is retained and everything restarts on demand.


## Every database you run, one control plane


The reason to build your AI DBA on a workflow engine rather than inside one database is that most teams don't run one database. Kestrel has native lifecycle control for[Supabase](https://docs.usekestrel.ai/integrations/supabase) (Postgres with branching, backups, PITR, read replicas, network restrictions),[PlanetScale](https://docs.usekestrel.ai/integrations/planetscale) (MySQL branches, deploy requests for schema migrations, backups, branch passwords),[Neon](https://docs.usekestrel.ai/integrations/neon) (serverless Postgres copy-on-write branches, compute suspend and autoscaling, reset-from-parent, credential rotation),[ClickHouse Cloud](https://docs.usekestrel.ai/integrations/clickhouse) (service lifecycle and scaling, IP access lists, backups, ClickPipes ingestion, usage costs), and[Databricks](https://docs.usekestrel.ai/integrations/databricks) (job runs and repairs, cluster lifecycle, Delta Live Tables pipelines, SQL warehouses). Each one ships an AI-powered read-only Investigate block. AWS databases come in through the[AWS integration](https://docs.usekestrel.ai/integrations/aws) : CloudTrail-backed **RDS Database Issue** and **DynamoDB Issue** triggers that fire on deletions, snapshot exposure, configuration changes, and policy or security group modifications, an **AWS Service Health Event** trigger for provider-side incidents, read-only cloud investigation, Cost Explorer analysis, and remediation through AI-generated Terraform or CloudFormation opened as a pull request or AWS CLI commands executed with approval. Database credential rotation runs through Vault.


More importantly, the database blocks live in the same engine as everything else your operations touch — so the workflow that reverts a failed schema deployment can also roll back the Vercel deploy that depended on it, page through PagerDuty, and open the Jira ticket, in one graph with one audit trail.


### Infra


[K8s](https://github.com/KestrelAI/Kestrel-Operator)


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


### Secrets


Vault


Infisical


OpenBao


### PM


Jira


Linear


Confluence


Notion


### Security


Sonar


Okta


Kyverno


Trivy


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


### IaC/GitOps


Terraform


Pulumi


OpenTofu


CFN


+ custom webhook triggers and HTTP API action blocks


## Where to start


Don't try to build the whole DBA at once. The sequence that works:


- Week one — read-only.


Build the backup verification sweep and the network restriction audit. Both are ungated, both find something real on almost every estate, and both let you judge the investigation quality before anything writes.
- Week two — incident response.


Add the investigate-and-escalate workflows for your loudest failure modes. The investigation runs automatically; the remediation stays a gate.
- Week three — self-service.


Give developers preview databases, restore points, and read-only investigations. This is the change your team will notice most, because it removes the DBA from the critical path of every pull request.
- Then — promote what has earned it.


Look at the workflows whose gates have been approved every time without discussion for a month. Those are your rung-three candidates. Remove the gates on the reversible ones and leave the rest exactly where they are.


## FAQ: building an AI DBA


### What is an AI DBA?


A system that takes on the recurring work of a database administrator — investigating incidents, provisioning and branching databases, running schema migrations, rotating credentials, auditing access, verifying backups, controlling cost — using AI for the diagnosis a human would otherwise do by hand. The category splits into self-driving engines, tuning and observability advisors, and workflow platforms where AI investigation feeds deterministic, approval-gated remediation across every database you run.


### Should an AI DBA make production database changes autonomously?


Not by improvising. AI investigates and diagnoses at runtime; every write executes as a deterministic, pre-reviewed workflow step, with approval gates on anything destructive. Per workflow, teams climb a trust ladder — read-only first, gated writes once diagnosis quality is proven, full automation only for well-understood, reversible actions like resetting a preview branch or stopping idle compute. The point is that the decision is yours per workflow, not a global autonomy mode a vendor chose.


### Can an AI DBA work across multiple database engines?


That's the main advantage of building one on a workflow platform. A self-driving database automates itself and nothing else, so a team on Supabase, PlanetScale, ClickHouse, and Databricks would need four autonomous products with four governance models. In Kestrel they're blocks in the same engine, with one access control model, one approval model, and one audit trail.


### What's the difference between an AI DBA and database monitoring?


Monitoring tells you something is wrong; an AI DBA does something about it. Monitoring platforms are excellent at detection and increasingly good at diagnosis, but the last mile stays manual — someone runs the migration, resets the branch, rotates the credential, restores the backup. An AI DBA built on workflows closes that loop, with the fix executing as a governed, auditable sequence rather than a suggestion in a Slack thread. Monitoring and an AI DBA compose well: let the monitor detect, let the workflow respond.


### How do you keep an audit trail of AI database operations?


Every run is recorded step by step: which trigger fired, what each block did, what the investigation concluded, who approved which gate and when, and what the database returned. Because the workflow graph is fixed before it runs, the audit trail describes the same sequence every time rather than a different improvised path per incident — which is what makes it reviewable in a change-management process. Access control scoping means it also documents what a workflow could not have done.


## Getting started


Connect the database that hurts most — most integrations take about five minutes — and build the two read-only workflows first: backup verification and a network restriction audit. The[Workflows Quickstart](https://docs.usekestrel.ai/quickstart/workflows) walks through your first workflow end to end, and the[Create Workflows guide](https://docs.usekestrel.ai/workflows/create-workflows) and[Integrations Setup](https://docs.usekestrel.ai/workflows/setup-integrations) cover connecting the rest of your stack. Prefer the terminal or an AI coding agent? Workflows can also be created and managed through the[CLI](https://docs.usekestrel.ai/workflows/cli) ,[Python SDK](https://docs.usekestrel.ai/workflows/sdk) , and[MCP](https://docs.usekestrel.ai/workflows/mcp) . The AI DBA you end up with won't be the one we designed — it'll be the one your team decided on, which is the entire point.


### Build your AI DBA with $1,000 in credits


Automate the database work you choose, gate the rest behind your own approval flows, and get an audit trail of every interaction — described in plain English and built for you. New accounts get $1,000 in usage credits to get started.


[Get Started](https://platform.usekestrel.ai/register)
