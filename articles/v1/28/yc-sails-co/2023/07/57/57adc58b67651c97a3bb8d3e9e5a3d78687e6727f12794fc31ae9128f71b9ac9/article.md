---
schema_version: "1.0.0"
document_id: "57adc58b67651c97a3bb8d3e9e5a3d78687e6727f12794fc31ae9128f71b9ac9"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-v1-5-6/"
published_at: "2023-07-08T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:f74999c01d8ec030fd08e9c9ae792b45e065203592d0e39e570f35913f623d76"
---

# Sails 1.5.6

This release of Sails merged a[PR](https://github.com/balderdashy/sails/pull/7285) that fixed the logic for displaying the local dev server URL when you run` sails lift` in development mode.


Prior to this release, running` sails lift` in development mode will display the port for the local dev server - usually` 1337` .


This made the developer manually type in` http://localhost:1337` in their browser to see their Sails application.


We wanted to improve this experience and` 1.5.6` fixed that by displaying the local dev server URL to your local Sails application when you run` sails lift` .


You can now copy or click on the URL to open up your Sails application in your browser.


Note, the local dev server URL will only be displayed when you are running your Sails application in development mode.


You can check out the[What’s new in Sails 1.5.6](https://youtu.be/XKQuucrvdfQ) screencast on YouTube.


Also see the[full changelog](https://github.com/balderdashy/sails/releases/tag/v1.5.6) on GitHub.


## ✅ Upgrading


To upgrade to **Sails 1.5.6** , run:


```text
npm   i   sails@latest
```
