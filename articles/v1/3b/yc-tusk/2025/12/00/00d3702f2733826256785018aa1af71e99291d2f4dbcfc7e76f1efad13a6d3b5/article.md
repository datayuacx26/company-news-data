---
schema_version: "1.0.0"
document_id: "00d3702f2733826256785018aa1af71e99291d2f4dbcfc7e76f1efad13a6d3b5"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/november-2025-changelog"
published_at: "2025-12-22T19:22:23.745+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:55742fbe2db20f4a22eacd328734da575ce461f71eba2f5ed4dd6daa387e78dc"
---

# November 2025 Changelog

## **Release Notes**


November marks the public beta launch of Tusk Drift, our API test automation product.


We've shipped an onboarding wizard to get teams up and running on Tusk Drift Cloud within minutes, along with instrumentations for Prisma, MySQL, and Upstash Redis for our Node SDK.


On unit test generation: we've released the ability for Tusk to commit changes to test helpers/utils outside of test files. This, alongside better handling of edge cases and large PRs, means better DevEx for the whole team.


Try Tusk Drift in under 15 seconds by opening our demo repo in a[GitHub Codespace](https://github.com/Use-Tusk/drift-node-demo?tab=readme-ov-file#quick-start) .


## **🏎️ Tusk Drift**


### Major Features


- **Cloud Onboarding Wizard** : New guided setup flow for Tusk Drift Cloud with step-by-step instructions, GitHub workflow examples, and comprehensive documentation to get you recording API traffic in minutes
- **New Instrumentations** : MySQL, Upstash Redis, and Prisma instrumentations with DB operations including lazy query execution and dynamic fragments
- **Real-Time SDK Alerts** : SDK alerts now stream directly to the CLI to surface issues for debugging
- **Analytics Dashboard** : Introduced new Dashboard page for Tusk Drift analytics


### Test Quality Improvements


- **Deviation Analysis Reliability** : Major improvements to unintended deviation detection consistency, making it easier to identify and resolve unintended API behavior changes
- **Environment Tracking** : Better environment context in trace analysis for more accurate test comparisons


### DevEx Enhancements


- **Replay Server Resiliency** : Improved error handling, retry logic, and connection management for more reliable test replays
- **Responsive Terminal UI** : Better support for narrow terminal windows with size warnings and improved text handling
- **Streamlined CLI Auth** : New modal to guide users through GitHub authentication when connecting from CLI
- **Faster Demo Codespace Setup** : GitHub Codespace setup time for our demo repo reduced from 2 minutes to 15 seconds with optimized container configuration
- **Animated Docs** : New animated diagrams in README files for clearer product visualization
- **Cancel In-Progress Runs** : Ability to cancel Tusk Drift runs while in progress
‍


## **🧪 Tusk Tester**


### Major Features


- **New Sign-Up Flow** : Users now get a personalized web app experience after completing our improved sign-up flow


### Test Quality Improvements


- **Auxiliary File Generation** : Inclusion of auxiliary file changes (e.g., test utils/helpers) when committing Tusk-generated unit tests
- **Enhanced Agentic Prompts** : Refined AI prompts for the agentic workflow to produce higher-quality, more contextually appropriate unit tests


### DevEx Enhancements


- **Global Search** : Added CMD+K global search functionality for faster navigation in web app
- **Faster File Classification** : Optimized file analysis and categorization for quicker test generation startup, especially in large monorepos
- **Concurrency Limits** : Added resource management controls for agentic analysis to prevent overloading on large PRs
- **Agentic Workflow Error Handling** : Better handling of edge cases when the agentic workflow encounters unexpected scenarios
- **PR Status Tracking Reliability** : Refactored handling for PR status changes for more reliable updates on Tusk runs
