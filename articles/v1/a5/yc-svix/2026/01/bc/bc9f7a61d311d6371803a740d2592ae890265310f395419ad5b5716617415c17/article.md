---
schema_version: "1.0.0"
document_id: "bc9f7a61d311d6371803a740d2592ae890265310f395419ad5b5716617415c17"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/changelog-01-2026/"
published_at: "2026-01-08T13:00:00+00:00"
first_seen_at: "2026-07-22T15:22:16.880049+00:00"
fetched_at: "2026-07-28T22:23:47.437673+00:00"
content_hash: "sha256:0845b46444e76a3d794e5d9b71fccece5aae94aa994e2ef9e50b69a25b66d858"
---

# Svix Changelog January 2026

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


January is here, and we're excited to share another round of updates from the Svix team.


Here are the updates for January:


## What should Svix build, fix, or improve in 2026?


This is not really a changelog item, but we just started 2026 and I wanted to take this opportunity to ask you: what should Svix build, fix, or improve in 2026?


We asked the same question last year and got a lot of invaluable and actionable feedback which we used to improve to product for everyone!


So please, let us know if you have any feedback or anything you'd like to see us build. We want to hear from you so that we can make the product better for everyone.


You can[contact us](https://www.svix.com/contact/) directly or join the discussion on[our community Slack](https://www.svix.com/slack/) .


## Background tasks: show progress in the app portal


While it was always possible to track the progress of background tasks using the API, this information wasn't easily accessible for users of the app portal.


With this change, the app portal now shows running tasks with their progress, displaying messages like "Replaying failed messages from A to B (45% complete)".


## Organization group user listing - MFA status


We've added MFA (Multi-Factor Authentication) status visibility to the organization group user listing. This makes it easier for administrators to see at a glance which users in their organization have enabled MFA for enhanced security.


## OpenAPI spec import improvements


We've significantly improved how we handle OpenAPI spec imports. Previously, we flattened JSON schemas for simplicity, but this approach was problematic with deeply nested schemas and broke entirely with reference loops.


We now changed it to traverse the import tree intelligently, keeping only what's needed and pruning everything else. This results in cleaner imports that handle complex schemas and circular references gracefully.


## Closing words


As always, please let us know if you have any feedback on the product, or anything else we can add or build to make your lives easier. A lot of the Svix product came from customer feedback, and we are sure` ${next_big_thing}` will come from feedback as well. No feedback is too small!


---


For more content like this, make sure to follow us on[Twitter](https://twitter.com/SvixHQ) ,[Mastodon](https://mastodon.social/@svixhq) ,[Github](https://github.com/svix) ,[RSS](https://www.svix.com/blog/rss/) , or[our newsletter](https://www.svix.com/newsletter/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .
