---
schema_version: "1.0.0"
document_id: "2b9cad59c8c7ddf6062c48e46878eccda76d72daa2ccf16bc3eb57b700382988"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-news-import-e788018b3f7d"
canonical_url: "https://resend.com/changelog/open-graph-image-previews"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T21:22:09.024444+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:a005402c426ecd72978ab7a9f3d1184959d62aca06c37553eb5c2745c199be14"
---

# Open Graph Image Previews

Emails should be visual and engaging, but support for modern web experiences is limited.


We've been building support for more rich experiences that take advantage of modern web technologies, like[𝕏 embeds](https://resend.com/changelog/new-x-twitter-component) and[YouTube previews](https://resend.com/changelog/new-youtube-component) , in a way email clients support.


Today, we're adding **native support for Open Graph image previews** in the editor.


## How it works


When you paste a public URL into the editor, we offer two options:


- **Paste as URL:** pastes the URL as a plain link
- **Paste as Link preview:** pastes the URL as a rich Open Graph preview


When you choose to paste as Link preview, we automatically fetch the Open Graph metadata, check for a valid image, upload it to the CDN, and display it as an image preview within the editor.


Email clients often do not support rich experiences, but simple image files are nearly universally supported. This gives you a media-rich experience that works across all email clients.


## What's next


We want to continue to add support for more media-rich experiences in the editor. Let us know what else you are missing.
