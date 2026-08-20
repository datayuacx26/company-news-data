---
schema_version: "1.0.0"
document_id: "ae1a284ece4979f62908518c21c5622a554d7a0348c383ac1ce13edaa83589fc"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/announcing-playwright-test-reporter"
published_at: "2024-12-03T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:776b5a84652f7fac0605d7fa687542558ffeccd2ee0838447da6ac2ebc67f513"
---

# Announcing Playwright Test Runner reporting in Artillery Cloud

December 3rd, 2024[Announcement](https://www.artillery.io/blog/tag/announcement)


# Announcing Playwright Test Runner reporting in Artillery Cloud


Hassy Veldstra


Today we are announcing support for Playwright Test Runner reporting in Artillery Cloud. With a two-line change in` playwright.config.ts` you will be able to:


- Monitor Playwright tests in real-time
- View HTML reports with screenshots and traces in Artillery Cloud
- Integrate with GitHub Actions with PR comments for successful and failed test runs


## Getting started


1.


Sign up for an Artillery Cloud account on[https://app.artillery.io](https://app.artillery.io/) and get an API key


2.


Install the Artillery Cloud reporter:


```text
npm   install   -D   @artilleryio/playwright-reporter
```


```text
pnpm   add   -D   @artilleryio/playwright-reporter
```


```text
yarn   add   --dev   @artilleryio/playwright-reporter
```


```text
bun   add   --dev   @artilleryio/playwright-reporter
```


3.


Enable the reporter in your` playwright.config.ts` file:


playwright.config.ts


```text
export   default   defineConfig  ({
reporter: [
[  '@artilleryio/playwright-reporter'  , { name:   'My Test Suite'   }],
],
// ... rest of your config
});
```


4.


Set your Artillery Cloud API key


```text
export   ARTILLERY_CLOUD_API_KEY  =  a9_your_api_key
```


You can create an API key in Artillery Cloud here:[https://app.artillery.io/settings/api-keys](https://app.artillery.io/settings/api-keys)


Then run your Playwright tests as normal, e.g. with:


```text
npx   playwright   test
```


The reporter will print a link for the test run to the console. You can track the progress of the test run in real-time in Artillery Cloud via that link.


If the test is running in GitHub Actions, the reporter will also post a comment on the PR with the link to the test run once it completes.


## How much does this cost?


This feature is available to all users of Artillery Cloud, including those on the free Developer plan. You can record up to 100 test suite runs every month for free. Detailed pricing information is available on[https://app.artillery.io/settings/billing](https://app.artillery.io/settings/billing)


## Learn more


Please see the documentation for the reporter on[https://www.artillery.io/docs/reference/extensions/playwright-reporter](https://www.artillery.io/docs/reference/extensions/playwright-reporter)
