---
schema_version: "1.0.0"
document_id: "45d3e62a03a53cff3037f633d6b4834789b204593abd4bb403091b0debf6ad51"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/svix-otel-capabilities/"
published_at: "2025-12-18T12:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T22:24:52.050374+00:00"
content_hash: "sha256:c751ccd68ae091a804c9a07ef0edff6e23de46d21d80ab51ecaaf482f21b002b"
---

# Svix's New OpenTelemetry Features

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


OpenTelemetry is a leading open-source toolkit for observing application performance, offering support for tracing, metrics, and logging. At Svix, we've been using OpenTelemetry for years both to monitor our platform internally and to make OpenTelemetry data available directly to our Enterprise customers, allowing them to better understand both our platform and their own customers' usage of it.


Recently, we've extended our OpenTelemetry capabilities for customers even further, allowing Enterprise customers to receive operational webhooks as traces instead of webhooks.


## Reporting Message Attempts as Traces


Svix has long supported sending message attempts as traces, allowing you to track the timing, outcome, and number of attempts required to deliver your events to your customers. See #[our documentation](https://docs.svix.com/opentelemetry-streaming) for more details.


## Powerful new features


Recently, Svix has added support for sending all[Operational Webhooks](https://docs.svix.com/incoming-webhooks) as traces. Now you can integrate operational webhook events directly into your APM and infrastructure monitoring tools!


If you register an endpoint using the "OTEL Operational Webhooks" Connector, you will receive traces instead of webhooks for the selected event types. Examples include events when endpoints are created (` endpoint.created` ) or updated (` endpoint.updated` ), when message deliveries to your customers are repeatedly failing (` message.attempt.failing` ), or when endpoints are disabled due to repeated failures (` endpoint.disabled` ). This information gives you broad visibility into your customers' usage of Svix, and even the performance of their infrastructure, that you can automatically integrate into your existing application monitoring tools. As always, you can also add your own custom OTEL attributes to the spans using application metadata.


## Summary


In conclusion, operational webhooks are no longer just webhooks anymore: You can consume them as OpenTelemetry traces! Got any thoughts or suggestions for how to further improve our OpenTelemetry capabilities? Hit us up!


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
