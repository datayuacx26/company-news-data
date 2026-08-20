---
schema_version: "1.0.0"
document_id: "91f63e32f0e9fe7d47839701b15a1eba08ab707d29a56acd33cd83dd90ff3bf8"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c3d7277c32d9"
canonical_url: "https://www.assemblyai.com/blog/universal-3-5-pro-realtime"
published_at: null
first_seen_at: "2026-07-21T08:04:02.365197+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:4e5cdc1f6fee41d3faf253510694500e44d8ac134f1062bf55dee266ca2c43d7"
---

# Universal-3.5 Pro Realtime: every turn in context | AssemblyAI

## Context goes beyond the words


It's also whose voice matters, what the full call reveals, and what you know that the model doesn't:


**It hears the speaker, not the room.** Background speech is worse than background noise: a TV or a passenger doesn't just add static, it gets transcribed as words, and phantom words fire false interruptions.` voice_focus` isolates the primary speaker and suppresses everything else. Use` near_field` for headsets and phones,` far_field` for rooms, kiosks, and drive-thrus.


**It gets a second look at every speaker.** Speaker labels are weakest in the opening seconds, before the model has heard enough of each voice to tell them apart. So the model labels speakers live, then re-clusters every voice when the stream ends and sends a single revision correcting any labels it now knows were wrong. Live labels during the call, async-grade accuracy within about half a second of it ending, up to 10 speakers.


**It takes the context you bring.** Feed it your domain vocabulary with keyterm prompting and "metoprolol succinate" doesn't turn into something else. Update the prompt over the same WebSocket as the call moves from verification to troubleshooting to escalation.


All of it shows up where voice agents are actually measured. On Pipecat's open STT benchmark, real agent conversations rather than clean read speech, Universal-3.5 Pro Realtime posts a market-leading pooled word error rate of 6.99%.
