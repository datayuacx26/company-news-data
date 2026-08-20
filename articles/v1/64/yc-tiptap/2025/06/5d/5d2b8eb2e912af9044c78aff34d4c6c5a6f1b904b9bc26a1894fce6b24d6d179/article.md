---
schema_version: "1.0.0"
document_id: "5d2b8eb2e912af9044c78aff34d4c6c5a6f1b904b9bc26a1894fce6b24d6d179"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/faster-lighter-and-more-reliable-docx-import-export-with-tiptap"
published_at: "2025-06-24T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:956f7ae9c4c611bb6dda00563a54eec773dee5ec4badaffbd721e037ba6dc23e"
---

# Faster, lighter, and more reliable DOCX import/export with Tiptap

## What changed?


- 60% less memory usage
- 47% faster processing
- Memory now scales linearly, even with documents over 100 pages


We completely reworked the internals. Smarter chunking, better garbage collection, more efficient parsing, and full Sentry tracing throughout the conversion pipeline.


## ‍ **Why this matters**


**‍** Apps that import/export Word files need to be reliable. If conversions crash or timeout, users get frustrated. And when they don’t trust the conversion, they fall back to copy/paste, which no one enjoys.


## ‍ **With these changes, you get:**


- Fewer crashes and out-of-memory errors
- Faster round-trips for large documents
- Cleaner logs and better debugging
- Happier users who stick around


## **Under the hood**


**‍** We added a **Smart Memory Monitor** to track usage at key steps and help clean up early. And a **Smart Chunked Processor** that adapts chunk sizes based on document complexity, without breaking list or table structures.


Everything is now optimized for large documents, including paragraphs, tables, images, and deeply nested formatting.


If you’ve hit conversion issues before, this update should help. And if not, you’re less likely to start now.


We’re already exploring Bun to push things even further.


## ‍ **Get started**


**‍** To start using the improved DOCX import and export, check out our updated[developer documentation](https://tiptap.dev/docs/conversion/import-export/docx) for setup instructions and examples.


## ‍ **We’d Love Your Feedback!**


**‍** Try out the new improvements and let us know how they work for you. If you have any feedback or suggestions, reach out athumans@tiptap.dev or join our[Discord Community](https://tiptap.dev/discord) .


Happy coding!
