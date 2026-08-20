---
schema_version: "1.0.0"
document_id: "dd16e61019cf329aaa9de719abf7c186b5076473c092c1e8274b96fdfb720dfe"
company_key: "yc-mintplex-labs"
company: "Mintplex Labs"
source_id: "yc-mintplex-labs-atom-5e945f201243"
canonical_url: "https://github.com/Mintplex-Labs/anything-llm/releases/tag/v1.11.2"
published_at: "2026-03-19T19:06:49+00:00"
first_seen_at: "2026-07-27T03:50:18.579971+00:00"
fetched_at: "2026-08-20T02:46:36.756632+00:00"
content_hash: "sha256:529056cc7a7243a0a220235e4ba6c35625a949a8b4572ba80883f607aa1cdd02"
---

# AnythingLLM v1.11.2

## More UI Improvements


changelog-1.11.2-uiv2.mp4


Now, in the main chat UI we added some much desired UI improvements and fixes.


- New prompt input
- Better Citations UI and reporting
- Metrics for Agent calls
- Report document and web-search citations during Agent calls!
- Ability to each toggle on/off Agent skills from the prompt
- Ability to select the provider and model for the workspace without leaving the page.


---


## What's Changed


- 5112 or stream metrics and finish reason by[@timothycarambat](https://github.com/timothycarambat) in[#5117](https://github.com/Mintplex-Labs/anything-llm/pull/5117)
- Fix bug where` yarn setup:envs` fails if any .env file already exists. by[@brianpursley](https://github.com/brianpursley) in[#5116](https://github.com/Mintplex-Labs/anything-llm/pull/5116)
- fix: show actionable error when LMStudio model listing fails or returns empty by[@elevatingcreativity](https://github.com/elevatingcreativity) in[#5131](https://github.com/Mintplex-Labs/anything-llm/pull/5131)
- Add automatic chat mode with native tool calling support by[@timothycarambat](https://github.com/timothycarambat) in[#5140](https://github.com/Mintplex-Labs/anything-llm/pull/5140)
- Sidebar updates by[@timothycarambat](https://github.com/timothycarambat) in[#5154](https://github.com/Mintplex-Labs/anything-llm/pull/5154)
- Remove Google web-search Programmable SERP by[@timothycarambat](https://github.com/timothycarambat) in[#5156](https://github.com/Mintplex-Labs/anything-llm/pull/5156)
- refactor: refactor agent skills settings page to use i18n translation keys by[@angelplusultra](https://github.com/angelplusultra) in[#5146](https://github.com/Mintplex-Labs/anything-llm/pull/5146)
- chore: add ESLint to` /collector` by[@angelplusultra](https://github.com/angelplusultra) in[#5128](https://github.com/Mintplex-Labs/anything-llm/pull/5128)
- chore: add ESLint to` /server` by[@angelplusultra](https://github.com/angelplusultra) in[#5126](https://github.com/Mintplex-Labs/anything-llm/pull/5126)
- Fix: Azure OpenAI model key collision by[@RALaBarge](https://github.com/RALaBarge) in[#5092](https://github.com/Mintplex-Labs/anything-llm/pull/5092)
- feat: Add tooltip for paperclip attach button when no files are parsed by[@angelplusultra](https://github.com/angelplusultra) in[#5139](https://github.com/Mintplex-Labs/anything-llm/pull/5139)
- fix: add missing /wiki to Confluence cloud citation URLs by[@MaxwellCalkin](https://github.com/MaxwellCalkin) in[#5167](https://github.com/Mintplex-Labs/anything-llm/pull/5167)
- Strip thinking from copy message outputs by[@timothycarambat](https://github.com/timothycarambat) in[#5179](https://github.com/Mintplex-Labs/anything-llm/pull/5179)
- Add custom fetch to embedder for Ollama by[@timothycarambat](https://github.com/timothycarambat) in[#5180](https://github.com/Mintplex-Labs/anything-llm/pull/5180)
- chore: add script to detect and prune unused translation keys by[@angelplusultra](https://github.com/angelplusultra) in[#5141](https://github.com/Mintplex-Labs/anything-llm/pull/5141)
- chore: add ESLint CI workflow by[@angelplusultra](https://github.com/angelplusultra) in[#5160](https://github.com/Mintplex-Labs/anything-llm/pull/5160)
- Implement v2 chat layout designs by[@timothycarambat](https://github.com/timothycarambat) in[#5074](https://github.com/Mintplex-Labs/anything-llm/pull/5074)
- Improve zh_TW Traditional Chinese locale by[@PeterDaveHello](https://github.com/PeterDaveHello) in[#5187](https://github.com/Mintplex-Labs/anything-llm/pull/5187)
- Improve build times for tests and lint by[@timothycarambat](https://github.com/timothycarambat) in[#5193](https://github.com/Mintplex-Labs/anything-llm/pull/5193)
- Support Agent stream metric reporting by[@timothycarambat](https://github.com/timothycarambat) in[#5197](https://github.com/Mintplex-Labs/anything-llm/pull/5197)
- Report citations for Agent call stacks by[@timothycarambat](https://github.com/timothycarambat) in[#5199](https://github.com/Mintplex-Labs/anything-llm/pull/5199)
- Add FileRow Indentation on Documents Picker by[@timothycarambat](https://github.com/timothycarambat) in[#5201](https://github.com/Mintplex-Labs/anything-llm/pull/5201)
- Remove` WelcomeMessages` from app - no longer used by[@timothycarambat](https://github.com/timothycarambat) in[#5206](https://github.com/Mintplex-Labs/anything-llm/pull/5206)
- feat: Add document count indicators to workspace document management modal by[@angelplusultra](https://github.com/angelplusultra) in[#5207](https://github.com/Mintplex-Labs/anything-llm/pull/5207)
- feat(agents): Add Perplexity Search API as web search provider by[@kesku](https://github.com/kesku) in[#5210](https://github.com/Mintplex-Labs/anything-llm/pull/5210)


## New Contributors


- [@brianpursley](https://github.com/brianpursley) made their first contribution in[#5116](https://github.com/Mintplex-Labs/anything-llm/pull/5116)
- [@elevatingcreativity](https://github.com/elevatingcreativity) made their first contribution in[#5131](https://github.com/Mintplex-Labs/anything-llm/pull/5131)
- [@RALaBarge](https://github.com/RALaBarge) made their first contribution in[#5092](https://github.com/Mintplex-Labs/anything-llm/pull/5092)
- [@MaxwellCalkin](https://github.com/MaxwellCalkin) made their first contribution in[#5167](https://github.com/Mintplex-Labs/anything-llm/pull/5167)
- [@PeterDaveHello](https://github.com/PeterDaveHello) made their first contribution in[#5187](https://github.com/Mintplex-Labs/anything-llm/pull/5187)
- [@kesku](https://github.com/kesku) made their first contribution in[#5210](https://github.com/Mintplex-Labs/anything-llm/pull/5210)


**Full Changelog** :[v1.11.1...v1.11.2](https://github.com/Mintplex-Labs/anything-llm/compare/v1.11.1...v1.11.2)
