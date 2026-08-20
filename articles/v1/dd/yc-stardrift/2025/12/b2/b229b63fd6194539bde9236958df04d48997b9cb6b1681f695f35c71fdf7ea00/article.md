---
schema_version: "1.0.0"
document_id: "b229b63fd6194539bde9236958df04d48997b9cb6b1681f695f35c71fdf7ea00"
company_key: "yc-stardrift"
company: "Stardrift"
source_id: "yc-stardrift-news-import-0d01feb63e61"
canonical_url: "https://stardrift.ai/blog/streaming-resumptions"
published_at: "2025-12-15T00:00:00+00:00"
first_seen_at: "2026-07-22T14:49:24.360366+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:78aadd66f1173ef59efcfac22e8b7f52ac5140c79360d694aa33c9ac3adbd783"
---

# Is resumable LLM streaming hard? No, it's just annoying.

Short answer, yes. The truly annoying aspect of streams and pub/sub systems in general are all the race conditions that come up.


So if you (like us) are thinking “why a whole separate Redis store just to track the status of a chat’s stream? Can’t we just look at the Redis stream directly?”, here’s why:


Even if we enforce that any given` chat_id` has only one active Redis stream at any given time (by having the worker delete the entire stream as soon as the agent is done), we still run into a race condition. The backend (as the subscriber) may not have read the final chunk yet when the stream is deleted, and therefore can’t reliably know that the stream is complete. If we instead move the` delete` operation to the backend, we tightly couple the subscriber to the stream's lifecycle, loosing the independence that's at the core to this flow.


In a more practical sense, though, the separate Redis store gives us an extra degree of flexibility in controlling our flow: we can explicitly track the state of streams from 'pending' to 'ongoing' to 'complete', and any other additional statuses we may want to know along the way.
