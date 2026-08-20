---
schema_version: "1.0.0"
document_id: "7dc8f257197b648bee786b890ff92b757363071f3d1a1d41ac512a9d362af98e"
company_key: "yc-unlayer"
company: "Unlayer"
source_id: "yc-unlayer-rss-50f115338a5a"
canonical_url: "https://docs.unlayer.com/changelog/latest-ym5n3Q6C"
published_at: "2026-07-29T17:50:00+00:00"
first_seen_at: "2026-08-10T04:27:59.075255+00:00"
fetched_at: "2026-08-10T04:28:00.874964+00:00"
content_hash: "sha256:281eeb0346bf839afd4f2d1153ce9874aedb40e4e5d6b3e32a25301a3838e332"
---

# Latest (available now)

The following features and fixes are available on the[latest](https://docs.unlayer.com/builder/latest/version-management#development-environment) channel, and are candidates for the next stable release:


## Changes


-


**1.456.0:** Improved loading and responsiveness across the editor for a faster, smoother experience. Text-editing toolbars also remain more stable and consistently positioned while editing or scrolling


-


**1.457.0:** Fixed images rendering with distorted proportions in Outlook 365


-


**1.462.0:** Fixed image dimension metadata causing distorted rendering in exported emails from API-created designs


## Breaking changes


-


**1.456.0:** Lodash underscore variable is not available globally by default anymore, to reduce our bundle size; if you use it in your custom tools, include lodash as cdn separately, as seem[here](https://examples.unlayer.com/custom_tools/accordion-custom-tool/)
