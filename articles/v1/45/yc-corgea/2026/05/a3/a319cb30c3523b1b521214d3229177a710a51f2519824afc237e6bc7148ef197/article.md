---
schema_version: "1.0.0"
document_id: "a319cb30c3523b1b521214d3229177a710a51f2519824afc237e6bc7148ef197"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-may-28-2026"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:a5b3eeb0b69f0fd8d58be841dff5f24468880c2abf8e40ea79e05ed920afd530"
---

# Changelog - May 28, 2026

This week’s changelog covers releases from May 20 through May 21, including dependency-focused SLAs, a new Security Design Review beta, expanded API and MCP access, and several reporting, scheduled scan, and reliability improvements.


## Top 3 features


### 1. SCA support in SLA Management


Corgea SLA Management now supports Software Composition Analysis findings, so dependency vulnerabilities can follow the same remediation and escalation workflows as code vulnerabilities. Teams can define separate SLA rules for SAST and SCA findings, set remediation and escalation windows by urgency, and track assignees, due dates, and status as findings move through triage.


The workflow also ties into Corgea’s notification system. When deadlines are missed, Corgea can send daily email summaries and trigger` sla.violation


`


webhooks, helping security teams route overdue dependency work to the right project owners, admins, or downstream workflow tools. Dependency issues also appear in Aging reporting when an SCA SLA applies, giving teams clearer visibility into overdue package risk.


### 2. Security Design Review beta


Corgea added beta support for Security Design Review, giving teams a dedicated place to review designs before implementation. The workflow is built around design-review analysis, recommendations, and security standards, so teams can evaluate security considerations earlier in the software development lifecycle instead of waiting for code or dependency scan results.


This release also improves the surrounding Security Review experience with Markdown support for pasted design documents and additional context. Admin permission groups now include Security Review permissions as well, so existing admins can manage these workflows without extra manual permission setup.


### 3. Expanded API and MCP access for security context


Corgea’s APIs and MCP server now expose more security data for automation and AI-assisted workflows. Teams and AI assistants can access Infrastructure-as-Code issues and dependency inventory with filters for scan, project, repository, branch, severity, provider, package, license, and dependency type.


This expands Corgea’s MCP use cases beyond SAST issue lookup into SCA, IaC, and dependency inventory exploration. The release also adds reachability details to issue APIs and MCP responses, plus repository, branch, and pull request ID filters for scan listings, making it easier to pinpoint the scan or finding that matters when investigating risk or planning remediation.


## More features and improvements


- Fixed Select All in scheduled scan project selection so teams can reliably select and deselect all eligible projects for project-scoped schedules.
- Added daily scheduled scan reports for new findings, with notification preferences and webhook delivery controls for scheduled scan summaries.
- Added auto learning for policy recommendations, so teams can review suggested policy improvements from product usage and feedback.
- Improved PR rule tables with clearer project and tag chips, source-control icons, overflow tooltips, and better behavior on narrow screens.
- Improved Reporting page developer feedback comments so longer comments are easier to view and recent feedback changes refresh more consistently.
- Improved plan-controlled feature updates so access changes from plan overrides take effect immediately after they are changed.
- Fixed manual full-scan triggering for affected source-control projects, with clearer repository selection feedback while scans start.
- Fixed SCIM-provisioned users so they are assigned to the SAM Default group when created.
- Fixed scheduled scan create and edit validation so unavailable scan types are hidden and rejected consistently.
- Replaced legacy scan-complete alerts with the newer notification and webhook delivery model for scan and SLA events.
- Improved SBOM processing reliability by preventing repeated generation loops when processing cannot complete.
- Improved batched API ingestion reliability to reduce database contention during large scan result uploads.
