---
schema_version: "1.0.0"
document_id: "cc17aa16a488cb00ca7d9dd4e7c21e7a39f7122763db1b18bd045483f47f62dc"
company_key: "yc-mintplex-labs"
company: "Mintplex Labs"
source_id: "yc-mintplex-labs-atom-5e945f201243"
canonical_url: "https://github.com/Mintplex-Labs/anything-llm/releases/tag/v1.14.0"
published_at: "2026-06-15T03:28:37+00:00"
first_seen_at: "2026-07-27T03:50:18.579971+00:00"
fetched_at: "2026-08-20T02:46:36.756632+00:00"
content_hash: "sha256:eeaf8d294a50a3b26b8d522b131191da411f0efbd5c318c23fd60892c554f316"
---

# AnythingLLM v1.14.0

## Improvements


- Cerebres provider
- The default chat thread is now killed when you create a new thread. If you have chats on the default thread, it will be available still. New workspaces or workspaces with no chats on default will no longer show it.
- All model providers are now *opt-out* of tool calling by default. Everything will call tools by default unless you opt-out offering better performance for agents everywhere
- STT Support for Deepgram, GenericOAI, Lemonade, & OpenAI
- TTS Support for KokoroTTS
- Web-scraping now will convert to markdown for better parsing and chat followup tasks with minimal context bloat
- Summary tool was overhauled. Now it will so better summaries with transparency as well as ask before continuing for longer summaries
- Improvements to the GenericOAI provider
- 24hour system variable formats
- Better LaTex rendering support


## Bug Fixes


- Context limit detection issue for agents:[#5716](https://github.com/Mintplex-Labs/anything-llm/pull/5716)
- SEARXNG double encoding:[#5723](https://github.com/Mintplex-Labs/anything-llm/pull/5723)
- Timeouts for all fetch requests[#5721](https://github.com/Mintplex-Labs/anything-llm/pull/5721)
- Escape illegal XML in word docs, etc[#5760](https://github.com/Mintplex-Labs/anything-llm/pull/5760)
- (Windows) On unisntall, checkbox to remove **all** AnythingLLM data is now present
- Tray Fixes when app starts in background or Desktop Assistant feature is toggled.


## What's Changed


- docs: list all cloud embedding providers by[@narutamaaurum](https://github.com/narutamaaurum) in[#5701](https://github.com/Mintplex-Labs/anything-llm/pull/5701)
- feat: add Cerebras as an LLM provider by[@officialasishkumar](https://github.com/officialasishkumar) in[#5699](https://github.com/Mintplex-Labs/anything-llm/pull/5699)
- fix provider override in agents by[@timothycarambat](https://github.com/timothycarambat) in[#5716](https://github.com/Mintplex-Labs/anything-llm/pull/5716)
- docs: fix self-hosted terms wording by[@Zhao73](https://github.com/Zhao73) in[#5737](https://github.com/Mintplex-Labs/anything-llm/pull/5737)
- fix: avoid double-encoding SearXNG search queries by[@trick77](https://github.com/trick77) in[#5723](https://github.com/Mintplex-Labs/anything-llm/pull/5723)
- Kill` default` thread by[@timothycarambat](https://github.com/timothycarambat) in[#5739](https://github.com/Mintplex-Labs/anything-llm/pull/5739)
- feat: add server-side speech-to-text with OpenAI provider by[@shatfield4](https://github.com/shatfield4) in[#5596](https://github.com/Mintplex-Labs/anything-llm/pull/5596)
- Kokoro TTS provider by[@shatfield4](https://github.com/shatfield4) in[#5679](https://github.com/Mintplex-Labs/anything-llm/pull/5679)
- add windows paths to isWithin by[@timothycarambat](https://github.com/timothycarambat) in[#5685](https://github.com/Mintplex-Labs/anything-llm/pull/5685)
- apply universal sdk timeout by[@timothycarambat](https://github.com/timothycarambat) in[#5721](https://github.com/Mintplex-Labs/anything-llm/pull/5721)
- fix: support Azure & Dell Pro AI Studio providers in agent summarization fallback by[@sanidhyasin](https://github.com/sanidhyasin) in[#5738](https://github.com/Mintplex-Labs/anything-llm/pull/5738)
- Turn HTML scraped sites to Markdown for better research by[@timothycarambat](https://github.com/timothycarambat) in[#5742](https://github.com/Mintplex-Labs/anything-llm/pull/5742)
- Improve agent summarizer tool by[@shatfield4](https://github.com/shatfield4) in[#5719](https://github.com/Mintplex-Labs/anything-llm/pull/5719)
- Generic OpenAI improvements by[@timothycarambat](https://github.com/timothycarambat) in[#5746](https://github.com/Mintplex-Labs/anything-llm/pull/5746)
- feat: make document sync stale-after interval configurable by[@sanidhyasin](https://github.com/sanidhyasin) in[#5747](https://github.com/Mintplex-Labs/anything-llm/pull/5747)
- fix: strip XML-illegal control characters from generated documents by[@sanidhyasin](https://github.com/sanidhyasin) in[#5760](https://github.com/Mintplex-Labs/anything-llm/pull/5760)
- feat(embed): opt-in deny-by-default for embeds with no allowlist by[@dmitrymaranik](https://github.com/dmitrymaranik) in[#5759](https://github.com/Mintplex-Labs/anything-llm/pull/5759)
- add 24 hours date and time formats to SystemVariables by[@timothycarambat](https://github.com/timothycarambat) in[#5778](https://github.com/Mintplex-Labs/anything-llm/pull/5778)
- Make native tool calling opt-out instead of opt-in by[@timothycarambat](https://github.com/timothycarambat) in[#5783](https://github.com/Mintplex-Labs/anything-llm/pull/5783)
- better LaTex support by[@timothycarambat](https://github.com/timothycarambat) in[#5779](https://github.com/Mintplex-Labs/anything-llm/pull/5779)


## New Contributors


- [@narutamaaurum](https://github.com/narutamaaurum) made their first contribution in[#5701](https://github.com/Mintplex-Labs/anything-llm/pull/5701)
- [@Zhao73](https://github.com/Zhao73) made their first contribution in[#5737](https://github.com/Mintplex-Labs/anything-llm/pull/5737)
- [@trick77](https://github.com/trick77) made their first contribution in[#5723](https://github.com/Mintplex-Labs/anything-llm/pull/5723)
- [@sanidhyasin](https://github.com/sanidhyasin) made their first contribution in[#5738](https://github.com/Mintplex-Labs/anything-llm/pull/5738)
- [@dmitrymaranik](https://github.com/dmitrymaranik) made their first contribution in[#5759](https://github.com/Mintplex-Labs/anything-llm/pull/5759)


**Full Changelog** :[v1.13.0...v1.14.0](https://github.com/Mintplex-Labs/anything-llm/compare/v1.13.0...v1.14.0)
