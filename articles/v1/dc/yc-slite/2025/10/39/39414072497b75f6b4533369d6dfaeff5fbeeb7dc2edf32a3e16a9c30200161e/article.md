---
schema_version: "1.0.0"
document_id: "39414072497b75f6b4533369d6dfaeff5fbeeb7dc2edf32a3e16a9c30200161e"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/maintain-your-knowledge-with-ease"
published_at: "2025-10-27T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:959af3eed2d18e143fa03c36e4959396c900be7218893ba4d4210e03bf19830f"
---

# Maintain your knowledge with ease

In the age of AI, trusted knowledge is the key to unlocking your team and AI workflows. That's what this update is all about.


### API updates: Programmatic knowledge management


New endpoints give you and your agents full control over document lifecycle and trustworthiness:


- ` PUT /notes/{noteId}/verify` - Mark documents as verified
- ` PUT /notes/{noteId}/flag-as-outdated` - Flag outdated content
- ` PUT /notes/{noteId}/owner` - Assign document ownership
- ` PUT /notes/{noteId}/archived` - Archive/unarchive documents
- ` GET /users` - List organization users
- ` GET /groups` - List groups
- ` /notes` and` /search-notes` now return` reviewState` for verification status


These endpoints let you automate knowledge maintenance and help AI agents distinguish trusted sources from stale content.


### Close the loop on knowledge gaps


We largely improved the` Ask Insights` panel experience. When your team asks questions that Slite can't answer well, admins now have a streamlined workflow to fix the problem at its source.


- Side-by-side view comparing original and updated answers
- Edit source documents directly in a modal, then regenerate answers instantly
- Refined access controls for Ask Insights


### Groups management improvements


Finding and organizing groups is now simpler:


- Quick search by group name
- Sort by creation date (newest or oldest)
