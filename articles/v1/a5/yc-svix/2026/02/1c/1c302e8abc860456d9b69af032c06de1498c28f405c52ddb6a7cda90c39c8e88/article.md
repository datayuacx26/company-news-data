---
schema_version: "1.0.0"
document_id: "1c302e8abc860456d9b69af032c06de1498c28f405c52ddb6a7cda90c39c8e88"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/changelog-02-2026/"
published_at: "2026-02-11T13:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T22:20:57.996558+00:00"
content_hash: "sha256:e3d06c16e4e69225b5f56a6ec9226c3b3b402c2a2bc5bebb950f50ae80f2f7ad"
---

# Svix Changelog February 2026

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


February is here, and we're excited to share another round of updates from the Svix team.


Here are the updates for February:


## Bulk replay successful messages


Svix now supports[bulk replay](https://api.svix.com/docs#tag/Endpoint/operation/v1.endpoint.bulk-replay) which lets you bulk replay all messages sent to an endpoint based on some common filters such as time, event type, status, and channel. This makes it easy for your customers to bulk redrive messages that they have already previously processed successfully.


This is in addition to the previously supported[recover failed](https://api.svix.com/docs#tag/Endpoint/operation/v1.endpoint.recover) and[replay missing](https://api.svix.com/docs#tag/Endpoint/operation/v1.endpoint.replay-missing) .


## App portal: support multiple email addresses for notifications


The app portal now supports configuring multiple email addresses for notifications. This makes it easier to keep your entire team informed about important webhook events and system updates without having to rely on a single point of contact or a common mailing address.


## App portal: support automatic dark mode detection


Until this change, dark mode was an explicit setting you would need to toggle for a customer based on their dark mode preferences. This works well if the preferences are set explicitly in your application, but required a bit of additional work for applications where dark mode is detected automatically from the system preferences.


That why we we've now added a new` darkMode=auto` option to set dark mode automatically based on your customer environment; so if your app does auto-detection, the app portal can do so as well.


## Improve performance of list attempted messages when there are no recent messages


We've optimized the performance of listing attempted messages, particularly in cases where there are no recent messages but there are a lot of historical ones. This results in faster load times and a more responsive experience when viewing message history.


## Play: highlighting more features in the play UI


We've enhanced the Play UI to better showcase its more advanced features. For example,[Svix Play has an API](https://docs.svix.com/play#programmatic-use-of-the-public-api) that lets you configure things like response status code (including at random), and automatically verifying endpoint signatures.


## Closing words


As always, please let us know if you have any feedback on the product, or anything else we can add or build to make your lives easier. A lot of the Svix product came from customer feedback, and we are sure` ${next_big_thing}` will come from feedback as well. No feedback is too small!


You can[contact us](https://www.svix.com/contact/) directly or join the discussion on[our community Slack](https://www.svix.com/slack/) .


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
