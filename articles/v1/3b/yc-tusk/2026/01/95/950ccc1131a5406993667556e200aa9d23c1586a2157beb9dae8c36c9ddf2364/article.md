---
schema_version: "1.0.0"
document_id: "950ccc1131a5406993667556e200aa9d23c1586a2157beb9dae8c36c9ddf2364"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/december-2025-changelog"
published_at: "2026-01-12T04:45:53.622+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:23:44.568277+00:00"
content_hash: "sha256:cf38979d8635872a02ec5b2790f3f81c90023feead98ab82a1114064a1e09391"
---

# December 2025 Changelog

## Release Notes


December brought the debut of our[AI Trace Assistant](https://www.usetusk.ai/third-party-integration-testing) , a debugging companion that has contextual understanding of your Tusk Drift observability data. Go beyond Sentry-level observability by drilling into request *and* response bodies.


We also shipped CoverBot support for Java/Kotlin repos and made reliability improvements to Tusk Drift's deviation analysis and test suite maintenance.


The[Node.js SDK](https://github.com/Use-Tusk/drift-node-sdk) now has Turbopack support for Next.js, while the Tusk Drift CLI has an onboarding agent that installs and initializes the SDK with a single command. We've given the CLI secure replay functionality that runs all test operations in a[local sandbox](https://github.com/Use-Tusk/fence) to prevent unintended external requests.


Not a bad way to close out 2025.


## **Platform**


### Major Features


- **AI Trace Assistant** : Chat with your traces to debug errors and analyze performance trends, comes with built-in sandbox integration, tool calling, and session management
- **Observability Dashboard** : Get full visibility into traces and spans recorded by Tusk Drift; drill down into the request and response bodies of individual spans
- **MCP Server for Tusk Drift** : Model Context Protocol integration for Claude Code and other MCP clients (only part of AI Trace Assistant currently)
- **Unit Test Selection for New Pipeline** : Allow test case selection when committing unit tests generated using our speedier test generation pipeline
- **Java/Kotlin Coverage Analysis** : CoverBot now supports Cobertura coverage provider


### Test Quality Improvements


- Enhanced Tusk Drift's deviation analysis and test replay functionality with better error handling
- Added intelligent API test validation before moving test from "Draft" to "In Suite"


### DevEx Enhancements


- Karma test framework support for Tusk Tester
- UI/UX revamp across Tusk Drift run pages in web app
‍


## **CLI**


### Major Features


- **Onboarding Agent** : Automatically install and initialize the Tusk Drift SDK in one CLI command
- **Secure Replay** : All test replay operations now run in a fenced local sandbox, preventing unintended external requests during testing
- **Suite-Wide Value Matching** : Improved mock matching for more reliable Tusk Drift test replay
- **Multiple Git Remotes Support** : Cloud onboarding now handles repos with multiple git remotes


### DevEx Enhancements


- Added JSON output option for trace list command, enabling programmatic processing
- Better terminal rendering when color support is unavailable
‍


## **SDKs**


### Node.js


- **Turbopack Bundler Support** : Next.js compatibility with modern bundler
- **Instrumentation Manifest Generation** : Build time manifest generation for improved performance


### Python


We're actively building out the Python SDK, will announce more updates in the coming weeks!
