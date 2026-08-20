---
schema_version: "1.0.0"
document_id: "b892d3267b2ffeb33b080b2e11ff94f1b8e4e2ed40e0c91bc6211f5613e57439"
company_key: "yc-replit"
company: "Replit"
source_id: "yc-replit-news-import-9d99ff8f4466"
canonical_url: "https://replit.com/blog/new-enterprise-governance-tools"
published_at: "2026-08-16T23:57:30.523+00:00"
first_seen_at: "2026-08-17T15:18:42.828480+00:00"
fetched_at: "2026-08-17T15:18:45.655149+00:00"
content_hash: "sha256:f1cd89921ee1738bfd8ee7f7e9a00cfcb5656017b7a10f569d21770c52e41015"
---

# Govern Replit at scale

### New Admin API, Audit Logs, and Workspace Settings give organizations more insight and flexibility into how they adopt AI with Replit.


Replit helps teams turn ideas into working software quickly. Most companies start with a handful of builders and a few prototypes. Then it works, and it spreads: more teams, more projects, more software running in production.


That shift creates work for a specific group of people. As AI tools spread across a company: IT, procurement, and admin teams absorb the cost. They field permission requests, run access reviews, make policy decisions tool by tool, and answer questions about what is running where. The requests pile up faster than those teams can grow.


We’re announcing a set of updates that will be continuously rolling out in coming weeks. These updates are aimed at supporting enterprise adoption of AI, built to take that work off admins' plates:


- **Comprehensive Audit Logs:** More than 50 events across deployments, identity, secrets, and agent activity, with native streaming to your SIEM. Available today.
- **Admin API:** Pull usage, workspace, member, and project data into the tools your teams already run. Beta available today.
- **Workspace Settings:** One policy baseline for the whole company, with approved exceptions for the teams that need them. First setting will be available end of the week.
- **Compliance API:** fetch the full contents of user messages for auditing & compliance. Available end of August.


### The questions every administrator needs answered


As adoption grows, the questions become practical:


- **Observability:** Who changed a setting or published a deployment?
- **Security & controls:** Can we keep a company-wide policy while giving one approved team the flexibility it needs?
- **Manage accounts at scale:** How do we programmatically manage all our Replit accounts?


These updates are designed to help teams answer those questions without putting a manual review step in front of every user.


### Comprehensive Audit Logs: see what happened


Comprehensive Audit Logs adds more than 50 new events across deployments, access and identity, workspace administration, project activity, secrets, connectors, domains, and agent activity.


That gives security and compliance teams a clearer trail when they need to investigate a change or prepare for a review.


Figure 1. Audit logs across deployments, access and identity, workspace administration, project activity, secrets, connectors, domains, and agent activity.


### Audit logs that work with your existing tooling


Replit audit events can stream to your SIEM.


What is supported today:


- Datadog
- Splunk
- Amazon S3
- Generic HTTP endpoint/webhook


Admins can view, filter, and bulk-export audit events.


Audit log retention is 30 days by default.[Contact](https://replit.com/support) Replit to discuss longer retention requirements.


## Workspace Settings: set a baseline without blocking every team


Teams work differently, and an open source model that's approved for an internal prototype may be off the table for a team handling customer data. When the only option is all or nothing, admins either approve the broadest set of models for everyone or spend the week fielding exception requests.


Workspace Settings gives account admins three choices for each policy control, and they can be mixed across the organization:


- **Set it once for everyone.** Apply the value org-wide and lock it.
- **Approve a different value for one workspace.** The exception is explicit and stays visible to you.
- **Delegate the setting to the workspace admin.** That team manages it themselves, within the controls you've opened up.


The first Workspace Setting we’re launching will allow enterprise administrators to:


- Opt in to using open source models, depending on company policy.
- Configure which model providers are used by the Replit Agent.
- Configure which individual models are allowed in each workspace for compliance and cost control.


We will continue adding more workspace settings to give administrators finer controls to ensure safe and compliant usage of Replit across the organization.


Figure 2. Administrators can control which agent modes and models employees can use based on company compliance policies


## The Admin API: manage accounts at scale


The Replit Admin API lets you pull workspace, member, project, and usage data into new Replit projects and the tools you already use: dashboards, Slack, Datadog, finance reports, etc…


For example an admin who wants spend broken out by Replit workspace can build the report in Replit, point it to the admin API, and get the analysis done that day.


The Replit Admin API is read-only in this first release, with 5 endpoints for getting usage data, workspaces & IDs, account members and projects. More updates will come to enable admins to programmatically manage Replit.


Figure 3. Create your admin API key to begin managing accounts at scale


### How to get started:


*What’s live:*


The[Admin API](https://docs.replit.com/teams/admin-api#admin-api) beta is live for all Enterprise customers. Create your Account API key in Settings→Developer→Create API Key.


To enable[Audit Logs](https://docs.replit.com/teams/identity-and-access-management/audit-logs) in Settings→Advanced→Audit Logs.


*Coming soon:*


For Workspace Settings go to Account Settings→ Advanced section→ Agent Modes and model providers (end of the week)


Compliance API **:** fetch the full contents of user messages for auditing & compliance (available end of August).


If you have additional questions on how Replit is helping enterprises become AI native,[contact our enterprise sales team](https://replit.com/enterprise)
