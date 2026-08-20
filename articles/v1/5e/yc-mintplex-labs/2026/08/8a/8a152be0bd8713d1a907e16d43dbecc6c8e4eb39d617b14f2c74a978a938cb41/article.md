---
schema_version: "1.0.0"
document_id: "8a152be0bd8713d1a907e16d43dbecc6c8e4eb39d617b14f2c74a978a938cb41"
company_key: "yc-mintplex-labs"
company: "Mintplex Labs"
source_id: "yc-mintplex-labs-atom-5e945f201243"
canonical_url: "https://github.com/Mintplex-Labs/anything-llm/releases/tag/v1.16.0"
published_at: "2026-08-13T21:38:01+00:00"
first_seen_at: "2026-08-13T23:38:54.481855+00:00"
fetched_at: "2026-08-20T02:46:36.756632+00:00"
content_hash: "sha256:6a0531b5ede9b663e8492e2f00883318234f3c65a118cb79dffb71e977642692"
---

# AnythingLLM v1.16.0

## Notable Changes


### Image Generation


Now you can generate images via` /img` when a[supported provider is set up](https://docs.anythingllm.com/features/image-generation#supported-providers) .


This flow also allows image attachments for edits, followups, or combination prompts if your provider supports image editing.


> Ability for an agent-tool to generate images is coming next update.


### Better File Picker UX


Now in the file picker you can drag entire folders and their hierarchy will be preserved in the upload panel. This will only import the **top level** files in the folder — it will be deep and recursive in a later update.


You can also now drag and drop a file into an existing folder directly without needing to move it.


Lastly, you no longer need to type` https` in the URL fetcher - it will just assume https when left out.


We also made some large performance improvements, like lazy-loading of folders, for those with thousands of documents in a folder.


### Tool Toggle Mid-Session


Now, during agentic chats you can freely toggle tools on/off at will without needing to reload or start a new session. Only tools that have been configured (if configuration is required) are available.


### Stop Generation


A large improvement is now that` aborting` a response actually fully kills the inference as well so that ghost inference does not continue when no longer wanted mid-stream.


## All Changes


- feat: increases the username character limit to 64 by[@guilopesn](https://github.com/guilopesn) in[#5914](https://github.com/Mintplex-Labs/anything-llm/pull/5914)
- Fixes typos and mistakes in Catalan translation by[@jordimas](https://github.com/jordimas) in[#5923](https://github.com/Mintplex-Labs/anything-llm/pull/5923)
- Update default fallback models for OpenAI/Anthropic by[@shatfield4](https://github.com/shatfield4) in[#5949](https://github.com/Mintplex-Labs/anything-llm/pull/5949)
- fix: disable temperature for claude-sonnet-5 model by[@petre](https://github.com/petre) in[#5947](https://github.com/Mintplex-Labs/anything-llm/pull/5947)
- fix: stream Anthropic non-streaming chat completions for REST API workspace chat by[@angelplusultra](https://github.com/angelplusultra) in[#5941](https://github.com/Mintplex-Labs/anything-llm/pull/5941)
- feat: add support for openai-compatible whisper api by[@guilopesn](https://github.com/guilopesn) in[#5952](https://github.com/Mintplex-Labs/anything-llm/pull/5952)
- fix: render kokoro-tts voice list for pre-0.3.x kokoro-fastapi servers by[@sanidhyasin](https://github.com/sanidhyasin) in[#5930](https://github.com/Mintplex-Labs/anything-llm/pull/5930)
- OC readme bump by[@timothycarambat](https://github.com/timothycarambat) in[#5966](https://github.com/Mintplex-Labs/anything-llm/pull/5966)
- fix: scope thread lookup to validated workspace in validWorkspaceAndThreadSlug by[@Joshua-Medvinsky](https://github.com/Joshua-Medvinsky) in[#5784](https://github.com/Mintplex-Labs/anything-llm/pull/5784)
- fix: MCP servers hidden from UI by post-connection errors and url-only definitions rejected by[@angelplusultra](https://github.com/angelplusultra) in[#5943](https://github.com/Mintplex-Labs/anything-llm/pull/5943)
- Agent SQL Connector Fixes by[@ss-rstocchi](https://github.com/ss-rstocchi) in[#5922](https://github.com/Mintplex-Labs/anything-llm/pull/5922)
- Fix scrollbar color on light mode by[@timothycarambat](https://github.com/timothycarambat) in[#5980](https://github.com/Mintplex-Labs/anything-llm/pull/5980)
- (OpenComputer) Update model version in use case demos by[@FI-Mihej](https://github.com/FI-Mihej) in[#5978](https://github.com/Mintplex-Labs/anything-llm/pull/5978)
- Give Downloaded Models its Own` <optgroup>` in Workspace Model Selector by[@angelplusultra](https://github.com/angelplusultra) in[#5967](https://github.com/Mintplex-Labs/anything-llm/pull/5967)
- Gate Workspace System Prompt Override in` Workspace.new()` by[@angelplusultra](https://github.com/angelplusultra) in[#5971](https://github.com/Mintplex-Labs/anything-llm/pull/5971)
- feat: add Windows x64 (WHPX) support to open-computer by[@Varun-Patkar](https://github.com/Varun-Patkar) in[#5982](https://github.com/Mintplex-Labs/anything-llm/pull/5982)
- test: fix isoDeviceArgs unit test after usb-storage change by[@Varun-Patkar](https://github.com/Varun-Patkar) in[#5987](https://github.com/Mintplex-Labs/anything-llm/pull/5987)
- Patch API Citations regression by[@timothycarambat](https://github.com/timothycarambat) in[#5994](https://github.com/Mintplex-Labs/anything-llm/pull/5994)
- Patch Generic OpenAI bug for OSA with Magic Echo by[@timothycarambat](https://github.com/timothycarambat) in[#5995](https://github.com/Mintplex-Labs/anything-llm/pull/5995)
- Add Translations for Create Scheduled Job Agent Tool by[@angelplusultra](https://github.com/angelplusultra) in[#5932](https://github.com/Mintplex-Labs/anything-llm/pull/5932)
- feat: create-scheduled-job agent tool by[@angelplusultra](https://github.com/angelplusultra) in[#5916](https://github.com/Mintplex-Labs/anything-llm/pull/5916)
- docs(bare-metal): fix env var name to match frontend/.env — VITE_API_BASE by[@unusdon](https://github.com/unusdon) in[#5996](https://github.com/Mintplex-Labs/anything-llm/pull/5996)
- oMLX LLM Provider by[@angelplusultra](https://github.com/angelplusultra) in[#5973](https://github.com/Mintplex-Labs/anything-llm/pull/5973)
- Toggle agent tools during an active agent session by[@shatfield4](https://github.com/shatfield4) in[#5856](https://github.com/Mintplex-Labs/anything-llm/pull/5856)
- fix: preserve TTS audio content type by[@ivan-digital](https://github.com/ivan-digital) in[#6012](https://github.com/Mintplex-Labs/anything-llm/pull/6012)
- Fix slash command presets not expanding into prompt text by[@shatfield4](https://github.com/shatfield4) in[#6038](https://github.com/Mintplex-Labs/anything-llm/pull/6038)
- Update Italian translation by[@albanobattistella](https://github.com/albanobattistella) in[#6018](https://github.com/Mintplex-Labs/anything-llm/pull/6018)
- i18n: add Lao (lo) language support by[@bounkirdni-2025](https://github.com/bounkirdni-2025) in[#6039](https://github.com/Mintplex-Labs/anything-llm/pull/6039)
- feat(i18n): add Indonesian (id) language support - closes[#4198](https://github.com/Mintplex-Labs/anything-llm/issues/4198) by[@samlehoy](https://github.com/samlehoy) in[#6037](https://github.com/Mintplex-Labs/anything-llm/pull/6037)
- Inline MCP tool $ref/$defs schemas for Anthropic agent tool calls by[@sanidhyasin](https://github.com/sanidhyasin) in[#5786](https://github.com/Mintplex-Labs/anything-llm/pull/5786)
- 5846 auto scroll by[@timothycarambat](https://github.com/timothycarambat) in[#6046](https://github.com/Mintplex-Labs/anything-llm/pull/6046)
- i18n: add Croatian (hr) language support by[@devanonyme42](https://github.com/devanonyme42) in[#6052](https://github.com/Mintplex-Labs/anything-llm/pull/6052)
- feat(agents): add You.com as web search provider by[@Souravrajvi0](https://github.com/Souravrajvi0) in[#6058](https://github.com/Mintplex-Labs/anything-llm/pull/6058)
- refactor: remove workspace profile picture feature by[@angelplusultra](https://github.com/angelplusultra) in[#6010](https://github.com/Mintplex-Labs/anything-llm/pull/6010)
- Add datetime to default system prompts by[@Adityai1411](https://github.com/Adityai1411) in[#6016](https://github.com/Mintplex-Labs/anything-llm/pull/6016)
- Fix: Restore Stop Generation Button During Agent Session Execution Loop by[@angelplusultra](https://github.com/angelplusultra) in[#5970](https://github.com/Mintplex-Labs/anything-llm/pull/5970)
- feat: folder upload with relative paths preserved in document manager by[@angelplusultra](https://github.com/angelplusultra) in[#6027](https://github.com/Mintplex-Labs/anything-llm/pull/6027)
- fix: keep watched document re-syncs from breaking pinned document de-duplication by[@ArslanRasheed60](https://github.com/ArslanRasheed60) in[#6029](https://github.com/Mintplex-Labs/anything-llm/pull/6029)
- Add input_audio support for audio attachments on generic OpenAI provider by[@shatfield4](https://github.com/shatfield4) in[#6055](https://github.com/Mintplex-Labs/anything-llm/pull/6055)
- Start model router cooldown after inference completes by[@shatfield4](https://github.com/shatfield4) in[#6047](https://github.com/Mintplex-Labs/anything-llm/pull/6047)
- fix: validate workspace update payloads to prevent misleading 200 responses by[@ArslanRasheed60](https://github.com/ArslanRasheed60) in[#6070](https://github.com/Mintplex-Labs/anything-llm/pull/6070)
- chore: route agent chats with no tools through native tooled completions by[@angelplusultra](https://github.com/angelplusultra) in[#6067](https://github.com/Mintplex-Labs/anything-llm/pull/6067)
- fix Bedrock model workspace selection by[@timothycarambat](https://github.com/timothycarambat) in[#6079](https://github.com/Mintplex-Labs/anything-llm/pull/6079)
- fix empty agent skill section in MuM by[@timothycarambat](https://github.com/timothycarambat) in[#6080](https://github.com/Mintplex-Labs/anything-llm/pull/6080)
- Reset agent clarifying-questions counter per turn by[@shatfield4](https://github.com/shatfield4) in[#6083](https://github.com/Mintplex-Labs/anything-llm/pull/6083)
- AWS Bedrock overhaul by[@timothycarambat](https://github.com/timothycarambat) in[#6081](https://github.com/Mintplex-Labs/anything-llm/pull/6081)
- feat: abort agent sessions end-to-end when the user stops generation by[@angelplusultra](https://github.com/angelplusultra) in[#6063](https://github.com/Mintplex-Labs/anything-llm/pull/6063)
- docs: fix broken service links in open-computer README by[@richboyneedcash](https://github.com/richboyneedcash) in[#6088](https://github.com/Mintplex-Labs/anything-llm/pull/6088)
- feat: add image generation via /img command by[@shatfield4](https://github.com/shatfield4) in[#5904](https://github.com/Mintplex-Labs/anything-llm/pull/5904)
- 6089 milvus count by[@timothycarambat](https://github.com/timothycarambat) in[#6095](https://github.com/Mintplex-Labs/anything-llm/pull/6095)
- feat: add Gitea data connector by[@ArslanRasheed60](https://github.com/ArslanRasheed60) in[#6087](https://github.com/Mintplex-Labs/anything-llm/pull/6087)
- Fix generic openai thinking parsers by[@timothycarambat](https://github.com/timothycarambat) in[#6105](https://github.com/Mintplex-Labs/anything-llm/pull/6105)
- fix(i18n): correct zh-TW scheduled job labels by[@nrps9909](https://github.com/nrps9909) in[#6112](https://github.com/Mintplex-Labs/anything-llm/pull/6112)
- feat: stream PiperTTS playback + harden long-message TTS by[@timothycarambat](https://github.com/timothycarambat) in[#6114](https://github.com/Mintplex-Labs/anything-llm/pull/6114)
- FoundryLocal overhaul by[@timothycarambat](https://github.com/timothycarambat) in[#6119](https://github.com/Mintplex-Labs/anything-llm/pull/6119)
- feat: support GitHub Enterprise URLs in the GitHub data connector by[@ArslanRasheed60](https://github.com/ArslanRasheed60) in[#6117](https://github.com/Mintplex-Labs/anything-llm/pull/6117)
- v1.16.0 tags by[@timothycarambat](https://github.com/timothycarambat) in[#6122](https://github.com/Mintplex-Labs/anything-llm/pull/6122)


## New Contributors


- [@Joshua-Medvinsky](https://github.com/Joshua-Medvinsky) made their first contribution in[#5784](https://github.com/Mintplex-Labs/anything-llm/pull/5784)
- [@ss-rstocchi](https://github.com/ss-rstocchi) made their first contribution in[#5922](https://github.com/Mintplex-Labs/anything-llm/pull/5922)
- [@FI-Mihej](https://github.com/FI-Mihej) made their first contribution in[#5978](https://github.com/Mintplex-Labs/anything-llm/pull/5978)
- [@Varun-Patkar](https://github.com/Varun-Patkar) made their first contribution in[#5982](https://github.com/Mintplex-Labs/anything-llm/pull/5982)
- [@unusdon](https://github.com/unusdon) made their first contribution in[#5996](https://github.com/Mintplex-Labs/anything-llm/pull/5996)
- [@ivan-digital](https://github.com/ivan-digital) made their first contribution in[#6012](https://github.com/Mintplex-Labs/anything-llm/pull/6012)
- [@albanobattistella](https://github.com/albanobattistella) made their first contribution in[#6018](https://github.com/Mintplex-Labs/anything-llm/pull/6018)
- [@bounkirdni-2025](https://github.com/bounkirdni-2025) made their first contribution in[#6039](https://github.com/Mintplex-Labs/anything-llm/pull/6039)
- [@samlehoy](https://github.com/samlehoy) made their first contribution in[#6037](https://github.com/Mintplex-Labs/anything-llm/pull/6037)
- [@devanonyme42](https://github.com/devanonyme42) made their first contribution in[#6052](https://github.com/Mintplex-Labs/anything-llm/pull/6052)
- [@Souravrajvi0](https://github.com/Souravrajvi0) made their first contribution in[#6058](https://github.com/Mintplex-Labs/anything-llm/pull/6058)
- [@Adityai1411](https://github.com/Adityai1411) made their first contribution in[#6016](https://github.com/Mintplex-Labs/anything-llm/pull/6016)
- [@ArslanRasheed60](https://github.com/ArslanRasheed60) made their first contribution in[#6029](https://github.com/Mintplex-Labs/anything-llm/pull/6029)
- [@richboyneedcash](https://github.com/richboyneedcash) made their first contribution in[#6088](https://github.com/Mintplex-Labs/anything-llm/pull/6088)
- [@nrps9909](https://github.com/nrps9909) made their first contribution in[#6112](https://github.com/Mintplex-Labs/anything-llm/pull/6112)


**Full Changelog** :[v1.15.0...v1.16.0](https://github.com/Mintplex-Labs/anything-llm/compare/v1.15.0...v1.16.0)
