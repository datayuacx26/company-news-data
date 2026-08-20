---
schema_version: "1.0.0"
document_id: "0e91e012ebabb352ac070c5ba6567be4bb4174323a793ccbdb60eb438cd80be5"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/performance-trends-http"
published_at: "2025-06-04T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:4f3f48fcd5a5041d3cd5c5049a1b94c2886a6ad6265e546cfb37e86b58e6d6e0"
---

# Performance trends for HTTP tests

June 4th, 2025[Dashboard](https://www.artillery.io/changelog/tag/dashboard)


# Performance trends for HTTP tests


You can now see trends across multiple runs of a load test in the new **Performance Trends** view in Artillery Cloud.


Performance Trends view provides a summary of test health, and changes across key metrics in a test:


- The number of virtual users (VUs), with a breakdown for successful/failed VUs
- Pass/fail breakdown of[ensure checks](https://www.artillery.io/docs/reference/extensions/ensure) , if defined
- HTTP request rate and throughput
- HTTP response time (p95 and p99)
- Test duration


Trends are calculated automatically for all load test runs, and are grouped by test name.


To see trends for a load test, you need to have at least 10 distinct runs of that test in the previous 90 days.


Performance Trends are available to all users on Team, Business, and Enterprise plans. There is no additional cost for using Performance Trends.
