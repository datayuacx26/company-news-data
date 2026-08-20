---
schema_version: "1.0.0"
document_id: "f91a4fe7cf90ace4337522df37699e0020ec14ad72b15e6e1313bc0e8eb01086"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-entity-resolution/"
published_at: "2026-07-22T19:00:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:851f8144bcb182b82d902f66ef2b39c9a271114ee97d3750b1486b0f5d5b5247"
---

# AWS Entity Resolution now supports advanced real-time matching

AWS Entity Resolution now supports real-time matching with advanced matching workflows, enabling customers to match records in milliseconds using complex rulesets through the GenerateMatchId API. Previously, real-time matching was limited to simple rule-based workflows, while advanced rulesets—which support operators like Exact and ExactManyToMany combined with AND/OR logic—could only be used for batch processing that took minutes to hours. This created a critical gap for customers needing real-time entity resolution with sophisticated matching logic.


With this launch, customers performing fraud detection, real-time account lookup, or website personalization can define advanced matching rules and get results in real-time without maintaining separate matching infrastructure or re-architecting applications. To enable advanced real-time matching, customers set the enableRealTimeMatching parameter to true on their matching workflow, then call the existing GenerateMatchId API—no new endpoints or migration required.


Advanced real-time matching is available in all[AWS Regions](https://aws.amazon.com/entity-resolution/faqs/) where AWS Entity Resolution is available.


To get started, see[Using GenerateMatchId](https://docs.aws.amazon.com/entityresolution/latest/userguide/generate-match-id.html) in the AWS Entity Resolution User Guide.


For more information about AWS Entity Resolution, visit the[product page](https://aws.amazon.com/entity-resolution/) .
