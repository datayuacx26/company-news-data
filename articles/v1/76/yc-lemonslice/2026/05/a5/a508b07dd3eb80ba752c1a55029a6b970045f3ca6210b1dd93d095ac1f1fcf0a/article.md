---
schema_version: "1.0.0"
document_id: "a508b07dd3eb80ba752c1a55029a6b970045f3ca6210b1dd93d095ac1f1fcf0a"
company_key: "yc-lemonslice"
company: "LemonSlice"
source_id: "yc-lemonslice-news-import-9806857f7d31"
canonical_url: "https://lemonslice.com/blog/couchverse"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-24T02:14:03.501451+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:5f3aba34f7ed3ae4b4c8bd7f94b11fb43242960b0d1939c10b11411964e6c4a2"
---

# Multiple Live Avatars + Chrome Extension

Did you know live avatars can comment on anything your browser is playing, no per-site integration required?


Most second-screen experiences are locked to a single platform: a Twitch overlay, a YouTube extension, a Spotify plugin.


This demo skips all of that. The extension grabs whatever audio is in your active tab with` chrome.tabCapture` , ships it over LiveKit, and two LemonSlice avatars in the side panel start cracking jokes about what they hear.


We open-sourced the whole stack so you can try it, fork it, and even create your own characters.


## Technical highlights


This demo includes the following:


- **Multiple avatars:** multiple avatars interacting with each other, each with their own personality
- **Live browser audio:** transcribes audio directly from your browser in real-time. This works across any site from Netflix to Twitch
- **Chrome extension:** an easy form-factor for usability and sharing


The pipeline is Groq for STT and LLM, ElevenLabs for voice, and LemonSlice for the avatars.


## Try It


Try it for free on the Chrome Extension Store. It takes <1 minute to install.


1. Install[Couchverse from the Chrome Web Store](https://chromewebstore.google.com/detail/dnnkfddbdggljpcfnondgbfmnpmcnaan?utm_source=item-share-cb)
2. Open a tab playing anything with audio, and open the extension the icon


Click the “Start Couchverse” button. The **Alien** and **Cat girl** characters will start commentating. Tip: You can edit their chattiness in the “OPTIONS” dropdown


## Open-Source Code


Github repo:[https://github.com/lemonsliceai/couchverse](https://github.com/lemonsliceai/couchverse)


## Make your own Characters


A persona in Couchverse is **one Python file** (for the LLM personality) and **one image.** See the full directions[here](https://github.com/lemonsliceai/couchverse/blob/main/docs/adding-a-persona.md) to modify the personas and make them your own.
