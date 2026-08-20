---
schema_version: "1.0.0"
document_id: "06f7ee286aad761f99a2815b3ddd76d024373156ed47922a102874061834a785"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-july-23-2026"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-25T03:24:53.102162+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:c0694eb45af8c7c8e8443641eca1c9b34ebf55b1af9929ffb2fb01b88fa63b84"
---

# Changelog - July 23, 2026

This week’s changelog post covers the latest public release notes currently published in Corgea Docs, centered on the July 20 release. The update expands scan coverage visibility for SAST and SCA, adds malicious dependency classification with pull-request blocking support, broadens Advanced Vulnerability Search and export workflows for SCA and IaC findings, and includes a wide set of UI and reliability improvements across the platform.


## Top 3 features


### 1. Better scan coverage visibility for SAST and SCA


Corgea added much clearer scan coverage visibility for both SAST and SCA, including skipped-file auditing, dependency-resolution coverage, per-engine file status in Scan Logs, and processed-file progress while scans are still running. That is a practical improvement for teams that do not just want findings, but also want confidence that the scanner actually covered the code and manifests they expected.


The supporting docs help explain why this matters. Corgea’s Scan Log is already the operational view for tracking run status, duration, branch, and reruns, while the project-management docs explain that future scans can intentionally skip ignored files and common generated or dependency paths. The SCA docs also show how much dependency analysis is doing behind the scenes across manifests, ecosystems, and exported dependency data. By surfacing skipped-file and per-engine coverage details directly in the scan experience, Corgea makes scan completeness much easier to verify instead of leaving teams to infer it indirectly.


### 2. Malicious dependency classification and blocking rules for pull requests


Corgea also added malicious dependency classification and Blocking Rules that can stop pull requests when known-malicious packages are detected. That pushes dependency security beyond the traditional vulnerable-versus-not-vulnerable model and gives teams a stronger supply-chain control right in the merge workflow.


The docs add useful context here. Corgea’s SCA coverage already draws from multiple authoritative vulnerability sources, including OpenSSF Malicious Packages, and the Blocking Rules docs show that dependency rules can prevent merges based on SCA findings, with configurable severity or CVSS thresholds and optional project or tag scoping. This release connects those capabilities more tightly, so organizations can treat known-malicious packages as an explicit merge-blocking event instead of relying only on later review or manual triage.


### 3. Broader Advanced Vulnerability Search and export coverage for SCA and IaC


The July 20 release also expanded Advanced Vulnerability Search and exports to include SCA and IaC findings, and added filtered CSV export from the Dependencies dashboard. That is a meaningful workflow upgrade for teams that want one investigation surface for code, dependency, and infrastructure findings instead of separate review paths for each scan type.


The docs make that broader workflow concrete. Advanced Vulnerability Search already lets teams filter across scan runs by vulnerability category, urgency, confidence, status, fix status, scan type, project, branch, policies, and other metadata while reviewing richer context in a side panel. On the dependency side, Corgea already supports filtered CSV exports that include package and vulnerability details, dependency type and reachability, project and branch, and the affected manifest path. Extending those search and export paths to SCA and IaC gives teams a much more unified way to slice, review, and share non-code findings.


## More features and improvements


- Enriched SARIF exports with finding guidance, security severity, policy taxonomies, call-graph context, fingerprints, and available remediation details.
- Added Harness source-control integration provisioning through the API.
- Redesigned Policy Playground with a resizable editor and preview workspace, improved file and classification selection, persistent context, clearer loading states, and more useful test results.
- Refreshed the Corgea interface with a more consistent design system across tables, forms, filters, cards, navigation, and empty states.
- Improved scan details with a unified repository, branch, commit, and scan-type header, clearer lifecycle statuses, and dedicated errors and warnings visibility.
- Added the current Corgea release version and a changelog link to User Settings.
- Fixed incremental CLI scan code previews, diff viewer line alignment, scan auto-refresh tooltips, and duplicate script loading on project pages.
- Improved AI Penetration Testing run controls to prevent accidental duplicate runs and keep page actions consistently positioned.
- Improved scheduled scans by preventing duplicate registrations, surfacing launch failures, and making Run Now reliably include eligible projects after older successful runs.
- Improved Azure DevOps pull request scan handling when branches are deleted or commit lookups are unavailable.
- Improved GitHub Team Sync for large organizations with rate-limit-aware processing, and fixed authentication edge cases during GitHub connection setup.
- Fixed Bitbucket pull request comment retrieval after credentials expire, allowing refreshed credentials to be used without lingering failures.
- Prevented case-variant repository names from creating duplicate projects and consolidated existing duplicates more safely.
- Improved fix delivery, webhook notifications, and scan launch responsiveness during periods of heavy scan activity.
