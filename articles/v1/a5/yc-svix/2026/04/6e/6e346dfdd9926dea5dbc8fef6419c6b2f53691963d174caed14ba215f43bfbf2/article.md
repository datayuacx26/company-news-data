---
schema_version: "1.0.0"
document_id: "6e346dfdd9926dea5dbc8fef6419c6b2f53691963d174caed14ba215f43bfbf2"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/changelog-04-2026/"
published_at: "2026-04-21T13:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:8c62bb6d9718994a9df156f976249b727e0cad3ff17631fc8e690f1c17cf18d6"
---

# Svix Changelog April 2026

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


April is here, and we're excited to share another round of updates from the Svix team!


First of all, in case you missed it, we announced[Diom](https://diom.svix.com/) , a Component Platform for building robust backends. More on that below.


Here are the updates for April:


## Diom: the Components Platform


We are very excited to announce the release of[Diom](https://diom.svix.com/) , a backend components platform for building robust services.


The Components Platform is a new approach that makes it easier to build better services. It's a self-contained service you can run in your own infrastructure that implements all the common primitives you need when building backends such as cache, rate-limiting, idempotency, queue, and more.


We've been working hard on Diom for the last 6 months, though there's still a lot we are looking to add to it; including very tight integration with Svix for both webhooks consumers and producers.


For more information please read[the full announcement post](https://www.svix.com/blog/announcing-diom/) .


## Application analytics


The application screen in the dashboard now offers richer analytics. You can filter message attempts by more flexible date ranges, break down attempts by event type, and see counts of succeeded vs. failed attempts.


## OAuth 2.0 token endpoint custom headers


You can now pass custom headers to the OAuth token endpoint when Svix fetches access tokens on your behalf. This is in addition to the existing support for custom body parameters.


While most customers won't need this, this is required for supporting some non-standard OAuth implementations.


## Server CA certificate override


Svix lets customers on the enterprise tier override the CA certificate used when making webhooks on a per endpoint basis, though until recently it was tied to mTLS configuration (you had to configure both).


This month we've decoupled the configuration so that you can override the CA certificate override without also setting up mTLS.


## App portal: endpoint secret visibility control


The "View endpoint secret" button in the app portal is now correctly disabled when the app portal capabilities don't permit it. Previously, the button appeared enabled even when the capability was restricted.


## Closing words


As always, please let us know if you have any feedback on the product, or anything else we can add or build to make your lives easier. A lot of the Svix product came from customer feedback, and we are sure` ${next_big_thing}` will come from feedback as well. No feedback is too small!


You can[contact us](https://www.svix.com/contact/) directly or join the discussion on[our community Slack](https://www.svix.com/slack/) .


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
