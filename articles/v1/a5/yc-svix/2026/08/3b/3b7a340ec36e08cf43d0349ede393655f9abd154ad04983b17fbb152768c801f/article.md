---
schema_version: "1.0.0"
document_id: "3b7a340ec36e08cf43d0349ede393655f9abd154ad04983b17fbb152768c801f"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/changelog-08-2026/"
published_at: "2026-08-17T13:00:00+00:00"
first_seen_at: "2026-08-18T05:16:10.600049+00:00"
fetched_at: "2026-08-18T13:00:01.324182+00:00"
content_hash: "sha256:33c43cba287bac5950c3c9d5b4b9c8529f378ee1c815842907092d2317826da9"
---

# Svix Changelog August 2026

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


Here are the updates for August:


## SDK v2 release candidates


The first release candidates of the v2 SDKs and CLI are out. v2 cleans up long-standing naming quirks:` update` operations are renamed to` upsert` (they were always create-or-update),` filterTypes` is now` eventTypes` , and deprecated operations are removed. See the full list of changes in the[release notes](https://github.com/svix/svix-webhooks/releases/tag/v2.0.0-rc.1) and give it a try before the stable release.


## Polling endpoints v2


A new v2 of the[polling endpoints](https://docs.svix.com/advanced-endpoints/polling-endpoints) API is now available, with updated instructions in the dashboard and docs. Consume it with the` AutoConfigConsumer` in the SDKs.


## GitHub Action for Webhooks AutoConfig


The new[webhooks-autoconfig-action](https://github.com/svix/webhooks-autoconfig-action) configures webhook endpoints straight from CI using[Webhooks AutoConfig](https://docs.svix.com/receiving/webhooks-autoconfig) . Declare the endpoint configuration in code and it stays in sync with every deploy, without manual changes in the management UI.


## AutoConfig for polling endpoints


Polling endpoints can now be set up with Webhooks AutoConfig too: a "use AutoConfig" option when creating one, with instructions for consuming it via the SDK's` AutoConfigConsumer` .


## Ingest: path and query forwarding


[Svix Ingest](https://docs.svix.com/ingest/receiving-with-ingest) now forwards trailing path segments and query parameters from the source URL to your endpoint, via the` svix-ingest-trailing-path-segments` and` svix-ingest-query` headers. Both are also available as transformation parameters.


## S3 endpoints on the Pro plan


The[S3 advanced endpoint](https://docs.svix.com/advanced-endpoints/object-storage) is now available on the Pro plan, no longer Enterprise-only. Deliver messages straight to an S3 bucket, no consumer code required.


## Local time in the dashboard


Stats graphs and other dashboard pages can now display times in your local timezone instead of UTC, with a toggle to switch between the two.


## Connectors for self-hosted Enterprise


Connectors are now available in the self-hosted Enterprise edition, so your users can send events to Slack, Discord, and other services with prebuilt integrations, same as on Svix Cloud.


## FIPS builds for self-hosted Enterprise


Enterprise images are now available as FIPS 140-3 builds on Chainguard bases (paid add-on), and transformations are covered too.


## Diom: HTTP sinks


[Diom](https://diom.svix.com/) can now automatically forward messages published on a topic to an HTTP endpoint with the new HTTP sink, including URL variable interpolation and custom headers. More sink types are on the way.


## Security advisory: SVIXSEC-2026-0001


We published a[full write-up](https://www.svix.com/blog/svixsec-2026-001/) of an SSRF bypass reported by an external researcher. Svix Cloud customers were not affected; open-source users should update to[v1.98.0 or later](https://github.com/svix/svix-webhooks/releases/tag/v1.98.0) .


## Closing words


As always, please let us know if you have any feedback on the product, or anything else we can add or build to make your lives easier. A lot of the Svix product came from customer feedback, and we are sure` ${next_big_thing}` will come from feedback as well. No feedback is too small!


You can[contact us](https://www.svix.com/contact/) directly or join the discussion on[our community Slack](https://www.svix.com/slack/) .


---


For more content like this, make sure to follow us on[X / Twitter](https://x.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
