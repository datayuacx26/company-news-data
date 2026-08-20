---
schema_version: "1.0.0"
document_id: "40f6f6e77537d2960e4226be116f0d17361ecf493bdce056677d123a259d927c"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/introducing-ai-dubbing-in-sync/"
published_at: "2026-05-14T17:00:00+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:b8d5deddaf3083b7cc7ce81fe5100449b6e01edba609f878fe869b8e9718e543"
---

# AI dubbing meets lip sync, in Sync

**Quick overview**


- AI dubbing is now built into sync. labs, powered by ElevenLabs for voice and sync. labs for lip sync
- Translate a video into Spanish, Hindi, French, Japanese, Portuguese, and more, then lip sync the speaker to the new audio in the same flow
- Available in Studio Lite, the Assistant, the Premiere plugin, and the API
- Multi-speaker support, so you can set the expected speaker count before dubbing starts


Video translation has always had one awkward gap: the voice can change languages, but the face stays behind. That mismatch is what makes a dubbed video feel dubbed, and it’s the gap we’re closing today.


[AI dubbing](https://sync.so/blog/what-is-dubbing/) is now an integrated step in the sync. labs video generation pipeline. Start with a source video or audio file, pick a target language, let ElevenLabs generate the translated voice, then have sync. labs lip sync the speaker to the new audio. One pass, no bouncing between an ai dubbing platform, a translator, and a separate lip sync tool.


Here’s the full translate, dub, and lip sync flow end to end:


Dubbing is available across the surfaces where people already make video with sync. labs. In Studio Lite, pick the target language, set the speaker count, and run dubbing as part of the standard generation flow. In the Assistant, you get a guided path from language selection through translation and lip sync. In the Premiere plugin, you can dub source audio without leaving your edit; the plugin handles upload, progress, and handoff to lip sync. On the API, pass dubbing parameters into a generation request and let the pipeline dub first, then lip sync.


Real videos are rarely one person talking. Podcasts, interviews, courses, and demos all need voice separation, so the dubbing flow takes a speaker-count field up front. That gives ElevenLabs the right context before it starts, and the lip sync step knows who is talking when.


Open a clip in[sync. labs](https://sync.so/login) and run a dub on your next[localization](https://sync.so/blog/video-localization/) pass, or add dubbing parameters to your[API](https://sync.so/docs/tutorials/dubbing) generation call.


The internet doesn’t watch in one language. Your videos shouldn’t have to either.
