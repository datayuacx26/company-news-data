---
schema_version: "1.0.0"
document_id: "ee7a395fad54dbb6afa4da8c0d830d4e07ea6aabdf8ab38aae1b3165228721cc"
company_key: "yc-ello"
company: "Ello"
source_id: "yc-ello-news-import-42717b83a57e"
canonical_url: "https://www.ello.com/blog/teaching-a-child-in-1000-ms"
published_at: "2026-07-07T12:00:00+00:00"
first_seen_at: "2026-07-21T17:55:34.949067+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:e79c5a99be24812a6d7cc1bae240f88ac9664b273e0bb4846f50382ea2630971"
---

# Teaching a child in <1000 ms: the architecture behind a real-time tutor

Most AI products build guardrails in serial with a model call or agent turn. A user won't notice when the token stream goes through a content filter and a developer is willing to wait for a CLI tool call to be auto-reviewed.


There's nowhere to hide in a real-time conversation with a five-year-old. Nor is there an undo: a child can't unhear what the tutor said. The safety system has to gate any action, on every turn.


Our safety classifier is an LLM that takes ~500-1000ms to run. Waiting to run the converser until that check completes adds a second of delay to every turn that we can't afford. Here’s another advantage of decoupling generation from execution in our harness.


The safety classifier blocks execution without blocking generation. As soon as the child finishes speaking, we dispatch both the classifier and a small model to generate the converser's first action in parallel. That model reacts quickly with an *eager response* that mirrors or acknowledges what the child said ("you like dinosaurs! me too").


While a rules-based check would be faster and cheaper, it wouldn't survive the ways a five-year-old actually talks. Every category we add to the safety policy adds tokens and requires re-tuning a non-deterministic classifier. Sometimes a transcription error spooks the classifier and triggers a false positive. We review these cases and use them to improve how the agent understands the child.


By the time that *eager action* has generated, the classifier has usually returned safe. That check unblocks the converser to generate while the eager action executes. The child hears one continuous turn despite the multiple model calls.
