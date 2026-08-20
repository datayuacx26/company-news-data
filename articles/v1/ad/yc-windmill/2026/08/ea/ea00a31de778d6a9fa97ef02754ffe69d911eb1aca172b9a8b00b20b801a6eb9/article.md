---
schema_version: "1.0.0"
document_id: "ea00a31de778d6a9fa97ef02754ffe69d911eb1aca172b9a8b00b20b801a6eb9"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-artifact-version-history"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-14T15:49:25.757524+00:00"
fetched_at: "2026-08-14T15:49:28.085235+00:00"
content_hash: "sha256:a7a339b4f5ce4078ff416c161935f7f38641be2780204fbbf525d0d2b848068c"
---

# Version history for AI session artifacts

### [Version history for AI session artifacts](https://www.windmill.dev/changelog/ai-artifact-version-history)


AI Chat


Windmill AI


[v1.784.0](https://github.com/windmill-labs/windmill/releases/tag/v1.784.0)


[Docs](https://www.windmill.dev/docs/core_concepts/ai_sessions#artifacts)


Every revision of an AI session artifact is now saved as a version, labelled with a short note describing what changed. A picker in the artifact viewer header lets you read an earlier version, with a banner marking the view as stale and a button back to the latest. The AI can list and read past versions too, so you can ask it to restore earlier wording instead of rewriting it. History is capped per artifact, and larger documents keep fewer versions.


#### New features


- Each content change to an artifact is saved as a version with a short change note written by the AI.
- The artifact viewer header gains a version picker once an artifact has more than one version; viewing a past version shows a stale banner with a "Back to latest" button.
- The AI can list artifact versions and read any of them, so it can restore earlier wording on request rather than rewriting from memory.
- The artifacts list above the chat input shows an artifact version number once it has been revised.
- History is bounded per artifact: up to 20 versions, fewer for large documents, so browser storage stays in check.
