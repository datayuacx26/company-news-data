---
schema_version: "1.0.0"
document_id: "e71adc46573458498df40184cb3fb2fc69ead7390ba586249126e9eb861e492a"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-july-2-2026"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-24T03:05:55.735939+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:bfcbc2be3fe285a011f0616313cf33c6ecacdc4d6cd38154faa8d137b0632e7f"
---

# Changelog - July 2, 2026

This week’s changelog covers the July 2 release, including the launch of AI Penetration Testing, new dependency inventory workflows in the Corgea Agent, documented notes on project-level ignore rules, and a set of access, review, and reliability improvements across the platform.


## Top 3 features


### 1. AI Penetration Testing for autonomous external assessments


Corgea added AI Penetration Testing, bringing autonomous external assessments into the platform. The changelog frames it as more than a one-shot scan: teams can manage targets, watch runs as they progress, revalidate findings, receive completion emails, and generate either technical or executive PDF reports when the assessment finishes.


That matters because it gives teams a fuller pentest workflow instead of just a findings list. In the docs, Corgea’s broader security-testing story already centers on combining security analysis with actionable remediation, and this release extends that model into external assessment workflows with visibility, repeatability, and reporting built in from the start.


### 2. Dependency inventory tools in the Corgea Agent


Corgea also added dependency inventory tools to the Corgea Agent, so teams can list dependencies and generate CSV export links directly from agent conversations. That moves dependency review closer to the place where developers and security teams are already asking questions, instead of forcing them to pivot into a separate export workflow for every request.


The docs add useful context around what this inventory is built on. Corgea’s dependency inventory commands can scan npm, Python, and Java manifests and lockfiles, build offline inventories without requiring login or network access, explain individual packages, diff dependencies across branches, and export results in multiple formats. The docs also note that CSV exports are designed for reporting and spreadsheet-style review, which makes the new agent workflow especially useful for audits, cleanup, and quick package reviews.


### 3. Comments on file ignore rules and CWE filters


The July 2 release also adds comments to project file ignore rules and CWE filters, giving teams a better way to explain why a project-specific exclusion exists. That may sound small compared with a new scanning surface, but it addresses a very real governance problem: exceptions tend to accumulate over time, and without context they become hard to review or safely remove.


The supporting project-settings docs make the benefit concrete. File ignore rules are used to skip project-specific paths in future scans, while CWE filters can ignore selected weakness types globally or only for matching file patterns. The docs now explicitly emphasize documenting why each rule exists so new contributors can understand whether the exclusion should remain in place, which makes these comments a practical improvement for long-lived policy hygiene.


## More features and improvements


- Added proactive expired-credential detection and admin email notifications for GitLab, Azure DevOps, Bitbucket, and Harness integrations, helping teams renew tokens before scans or repository access break.
- Improved Security Review recommendations so pending and processing reviews refresh automatically as recommendations become available.
- Added pagination to Content Access Management, improving performance and navigation for workspaces with many projects.
- Fixed scan detail pages so the selected scan tab, dependency tab redirects, filters, and badges render cleanly before Alpine finishes loading.
- Improved GitHub Team Sync for large organizations by syncing teams in smaller background jobs and reducing organization-wide timeout risk.
- Improved Azure DevOps pull request diff handling so findings in edited files are matched against the scanned PR diff more accurately.
- Improved source-control webhook handling for GitHub and Bitbucket, reducing noisy retry and comment-thread failures.
- Fixed GitHub branch loading errors so repository branch fetch failures show the affected repository name correctly.
