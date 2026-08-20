---
schema_version: "1.0.0"
document_id: "4b983872eaa621d9c740bfe728d9e4237652e5d20ab8e039d94e61c685349768"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/changelog-05-2026/"
published_at: "2026-05-12T13:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:a0257577d62c8a19bda6a1aa8220a971c31234d2dfdbf03da1c724dcfe941dc5"
---

# Svix Changelog May 2026

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


May is here, and we're excited to share another round of updates from the Svix team.


In case you missed it, we have previously[released Diom](https://www.svix.com/blog/announcing-diom/) , a new open source project for building robust backend applications.


Here are the updates for May:


## New docs website


We gave[the docs website](https://docs.svix.com/) a facelift as well as added better search, navigation, and general experience.


We've also added a dedicated docs page for message tags and a few other content improvements!


## Listing attempts shows canceled messages as canceled


Before this change, attempts that were canceled[due to transformations](https://docs.svix.com/transformations) would show as "Successful". With this change, canceled attempts now correctly show as canceled. The API still returns "Successful" for these messages in order to not break backwards compatibility, though we are working on a new version that will support it in the API as well.


## FIPS-140-3 compliant Enterprise Edition image


The Svix Enterprise Edition image is now available in a FIPS-140-3 compliant configuration. Customers with strict cryptographic compliance requirements can now run Svix on-premise while meeting their FIPS obligations.


We've had customers using it since early 2025, but it's finally out of incubation!


## App portal Replay button redesign


The Replay control in the app portal has been redesigned to be easier to discover. The icon-only button in the messages table has been replaced with a clearly labeled "Replay" text button, and the endpoint header now exposes Replay actions and endpoint edit/delete actions as separate buttons rather than hiding them behind a single ellipsis menu.


## Ingest improvements


We've shipped a batch of fixes and improvements to Ingest based on user feedback, including a fix for the app portal breaking when switching tabs, plus ongoing work on better activity visibility for FIFO endpoints and a usage screen tailored to Ingest. This is in addition to adding new ingest types.


## Closing words


As always, please let us know if you have any feedback on the product, or anything else we can add or build to make your lives easier. A lot of the Svix product came from customer feedback, and we are sure` ${next_big_thing}` will come from feedback as well. No feedback is too small!


You can[contact us](https://www.svix.com/contact/) directly or join the discussion on[our community Slack](https://www.svix.com/slack/) .


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
