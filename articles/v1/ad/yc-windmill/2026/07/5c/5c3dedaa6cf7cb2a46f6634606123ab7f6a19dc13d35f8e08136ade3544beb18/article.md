---
schema_version: "1.0.0"
document_id: "5c3dedaa6cf7cb2a46f6634606123ab7f6a19dc13d35f8e08136ade3544beb18"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-sessions"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-27T09:58:44.512719+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:c533837b202847ded60cf37a3cc6fe986c9bff37552db36c52a12e5f45ba3751"
---

# AI sessions (beta)

### [AI sessions (beta)](https://www.windmill.dev/changelog/ai-sessions)


AI


[v1.766.0](https://github.com/windmill-labs/windmill/releases/tag/v1.766.0)


[Docs](https://www.windmill.dev/docs/core_concepts/ai_sessions)


AI sessions are now in beta and enabled by default. Each session ties an AI chat to a workspace or fork with a live preview panel; the AI builds scripts, flows and apps as drafts you review in a unified diff drawer and deploy. Run many sessions in parallel; a banner switches back to the legacy chat.


#### New features


- Enabled by default during the beta: a banner under the session chat switches back to the legacy side panel chat (stored per browser), and a matching banner in the legacy chat reactivates sessions. Operators keep the legacy chat.
- Each session is tied to a workspace or fork, picked before the first message; forks are only created when the conversation actually starts.
- The preview panel opens edited items as live editors and workspace pages (runs, schedules, settings) as tabs, refreshing only what the AI touched.
- A chat-scoped edits bar and unified diff drawer track what the conversation changed, with per-item deploy and discard, and fork promotion via the compare page.
- Jobs the AI runs detach to a background jobs tray with live status, cancel and approval actions, and survive page reloads.
- The AI writes plans and specs as markdown artifacts that open in the preview panel with copy and download actions.
- With a full-code app preview open, the AI debugs the running app: console logs, job runs, live DOM queries (search_dom, read_dom), screenshots, and a click-to-attach element inspector.
- Slash commands: /compact summarizes the conversation in place, /clear starts a fresh chat, and workspace AI skills appear as commands.
- Sessions are grouped by workspace family in the sidebar, with unread badges and parallel draft sessions persisted in the browser.
