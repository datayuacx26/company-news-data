---
schema_version: "1.0.0"
document_id: "dbbd584cadcac2782fcc26ada28dc41e2e07c6f48bb604ca73be31f45c22a7a0"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-july-9-2026"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:cb53245be91de98a881bc2063daac6e9030d72f2905bb00a7429dbf188d9508d"
---

# Changelog - July 9, 2026

This week’s changelog covers the July 6 release, including new reporting metrics for generated fixes and duplicate cleanup, plus more reliable GitHub check-run updates on pull requests.


## Top 3 features


### 1. Reporting metrics for fixes generated


Corgea added reporting metrics for fixes generated, giving teams a clearer view into how much remediation work the platform is helping drive. That makes it easier for security leaders and engineering managers to move beyond raw issue counts and see whether scans are leading to concrete remediation activity.


The reporting docs add useful context for why this matters. Corgea’s Reporting area is built to track scan activity, security outcomes, remediation progress, MTTR, burn-down trends, and broader developer insights across the organization. By adding fix-generation metrics into that surface, teams get a more direct signal for estimating hours saved and showing how auto-remediation support contributes to real security work getting completed.


### 2. Reporting visibility into report-upload duplicates removed


The same reporting update now includes metrics for report-upload duplicates removed. That gives teams better visibility into cleanup work that prevents dashboards and reports from being inflated by repeated findings, especially when ingesting security results from multiple runs or external uploads.


That lines up well with Corgea’s fingerprinting and reporting model in the docs. Corgea uses issue fingerprints to recognize the same vulnerability across scans and avoid duplicate issue reporting, and those fingerprints feed reporting views that are meant to show unique vulnerability counts and cleaner trend analysis over time. Surfacing duplicate-removal metrics makes that deduplication work more visible instead of leaving it as a hidden background process.


### 3. More reliable GitHub check-run updates on pull requests


Corgea also improved GitHub check-run updates so temporary GitHub server errors are retried more reliably when Corgea updates pull request checks. For teams that rely on PR-based workflows, that should reduce cases where a transient GitHub-side failure makes scan status look stale or incomplete.


The GitHub integration docs explain why this is an important quality-of-life improvement. Corgea uses GitHub checks to create and update check runs on pull requests, report security findings, and enforce merge-blocking policies, while also scanning pull requests and anchoring comments to changed lines in the diff. Making those check-run updates more resilient helps keep scan feedback dependable inside the workflow where developers are already reviewing code.


## More features and improvements


- This release was tightly focused on the three updates above; no additional public changelog items were listed for the July 6` v1.68.0


`


release.
