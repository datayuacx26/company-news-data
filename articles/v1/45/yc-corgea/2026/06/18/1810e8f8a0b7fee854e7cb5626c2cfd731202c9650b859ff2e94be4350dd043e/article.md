---
schema_version: "1.0.0"
document_id: "1810e8f8a0b7fee854e7cb5626c2cfd731202c9650b859ff2e94be4350dd043e"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-june-25-2026"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:3dbfa2889846442d2c10030565f8a2180e6acc5f288af6b8e10451de6ac9d986"
---

# Changelog - June 25, 2026

This week’s changelog post covers the latest public release notes currently published in Corgea Docs, centered on the June 12 release. The update introduces the new Skills Registry, policy API access, bulk Content Access Management workflows, and a set of search, integration, and reliability improvements across the platform.


## Top 3 features


### 1. Skills Registry for governed agent-skill rollouts


Corgea added a Skills Registry that gives teams a structured way to create, review, approve, and distribute internal agent skills. Instead of sharing one-off prompt files informally, teams can now manage skills as versioned assets with an approval step before they become installable.


The product docs show how that workflow is meant to operate in practice. Teams can upload a` SKILL.md


`


, optionally assign a version, and submit each version independently for review. Only approved versions become installable through the CLI, while pending, rejected, or failed versions remain visible so teams can inspect status, review notes, and version history before publishing an update.


### 2. Policy API access for automation and versioned governance


Corgea now exposes API access for policies, making it possible to manage policy workflows programmatically instead of only through the UI. That is especially useful for teams that want to automate policy administration, synchronize policy state with internal tooling, or embed policy management into broader AppSec workflows.


The API docs show that the new surface covers the full policy lifecycle. Teams can list policies with` GET /policies


`


, create them with` POST /policies


`


, update them with` PATCH /policies/{policy_id}


`


, and deactivate them with` DELETE /policies/{policy_id}


`


. The most important implementation detail is that content changes create a new policy version, while toggling active status updates the same policy record, which gives teams cleaner version history without making simple operational changes more cumbersome.


### 3. Bulk actions in Content Access Management


Corgea added bulk actions to Content Access Management, giving admins a faster way to maintain project-level access control across large project sets. For organizations with many repositories, that turns what used to be repetitive one-by-one membership maintenance into a much more scalable workflow.


The docs show that admins can open **Edit Members** , filter the projects they want to update, select the projects in scope, and then apply a bulk action in one pass. Those actions include adding users as members or owners, removing users from member and owner roles, adding or removing teams, replacing one owner with another, and removing inactive users from selected projects. The same guide also emphasizes that this is especially useful for role changes, departures, and other ongoing access cleanup work.


## More features and improvements


- Added SLA status filtering to Advanced Vulnerability Search, making it easier to find issues that are within SLA, overdue, or escalated.
- Added editing for Jira integrations, allowing teams to update an existing Jira connection without recreating it.
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
