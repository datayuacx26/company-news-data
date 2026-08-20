---
schema_version: "1.0.0"
document_id: "53be8a0bb733a18e3e6c018e4050fafcbccd11a71b5cbd5cd65501879ce038ac"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/lipsync-2-0/"
published_at: "2025-08-20T17:02:42+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:35c76d7ffe9dbbca43170feb824bca2f2ae31b60a82c936c6e3d1e87a69acf8b"
---

# lipsync 2.0

**Quick overview**


- lipsync-2, the most natural video-to-video lip sync model in the world
- Zero-shot. No actor, clone, or avatar to train before you can use it.
- Learns each speaker’s unique style and generates with it
- Works on live-action, animation, and AI-generated humans
- Use it for video translation, word-level dialogue editing, and character re-animation (including realistic AI UGC)


## A whole new model


lipsync-2 is the first zero-shot lipsyncing model that preserves how a specific person speaks, without any extra training or fine-tuning. The model watches the input, builds a style representation of the speaker on the fly, and uses it for every frame it generates.


It’s a step forward across the things that actually matter: realism, expressiveness, control, quality, and speed. Live-action, animation, AI-generated humans, the same model handles all of it.


## Features


**Zero-shot style preservation.** The model picks up speaker style from the input alone, with no separate training pass. Watch it hold Nicolas Cage’s mannerisms across languages, no other zero-shot model does this.


**Temperature control.** Dial how expressive the lipsync gets, from subtle to extreme.


**Active speaker detection.** For long videos with multiple people, we built **ASD-1** , a state-of-the-art active speaker detection pipeline that ties each voice to the right face and only applies lipsync when that person is actually speaking.


**Animation that holds up.** *Pixar-grade animation, AI-generated characters, anything in between. Translation is one use case; the bigger one is editing dialogue freely in post and rethinking what video production looks like.*


**Record once, edit forever.** *The take is final the moment you hit stop. lipsync-2 lets you rewrite a line later while keeping the original speaker’s style intact, with no pre-training.*


**AI Video**


*When you can generate any video by typing a few sentences, the camera stops being a constraint.*


We see[ai lip sync](https://sync.so/blog/what-is-ai-lip-sync/) as the first surface, not the final one.


This is a strange moment to be making things. A high schooler can shoot a masterpiece on an iPhone. A studio can deliver a film at a tenth of the cost and ten times faster than five years ago. A single video can land in every language on the same day. The goal at sync. is to make video as malleable as text.


**Additional Resources**


> [Docs](https://docs.sync.so/introduction)
