---
schema_version: "1.0.0"
document_id: "aea3333055019f854cfaefb40461ca668323a21f3ad6c4f3512420c6611dff4d"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/v4"
published_at: "2025-05-28T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:d59ed804fb5b09a0ba2d0ee78e9d9159d64100dd4b33ad15563a362f2b4bb725"
---

# Sourcebot v4 release

Sourcebot v4 introduces code navigation, changes to authentication, search result performance improvements, and more!


### Code Navigation


Sourcebot now lets you navigate across symbols in your code base with “go-to definition” and “find all references” support. This feature requires an enterprise license, feel free to[reach out](https://www.sourcebot.dev/contact) if you’d like a trial!


### Authentication Changes


Beginning in v4, authentication will be required in Sourcebot. The first account to register onto a Sourcebot deployment will be the owner, and all new accounts that register must be approved by the owner. Credential and email code registration is supported in core, with SSO/OAuth providers supported in enterprise. Check out our[auth docs](https://docs.sourcebot.dev/self-hosting/configuration/authentication) for more info, and[reach out](https://www.sourcebot.dev/contact) if you’d like a trial license!


### Performance improvements


We’ve overhauled how code results are rendered, which results in significant performance improvements when scrolling through a large amount of search results.


### Dynamic filter updates


Filter by repository and language will now dynamically update, ensuring that the filter options accurately reflect the current filter state.


### Filter panel visibility shortcut


You can now toggle the visibility of the filter panel with command+B


Upgrading to v4 is super easy, check out our[upgrade guide](https://docs.sourcebot.dev/self-hosting/upgrade/v3-to-v4-guide) for more info.


If you have any feedback on the release or want to suggest additional changes, please check out our[discord](https://discord.com/invite/6Fhp27x7Pb) or[github discussions](https://github.com/sourcebot-dev/sourcebot/discussions) page. To request a trial license to try out our new enterprise features, reach out to us using our[contact form](https://www.sourcebot.dev/contact) .
