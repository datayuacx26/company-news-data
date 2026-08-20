---
schema_version: "1.0.0"
document_id: "02aecbf28a2edd782aff9d0bd8726d2447f1ae94117dcedaa02fd183aa2429f4"
company_key: "yc-mintplex-labs"
company: "Mintplex Labs"
source_id: "yc-mintplex-labs-atom-5e945f201243"
canonical_url: "https://github.com/Mintplex-Labs/anything-llm/releases/tag/v1.14.1"
published_at: "2026-06-16T21:17:40+00:00"
first_seen_at: "2026-07-27T03:50:18.579971+00:00"
fetched_at: "2026-08-20T02:46:36.756632+00:00"
content_hash: "sha256:4da9316b12bd2b9e5ae5dacc6f185d7611a5f4368d805b5eb2f5134c997cd146"
---

# AnythingLLM v1.14.1

## Meeting Assistant Overhaul


*Meeting Assistant is desktop app only*


We have overhauled a large portion of the Meeting Assistant to make it smaller, faster, and more efficient across all devices and platforms.


-


Now supports Intel, AMD, and NVIDIA GPUs for a 92% smaller binary and 15% faster processing times.


- If you already have the NVIDIA GPU binary installed, you can safely delete it if you want. It will still work and is backwards compatible.


-


Support for Developer API for transcription on audio (POST:` /v1/transcription/transcribe` )


-


Meeting Assistant context window overflow handling is much better now - so small models can summarize longer meetings.


-


Introduction of[Basic Speaker Identification](https://docs.anythingllm.com/meeting-assistant/features#speaker-identification) for 60% better summarizes from any audio.


-


Dual channel stero recordings for meetings now - leading to 80% better speaker identification in "Full Diarization" mode.


## Improvements


- Linux AppImage now **91%** smaller in size and caches Ollama engine downloads for faster startup times.
- Meeting Assistant title fix on meetings post-summary now auto-updates in UI
- AgentFLow variable highlight so its clear what is and is not a valid variable
- "Copy chat link" in UI to quickly re-open a chat in the desktop/self-hosted app via deeplinking.
- Re-enabled audio and video uploads via chat UI - uses Tinyscribe engine now.
- Export Chat as (PDF, JSON, Markdown, etc) from chat UI.
- Desktop Assistant Setting - HD screenshots now available for screenshot capture area.
- Request approval internal function is now available for custom skills.


## Bug Fixes


- Removed DPAIS and HuggingFace providers from AnythingLLM (unmaintained)
- Fixed memory leak in embedder from it constantly reloading in server process
- Fixed text clearing bug when dragging and dropping files into the chat and text was already present in prompt.
- Massive performance improvements to the frontend UI for long running chats.
- Cohere SDK removed and ported to OpenAI SDK for compatibility.
- Desktop Assistant Capture Area not showing on windows multi-monitor setups.
- Strip thinking from fork thread name when forking a chat that had thoughts.
- Fix toast light mode always showing regardless of system theme.
- Mistral embedder encoding issue fixed.
- Better error messages for API
- Omit temp in Claude Bedrock for Claude 4.8
- Fixed event emitter leak in server process for web-scraping and summarize process


---


## What's Changed


- fix: prevent EventEmitter memory warning in AIbitat agent framework by[@simongonzalezdc](https://github.com/simongonzalezdc) in[#5790](https://github.com/Mintplex-Labs/anything-llm/pull/5790)
- Remove unused providers by[@timothycarambat](https://github.com/timothycarambat) in[#5793](https://github.com/Mintplex-Labs/anything-llm/pull/5793)
- Patch reloading of embedder and lanceDB connection by[@timothycarambat](https://github.com/timothycarambat) in[#5804](https://github.com/Mintplex-Labs/anything-llm/pull/5804)
- Better API errors by[@timothycarambat](https://github.com/timothycarambat) in[#5805](https://github.com/Mintplex-Labs/anything-llm/pull/5805)
- Mistral Embedding Encoding by[@timothycarambat](https://github.com/timothycarambat) in[#5806](https://github.com/Mintplex-Labs/anything-llm/pull/5806)
- fix: omit temperature param for Bedrock Claude Opus 4.8 by[@kimnamu](https://github.com/kimnamu) in[#5822](https://github.com/Mintplex-Labs/anything-llm/pull/5822)
- feat: expose requestToolApproval to custom agent skills by[@angelplusultra](https://github.com/angelplusultra) in[#5795](https://github.com/Mintplex-Labs/anything-llm/pull/5795)
- feat: highlight variable references in agent flow blocks by[@angelplusultra](https://github.com/angelplusultra) in[#5799](https://github.com/Mintplex-Labs/anything-llm/pull/5799)
- chat link in chat settings by[@timothycarambat](https://github.com/timothycarambat) in[#5837](https://github.com/Mintplex-Labs/anything-llm/pull/5837)
- WorkspaceChat refactor by[@timothycarambat](https://github.com/timothycarambat) in[#5833](https://github.com/Mintplex-Labs/anything-llm/pull/5833)
- More Frontend optimizations by[@timothycarambat](https://github.com/timothycarambat) in[#5838](https://github.com/Mintplex-Labs/anything-llm/pull/5838)
- Migrate Cohere to OpenAI sdk by[@shatfield4](https://github.com/shatfield4) in[#5813](https://github.com/Mintplex-Labs/anything-llm/pull/5813)
- feat: export workspace chat/chat thread to pdf by[@shatfield4](https://github.com/shatfield4) in[#5800](https://github.com/Mintplex-Labs/anything-llm/pull/5800)


## New Contributors


- [@simongonzalezdc](https://github.com/simongonzalezdc) made their first contribution in[#5790](https://github.com/Mintplex-Labs/anything-llm/pull/5790)
- [@kimnamu](https://github.com/kimnamu) made their first contribution in[#5822](https://github.com/Mintplex-Labs/anything-llm/pull/5822)


**Full Changelog** :[v1.14.0...v1.14.1](https://github.com/Mintplex-Labs/anything-llm/compare/v1.14.0...v1.14.1)
