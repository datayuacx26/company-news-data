---
schema_version: "1.0.0"
document_id: "ea163a92ff3ba9e49f8688a723df396b54a56df1f35956a0d0b3cce95f160a05"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-july-16-2026"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:77421053139c427fbf3eaac81a13445cd6c36c082754d49b8c90eb8f819af466"
---

# Changelog - July 16, 2026

This week’s changelog post covers the latest public release notes currently published in Corgea Docs, centered on the July 6 release. The update adds new reporting visibility for fixes generated and report-upload duplicates removed, plus more reliable GitHub check-run retries for pull request workflows.


## Top 3 features


### 1. Reporting metrics for fixes generated


Corgea added reporting metrics for fixes generated, giving teams a clearer view into how much remediation work the platform is helping produce. That is a meaningful shift for teams that want reporting to show not just how many findings exist, but how often those findings are turning into concrete fix activity and estimated time savings.


The reporting docs help explain why this matters. Corgea’s reporting experience already tracks unique vulnerability counts, remediation progress, MTTR, burn-down trends, scan activity, and developer insights across the organization. Adding a dedicated metric for generated fixes extends that surface with a more outcome-oriented signal, making it easier for security and engineering leaders to tie scanning activity to real remediation throughput.


### 2. Reporting visibility into report-upload duplicates removed


The same reporting update now surfaces metrics for report-upload duplicates removed. That gives teams more transparency into the cleanup work Corgea is doing behind the scenes when repeated findings arrive through multiple uploads or recurring scans, and it helps explain why dashboard totals stay clean instead of drifting upward with noise.


The supporting fingerprinting docs add useful context here. Corgea uses fingerprints to recognize the same vulnerability across scans, prevent duplicate issue reporting, preserve issue history, and keep reports focused on unique vulnerability counts and reliable trend analysis. By exposing duplicate-removal metrics directly in reporting, Corgea makes that deduplication work visible instead of leaving it as an invisible background behavior.


### 3. More reliable GitHub check-run updates on pull requests


Corgea also improved GitHub check-run updates so temporary GitHub server errors are retried more reliably when Corgea updates pull request checks. For teams that depend on PR-native security workflows, that should reduce cases where a transient GitHub-side failure makes scan status appear stale, incomplete, or slower to refresh than expected.


The GitHub integration docs show why that reliability matters. Corgea’s GitHub App creates and updates check runs on pull requests to report security findings and enforce merge-blocking policies, while also scanning PR diffs and anchoring comments to changed lines. Making those check-run updates more resilient helps keep security feedback dependable inside the review workflow where developers are already working.


## More features and improvements


- The latest public July 6` v1.68.0


`


release notes list only the three updates above; no additional public changelog items are currently published in Corgea Docs.
