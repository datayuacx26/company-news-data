---
schema_version: "1.0.0"
document_id: "54500e2c2eae99992fc3df8aff7e0431aebfa3640a3201e6369a953270c74ad3"
company_key: "yc-posthog"
company: "PostHog"
source_id: "yc-posthog-rss-39b8c8c5a5d1"
canonical_url: "https://posthog.com/blog/is-google-analytics-hipaa-compliant"
published_at: "2022-02-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:52.157750+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:741d74b9c2f63beb1f88bcae85d2d430e9633f7383a870658b2f3c6f8a46e210"
---

# Is Google Analytics HIPAA compliant?

# Is Google Analytics HIPAA compliant?


- [Andy Vandervell](https://posthog.com/community/profiles/30208)


Feb 21, 2022


- [Privacy](https://posthog.com/blog/privacy)


,
- [Explainers](https://posthog.com/blog/explainers)


#### Contents


-
-
-


HIPAA, which stands for the Health Insurance Portability and Accountability Act, regulates how individuals and organizations are required to secure, handle and transmit protected health information (PHI) – and the stringent penalties for failing to do so.


Put simply, you should not use Google Analytics if your business is a 'Covered Entity' or 'Business Associate' under HIPAA: **Google Analytics is not HIPAA-compliant** and using it could result in a breach and substantial fines.


This means other tools which rely on Google Analytics, such as the experimentation platform[Google Optimize](https://posthog.com/blog/optimize-to-posthog)


, are also not HIPAA-compliant. Read our guide to[HIPAA-compliant split testing tools](https://posthog.com/blog/best-hipaa-compliant-ab-testing-tools)


if you need an alternative to Optimize as well.


In this article, we'll explain:


1. Why Google Analytics isn't HIPAA-compliant
2. Why product analytics is a better alternative
3. Why self-hosting your analytics is the best way to stay HIPAA-compliant


###


Common HIPAA terms explained


-


**Protected Health Information (PHI):** Also known as personal health information, PHI includes any health data on an individual and any identifying information (e.g. emails, phone numbers, etc.) connected to it. IP addresses, device identifiers and URLs are among the[18 recognized identifiers](https://cphs.berkeley.edu/hipaa/hipaa18.html)


-


**Covered Entity:** A first-party organization (hospital, healthcare provider, etc.) or product (health app, website, wearable device, etc.) that collects any kind of PHI


-


**Business Associate:** A third party that receives and / or manages data on behalf of a Covered Entity


-


**Business Associate Agreement:** An agreement between a covered entity and third party that handles their PHI; it ensures the Business Associate shares the same legal requirements and liability as the Covered Entity


##


Why isn't Google Analytics HIPAA-compliant?


Because Google doesn't allow Covered Entities to enter into a BAA with it, as this[disclaimer](https://support.google.com/analytics/answer/6366371#zippy=%2Cin-this-article)


on its website explains:


> Unless otherwise specified in writing by Google, Google does not intend uses of Google Analytics to create obligations under the Health Insurance Portability and Accountability Act, as amended, (“HIPAA”), and makes no representations that Google Analytics satisfies HIPAA requirements. If you are (or become) a Covered Entity or Business Associate under HIPAA, you may not use Google Analytics for any purpose or in any manner involving Protected Health Information unless you have received prior written consent to such use from Google.


A BAA is necessary because Google Analytics transmits data to Google-owned servers when you use it. Were you to use Google Analytics to process or transmit PHI you would be liable for investigation and a fine.


##


HIPAA fines, explained


HIPAA fines operate on a sliding scale based on the severity of the breach and the total number of breaches. This means they get expensive very quickly, especially for products or businesses with large user bases. The largest HIPAA fine to date is[$16 million](https://www.hipaajournal.com/anthem-inc-settles-state-attorneys-general-data-breach-investigations-and-pays-48-2-million-in-penalties/)


against health insurer Anthem.


Fines aren't limited to large businesses. In 2017, a[children's charity was fined](https://www.hhs.gov/hipaa/for-professionals/compliance-enforcement/agreements/ccdh/index.html)


due to storing PHI on a third-party platform without a BAA.


Subscribe to our newsletter


#### build mode


Read by 75,000+ founders and builders


We'll share your email with Substack


> PostHog is the leading platform for building self-driving products. With a full suite of developer tools –[AI observability](https://posthog.com/ai-observability) ,[product analytics](https://posthog.com/product-analytics) ,[session replay](https://posthog.com/session-replay) ,[feature flags](https://posthog.com/feature-flags) ,[experiments](https://posthog.com/experiments) ,[error tracking](https://posthog.com/error-tracking) ,[logs](https://posthog.com/logs) , and more – PostHog captures all the context agents need to diagnose problems, uncover opportunities, and ship fixes. A[data warehouse](https://posthog.com/data-stack) and[CDP](https://posthog.com/cdp) tie it all together, unifying that context into one source agents can read across. You can steer it all from[Slack](https://posthog.com/slack) ,[the web app](https://posthog.com/ai) , the desktop ([PostHog Desktop](https://posthog.com/desktop) ), or your own editor via[the MCP](https://posthog.com/mcp) .


### Community questions
