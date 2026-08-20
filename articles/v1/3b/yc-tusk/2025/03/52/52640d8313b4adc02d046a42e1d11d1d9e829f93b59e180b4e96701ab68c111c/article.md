---
schema_version: "1.0.0"
document_id: "52640d8313b4adc02d046a42e1d11d1d9e829f93b59e180b4e96701ab68c111c"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/march-2025-changelog"
published_at: "2025-03-31T09:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:29df1183549921e91887d5b90c5c97156cc0036a2ae1a4834cfa4fe24ebb7bff"
---

# March '25 Changelog

# What's new in Tusk


This March, we've rolled out key improvements to make Tusk faster and more powerful for your unit testing needs.


We've integrated SOTA models like **Claude 3.7** and **Gemini 2.5** into our pipeline. These models allow Tusk to better understand your code and create more relevant, comprehensive tests with less guidance.


Speed was a major focus in this release. Our new **parallel test execution** capability distributes work across multiple test sandboxes, dramatically reducing latency and getting you results faster than ever before.


We've also given you greater flexibility in accepting Tusk-generated tests. You can now select specific Tusk tests (at a test case or symbol level) to incorporate into your PR/MR, as well as incorporate tests from older commits.


## Release notes


- Intelligent usage of SOTA models (Claude 3.7, Gemini 2.5)
- Improved context gathering for test generation
- Ability to load balance across multiple test sandboxes, reduced latency with parallel test execution
- Flexibility when incorporating tests - specific tests, older commits
- Ability to provide feedback on individual tests
- Ability to retry test generation
- Show code coverage gains for pytest and Jest
- Enhanced test iteration (e.g., learn from iteration history, better determination of required action)
- UI/UX improvements for Tusk’s test generation page and PR/MR comments
- Routing requests through static IPs for on-prem GitHub/GitLab deployments
- Better support for Ruby repos


---


TRY TUSK NOW


## AI test generation with codebase context for quality-obsessed teams.


Cover your blind spots, catch verified critical bugs, merge PRs 25% faster with peace of mind.
