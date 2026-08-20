---
schema_version: "1.0.0"
document_id: "f9a4483f5c2f34d7ef344518fa3a80c8772426646fa9952f82b378032887bd08"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/mediatail-configurable-ad-timeout-and-concurrency"
published_at: "2026-07-27T16:09:00+00:00"
first_seen_at: "2026-07-27T17:22:15.810094+00:00"
fetched_at: "2026-07-28T20:32:04.512542+00:00"
content_hash: "sha256:6e100bdb012d2d4def3df2d99c473d548e249909d46be7dc446675626a77d7a1"
---

# AWS Elemental MediaTailor adds configurable ad timeout and concurrency controls for improved ad fill and faster startup

AWS Elemental MediaTailor now gives you direct control over ad decision server (ADS) timeout. Previously, changing these settings required contacting AWS Support. You can now configure individual HTTP ad request timeouts, total ad personalization time budgets for live, VOD, and live ad prefetch, and enable parallel ADS requests.


These settings allow you to optimize ad delivery performance for your specific workflows. For example, you can increase the personalization time budget for live events to improve ad fill rates or enable parallel ADS requests in VOD workflows to reduce overall response time for faster video startup. New prefetch-specific timeout settings give you additional granularity for livestream ad retrieval.


You can configure these settings through the AWS Elemental MediaTailor console, AWS CLI, or AWS SDKs using the new AdsPersonalizationTimeouts and AdsPersonalizationConcurrency parameters on your playback configurations.


This feature is available in all AWS Regions where AWS Elemental MediaTailor is available.


To learn more about configuring ADS request timeouts, personalization time budgets, and concurrency, see[Advanced settings](https://docs.aws.amazon.com/mediatailor/latest/ug/configurations-create.html#configurations-advanced-settings) in the AWS Elemental MediaTailor User Guide.
