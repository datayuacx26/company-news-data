---
schema_version: "1.0.0"
document_id: "5fefcb94c56960684a9b0dac006be90e0c68da57cd824c63aaad449447eecd2a"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-rss-5a4068f5753f"
canonical_url: "https://forum.openreplay.com/t/wordpress-application-styles-are-not-recording-on-openreplay/557"
published_at: "2026-01-23T06:12:02+00:00"
first_seen_at: "2026-08-17T14:12:50.872421+00:00"
fetched_at: "2026-08-17T14:12:52.808979+00:00"
content_hash: "sha256:5872812d62e2d0abdf0043816ca54cc128406002548af6b5749794da0b86fd2a"
---

# WordPress application – Styles are not recording on OpenReplay

[rituKale](https://forum.openreplay.com/u/rituKale)


January 23, 2026, 6:12am 1


Hi team,


I’m experiencing an issue with OpenReplay session recordings in my WordPress application (hosted on ESC with Docker). The session content is being captured, but the recordings show only plain HTML—none of the CSS styles are applied.


Has anyone encountered this problem before, or does anyone know how to get OpenReplay to record and display sessions with the correct styling?
Any tips or troubleshooting steps would be greatly appreciated!


Thank you!


[mehdi](https://forum.openreplay.com/u/mehdi)


January 25, 2026, 6:03pm 2


That means the css files are not accessible for the OpenReplay VM/instance. This happens sometimes when they’re behind a CDN (like CloudFlare) and in that case, make sure to whitelist the IP of your OpenReplay VM.


Alternatively, you can simply add the` inlineCss: 3` option to your tracker. This way, you force stylesheets to captured with each session (rather than cached by the VM).
