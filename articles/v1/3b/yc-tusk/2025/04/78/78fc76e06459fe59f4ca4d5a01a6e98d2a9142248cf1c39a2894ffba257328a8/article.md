---
schema_version: "1.0.0"
document_id: "78fc76e06459fe59f4ca4d5a01a6e98d2a9142248cf1c39a2894ffba257328a8"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/april-2025-changelog"
published_at: "2025-04-30T09:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:387ea56eb07dedbc2843859b0d0d01cdb945b74353d1558b266bfbe3734114f2"
---

# April '25 Changelog

# What's new in Tusk


The big news this April is that we've released our[self-serve onboarding flow](https://docs.usetusk.ai/automated-tests/self-serve) so that teams can set Tusk up and start seeing unit test generation in their repo within 10 minutes.


You can now enjoy faster test generation with Tusk. We've reduced run latency by 25% with **caching for LLM requests** and better test concurrency controls.


The latency reduction is complemented by our new **Strict Mode** feature that focuses test generation specifically on code changes in your PR, saving time and improving relevance.


We've also added **AI-powered test generation summaries** in Tusk's PR comment, giving you more immediate insights into test results and potential issues in your code.


## Release notes


🚀 **Major Features**


- **Self-Serve Onboarding** - Released end-to-end self-serve workflow to set up Tusk test environments
- **Strict Mode for Focused Test Generation** - Added new mode that restricts Tusk's test generation only to changes introduced in PRs
- **Support for Frameworks Without Unique Test Files** - Added configuration to support test frameworks like JUnit that require test file paths and class names to match
- **Model Selection Optimizations** - Optimized model usage across Tusk's workflow for faster and more accurate test generation
- **AI Test Generation Summary** - Added AI-powered summaries in Tusk's PR comment to highlight test results and key issues with the PR


💫 **Performance Improvements**


- **LLM Request Caching** - Added caching for LLM requests to reduce test generation latency
- **Task Timeout Increase** - Extended testing sandbox manager timeout to better handle large projects
- **Improved Error Handling** - Better error propagation and logging for workflow issues


🎨 **UI/UX Enhancements**


- **Progress Bar** - Added detailed progress tracking for Tusk's test generation in the PR for better visibility
- **Hiding Tests From View** - Added ability to hide/unhide tests in the testing commit check page for cleaner review
- **Improved Context Handling** - Better error handling for large PRs and dependencies


---


TRY TUSK NOW


## AI test generation with codebase context for quality-obsessed teams.


Cover your blind spots, catch verified critical bugs, merge PRs 25% faster with peace of mind.
