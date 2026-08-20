---
schema_version: "1.0.0"
document_id: "f7ce3c0f59aeff032a775409e97a2503afc518e59b65dd1fdd2ba86a4d7d59ad"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-may-21-2026"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:f00ddc1166431a3a030657c21aa181b59703df8fd011dcf26f488cf8924b89f5"
---

# Changelog - May 21, 2026

This week’s changelog covers releases from May 15 through May 19, including more focused scan automation, easier PR rule management at scale, and several scanning engine improvements for clearer results.


## Top 3 features


### 1. Scheduled scan filters for webhooks


Corgea webhooks can now subscribe to scan events from specific scheduled scans instead of receiving every scan event. Teams that route scan updates into Slack, ticketing systems, SIEMs, or custom automation can keep those downstream workflows focused on the schedules they actually care about.


This builds on Corgea’s webhook filters for projects and issue statuses. For scan events, the scheduled scan filter applies to` scan.started


`


,` scan.completed


`


, and` scan.failed


`


, so teams can trigger targeted workflows for a production cadence, a compliance schedule, or any other recurring scan program without adding extra filtering logic outside Corgea.


### 2. Project-tag scoped PR rules


Blocking Rules and PR Scan & Comment Rules now support project tag scoping. Instead of maintaining long lists of individual projects, teams can apply pull request blocking policies and PR scan/comment behavior to every project with a matching tag.


This is especially useful as organizations scale. Blocking Rules already help prevent non-compliant code from being merged based on SAST or SCA findings, while PR Scan & Comment Rules control when Corgea scans pull requests and posts findings back to the diff. With tags, those controls can follow service tiers, environments, teams, or application groups as projects are added and reorganized.


### 3. Broader and cleaner scan analysis


The scanning engine now produces clearer scan summaries, expands code analysis coverage for additional languages, and reduces duplicate secret scan findings. Together, these updates make scan output easier to review while improving issue detection and false-positive identification across more codebases.


Corgea’s AI-native SAST combines LLM reasoning with static analysis to understand project context, frameworks, authentication and authorization flows, business logic, and traditional vulnerability patterns. Expanded language coverage gives that analysis more places to operate, while cleaner summaries and deduplicated secret findings help developers and security teams spend less time sorting noise.


## More features and improvements


- Added options in Advanced Vulnerability Search to include issues marked as false positives by status or fix status when those results need to be reviewed.
- Added Harness pull request links on PR scans, making it easier to jump from a Corgea scan back to the originating Harness PR.
- Improved PR rule setup with clearer field tooltips, direct documentation links, better tag-based rule counts, and Critical/High selected by default when adding new PR rules.
- Improved scan download controls so CSV, SARIF, SBOM, and dependency exports are disabled until scan processing completes, with clearer guidance when downloads are not ready.
- Improved policy retrieval so prework-generated policies are included where applicable, keeping policy review and dependency policy views consistent.
- Fixed Bitbucket PR comment handling so Corgea recognizes hyphenated bot mentions correctly.
- Improved source-control project selection error messages when an integration is unavailable or cannot be reached.
- Improved project creation and deletion cache handling so plan limits and repository lists update more consistently after project changes.
- Improved production asset cache busting so updated JavaScript, CSS, animations, and email assets load reliably after deployments.
- Improved dependency scan reliability by preventing repeated retries when a scan cannot continue.
- Fixed dependency issue triage reporting so unsuccessful triage attempts are reflected correctly.
- Improved engine dependency handling for more consistent scan execution.
- Fixed Blocking Rule create and edit workflows so selected projects, project tags, and classifications are saved and shown consistently.
- Fixed Scan & Comment Rule create and edit workflows so selected projects, project tags, classifications, and integrations are preserved reliably.
- Improved scheduled scan editing by showing the projects already selected for a schedule, making it easier to review, remove, or clear selected projects.
- Improved issue fix downloads so download actions only appear when a suggested fix is ready, reducing confusion for issues without an available fix.
- Improved SCA pull request comments so they show direct or transitive dependency context and focus on dependency findings tied to files touched in the pull request.
- Optimized the Projects page so larger workspaces load local projects faster while remote repository data refreshes separately.
- Improved code scan issue cleanup to reduce duplicate active findings in scan results.
