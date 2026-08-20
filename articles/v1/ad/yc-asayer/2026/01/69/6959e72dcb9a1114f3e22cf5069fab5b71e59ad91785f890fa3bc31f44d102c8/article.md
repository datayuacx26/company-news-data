---
schema_version: "1.0.0"
document_id: "6959e72dcb9a1114f3e22cf5069fab5b71e59ad91785f890fa3bc31f44d102c8"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/open-replay-icons-issue/555"
published_at: "2026-01-15T11:40:43+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:dee5ec8c92cf6aff924adf01a6c109aee351e3ed79575ae4da73b3c0861f7f26"
---

# Open replay icons issue

[akhileshulhe](https://forum.openreplay.com/u/akhileshulhe)


January 15, 2026, 11:40am 1


**Issue:**
Icons are not rendering correctly in OpenReplay session replays and appear as crossed-out boxes, even though they display properly in the live React application.


**Cause:**
The icon font CSS is imported from` node_modules` and references font files using relative paths (e.g.,` ./webfont.woff2` ). While these paths resolve correctly at runtime in the application, the OpenReplay replay iframe cannot access` node_modules` paths.


[mehdi](https://forum.openreplay.com/u/mehdi)


January 17, 2026, 12:35am 2


What tracker version are you using?


[akhileshulhe](https://forum.openreplay.com/u/akhileshulhe)


January 20, 2026, 4:44am 3


it’s version 16 (“ @openreplay


/tracker”: “^16.4.10”,)


[mehdi](https://forum.openreplay.com/u/mehdi)


January 27, 2026, 11:34pm 4


Are you recording sessions in localhost? Have you tried after deploying?
