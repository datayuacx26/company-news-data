---
schema_version: "1.0.0"
document_id: "a4f6e9144064214c42962958f0c1c287a322ee86a049a2476d412c41d9ce2500"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-june-11-2026"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:94f03fddb2b113583758dc0725571ed291aadeec756039d85f0b860a2bb935bb"
---

# Changelog - June 11, 2026

This week’s changelog covers releases from June 1 through June 4, including on-demand fix generation, deeper reporting filters, richer dependency advisory context, and several workflow, reliability, and UX improvements.


## Top 3 features


### 1. On-demand fix generation


Corgea now supports generating fixes on demand when an issue is opened, instead of requiring every suggested fix to be prepared ahead of time. That helps teams move faster on large scans and long backlogs, because fixes can be created at the moment a developer is ready to review and act.


The docs show how this fits into Corgea’s existing remediation workflow. Issues can appear with a` Fix Available


`


or` On Demand


`


status, and when an` On Demand


`


issue is opened Corgea starts generating the fix automatically, usually within 15 to 30 seconds. Corgea also runs AI-assisted quality checks before surfacing the result, and teams can then create a pull request, download a patch, or apply the fix through the IDE extensions. In practice, this makes fix generation feel more responsive without changing the review controls teams already rely on.


### 2. Branch filtering in Reporting


Corgea added branch filtering to Reporting, making it easier to isolate the exact slice of security and quality data a team cares about. Instead of looking at only organization-wide trends, teams can now narrow reporting views to the branch they are actively investigating.


That matters because Reporting spans much more than a single chart. The docs show that Reporting includes Code Vulnerabilities, Code Quality, SCA, IaC, and Developer Insights views, plus scan operations and vulnerability aging reports. Corgea already supports project, tag, severity, and time-based filtering across much of that reporting surface, so branch filtering adds another valuable layer for debugging branch-specific regressions, reviewing release branches, or comparing long-lived branches without losing the broader context.


### 3. Advisory details on SCA issue pages


Corgea added advisory details directly to SCA issue pages, giving teams more context when triaging dependency findings. Instead of seeing only the package and severity at a high level, teams can review the underlying advisory information closer to where they make remediation decisions.


This builds on Corgea’s broader SCA experience in the docs, which already includes CVE identifiers, CVSS scores, affected versions, remediation guidance, advisory references, publication dates, and direct-versus-transitive dependency context across more than 25 ecosystems. Surfacing more of that advisory detail on the issue page should make it easier for developers and AppSec teams to understand why a dependency finding matters and decide whether to update, suppress, or prioritize it.


## More features and improvements


- Added team scoping for company notification defaults, so admins can limit email defaults to selected teams while keeping webhook defaults separate.
- Added direct repository file links in issue details, making it faster to jump from a finding to the affected file and line in source control.
- Added pull request links to Agent feedback history, so teams can trace feedback entries back to the originating PR or merge request.
- Blocked an additional disposable email domain during sign-up to reduce low-quality or abusive account creation.
- Improved the plan override admin widget so it renders cleanly in dark and light admin themes.
- Improved expired integration messages for Azure DevOps, Bitbucket, and Harness scans, giving users clearer guidance to update the integration and retry.
- Improved issue selection, scan details, and scan list performance for workspaces with large issue volumes.
- Fixed source-sink analysis handling when fix data needs to be generated on demand.
- Added severity filtering to IaC scan results, so teams can narrow infrastructure findings by Critical, High, Medium, or Low severity.
- Added pull request links to the scans list, making it faster to jump from a PR scan back to the originating pull request.
- Improved GitHub retry handling so temporary GitHub rate limits and permission responses no longer hold scan workers for extended periods.
- Fixed SLA Slack notifications so channel alerts are sent once per SLA event, retry more reliably, and handle Slack rate limits more gracefully.
