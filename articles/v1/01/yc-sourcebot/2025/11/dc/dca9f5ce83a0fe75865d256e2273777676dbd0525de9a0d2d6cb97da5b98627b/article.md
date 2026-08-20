---
schema_version: "1.0.0"
document_id: "dca9f5ce83a0fe75865d256e2273777676dbd0525de9a0d2d6cb97da5b98627b"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/account-linking"
published_at: "2025-11-04T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:5d08326f41cf253dab64970f112f417860cfc24cf089a5109cd8c5143b053962"
---

# Account linking

Please note that the features outlined in this changelog require an[enterprise license](https://www.sourcebot.dev/pricing) .


We’ve[added](https://github.com/sourcebot-dev/sourcebot/pull/595) the ability to link multiple external identity providers to a Sourcebot user. This allows you to connect to multiple different code host platforms and sync permissions across all of them. In addition, this unblocks deployment scenarios where you don’t want to use your code host platform for SSO (ex. you want to login using Okta, but you still need GitHub permission syncing).


Please check out the[external identity provider docs](https://docs.sourcebot.dev/docs/configuration/idp) for more info.
