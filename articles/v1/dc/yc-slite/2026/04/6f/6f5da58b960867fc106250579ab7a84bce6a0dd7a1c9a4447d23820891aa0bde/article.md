---
schema_version: "1.0.0"
document_id: "6f5da58b960867fc106250579ab7a84bce6a0dd7a1c9a4447d23820891aa0bde"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/digests-just-got-a-major-upgrade"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:a0fd203f80ed93b7d369694acc19b2c8eee8c6fcb8303337408f4d2128a746e7"
---

# Digests just got a major upgrade

Digests used to send a single global report to everyone, based on the intersection of permissions. Now they're personalized per recipient, scoped to what each person actually has access to.


- You can now chain steps together to gather, summarize, and format data in one run.
- The new engine handles far larger volumes than before. For example, it can now process up to 3,000 Intercom conversations per run, up from ~150–200.
- Every scheduled run is saved to your Super history, so recipients can revisit past reports and ask follow-up questions to dig deeper into the data.
- The digest editor has moved into the main Super workflow editor, giving you one place to build both assistants and digests.


### Transparent data source permissions


We've rolled out a cleaner permission system for data sources. You can see exactly who has access to what, and restrict sources that were previously accessible to all members (Files, Custom sources, HubSpot, Git...).


PS - **Sources mirroring permissions** — like Google Drive, Slack, or Linear — work as before: Super matches each user to their account in the connected tool, so people only see what they have access to there.


### Other improvements


- **User management redesign** — The settings screen has been redesigned with a cleaner layout, fixed pagination and ordering, the ability to sort users by Super seat, new filters (including login method), live seat count, group membership display, and improved archiving UX.
- **Mobile history fix** — Super history answers now render at the correct font size on iOS Safari and Chrome mobile. Previously, text was being incorrectly enlarged when opening past answers.


### Bug fixes


- Tables in Super answers were being cropped — fixed so table content displays correctly in the message renderer.
- Fixed Intercom source IP validation to properly handle client IP headers.
- Fixed Zoom webhook synchronization date handling to ensure new transcripts are reliably indexed after the initial sync.
