---
schema_version: "1.0.0"
document_id: "7ff83193f0e86d741f8c4721465c4fb72ecdf1769c049aad073407bf6d41bcf7"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-6969ef4af7f4"
canonical_url: "https://www.windmill.dev/changelog/ai-chat-text-file-attachments"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-27T09:58:44.512719+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:9db73ac740341d3f457d04531d9ee0fa08c5914b0a5dfc4c78d8d45afa2704e5"
---

# Attach text files to AI chat messages

### [Attach text files to AI chat messages](https://www.windmill.dev/changelog/ai-chat-text-file-attachments)


AI


AI Chat


[v1.765.0](https://github.com/windmill-labs/windmill/releases/tag/v1.765.0)


[Docs](https://www.windmill.dev/docs/core_concepts/ai_generation#attaching-files)


Text files can now be attached to individual AI chat messages via picker, drag and drop, or paste. They show as chips in the input box and are sent with the next message. Content is read on demand by the AI through file tools instead of being inlined, so it is never resent with the conversation history.


#### New features


- Attach text files to a chat message via the attach button, drag and drop, or paste
- Up to 8 files per message, 1 MB per file
- Files appear as chips in the input box, sent with the next message and cleared after send
- The AI reads file content on demand via read_file and search_files tools, keeping the conversation history small
- Linked folders remain session-wide assets, shown in the footer next to the mode picker
