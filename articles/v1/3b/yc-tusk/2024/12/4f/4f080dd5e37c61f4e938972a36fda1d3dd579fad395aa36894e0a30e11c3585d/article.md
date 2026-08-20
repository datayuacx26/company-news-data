---
schema_version: "1.0.0"
document_id: "4f080dd5e37c61f4e938972a36fda1d3dd579fad395aa36894e0a30e11c3585d"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/december-2024-changelog"
published_at: "2024-12-31T09:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:3b82042f78a3a8cbe14b59e7cc936ae6282b8db56d463c885924a4bfeb04d5d2"
---

# December '24 Changelog

# What's new in Tusk


Improved format for Tusk's test generation output in GitHub


Our goal in December was to build both reliability and speed into our AI test generation agent.


Across our customers, **69% of Tusk-generated test suites** were incorporated into PRs. Tusk's test generation run now takes between **4 to 8 minutes** , depending on the size of the PR.


We did this by boosting the quality of our net new tests (i.e., unit and integration tests written where there were **no existing tests** ) with better handling of context and filtering of test scenarios.


We also made Tusk's retrieval of codebase context (e.g., symbol definitions and usage) **3x faster** using the Language Server Protocol.


## New releases


- Improved ability to generate net new tests for PRs without existing tests
- Increased speed of codebase context retrieval by 3x using the Language Server Protocol
- Implemented AST to find symbol definition and usage
- PHP and Go support
- Consider code comments when determining expected behavior
- Improved test file path localization
- Support for multiple test execution configs per repo
- Mark as helpful/unhelpful feature on Tusk GitHub check
- Updated Tusk check output for clarity
