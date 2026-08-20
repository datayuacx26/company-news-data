---
schema_version: "1.0.0"
document_id: "f36a1508dfa9e4b55bf1d39dcd846347e63a15164577a20f1654e00b94478e7c"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-2c90f49ee813"
canonical_url: "https://www.artillery.io/changelog/new-playwright-report"
published_at: "2025-05-26T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.020779+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:a18d3e9286c2ea379ff65d17c6eff1586c126b2a475d09d1ef7f21f4c5f4cc4e"
---

# Playwright Test Runner reporting

May 26th, 2025[Dashboard](https://www.artillery.io/changelog/tag/dashboard)


# Playwright Test Runner reporting


Edmundo Santos


We’ve rebuilt Playwright Test Runner reporting in Artillery Cloud from the ground up with a focus on performance and usability.


## Real-time reporting


Playwright test reports now update in real-time as a test is running, and you can start investigating test failures as soon as they happen.


Traces, screenshots, and other attachments are available for each test case immediately, and can be viewed directly in the report.


## Share reports


You can create a sharing URL with the **Share** button to share reports with people outside of your Artillery Cloud organization.


## Track Web Vitals metrics (optional)


[Web Vitals](https://web.dev/articles/vitals) metrics can be captured and reported for every page in your tests automatically.


This feature needs to be enabled in your tests - see the[@artilleryio/playwright-reporter docs](https://www.npmjs.com/package/@artilleryio/playwright-reporter/v/1.1.3#performance-tracking) for more informaion.


## ”Copy prompt” support


Playwright v1.51.0 and later automatically attaches a prompt with useful context for every failing test case. You can copy that prompt with the “Copy prompt” button in the report and in the trace viewer.


This feature requires Playwright v1.52 or later.


## ` captureGitInfo` support


The report will show the details of the Git commit if` captureGitInfo: { commit: true }` is enabled in your` playwright.config.ts` file (see[Playwright docs](https://playwright.dev/docs/api/class-testconfig#test-config-capture-git-info) for details).
