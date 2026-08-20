---
schema_version: "1.0.0"
document_id: "a041b7ecb3e62c5fee51ffa14b16d4b8f00d22180227e1c92d79a3c9289c6e46"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/september-2025-changelog"
published_at: "2025-10-03T18:22:04.210+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:ae930dd3a727b6053dbf737a3e173dbc4546b2a6bb1cb4cf7633d69e758b83c8"
---

# September 2025 Changelog

## Release Notes


We’ve released[Tusk Drift](https://www.usetusk.ai/tusk-drift?utm_source=tuskblog) , our API test automation product, in private beta. Developers can use our new SDK and CLI to record real-world traffic and replay them as API tests locally or in CI.


Behind the scenes, we've expanded language support with Java and Go coverage providers, added the ability to log in with GitHub and GitLab, and improved CI workflow error handling.


‍


‍


## Major Features


- **Tusk Drift (Private Beta)** : Released Node.js SDK and CLI tool for Tusk Drift. Developers can now generate API test suites from traces recorded from real application behavior.
- **GitHub / GitLab Log-In** : Allow users to create a Tusk account with GitHub and GitLab log-in and associate it with an organization
- **User Roles** : Introduced admin/member role management, giving admins granular control over organization-wide permissions and preferences
- **Java / Go Coverage Support** : Added JUnit (with JaCoCo) and native Go test coverage analysis, expanding coverage support for PR check and CoverBot


## Testing Improvements


- **Improved Language Server Integration** : Refactored language server integration for resiliency and to better support new languages
- **Coverage Timeout Extensions** : Increased GitHub Actions coverage timeout to prevent test generation from stopping early for complex repos
- **Dashboard Table Filters** : Added advanced filters to Usage Insights table, allowing users to filter PRs by columns


## DevEx Enhancements


- **Clearer Workflow Verification** : Improved GitHub Actions workflow error handling with specific error types and actionable guidance for faster debugging
- **Dark Mode** : Introduced dark mode theme for UI
- **Timestamp Display** : Added creation timestamps to Test Run pages, making it easy to differentiate between multiple runs on the same PR
