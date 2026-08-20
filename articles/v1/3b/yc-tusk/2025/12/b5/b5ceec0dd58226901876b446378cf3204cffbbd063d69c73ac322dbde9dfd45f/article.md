---
schema_version: "1.0.0"
document_id: "b5ceec0dd58226901876b446378cf3204cffbbd063d69c73ac322dbde9dfd45f"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-news-import-c7466264c659"
canonical_url: "https://www.usetusk.ai/resources/october-2025-changelog"
published_at: "2025-12-22T19:22:23.745+00:00"
first_seen_at: "2026-07-26T03:20:25.565537+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:cc5f3f625754e2fad5171e19668ce7536958cff79bdce9754215bb4771d7b957"
---

# October 2025 Changelog

## Release Notes


October brings Tusk Drift closer to full public beta.


Our API test automation product now has comprehensive instrumentations for modern software enginering teams. Drift SDK now supports Next.js apps and instruments MySQL, Redis, Firestore, and gRPC, among other commonly-used packages.


We've also added AI-powered deviation analysis to our Cloud offering to help developers identify unintended API behavior changes and get suggested fixes. You can play around with the product in a GitHub Codespace in our[public repo](https://github.com/Use-Tusk/drift-node-demo?tab=readme-ov-file#quick-start) .


On the unit test generation side (Tusk Tester), we've introduced a new agentic workflow with coverage-guided testing, and refreshed our model selection. These improvements make Tusk smarter, 2x faster, and more adaptable to large repos––whether web or mobile.


## **🏎️ Tusk Drift**


### Major Features


- **Automated Deviation Analysis** : Automatically identify API response deviations when trace tests fail, backed up by AI-powered root cause analysis and suggested fixes
- **Next.js Support** : Full framework support for Next.js apps
- **Database Instrumentations** : Comprehensive database recording support for MySQL2, ioredis (Redis), and Firestore with connection pooling
- **gRPC Support** : Complete gRPC instrumentation for outbound requests in the Node SDK
- **Docker Support** : Full Docker integration in the CLI for running Tusk Drift in containerized environments with improved onboarding wizard
- **Advanced Mock Matching** : Levenshtein-based similarity scoring for intelligent schema-level mock matching


### Test Quality Improvements


- **Performance Filtering** : Multi-level sampling rates, automatic filtering of static endpoints, and blocking of oversized traces (>1MB) to keep test suites lean
- **Test Result Validation** : LLM-generated trace test descriptions and improved test result displays showing all outbound calls
- **Smart Test Suite Maintenance** : Automatic removal of failed trace tests from suite on PR merge to prevent flaky test accumulation
- **API Resilience** : Automatic retry logic for Tusk API requests


### DevEx Enhancements


- **Performance Optimization** : Automated cron jobs to remove orphaned spans and improve application performance
- **Enhanced Non-Interactive Mode** : Streaming results and progress bars for better CI/CD integration
- **Polished Terminal UI** : Clearer deviation displays with actionable next steps included
- **GitLab CI Support** : Native support for GitLab environment variables for seamless CI metadata validation


## **🧪 Tusk Tester**


### Major Feature


- **Speedier Agentic Workflow** : Deployed new agentic test generation workflow for select customers; unit tests are generated 2x faster for large monorepos


### Test Quality Improvements


- **Improved Coverage Parsing** : Better Go coverage parsing for more accurate coverage-guided test generation
- **Model Refresh** : Upgrades to latest SOTA models for faster, higher-quality test generation


### DevEx Enhancements


- **Faster CI Checks** : Dependency caching for frontend and backend CI checks significantly reduces build times
- **Test Environment Deletion** : Ability to delete test execution environments to help with cleaning up repo configurations
- **Complete Dark Mode** : Finished dark mode support across all application surfaces


‍
