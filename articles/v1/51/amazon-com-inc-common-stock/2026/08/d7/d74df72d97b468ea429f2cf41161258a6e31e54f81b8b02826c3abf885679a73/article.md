---
schema_version: "1.0.0"
document_id: "d74df72d97b468ea429f2cf41161258a6e31e54f81b8b02826c3abf885679a73"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ses-supports-customurl-deeplinking"
published_at: "2026-08-14T07:00:00+00:00"
first_seen_at: "2026-08-14T20:55:53.420903+00:00"
fetched_at: "2026-08-14T20:55:58.206980+00:00"
content_hash: "sha256:7ae1cb9a5d78c0725c09b11fa57f76611d987e273da27160fb2b02edabd61015"
---

# Amazon SES click tracking now supports custom URL paths for mobile app deep linking

[Amazon Simple Email Service (SES)](https://aws.amazon.com/ses/) now makes it easier to support mobile deep linking with the new ses:custom-path HTML attribute. When you add this attribute to an <a> tag, SES carries your path segment through to the tracking URL, so mobile operating systems can match it to your app's Universal Links (iOS) or App Links (Android) configuration. This enables you to use mobile deep linking without disabling engagement tracking.


This feature is available in all AWS Regions where Amazon SES is available. To use this feature, you need a custom redirect domain for click tracking with an Apple App Site Association (AASA) or Digital Asset Links verification file hosted on that domain. Then, add the ses:custom-path attribute to links in your HTML emails.


To learn more, see[Configuring custom domains to handle open and click tracking](https://docs.aws.amazon.com/ses/latest/dg/configure-custom-open-click-domains.html) and the[Amazon SES email sending metrics FAQs](https://docs.aws.amazon.com/ses/latest/dg/faqs-metrics.html) in the Amazon SES Developer Guide.
