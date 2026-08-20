---
schema_version: "1.0.0"
document_id: "76e6886e7ec2a94d691b87791cb6eb6fc6e7dd358562aff947b8ba1df2e5cbeb"
company_key: "yc-vogent"
company: "Vogent"
source_id: "yc-vogent-news-import-1ff82337d4a1"
canonical_url: "https://blog.vogent.ai/posts/build-ultrarealistic-voice-agents-with-sesame"
published_at: "2025-04-06T18:27:42.788+00:00"
first_seen_at: "2026-07-22T19:07:51.499791+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:d377821b9e0e03e29382a1e196151b8078a4c06575fdf9279e0202053f86c927"
---

# Build Ultrarealistic Voice Agents with Sesame

We're excited to announce that Vogent now supports ultra-realistic voices from Sesame's CSM-1B model. This wasn't a simple integration; our team re-architected Sesame's voice model from the ground-up to make it *super low-latency* and *available at no additional cost* . In our testing, Vogent's CSM-1B implementation generates audio within 200-400 milliseconds, which is faster than even the fastest text-to-speech vendors on the market. Furthermore, as Vogent hosts the rearchitected model, the Sesame voices are available at **no additional charge** , and are included in **HIPAA-compliant workspaces** .


## Accessing Sesame Voices


To use a Sesame voice, choose a voice with the **Sesame** badge in the **Voice** dropdown on your agent's **Config** page.


Vogent's Sesame inference is based in the US, which is where latency benchmarks were measured. There may be additional latency for users outside the country.


## Private Beta Features


### Voice Cloning


Vogent also supports creating Sesame voice clones. This feature is in beta -- to access, emailjagath@vogent.ai with the subject line **Sesame Voice Clone** , and we'll enable it in your workspace.


### Text-to-speech API


We're also releasing our low-latency CSM-1B implementation as a TTS API. The API is in private beta, and will release soon. For early access, emailjagath@vogent.ai with the subject line **Sesame TTS API** , and we'll provide you with endpoints and keys.
