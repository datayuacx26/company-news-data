---
schema_version: "1.0.0"
document_id: "ffe59213d85aa9fe88705f681b15121f1114a3ffcf50f1d6986fe9bea0bbb50e"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-sessions-plan-mode"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-17T14:49:57.045379+00:00"
fetched_at: "2026-08-17T14:50:00.107667+00:00"
content_hash: "sha256:54b6e8b66aa640f7b8500044b533fd8850ff739890ad6bacec79a98cf48d26a6"
---

# Plan mode for AI sessions

### [Plan mode for AI sessions](https://www.windmill.dev/changelog/ai-sessions-plan-mode)


AI


[v1.790.0](https://github.com/windmill-labs/windmill/releases/tag/v1.790.0)


[Docs](https://www.windmill.dev/docs/core_concepts/ai_sessions#plan-mode)


Plan mode is a read-only autonomy posture for AI sessions, picked from the chat footer. The assistant can only investigate; anything that writes, runs or deploys is refused until it hands over a plan and you approve it, which restores the posture you were in before. Session chats only, and never persisted.


#### New features


- Plan (read-only) joins Ask permission and Yolo in the autonomy picker of a session chat.
- Only read-only tools run while planning; writes, job runs and deploys are refused with a "Blocked in plan mode" row.
- You enter plan mode from the picker, or by accepting the "Start planning?" card the assistant raises on open-ended tasks.
- The proposed plan is shown for approval with "Approve and implement" or "Keep planning"; approving restores the exact posture that preceded plan mode.
- One versioned plan document per session, pinned to the top of the artifacts list, with the approved version marked and unapproved revisions labeled as drafts.
