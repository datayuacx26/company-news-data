---
schema_version: "1.0.0"
document_id: "206db15bf22f6b464b2cf1baca4f8bf75d40b59ec557b677413454977a7cb481"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/january-2025-changelog"
published_at: "2025-01-31T09:32:14+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:78de67582b7a3e620989a006a22757d6310d4472e4cb85c0273024eb32066188"
---

# January '25 Changelog

# What's new in Tusk


Happy New Year from the Tusk team!


This January, we've added **GitLab** support, **Ruby-based** testing frameworks, as well as integrations with **Jira and Linear** for business context.


We've improved the reliability and accuracy of our test generation agent. Customers can now also provide **custom instructions** to Tusk via our UI so that it generates tests in line with their testing guidelines.


## New releases


- GitLab support
- Ruby-based testing frameworks support
- Jira and Linear integration for extracting business context from tickets
- Better handling of monorepos with large number of packages
- Improved agent self-iteration on test code when encountering errors
- Improved agent filtering on important test cases
- Gathering additional symbol context from codebase when generating test cases
- Improved classification of PRs to reduce noise from Tusk Tester
- Show potential fix when Tusk detects a bug in a PR
- Tusk UI - Customization page for providing testing guidelines to the agent
- Tusk UI - Viewing test generation output
