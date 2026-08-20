---
schema_version: "1.0.0"
document_id: "a97b4a174a9fcad4d8c857880b2d30ee30fe40cceabc35b6100fc0ed9d0a79a6"
company_key: "yc-reworkd"
company: "Reworkd"
source_id: "yc-reworkd-news-import-d89d796ce0ad"
canonical_url: "https://www.reworkd.ai/blog/april-2025-product-update"
published_at: null
first_seen_at: "2026-07-22T11:47:56.930734+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:8438cdb4b09f7a961b352b32b921bb50c0a85e02aabb268ccce474930df69e1c"
---

# April Product Update

This month we launched browser tracing, inline code diffs, and a weekly summary email to keep you in the loop.


# Browser Traces


Initially, we used Playwright for viewing traces but their trace viewer is buggy, hard to understand, and lacks key trace information.


We’ve now built our own trace viewer that captures and displays Reworkd/Playwright events, console logs, network activity, and CDP events—all in one place.


This allows you to pinpoint exactly where and why a job failed (e.g., blocked resources, network timeouts or slow loads, script errors, and more) so you can debug and fix issues quickly.


# Inline Code Diffs


Previously, it was difficult to understand which lines of code the agent changed when new code was generated.


With inline code diffs, you can now see the exact side-by-side additions and deletions between the newly generated code and the previous code, making it easy to spot changes and revert them if needed.


# Weekly Email


Pro & Enterprise users now receive a weekly report email with metrics like:


- Total new outputs count (with week-over-week percentage change)
- Sources with the largest week-to-week data swings—biggest spikes or dips.


This email helps you stay up to date on scraper health without logging into the platform and gives you a better understanding of where your data is coming from.


---


Stay tuned for more exciting product updates by following us on[Twitter](https://x.com/ReworkdAI) or[LinkedIn](https://www.linkedin.com/company/reworkd) .


If you're interested in trying any of these features, test them at[https://app.reworkd.ai/](https://app.reworkd.ai/)
