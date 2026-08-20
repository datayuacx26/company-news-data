---
schema_version: "1.0.0"
document_id: "dc0525d87fd5820112a23f3dc9be8f403f28b8c70d1265f53c67ca0e2e1af584"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/may-2025-changelog"
published_at: "2025-05-31T09:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:371c1e51f5f0d19df4ed5855e1be2ede5390e3edfc3ee7f14c6b4c53a12e2bad"
---

# May '25 Changelog

# What's new in Tusk


Tusk now auto-generates its GitHub workflows from your existing stack and CI configurations, removing setup barriers that previously slowed down self-serve onboarding.


We've introduced a repo configuration that, when enabled, triggers full test file creation only on incorporation, allowing developers to see Tusk's test scenarios on their PR/MRs 3-5 mins sooner.


Tusk now also has enterprise-grade features like unified repository management and a new analytics dashboard to provide engineering leaders visibility into test generation effectiveness and team productivity metrics at the level of the PR/MR, user, and repo.


Bottom line: faster customer onboarding and more personalized developer experience while giving managers the data they need to optimize team performance.


## Release notes


🚀 **Major Features**


- **Intelligent Workflow Setup** - Auto-generate Tusk GitHub workflows from existing stack and CI configurations to eliminate setup friction
- **Fast Test Generation** - See Tusk's test results quicker by deferring full test file generation until the tests are incorporated
- **Unified Repository Management** - New "Repositories" page for syncing repos, creating test environments, enabling repo configurations, and adding repo secrets
- **Analytics Dashboard** - New "Home" page with test generation analytics, PR-level insights, and user activity


💫 **Performance Improvements**


- **Reduced Run Latency** - Halved latency for applicable repos by introducing per-sandbox file limits that improve multi-file processing and RAM-based operations for large repositories
- **Improved Fallbacks** - Implemented safeguards to increase test incorporation reliability
- **Test Quality Improvements** - Implemented solutions to use mocks better and prevent re-implementation of source functions in tests


🎨 **DevEx Enhancements**


- **Branch Coverage Tracking** - Automated reporting for Jest/Mocha frameworks
- **File Mentions** - @-mention files in custom instructions to provide localized context
- **Test Diff Visualization** - Clearer display of diffs in test code for a single scenario
- **Ability to Pause Run** - Pause Tusk's test generation run by adding the` Tusk - Pause For Current PR` label to a PR (GitHub only for now)


---


TRY TUSK NOW


## AI test generation with codebase context for quality-obsessed teams.


Cover your blind spots, catch verified critical bugs, merge PRs 25% faster with peace of mind.
