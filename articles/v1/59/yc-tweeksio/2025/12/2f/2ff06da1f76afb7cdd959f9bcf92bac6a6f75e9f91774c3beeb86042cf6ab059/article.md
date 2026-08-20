---
schema_version: "1.0.0"
document_id: "2ff06da1f76afb7cdd959f9bcf92bac6a6f75e9f91774c3beeb86042cf6ab059"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2025-11-monthly-changelog"
published_at: "2025-12-02T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:25:06.703683+00:00"
content_hash: "sha256:a33c3cde6f51ae7b60e424ba8e2bafcc66980b7dd09ca6a00d1e40b78b665094"
---

# Changelog: November 2025

You no longer have to build every tweek yourself. Discover launched this month and suggests curated tweeks for the sites you already visit, both on the web and inside the extension. Share pages were redesigned, profiles moved to their own page, and Tweeks Pro arrived.


## New


- **Discover** — Tweeks now suggests curated tweeks for the site you're on. A notification appears when tweeks are available for the current page, the Library tab shows what's popular for that site, and a new Discover page on the web lets you browse everything. Dismiss a suggestion and it stays dismissed. The catalog already has strong picks like[Remove Sponsored Results from Google Search](https://www.tweeks.io/t/b3bb5a6573f949899f779e86) , an[NYT Strands solver](https://www.tweeks.io/t/7a955c910812467eaa36f569) , and a[quick "Not interested" button for X](https://www.tweeks.io/t/2c8ac84d6d8e450696310639) .


- **Share pages with image galleries** — Shared tweeks now support custom titles, descriptions, and screenshot galleries, so people can see what a tweek does before installing it.
- **Profile page** — Your tweeks now live on a dedicated profile page with search, filters, sorting, and pagination.
- **Tweeks Pro** — You can now upgrade to Pro from your profile and manage billing yourself.
- **Generation queue** — Click the generation counter in the popup to see everything currently generating, with a live timer and a cancel button for each.
- **One-click installs from GreasyFork** — Tweeks adds an install button directly on GreasyFork script pages, so you can pull any userscript into your library without leaving the site.
- **Tweeks Discord** — We opened a[community Discord](https://tweeks.io/discord) . Come share what you've built, or get help with a stubborn tweek.
- **Synced sign-in** — Signing in on the website now signs you in to the extension, and vice versa.


## Improved


- **Clearer generation history** — Items in the generation list now show whether they created a new tweek or updated an existing one.
- **Apply without reloading** — Newly enabled scripts show an Apply button so you can run them on the current page right away.
- **Heavier pages** — Generation can now handle much larger pages, so it works on complex sites that used to fail.
- **Friendlier errors** — Failed generations now show an error notification with a readable message.
- **Onboarding refresh** — New visuals and clearer guidance in the onboarding flow.


## Fixed


- **Signed in but can't generate** — Fixed an authentication bug that could block generation even while signed in.
- **Site matching on Chrome** — Tweeks targeting URLs with query parameters now run reliably.
- **Share page titles** — Shared tweeks now always display the right title.


## Reliability


Generation now retries transient errors on its own, and page-capture uploads are more resilient. You should see fewer failed generations without doing anything differently.


## Extension Version Coverage


This changelog covers extension versions 0.0.2.3 through 0.0.3.1. All changes are cross-browser unless noted otherwise.
