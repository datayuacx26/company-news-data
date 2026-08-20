---
schema_version: "1.0.0"
document_id: "f052d7e9ee6046dc826205d37e6e1a4f28b0580943af19800eadf3842f955d05"
company_key: "yc-sourcebot"
company: "Sourcebot"
source_id: "yc-sourcebot-news-import-69a1f8dc01ea"
canonical_url: "https://www.sourcebot.dev/changelog/share-links"
published_at: "2025-01-10T00:00:00+00:00"
first_seen_at: "2026-07-22T14:19:52.577262+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:6bddce5d8db3221ff4b20280adae46eb27e2a641b41317159c6cec5f2c047ebb"
---

# Share links

After finding some code using search, it can be handy to share it with your team to callout a bug, request feedback, roast someone etc.


To address this, we've added 'share links' to easily link to a selection of code.


Highlighting a section of code in the preview window will surface the "Share selection" button. Clicking it will create a link to the file with the selection highlighted.


You can then share this link with your team. Checkout the snippet in the screenshot using[this link](https://sourcebot.dev/search/browse/github.com/sourcebot-dev/sourcebot@HEAD/-/blob/packages/web/src/app/search/page.tsx?highlightRange=46%3A5%2C59%3A41) 🔗 🤠


### Sync raw .git repositories


We also added support for any .git repository, regardless if it's hosted in a supported code host. To configure, use the` git` type in the` config.json` .
