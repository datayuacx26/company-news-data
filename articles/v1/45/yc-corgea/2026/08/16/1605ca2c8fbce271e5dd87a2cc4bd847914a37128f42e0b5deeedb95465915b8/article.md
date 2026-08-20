---
schema_version: "1.0.0"
document_id: "1605ca2c8fbce271e5dd87a2cc4bd847914a37128f42e0b5deeedb95465915b8"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-august-13-2026"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-14T04:09:44.558770+00:00"
fetched_at: "2026-08-14T04:09:46.895403+00:00"
content_hash: "sha256:d5c9ece437d5cacef7aeb86f5e0464b23fb779541805be380fe39b4b126ee838"
---

# Changelog - August 13, 2026

This week’s changelog post covers the public Corgea releases published between August 6 and August 12, 2026. The update introduces a new Vulnerability Workbench, adds bulk triage ingestion for SAST and SCA findings, and strengthens sign-in resilience with email one-time password verification and SSO fallback, alongside new blocking-rule and scan-metadata controls, smarter pagination, and a broad set of scanning and reliability improvements.


## Top 3 features


### 1. Vulnerability Workbench for faster cross-finding triage


Corgea now includes a Vulnerability Workbench that brings together finding-type tabs, drill-down views, detailed drawers, filtered CSV exports, and bulk assignment and triage actions. That gives security teams a much more centralized place to review and act on findings without bouncing between separate pages or repeating the same workflow one finding at a time.


The supporting docs show that the workflow goes beyond a new table view. Teams can bulk-select individual findings or all findings matching the current filters, then assign, reopen, suppress as accepted risk, mark false positives, or export the selection. Bulk selections can span up to 1,000 findings, status changes require a comment, and CSV exports carry useful reporting fields like issue IDs, scan IDs, classifications, severities, assignees, and scan metadata. Together, that makes the Workbench a much stronger operational surface for high-volume triage and reporting.


### 2. Bulk triage ingestion API with approval workflows and audit history


Corgea also added bulk triage ingestion for SAST and SCA findings, making it easier to apply status changes programmatically across large sets of findings. This is a big step for teams that manage remediation through automation, scheduled workflows, or external systems and need those actions to be both scalable and reviewable.


The API docs show that` /triage/findings


`


can preview or apply rule-based status changes to as many as 5,000 accessible findings. When an accepted-risk action crosses the company’s approval threshold, Corgea returns a pending response instead of silently applying the change, and a different company admin can approve or reject it. Each bulk action can also be retrieved later with immutable finding-level audit records, which gives teams a clearer trail for governance and change review.


### 3. Stronger sign-in verification for password and SSO users


This week’s releases also tighten sign-in resilience with mandatory email one-time password verification for password-authenticated users and an email one-time password fallback for enabled SSO workspaces. That adds an extra verification step for direct password logins while also giving organizations a safer way to keep users moving if their SAML identity provider is temporarily unavailable.


The docs spell out the flow in detail. For password-authenticated users, Corgea sends a one-time code after sign-in or password setup, the code expires in 10 minutes, and requesting a new code invalidates the previous one. For SSO customers, eligible organizations can enable a passwordless email fallback that presents a **Sign in with a one-time password** option on the login screen, letting active users on the configured domain regain access during an IdP outage without abandoning SSO as their normal authentication path.


## More features and improvements


- Added CI-specific Blocking Rules that can be selected by rule ID, plus license-compliance rules for restricting dependency license families.
- Added read-only scan metadata across scan lists, scan details, issue views, API responses, and MCP results, with filtering for metadata keys and values.
- Added PDF exports for scans with no findings so clean scans can still be shared as complete reports.
- Improved dependency, container, and IaC finding tracking so matching issues keep their identity, triage history, and assignment context across scans.
- Improved web app responsiveness and database reliability by reusing healthy database connections between requests.
- Improved Harness repository search so matching repositories appear even in projects with more than 100 repositories.
- Added an API endpoint that reports the running Corgea web app version for compatibility checks.
- Improved pagination across scan lists and other large result sets so controls stay compact on dense views.
- Simplified the File Logs tab by removing the Limited skip detail warning when full skip-reason coverage is unavailable.
- Updated Vulnerability Workbench access so navigation and actions only appear when enabled for the workspace’s plan.
- Improved CLI scan accuracy for worktrees with uncommitted changes by falling back to a full scan and surfacing a` dirty


`


indicator in scan details and API responses.
- GitHub pull request scans are now deferred while a pull request is in draft and begin once it is marked ready for review.
- Improved IaC scan processing and triage carry-forward performance so large finding histories do not disrupt ingestion.
- Fixed scheduled scan forms so project names, headings, selections, and summaries stay readable in both light and dark themes.
- Improved page title alignment so subtitles and badges display more consistently.
- Improved GitHub pull request scan reliability by preserving and retrying webhook events when background processing is temporarily unavailable.
- Improved IaC finding performance, reducing load times for scans with large result sets.
- Improved batch IaC API performance, reducing delays when processing multiple findings.
- Improved post-scan SCA history processing for projects with extensive scan histories, reducing delays after scans complete.
- Added pull request scan scope guidance to Scan Logs, clarifying that only files in the pull request are shown.
- Added scanner-provided Fortify metadata to issue details, making uploaded findings easier to trace back to their source.
- Improved company notification settings by removing the ambiguous` Inherit


`


option.
- Fixed project scan pagination so large scan histories no longer overflow the page.
- Improved large project deletion so projects with extensive scan history can be removed without disrupting the database.
- Fixed SCA findings whose paths contain` tmp


`


so dependency tables and finding details continue to load correctly.
- Improved scan completion processing so pull request checks and SCA comments update more reliably during heavy activity.
- Expanded webhook URL support for longer endpoints.
