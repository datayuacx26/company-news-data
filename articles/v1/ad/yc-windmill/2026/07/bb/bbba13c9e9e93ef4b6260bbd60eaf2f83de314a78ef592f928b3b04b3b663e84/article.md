---
schema_version: "1.0.0"
document_id: "bbba13c9e9e93ef4b6260bbd60eaf2f83de314a78ef592f928b3b04b3b663e84"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-session-artifacts"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-08-14T15:49:25.757524+00:00"
fetched_at: "2026-08-14T15:49:28.085235+00:00"
content_hash: "sha256:d95e0722bbef7adcc70c5dfbe17095757332f02c02ffa152b891cf3fa5ce291c"
---

# Markdown artifacts in AI sessions

### [Markdown artifacts in AI sessions](https://www.windmill.dev/changelog/ai-session-artifacts)


AI Chat


Windmill AI


[v1.761.0](https://github.com/windmill-labs/windmill/releases/tag/v1.761.0)


[Docs](https://www.windmill.dev/docs/core_concepts/ai_sessions#artifacts)


In AI sessions, the assistant can now save longer structured output like plans, specs and design write-ups as markdown artifacts that persist for the whole session. Artifacts open in the session preview panel with Copy, Download as .md and a Preview / View source toggle, and are listed in the new status line above the chat input that gathers the session's drafts, artifacts and jobs. They are stored locally in your browser, restored when you reopen the session and removed when the session is deleted.


#### New features


- The AI can create markdown artifacts (plans, specs, design write-ups) in sessions and revise them in place instead of duplicating them.
- Artifacts open in the session preview panel rendered as markdown, with Copy, Download as .md and a Preview / View source toggle.
- The session composer now gathers drafts, artifacts and jobs into a single status line above the input; the artifacts entry lists each artifact with open, download and delete actions.
- Artifacts are session-scoped and stored locally in your browser: restored when you reopen the session, removed when the session is deleted.
