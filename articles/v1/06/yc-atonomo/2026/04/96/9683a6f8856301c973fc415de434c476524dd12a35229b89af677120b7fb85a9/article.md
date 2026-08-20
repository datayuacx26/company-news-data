---
schema_version: "1.0.0"
document_id: "9683a6f8856301c973fc415de434c476524dd12a35229b89af677120b7fb85a9"
company_key: "yc-atonomo"
company: "Atonomo"
source_id: "yc-atonomo-news-import-1e84588cb145"
canonical_url: "https://www.codecanary.ai/blog/even-trillion-dollar-companies-apps-get-important-bugs"
published_at: "2026-04-23T00:00:00+00:00"
first_seen_at: "2026-07-21T08:26:03.097679+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:debdf0a3b29c5e5351d9e8be6dc66a38ea477ed656fa301c1c11e7bd3f41eae9"
---

# Even trillion dollar companies' apps get important bugs

One of the funniest sales objections people have to buying CodeCanary is that "we don't have bugs".


This is sometimes substantiated by pointing to their observability platform like Sentry getting less than 10 uncaught JavaScript exceptions per day. Sentry notices a narrow fraction of the universe of bugs! Black text on a black background doesn't throw an exception, it just turns 32% churn into 33% churn.


(Yes, that kind of bug is what[we detect](https://www.codecanary.ai/blog/watching-session-replays-with-ai) then[fix automatically](https://www.codecanary.ai/blog/codecanary-uses-codecanary-to-improve-itself) )


I recently upgraded our Google Workspace account to enable Gemini in Google Sheets and was surprised to see a really big bug in the upgrade experience: your team's Google Drive is **completely broken** for an hour after upgrading, where all alloted usage is capped at **0 bytes** :


This wasn't an aesthetic error, but rather using Google Drive (and the spreadsheet I was working on) was blocked:


This is of course despite having a paid plan with Google for years and their dashboard acknowledging that we pay for 2 terabytes of storage.


I had the pleasure of talking to an AI agent at Google Workspace support, and it claimed that it takes 24 hours for new storage limits to apply.


I think the funny part is Google Workspace has been around for years, was not vibe coded by Claude Sonnet 3.5, and probably generates $50-500M USD per year (not broken down by GOOG investor presentations). This is now in a new, emerging class of software bugs that appear in the DOM and can be automatically fixed by AI, which we're solving with[CodeCanary](https://www.codecanary.ai/) .
