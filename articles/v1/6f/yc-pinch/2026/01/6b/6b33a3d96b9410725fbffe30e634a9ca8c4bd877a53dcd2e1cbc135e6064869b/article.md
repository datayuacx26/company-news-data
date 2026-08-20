---
schema_version: "1.0.0"
document_id: "6b33a3d96b9410725fbffe30e634a9ca8c4bd877a53dcd2e1cbc135e6064869b"
company_key: "yc-pinch"
company: "Pinch"
source_id: "yc-pinch-news-import-5efd69a4f207"
canonical_url: "https://startpinch.com/research/en/falcon-v1-3-is-live"
published_at: "2026-01-28T00:00:00+00:00"
first_seen_at: "2026-07-23T20:26:36.653996+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:95fdebfe4cc36bcb5fe55a01ba6524bae0814dcdce819ecbd57de2c263a91638"
---

# Falcon v1.3 is live!

Falcon v1.3 - a new iteration of our translation model at Pinch.


**What’s new in v1.3:**


-


Real-time voice cloning (streaming): the translation can speak in *your* voice - adapting tone + timbre on the fly while you’re talking.


Voice cloning is often done by recording a sample of your voice from 5-30 seconds to use as a reference. With our Falcon model, we handle partial voice references naturally and perform streaming voice cloning.


We do this by dynamically capturing fingerprints of your voice while you speak. From that fingerprint, Falcon can output translations with your speaker identity - capturing things like timbre and pitch tendencies.


The goal is to keep the speaker’s voice characteristics consistent but adaptive, even when the spoken words are different.


-


Faster TTFT (time-to-first-translation): language-specific improvements that reduce latency and make interim / partial speech behave better, so live conversations feel smoother.


TTFT is not just about raw inference speed - it’s about *when* the system is confident enough to start speaking. In streaming translation, early audio is often ambiguous, and many systems wait too long before committing, which makes conversations feel turn-based even if total latency is low.


In Falcon v1.3, we optimized for early, partial commitment. The model is trained to emit useful intermediate translations sooner, while remaining rewrite stable as more context arrives. The result is a system that feels responsive in real conversations - especially when paired with real-time voice cloning, where latency is much more perceptible.


Live translation shouldn’t feel like a separate narrator interrupting you.


It should feel like *you* - just multilingual.
