---
schema_version: "1.0.0"
document_id: "d470f1eb96b899d7ef949f0bb76b62b97fc11d1b73f2566391cf1e261b9ce56f"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/january-2026-changelog"
published_at: "2026-02-05T00:27:00.690+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:21:35.013216+00:00"
content_hash: "sha256:5b60226701b87c68267a0703210d3615b5c35bc38de6b99906ee8151ffc8b883"
---

# January 2026 Changelog

## **Release Notes**


January marked the release of two-click onboarding **** for unit test generation as well as our Python SDK for Tusk Drift.


The former is part of a major initiative to provide agentic setup for both our unit test and API test generation products. In a few clicks, users can go from an untested repo to generating/running unit tests in minutes, with our agent handling Dockerfile creation, test scripts, and coverage setup.


The latter means our API test automation now supports Python repos (3.9+) with Django, Flask, and FastAPI services. It has instrumentations like gRPC, aiohttp, urllib3, HTTPX, Kinde, and PyJWT. In other news, we revamped our global UI, including a streamlined billing experience.
‍


## **Platform**


### Major Features


- **Two-Click Unit Test Onboarding** : Automatically start generating unit tests for your repo in two clicks; our setup agent detects test frameworks, creates config files, and verifies the test environment works within minutes
- **Streamlined Billing** : Improved billing experience with native checkout and subscription management. All sign-ups now come with 14-day free trials of our Business Plan.
- **UI Revamp** : Fresh new design with a top navbar, font update, refined dark mode, and improved hover states and icons across the app
- **Prompt Fix in PR Comment** : Provides a copy-paste prompt for other AI coding agents to make suggested fixes for failing tests


### Test Quality Improvements


- **CoverBot Selection Guidelines** : Configure file and symbol selection criteria using natural language (e.g., “focus on utility functions”)
- **Improved Deviation Analysis** : Addressed edge case causing inefficient deviation analysis on large PR diffs


### DX Enhancements


- **Branch Name Customization** : Configure custom branch naming templates for both PR test incorporation (e.g.,` tusk-tests-{{prBranchName}}-{{timestamp}}` ) and CoverBot runs (e.g.,` tusk-coverbot-{{configName}}-{{datetime}}` )
- **Improved Repo Management** : Repo organization into enabled/disabled sections and glob pattern support for ignoring base branches
‍


## **CLI**


### Major Features


- **Setup Verification** : Validate that an existing Tusk Drift setup is working correctly with` tusk setup --verify` . Re-records and replays traces against configured endpoints without going through the full setup agent. **‍**
- **Automated Cloud Setup Flow** : Setup agent does complete end-to-end setup for Tusk Cloud with trace upload, suite validation, and automatic authentication


- **Python SDK Setup** : Setup agent now fully supports onboarding Python projects with Django, Flask, and FastAPI services
- **Auto-Update (Enabled by Default)** : The CLI automatically checks for and applies updates. Disable with` --no-auto-update` if needed
- **Span Caching** : Local caching of recorded spans for faster replay execution and reduced network overhead


### DX Enhancements


- **Improved TUI Stability** : Fixed escape key handling, optimized log panel rendering, and resolved potential deadlocks in interactive mode
- **Clearer Setup Guidance** : Improved messaging during cloud setup with better next-step guidance and phase descriptions
- **Auto-Update .gitignore** : The setup agent automatically adds Tusk-specific entries to your` .gitignore`
- **Span Tree View** : The` tusk list` command now displays spans in a hierarchical tree structure for easier trace visualization
‍


## **SDKs**


### Python


- **New Instrumentations** : Added support for gRPC clients, Kinde auth, aiohttp, urllib3, httpx, and pyJWT. Python 3.9+ is supported.
- **Performance & Resilience Improvements** : Optimized SDK for production workloads with reduced overhead and improved resilience under high-throughput conditions


### Node.js


- **Improved Mock Matching Logs** : Human-readable match metadata logging with type, scope, description, and score for easier debugging
- **HTTP Error Status Fix** : Only HTTP 4xx+ responses are now treated as errors (previously 3xx+ were incorrectly flagged). HTML responses are now accepted content types.


‍
