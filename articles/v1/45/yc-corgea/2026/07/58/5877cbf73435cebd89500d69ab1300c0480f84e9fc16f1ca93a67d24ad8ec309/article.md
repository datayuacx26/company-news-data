---
schema_version: "1.0.0"
document_id: "5877cbf73435cebd89500d69ab1300c0480f84e9fc16f1ca93a67d24ad8ec309"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-july-30-2026"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T22:23:31.729576+00:00"
fetched_at: "2026-07-31T22:23:33.180676+00:00"
content_hash: "sha256:9baf0ae24ae6f2a5731d589639319dd0616fab5ec66b2ae03e440084e3bc7fe5"
---

# Changelog - July 30, 2026

This week’s changelog post covers the public Corgea releases published between July 27 and July 29, 2026. The update adds Linear ticketing directly from findings, branded PDF exports for scan reports, and a more guided self-service SSO group mapping workflow, alongside new blocking-rule controls, API and MCP coverage for code quality findings, self-hosted GitLab support, and a broad set of UI and reliability improvements.


## Top 3 features


### 1. Linear integration for filing tickets directly from findings


Corgea now includes a Linear integration that lets teams create Linear tickets directly from both vulnerability findings and dependency findings. That closes an important gap between finding an issue and handing it off for remediation, especially for organizations that already manage engineering work in Linear and want security issues to flow into the same planning and tracking system.


The docs show that this is more than a basic webhook connection. Admins can add one or more Linear workspaces from the **Ticketing Integrations** area using a Linear personal API key, optionally set default teams and projects, and then create tickets directly from a finding’s detail view. Corgea automatically carries over useful context like the finding explanation, affected file or package, project and scan details, links back to Corgea, and any proposed fix diff, which makes the handoff much more actionable than copying issue details manually.


### 2. Branded PDF exports for shareable scan reports


This week’s releases also added branded PDF exports for scan findings. That gives teams a cleaner way to share scan results with stakeholders who do not live inside Corgea every day, such as engineering leaders, compliance reviewers, or customers who need a presentation-ready summary instead of raw CSV or SARIF output.


The export docs explain that the PDF report is designed as a polished deliverable rather than a raw data dump. It includes a cover page, severity breakdown, findings table, direct links back to findings, and filtering based on issue type and urgency so teams can tailor what gets shared. The same docs also note that false positives are excluded from the report and that PDF export is intentionally a web-app workflow, which reinforces that this feature is aimed at communication and review rather than machine-to-machine automation.


### 3. Self-service SSO group mapping for roles and project access


Corgea also introduced a self-service SSO group mapping editor, giving admins a more guided way to translate identity-provider groups into Corgea roles and project access. For organizations with a lot of teams, repositories, or line-of-business boundaries, that should reduce the friction of keeping access aligned with existing identity systems.


The SSO docs show how flexible those mappings can be. After a SAML integration is connected, admins can define IdP group mappings that assign an existing Corgea role and choose whether the group gets no project access, access to all projects, or access only to projects that match a pattern. The matching system supports placeholders like` {APPLICATION}


`


,` {LOB}


`


, and` {REPOSITORY}


`


, which makes it practical to map naming conventions from an identity provider into scoped Corgea access without manually assigning users one project at a time.


## More features and improvements


- Added issue-type filtering to code Blocking Rules so teams can target vulnerabilities, code quality findings, or both in pull request enforcement.
- Added email alerts for project owners and company admins when the same scheduled scan fails three times in a row, and automatically pauses that project on future scheduled runs until a successful retry.
- Added repository paths to scan lists and scan details to better distinguish similarly named repositories across providers.
- Improved webhook configuration by keeping the webhook type selector visible and adding a human-readable message to event payloads.
- Improved GitHub and GitLab pull request fix suggestions so invalid suggestion ranges no longer create syntax-breaking changes.
- Fixed vulnerability report loading for large result sets to reduce timeout failures.
- Fixed Jira ticket creation for findings with long summaries while preserving useful context in the title.
- Expanded SAML group recognition for additional identity-provider attributes.
- Fixed the Projects list so repositories with the same name remain distinct while duplicate entries for the same project are removed.
- Added code quality findings to the API and MCP so teams and connected assistants can list and inspect them programmatically.
- Added support for self-hosted GitLab instances, including custom hosts and broader accessible-project discovery options.
- Added commit SHA tracking and scan metadata for CLI scans to make prior results easier to identify and reuse.
- Streamlined onboarding with a guided connect-and-scan flow, a sample project option, and clearer upgrade guidance.
- Added searchable assignee selection for code findings and fixed navigation when opening findings from issue views.
- Added a user-selectable display timezone and improved consistency across forms, tables, filters, pagination, dialogs, and loading states.
- Improved user administration with clearer account details, newest-first sorting, and compact team badges with full membership visibility.
- Updated project pages to prefer the latest full-project scan for more representative project-wide results.
- Expanded repository URL support for longer source-control URLs.
- Improved SAML sign-in with case-insensitive email-domain matching, Duo Security metadata support, and better first-time access handling for users who satisfy group restrictions.
- Improved GitHub Team Sync by matching users by email and avoiding membership loss when member fetches fail.
- Improved Azure DevOps pull request comments by reducing reply-lookup delays on large PRs.
- Improved GitLab project discovery with more efficient, retry-aware requests.
- Fixed pagination totals so counts remain accurate after navigating beyond the final page.
- Improved large scan result processing to reduce memory-related failures and retry temporary storage errors more reliably.
- Improved MCP service stability by removing unsupported SSE endpoint handling.
