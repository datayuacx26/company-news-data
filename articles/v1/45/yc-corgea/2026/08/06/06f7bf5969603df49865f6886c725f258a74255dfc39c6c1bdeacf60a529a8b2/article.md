---
schema_version: "1.0.0"
document_id: "06f7bf5969603df49865f6886c725f258a74255dfc39c6c1bdeacf60a529a8b2"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-august-6-2026"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T03:31:25.502707+00:00"
fetched_at: "2026-08-07T03:31:27.642774+00:00"
content_hash: "sha256:9e8dbce1879a1ce9e254b6968db7219c77b7511aafe06fad5733167c3118758e"
---

# Changelog - August 6, 2026

This week’s changelog post covers the public Corgea releases published between July 30 and August 4, 2026. The update adds dynamic team access based on project tags and repository URL fragments, brings IaC and container scanning into recurring scheduled scans, and makes scan failures easier to diagnose through clearer API response details, alongside agent-skill onboarding, smoother scan results UX, and a broad set of reliability fixes.


## Top 3 features


### 1. Dynamic team access based on project tags and repository URL fragments


Corgea now lets teams gain project access dynamically based on project tags and repository URL fragments. That is a meaningful upgrade for organizations that do not want to manage access one project at a time, especially when repositories are constantly being added, renamed, or re-tagged as teams and services evolve.


The supporting docs make the workflow concrete. In Content Access Management, a team can now inherit member-level project access through selected project tags or through repository URL fragments that match case-insensitively, and those selectors are combined with any explicit project assignments using OR logic. New or re-tagged projects begin matching automatically, which means access policies can stay aligned with how repositories are organized instead of depending on ongoing manual assignment work.


### 2. Scheduled scans now include IaC and container scanning


Scheduled Scans can now include IaC and container scanning, which expands recurring security coverage beyond application code and dependency manifests. For teams using schedules as their baseline hygiene workflow, that means infrastructure and container risks can now be checked on the same recurring cadence as the rest of their scanning program.


The docs show why that matters. Scheduled Scans already let teams target projects by explicit selection, tags, or teams, combine multiple scan types in a single schedule, and run them on frequencies from daily to yearly. Corgea’s IaC scanning covers technologies like Kubernetes, Terraform, Docker, CloudFormation, Azure ARM Templates, and Helm, while container scanning identifies vulnerabilities in base images and packages discovered from Dockerfiles and Docker Compose files. Putting those scanners on a schedule makes it much easier to catch infrastructure drift and image-layer risk before those issues sit unnoticed between manual reviews.


### 3. Clearer failure reasons and scanner-level errors in scan API responses


Corgea also added failure reasons and scanner-level errors to scan API responses, making incomplete or partially successful scans much easier to diagnose programmatically. That is especially useful for teams that automate scan review through the API and need to distinguish a clean run from one that finished with actionable failure context.


The docs already show several places where that extra context matters. Scan APIs expose lifecycle status including an` incomplete


`


state when a scan fails, Scheduled Scans surface per-project failure reasons in the UI, and webhook payloads include both a human-readable` message


`


plus raw` error


`


and` scan_errors


`


details for failed scans. Extending that diagnostic detail into scan API responses gives automation and reporting workflows a much better way to explain what went wrong without forcing users back into the UI to investigate.


## More features and improvements


- Added agent-skill onboarding to the Add Project flow, with copyable commands for installing the Corgea scan skill in a project or globally.
- Improved Jira and Linear integration dialogs so errors display clearly, clear when the dialog closes, and do not reappear when reopened.
- Fixed the New Scan menu position on scan details pages so scan options appear directly below the button.
- Fixed SARIF exports so result identifiers conform to the SARIF schema and import reliably into compatible tools.
- Improved GitHub sign-in handling so email lookup failures show clearer guidance, only verified email addresses are used, and organization sign-in requirements apply more consistently.
- Fixed dependency usage analysis for findings with long file paths or many affected lines so results save more reliably.
- Fixed scan webhook delivery when pull request identifiers are numeric so scan events continue to include pull request context.
- Improved in-progress SCA and IaC results so available findings remain visible alongside accurate tab counts and scan status.
- Fixed the loading indicator shown during the final Dropsite upload step so its status text no longer spins.
- Improved pull request scanning by skipping binary files when building diff artifacts, preventing unsupported file content from interrupting scans.
- Fixed bulk Content Access Management updates for large project and member selections, preventing request failures.
- Improved dependency usage analysis so incomplete or oversized usage details no longer leave findings stuck in repeated triage attempts.
- Fixed scan details controls and results so they refresh when a scan completes, making export and new scan actions available without reloading the page.
