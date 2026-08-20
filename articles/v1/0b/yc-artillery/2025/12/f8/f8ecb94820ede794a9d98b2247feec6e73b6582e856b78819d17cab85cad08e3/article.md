---
schema_version: "1.0.0"
document_id: "f8ecb94820ede794a9d98b2247feec6e73b6582e856b78819d17cab85cad08e3"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/load-test-report-improvements"
published_at: "2025-12-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:c8e7b1e6517568cb5ca5ff2c7f44da3003dd330c965e627ece0d24a12a0dd08a"
---

# Load test report improvements

December 8th, 2025[Dashboard](https://www.artillery.io/changelog/tag/dashboard)


# Load test report improvements


We have significantly improved the performance of load test reports for tests with many distinct metrics: memory consumption was reduced, Text logs and Activity widgets open much faster, and the Chart builder widget can now handle thousands of metrics.


## Chart tooltip sync


Tooltips are now synchronized across visible charts making it easier to compare metrics. We also improved the tooltip design and tweaked chart legends to take less space on the page, further improving the readability of reports.


## Export as PDF


We have added a new option under the test view *more* menu (…) so you can now export test reports as PDF.


## Misc


- Add search functionality to Text log widget
- Add loading state to Metrics page
- Add missing empty state to Apdex widget
- Fix chart navigator not showing reliably across different charts
- Fix charts having incorrect zoom level on page load
- Fix some dates not showing correctly when using UTC
