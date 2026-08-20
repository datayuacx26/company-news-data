---
schema_version: "1.0.0"
document_id: "76a1a4f7918232d0eac6c3f71d7121ce9171981ab5afed22a22fafa63d67d4a1"
company_key: "yc-mergent"
company: "Mergent"
source_id: "yc-mergent-rss-c4a67b378e23"
canonical_url: "https://blog.mergent.co/introducing-task-rate-limits"
published_at: "2024-02-22T16:53:25+00:00"
first_seen_at: "2026-07-25T13:51:58.649676+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:f6bf0f8a21a0af623c71025a3b938f6aabceefebdd858a88a2974d85a04c1e10"
---

# Introducing Task Rate Limits (Beta)

We're excited to announce Task Rate Limits! This new feature allows you to set maximum dispatch rates for tasks to your applications, with limits ranging from 1 to 500 tasks per second or minute.


Task Rate Limits are implemented to help avoid overloading applications with an excessive number of tasks simultaneously. By leveraging a *sliding log algorithm* , Mergent accurately maintains the dispatch rate within the defined limits across any given time window.


Rate limits can be adjusted anytime via the Mergent Dashboard, enabling developers to dynamically manage application throughput based on current performance and workload expectations.


**Default Configuration** : The default rate limit is set to 500 tasks per second per project.


This feature is available in beta today, and we'll be collecting feedback and data for further improvements. Let us know what you think.


Head over to the[task rate limits docs](https://docs.mergent.co/concepts/task-rate-limits) to learn more.
