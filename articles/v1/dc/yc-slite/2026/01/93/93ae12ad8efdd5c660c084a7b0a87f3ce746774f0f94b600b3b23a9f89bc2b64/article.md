---
schema_version: "1.0.0"
document_id: "93ae12ad8efdd5c660c084a7b0a87f3ce746774f0f94b600b3b23a9f89bc2b64"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/smarter-conversations"
published_at: "2026-01-14T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:43d1becebe12e0f036b8779c7163cf0779261ee4b07e27641323ac4052f3369e"
---

# Smarter conversations

Since V2, we've been focused on making Super conversations feel smoother and data sources more reliable, while preparing our next major launches.


### Slack improvements


**Super steps in Slack bot** — You can now have visibility into what Super is doing while the answer is being generated.


Sources in slack answers are also now rendered as **clickable links** and we fixed crashes when answers exceeded the maximum size of Slack messages.


### Reliable conversations


**Chat titles in the sidebar** — Your history now shows readable titles like *"Mobile redesign status"* or *"Issues assigned to me"* .


**Thread auto-summarization** — Long back-and-forth conversations used to hit context limits. Super now intelligently compresses older exchanges to keep your threads going.


### Better Buttons & Extension


Contextual buttons now **work without need for specifying CSS selectors** . If you leave these complex fields blank, Super will extract the entire body by default.


The[Super Extension](https://chromewebstore.google.com/detail/superwork-ai-search-on-al/omlhdlhgiaabehoojbfbnicimdjbdonk) also extracts your page content more reliably.


### Other Improvements


- We rebuilt the Intercom integration with support for **multiple teams** and **near real-time indexation** via webhooks
- Digests are now processed as background jobs, eliminating timeout-related delivery failures.
- Website sources now display favicons and original URLs, and can be edited without removing and re-adding
- Fixed formatting loss from Super Intercom buttons
- Fixed questions with date references showing error banners
- Fixed assistants pulling data from outside specified sources
