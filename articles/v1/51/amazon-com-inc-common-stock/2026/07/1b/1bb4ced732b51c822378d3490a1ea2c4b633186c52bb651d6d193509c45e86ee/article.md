---
schema_version: "1.0.0"
document_id: "1bb4ced732b51c822378d3490a1ea2c4b633186c52bb651d6d193509c45e86ee"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/07/aws-waf/"
published_at: "2026-07-29T15:00:00+00:00"
first_seen_at: "2026-07-29T23:44:37.452284+00:00"
fetched_at: "2026-07-29T23:44:39.530052+00:00"
content_hash: "sha256:af579d2506526be173ad9ca51f9e0b8b29365a3a0ec5f1d5fbe847ca023004e1"
---

# AWS WAF adds pre-parse text transformations and new text transformations

Today, AWS WAF adds pre-parse text transformations for query arguments and ten new text transformations for use in any rule statement. Both help you normalize request content so that AWS WAF inspects requests the same way your application interprets them.


Pre-parse text transformations normalize a raw query string before AWS WAF parses it into key-value pairs, closing HTTP parameter pollution and parser differential evasion gaps. You can chain up to ten transformations, including URL decode, Combine Duplicate Query Arguments by Comma, and Replace Semicolons with Ampersands, then layer standard post-parse transformations on top within a single rule statement.


The new text transformations give you more ways to normalize content before inspection, including industry-standard options such as Uppercase, Trim, Remove Whitespace, and SHA256, plus operating-system-aware command line and JavaScript decoding functions developed by the Amazon Threat Research Team.


Each new transformation consumes 10 WCUs, with no additional charge beyond standard AWS WAF pricing, and is available in all AWS Regions. To get started, see the following resources:


-


Pre-parse text transformations in AWS WAF:[https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-preparse-transformation.html](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-preparse-transformation.html)


-


Text transformations in AWS WAF:[https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-transformation.html](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-transformation.html)


-


Getting started with AWS WAF:[https://docs.aws.amazon.com/waf/latest/developerguide/getting-started.html](https://docs.aws.amazon.com/waf/latest/developerguide/getting-started.html)
