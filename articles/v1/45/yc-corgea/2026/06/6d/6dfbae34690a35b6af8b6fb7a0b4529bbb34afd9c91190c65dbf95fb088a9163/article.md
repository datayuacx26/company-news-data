---
schema_version: "1.0.0"
document_id: "6dfbae34690a35b6af8b6fb7a0b4529bbb34afd9c91190c65dbf95fb088a9163"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-june-18-2026"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:07e47ca0556da06614bbc9af476c75d84083be444a9b16e1bd9dc80477147648"
---

# Changelog - June 18, 2026

This week’s changelog covers the June 12 release, including the new Skills Registry, policy API access, SLA-aware vulnerability search, and a set of workflow and reliability improvements across the platform.


## Top 3 features


### 1. Skills Registry for versioned agent-skill workflows


Corgea added a Skills Registry that gives teams a structured, versioned workflow for creating, reviewing, approving, and installing agent skills. Instead of passing around ad hoc prompt files, teams can now manage reusable skills with clearer review and approval steps before those skills are distributed internally.


The supporting docs make this more concrete. Approved skills can be installed through the Corgea CLI into supported coding agents such as Cursor, Claude Code, Codex, GitHub Copilot, Windsurf, and others, either at the user or project scope. Teams can also install a specific skill version when they need repeatable setups, which makes the registry especially useful for standardizing internal workflows and rolling out curated agent behaviors in a controlled way.


### 2. Policy API access for automation and policy management


Corgea now exposes API access for policies, so teams can list, create, retrieve, update, and deactivate policies programmatically. That is a meaningful step for teams that want to manage AppSec policy workflows through automation instead of relying only on the UI.


The docs show that the new API covers the full lifecycle: listing policies, creating new ones, fetching a policy by ID, partially updating a policy, and deactivating it when it is no longer needed. They also show an important detail for change management: when policy content is updated, Corgea creates a new version, while simple active-status changes update the existing policy in place. That gives teams a cleaner way to automate policy administration without losing version history.


### 3. SLA status filtering in Advanced Vulnerability Search


Corgea added SLA status filtering to Advanced Vulnerability Search, making it easier to isolate issues that are within SLA, overdue, or escalated. For security teams trying to stay ahead of remediation deadlines, this turns the search view into a much more practical operational tool.


That matters because Advanced Vulnerability Search already acts as a cross-scan investigation surface with filters for project, branch, scan type, status, fix status, and other metadata. The SLA docs also show the broader context behind this new filter: Corgea supports separate SAST and SCA SLAs, configurable remediation and escalation windows by severity, daily deadline checks, email and webhook notifications, and aging reports for overdue issues. With SLA status filtering added to search, teams can more quickly zero in on the findings that need immediate attention.


## More features and improvements


- Added bulk actions to Content Access Management, so admins can add or remove members and teams, replace owners, and remove inactive users across selected projects more efficiently.
- Added editing for Jira integrations, so teams can update an existing Jira connection without recreating it from scratch.
- Improved scheduled scan Run Now so scans start in the background and return users to the scan list while the scan begins.
- Fixed issue detail previews so switching between findings shows the selected issue content instead of stale content.
- Fixed Harness pull request scans so PR numbers and links appear consistently on scan views.
- Fixed code quality scan views so plan-based access is applied consistently.
- Improved incremental full scans for projects with many issues or fixes, reducing scan processing slowdowns.
- Improved SCA dependency parsing for Maven SBOMs so direct dependencies are classified more accurately.
- Improved scheduled scan create and edit flows for large project selections, reducing form submission failures.
- Improved GitHub pull request checks and PR comments so check runs update more reliably and duplicate comments are reduced.
- Improved pull request scan precision by ignoring lines changed only by formatting.
- Improved large scan upload and ingestion reliability, reducing memory-related failures and returning clearer validation errors for missing scan run IDs.
