---
schema_version: "1.0.0"
document_id: "8eaa930b068a9eca0c41567b08df3c76a6907f6b0d941ea551b5346f2f4d70d3"
company_key: "yc-mintplex-labs"
company: "Mintplex Labs"
source_id: "yc-mintplex-labs-atom-5e945f201243"
canonical_url: "https://github.com/Mintplex-Labs/anything-llm/releases/tag/v1.12.0"
published_at: "2026-04-02T20:56:02+00:00"
first_seen_at: "2026-07-27T03:50:18.579971+00:00"
fetched_at: "2026-08-20T02:46:36.756632+00:00"
content_hash: "sha256:6fb17a6daddcf6766232340a4d3270d05992a39019bcad4ef5350747caf23572"
---

# AnythingLLM v1.12.0

## Major Features


### Automatic Mode for native tool calling


For[Select providers](https://docs.anythingllm.com/features/chat-modes#available-chat-modes) that support native tool calling, you no longer need to use` @agent` to use tools. You can now just use the tools without asking.


If your prompt input does not have the "@" symbol, your chats will automatically use tools as needed.


docs-agent-example.1.mov


## Intelligent Tool Selection


We have added a new feature called[Intelligent Tool Selection](https://docs.anythingllm.com/agent/intelligent-tool-selection) . This feature allows you to load **unlimited** tools for your agent to use into context with better performance and save up to 80% on token usage every single chat.


## Filesystem Agent


We have added a new feature called[Filesystem Agent](https://docs.anythingllm.com/agent/usage#file-system-agent) . This feature allows you to use the filesystem of your host machine to search for files and directories.


## Document Generation Agent


We have added a new built-in agent for[Document Generation](https://docs.anythingllm.com/agent/usage#document-generation-agent) . With document generation, you can generate text files, PDFs, Excel files, Docx, and even entire PowerPoint presentations.


docgen.1.mp4


## Telegram Bot


AnythingLLM Docker and Desktop now support a[Telegram bot](https://docs.anythingllm.com/channels/telegram) so you can connect to your AnythingLLM instance anywhere in the world.


**Supports** :


- Text chat (streaming & thinking)
- Image understanding
- Voice messages & Attachments
- Automatic mode and[@agent](https://github.com/agent) support
- Workspace and thread selection
- Model selection
- Citations
- Any agent skill available in AnythingLLM


## What's Changed


- update exa search provider description by[@theishangoswami](https://github.com/theishangoswami) in[#5225](https://github.com/Mintplex-Labs/anything-llm/pull/5225)
- Automatic mode for workspace (Agent mode default) by[@timothycarambat](https://github.com/timothycarambat) in[#5143](https://github.com/Mintplex-Labs/anything-llm/pull/5143)
- MCP tool manager by[@timothycarambat](https://github.com/timothycarambat) in[#5230](https://github.com/Mintplex-Labs/anything-llm/pull/5230)
- Intelligent Skill Selection by[@timothycarambat](https://github.com/timothycarambat) in[#5236](https://github.com/Mintplex-Labs/anything-llm/pull/5236)
- README updates by[@timothycarambat](https://github.com/timothycarambat) in[#5238](https://github.com/Mintplex-Labs/anything-llm/pull/5238)
- fix(collector): infer file extension from Content-Type for URLs without explicit extensions by[@Lyt060814](https://github.com/Lyt060814) in[#5252](https://github.com/Mintplex-Labs/anything-llm/pull/5252)
- feat: add Lithuanian locale and register in resources by[@arvydev](https://github.com/arvydev) in[#5243](https://github.com/Mintplex-Labs/anything-llm/pull/5243)
- Telegram bot connector by[@shatfield4](https://github.com/shatfield4) in[#5190](https://github.com/Mintplex-Labs/anything-llm/pull/5190)
- Add User-Agent header for Anthropic API calls by[@mikelambert](https://github.com/mikelambert) in[#5174](https://github.com/Mintplex-Labs/anything-llm/pull/5174)
- add Dynamic` max_tokens` retreival for Anthropic models by[@timothycarambat](https://github.com/timothycarambat) in[#5255](https://github.com/Mintplex-Labs/anything-llm/pull/5255)
- fix Firefox LaTeX rendering by[@timothycarambat](https://github.com/timothycarambat) in[#5258](https://github.com/Mintplex-Labs/anything-llm/pull/5258)
- add ask to run prompt for tool calls (demo) by[@timothycarambat](https://github.com/timothycarambat) in[#5261](https://github.com/Mintplex-Labs/anything-llm/pull/5261)
- Refactor onboarding welcome screen to v2 design by[@angelplusultra](https://github.com/angelplusultra) in[#5262](https://github.com/Mintplex-Labs/anything-llm/pull/5262)
- Filesystem Agent Skill overhaul by[@timothycarambat](https://github.com/timothycarambat) in[#5260](https://github.com/Mintplex-Labs/anything-llm/pull/5260)
- feat : auto-select newly uploaded docs/URLs in my documents list by[@nehaaprasad](https://github.com/nehaaprasad) in[#5222](https://github.com/Mintplex-Labs/anything-llm/pull/5222)
- feat: add missing Lemonade LLM provider env vars to .env.example by[@angelplusultra](https://github.com/angelplusultra) in[#5275](https://github.com/Mintplex-Labs/anything-llm/pull/5275)
- feat: add optional API key support for Lemonade provider by[@angelplusultra](https://github.com/angelplusultra) in[#5281](https://github.com/Mintplex-Labs/anything-llm/pull/5281)
- File creation agent skills by[@timothycarambat](https://github.com/timothycarambat) in[#5280](https://github.com/Mintplex-Labs/anything-llm/pull/5280)
- Redesign Telegram bot settings UI by[@shatfield4](https://github.com/shatfield4) in[#5306](https://github.com/Mintplex-Labs/anything-llm/pull/5306)
- Fix chat UI event listener bloat by[@timothycarambat](https://github.com/timothycarambat) in[#5323](https://github.com/Mintplex-Labs/anything-llm/pull/5323)


## New Contributors


- [@theishangoswami](https://github.com/theishangoswami) made their first contribution in[#5225](https://github.com/Mintplex-Labs/anything-llm/pull/5225)
- [@Lyt060814](https://github.com/Lyt060814) made their first contribution in[#5252](https://github.com/Mintplex-Labs/anything-llm/pull/5252)
- [@arvydev](https://github.com/arvydev) made their first contribution in[#5243](https://github.com/Mintplex-Labs/anything-llm/pull/5243)
- [@mikelambert](https://github.com/mikelambert) made their first contribution in[#5174](https://github.com/Mintplex-Labs/anything-llm/pull/5174)


**Full Changelog** :[v1.11.2...v1.12.0](https://github.com/Mintplex-Labs/anything-llm/compare/v1.11.2...v1.12.0)
