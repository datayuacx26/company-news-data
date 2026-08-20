---
schema_version: "1.0.0"
document_id: "2f1d754fdadaebd4e21bc8e6e99bf1ea1853e0201cb780796f307d06843e83fa"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/introducing-infracost-agents-find-cloud-waste-fix-it-in-code"
published_at: "2026-07-21T15:36:00+00:00"
first_seen_at: "2026-07-23T11:56:51.837513+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:2c200ed70f5160668ba84b97bf164e99d3946d74b6494683135d54eae4de243c"
---

# Introducing Infracost Agents: Find Cloud Waste, Fix It in Code

One of our design partners was walking through findings surfaced by the new Infracost Idle & Unused Resources agent when she spotted something unexpected: two orphaned load balancers in an account that had *just* been cleaned up. She clicked the link in the finding, confirmed the resources were still running in AWS and wasting money every day. The agent went beyond just flagging the resources as idle. In it's investigation it identified that the Terraform modules managing these resources had been removed from the codebase, but the live infrastructure was never actually destroyed. "I actually thought everything was removed," she said. "The team must have missed some cleanups."


Simple human errors compound — quietly, on your bill, every month.


This is the gap that Infracost Agents is built to close. Find issues across your entire cloud estate. Map them to code. Investigate them. Fix them.


## Reports aren't remediation


Cloud cost tools are good at finding problems. They surface dashboards, flag anomalies, and generate reports that tell you exactly where money is being wasted. What they don't do is investigate or fix anything. The finding sits in a list. Someone has to investigate it, trace it back to the right codebase, figure out who owns it, open a PR, get it reviewed, and get it merged. For a FinOps team managing thousands of resources across dozens of accounts, that is not a workflow — it is a backlog that grows faster than anyone can drain it.


Infracost Agents is different. It doesn't just find issues in your cloud environment — it traces them back to infrastructure code, investigates them, and drives remediation through the workflows your engineering team already uses: pull requests, tickets, and AI coding assistants. The fix happens where it belongs, in code, so your IaC stays the source of truth and drift doesn't compound.


That includes resources that were never in IaC to begin with. ClickOps resources — provisioned directly in the console, outside your Terraform or AWS CDK — have historically been invisible to Infracost. Agents surfaces them too. When it can't trace a resource back to code, it still raises the finding so you can decide how to handle remediation.


## How it works


Agents run scheduled checks against your cloud environment and billing data. When a check detects something wrong — an idle resource, a tagging violation, an anomalous cost spike — it triggers an investigation. The investigation is where the work happens: the agent traces the resource to its IaC, infers ownership, enriches the signal with usage and billing context, and produces a finding with the tasks needed to resolve it.


From there, you choose how to act. Have the agent open a PR to fix the issue directly in code — no manual changes, no drift. Route it to your engineering backlog as a Jira or Linear ticket, pre-populated with the agent's investigation. Or hand off its findings to your AI coding assistant so you or a developer on your team can collaborate on a fix.


Every decision you make teaches the agent. Dismiss a finding because that idle load balancer is tied to a roadmap project that isn't live yet? The agent records that context and won't surface it again until something changes. Over time, the agents get smarter about your environment, your policies, and your team's intent.


## The first three agents


Infracost Agents launches in private beta with three agents:


**Idle and Unused Resources** monitors your cloud estate for resources consuming spend without delivering value — idle EC2 instances, unattached volumes, load balancers with no traffic, clusters with no workloads. It also surfaces orphaned resources that have been removed from code but never destroyed in the cloud. For each finding, it traces the resource to its IaC and opens a removal PR when you're ready.


**Tagging** enforces your tag policies at scale. It scans both IaC-managed and manually created resources, identifies violations, and opens PRs to fix them in code. For resources outside IaC, it flags them explicitly so you know what's drifted from policy.


**Cost Anomaly Detection** ingests anomalies from AWS and thoroughly investigates them — correlating spikes to application and infrastructure changes, identifying the likely root cause, and surfacing the context your team needs to act before the cost compounds.


These are the first three. More agents are in development, covering areas like Kubernetes rightsizing, AI cost optimization, and compute right-sizing. Private beta gives you a seat at the table as we build them.


## What private beta means


Infracost Agents is in active development. Private beta customers get hands-on access to the product, direct input into roadmap priorities, and a dedicated channel with the Infracost team. We're looking for engineering and FinOps teams who are serious about reducing cloud waste and willing to help shape what this becomes.


If that's you, join the waitlist. We'll reach out when Infracost Agents is ready for your environment.


[Join the waitlist →](https://www.infracost.io/lp/agent-waitlist)


*Already an Infracost customer? Contact your account team to discuss private beta eligibility.*
