---
schema_version: "1.0.0"
document_id: "b18512b9c4bd984c060c1b04a2dfa39d9dda542f39c226ddf6749154ec87e00f"
company_key: "yc-corgea"
company: "Corgea"
source_id: "yc-corgea-news-import-efe6052ddd93"
canonical_url: "https://corgea.com/blog/changelog-may-13-2026"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-21T15:02:54.375122+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:8f4ffbed289476594af51882842f01f5b40d85471c82c3af5f2594c3ca578cfd"
---

# Changelog - May 13, 2026

This week’s changelog covers releases from May 5 through May 11, including a new source control integration for Harness Code and several scanning engine improvements focused on accuracy, coverage, and reliability.


## Top 3 features


### 1. Harness Code integration


Corgea now supports Harness Code as a first-class code repository integration. Teams can connect Harness repositories, bind them to Corgea projects, run pull request scans, receive inline Corgea comments and status checks, and create fix pull requests directly back into Harness.


The integration works with Harness Code independently of Harness CI/CD pipelines. Once configured, Corgea discovers repositories available to the Harness token, registers per-repository webhooks, triggers incremental scans on pull request activity, and writes a` corgea-security-scan


`


commit status that teams can use in PR merge rules.


### 2. Sharper secret scanning with improved context analysis


The scanning engine now has enhanced secret scanning accuracy, using improved context analysis to reduce false positives while maintaining broad coverage. This builds on Corgea’s secret scanning approach, which combines pattern matching, entropy analysis, and AI-powered contextual understanding to distinguish real credentials from test data, examples, and placeholders.


That means teams get more useful results across hardcoded credentials, API keys, access tokens, database connection strings, private keys, CI/CD secrets, and other sensitive values, while still fitting into pull request, CI/CD, IDE, and scheduled scan workflows.


### 3. Better endpoint discovery and call graph analysis


Endpoint discovery has been upgraded for better API detection and call graph analysis. This improves the scanning engine’s ability to understand how application components interact, which is especially important for finding issues that depend on request flows, authorization paths, and framework-specific routing behavior.


Corgea’s AI-native SAST already combines LLM reasoning with static analysis and project-level context. Stronger endpoint discovery gives that analysis a better map of the application, helping produce clearer findings for business logic, authentication, authorization, and traditional code vulnerabilities.


## More features and improvements


- Improved SBOM generation reliability and accuracy across multiple package managers.
- Enhanced language detection for secret scanning with support for additional file extensions.
- Optimized batch processing for large-scale scans, improving throughput for enterprise environments.
- Enhanced error handling and recovery mechanisms for more robust scan execution.
- Improved memory efficiency when processing large codebases.
- Improved handling of incremental versus full scan detection logic.
- Fixed scan metadata reporting and GPT error breakdown tracking issues.
- Resolved file processing edge cases that could cause scans to fail.
- Improved parent dependency false positive checking.
- Added general scanning engine stability improvements and performance optimizations.
- Fixed repository selection for workspaces where Harness is the only connected source control integration.
- Added Git service icons to Content Access project selection for easier project identification.
- Fixed code highlighting in false-positive explanations so inline code renders clearly and safely in issue details.
- Improved scan log file path display.
- Added Scan ID filtering to Advanced Vulnerability Search for more precise scan investigation.
- Improved agent pull request comment handling so Corgea responds more accurately to direct mentions and replies.
- Corrected duplicate project detection for repositories with the same name in different namespaces.
- Improved GitHub retry behavior for transient server and rate-limit responses.
