---
schema_version: "1.0.0"
document_id: "aba25b071e07f76f0f4aad92e32f9fe922bba0741eb03526c4e2e1393a520783"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/amazon-ses-automated-email-interactions/"
published_at: "2026-08-07T07:00:00+00:00"
first_seen_at: "2026-08-07T03:27:14.540712+00:00"
fetched_at: "2026-08-07T03:27:16.648914+00:00"
content_hash: "sha256:2b587485ad81c8ddd0f30dd2ac96a2a6cb73f2bfa1ca96d2718e259cd27a4572"
---

# Amazon SES now helps identify automated open and click events in event notifications

Amazon Simple Email Service (SES) now helps you identify automated email interactions. Open and Click event notifications now include an isBotEvent field that indicates whether the event was likely triggered by an automated system or a human recipient.


Customers who publish Open or Click events through a configuration set event destination will now see an isBotEvent field with a value of Likely or Unlikely in event notifications. You can use this field as a signal to help you better understand how much of your engagement is driven by human recipients versus automated systems.


This feature is available in all AWS Regions where Amazon SES is available. If you already publish Open or Click events to an event destination, the isBotEvent field is automatically included, no additional configuration is needed.


To learn more, see[Contents of event data that Amazon SES publishes to Amazon SNS](https://docs.aws.amazon.com/ses/latest/dg/event-publishing-retrieving-sns-contents.html) and[Contents of event data that Amazon SES publishes to Firehose](https://docs.aws.amazon.com/ses/latest/dg/event-publishing-retrieving-firehose-contents.html) in the Amazon SES Developer Guide.
