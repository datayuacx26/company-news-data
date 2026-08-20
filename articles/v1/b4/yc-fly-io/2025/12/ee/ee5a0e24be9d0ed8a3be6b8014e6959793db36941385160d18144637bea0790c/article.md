---
schema_version: "1.0.0"
document_id: "ee5a0e24be9d0ed8a3be6b8014e6959793db36941385160d18144637bea0790c"
company_key: "yc-fly-io"
company: "Fly.io"
source_id: "yc-fly-io-news-import-54d37e81cc45"
canonical_url: "https://fly.io/blog/litestream-vfs/"
published_at: "2025-12-11T00:00:00+00:00"
first_seen_at: "2026-07-21T20:38:55.214316+00:00"
fetched_at: "2026-07-28T22:25:02.700615+00:00"
content_hash: "sha256:d9eded396ac23b1c47adcc5b1f09d70afe5aaf0de8bdfae215128eb2b8cb6cd9"
---

# Litestream VFS

**We’ve got one last trick up our sleeve.**


Quickly building an index and restore plan for the current state of a database is cool. But we can do one better.


Because Litestream backs up (into the L0 layer) once per second, the VFS code can simply poll the S3 path, and then incrementally update its index. **The result is a near-realtime replica.** Better still, you don’t need to stream the whole database back to your machine before you use it.
