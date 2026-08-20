---
schema_version: "1.0.0"
document_id: "35c63a7d6d4edf9c997cc16dd717a16c329c82bd8d5284a23e9a6330fe2e0073"
company_key: "yc-sunsama"
company: "Sunsama"
source_id: "yc-sunsama-news-import-3d5f9384ecf1"
canonical_url: "https://roadmap.sunsama.com/changelog/weekly-product-changelog-july-24-2026"
published_at: null
first_seen_at: "2026-07-28T20:57:03.251341+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:9e0936048e5b04217185bb7cb6f8b4155b4f51b6597e357307bdd3630ec7ffb7"
---

# Weekly Product Changelog: July 24, 2026

[Sunsama](https://roadmap.sunsama.com/)


[Home](https://roadmap.sunsama.com/)[Feedback](https://roadmap.sunsama.com/improvements)


Feedback


[Changelog](https://roadmap.sunsama.com/changelog)


[← Back to changelog](https://roadmap.sunsama.com/changelog)[Powered by Canny](https://canny.io/powered-by-canny?utm_source=changelog_subdomain&utm_medium=powered&utm_campaign=sunsama&company=Sunsama)


Improved:


- Timer-aware subtask completion


: Pressing C


while a subtask's timer is running now completes that subtask instead of the whole parent task — press again to complete the parent.
- Microsoft Planner


: Personal Teams, Loop, and Project-backed plans now show up in the Planner picker. Previously these were invisible in Sunsama even though you could see them in[planner.cloud.microsoft](http://planner.cloud.microsoft/) .
- Search


: Task search is now case and punctuation insensitive. Exact matches still rank highest.


Fixed:


- Outlook email parsing


: Outlook and mixed Outlook/Gmail email threads now split into individual messages instead of collapsing into one blob, so daily highlight summaries for Outlook accounts aren't empty or truncated anymore.
- Editor


: Typing right after a pasted link with a custom protocol (like a Shortcuts link) followed by a space no longer swallows what you type.
- Asana panel


: Fixed a bug where the Asana panel could show up blank on desktop (My Tasks and projects failing to load) even though the same account worked fine on web.
- Gmail tasks


: Tasks and subtasks created via Autoplan or MCP with a linked Gmail thread now open the correct email, and their Gmail icons are clickable again.
- Ubuntu


: Alt-drag task merging works again; it no longer silently drops the task in as a reorder instead of merging.
- Jira


: Tasks added via MCP with a linked Jira ticket now show the ticket ID on the card instead of "-undefined."
