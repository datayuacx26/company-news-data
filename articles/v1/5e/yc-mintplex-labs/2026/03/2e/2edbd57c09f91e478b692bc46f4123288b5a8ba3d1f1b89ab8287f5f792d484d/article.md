---
schema_version: "1.0.0"
document_id: "2edbd57c09f91e478b692bc46f4123288b5a8ba3d1f1b89ab8287f5f792d484d"
company_key: "yc-mintplex-labs"
company: "Mintplex Labs"
source_id: "yc-mintplex-labs-atom-5e945f201243"
canonical_url: "https://github.com/Mintplex-Labs/anything-llm/releases/tag/v1.11.1"
published_at: "2026-03-02T17:26:08+00:00"
first_seen_at: "2026-07-27T03:50:18.579971+00:00"
fetched_at: "2026-08-20T02:46:36.756632+00:00"
content_hash: "sha256:56df8cca2f20d532bb07d93b86b649e7a19ec52c6ea1f84800ec0f29d79c01ac"
---

# AnythingLLM v1.11.1

## Homepage Redesign


The main AnythingLLM homepage has been completely redesigned to be more modern and user-friendly so you can instantly start chatting the second you open the app after onboarding.


## Native Tool Calling


Native tool calling is the best performance and experience for tool calling with your LLM provider and model. If you can enable it, you should.


*this only applies to local LLM providers. It has no impact on cloud LLMs like OpenAI, Anthropic, or Azure.*


We have completely overhauled how` @agent` tool calling works. Now, we will leverage the new native tool calling abilities of your LLM provider and model.


**What this means for you:**


- You can now run complex, **multi-step** tool calls with your LLM provider and model.
- Your model will now continue to work until your final response is generated or determined to be complete.
- You will get 100x better responses from even small tool-calling models


We have implemented safeguards as well to prevent infinite loops with a maximum of 10 tool calls per response to prevent runaway tasks.


### Limitations


Most providers do not allow us to probe for if a model supports native tool calling.


The following local LLM providers will automatically support native tool calling if your model supports it:


- Default Built in LLM Provider (AnythingLLM Default)
- Ollama
- LM Studio


For others, you will need to set an ENV variable to enable native tool calling for supported providers.


- Generic OpenAI
- Groq
- AWS Bedrock
- Lemonade
- LiteLLM
- Local AI
- OpenRouter


This can be set via the[PROVIDER_SUPPORTS_NATIVE_TOOL_CALLING](https://github.com/Mintplex-Labs/anything-llm/blob/v1.11.1/configuration#native-tool-calling-for-llm-providers) environment variable.


```text
PROVIDER_SUPPORTS_NATIVE_TOOL_CALLING="bedrock,generic-openai,groq,lemonade,litellm,local-ai,openrouter"


```


## Lemonade by AMD Integration


[Lemonade](https://lemonade-server.ai/) by AMD is an[open-source](https://github.com/lemonade-sdk/lemonade) local model runtime that optimizes performance and efficiency for local models (LLM, ASR, TTS, Image Generation, etc.) for all types of hardware including AMD GPUs and NPUs.


We have added first class support so you can use your local models running via Lemonade within AnythingLLM for the best application experience on top of your local hardware.


---


## What's Changed


- fix: typo in contribution guidelines, update project metadata and pull_request_temp...md by[@dipanshurdev](https://github.com/dipanshurdev) in[#5010](https://github.com/Mintplex-Labs/anything-llm/pull/5010)
- feat: update light mode UI sidebar by[@angelplusultra](https://github.com/angelplusultra) in[#4996](https://github.com/Mintplex-Labs/anything-llm/pull/4996)
- fix(frontend): fix event listener memory leak in useIsDisabled hook by[@dipanshurdev](https://github.com/dipanshurdev) in[#5027](https://github.com/Mintplex-Labs/anything-llm/pull/5027)
- feat: dedicated dark theme option with system preference support by[@angelplusultra](https://github.com/angelplusultra) in[#5007](https://github.com/Mintplex-Labs/anything-llm/pull/5007)
- Implement new home page redesign by[@shatfield4](https://github.com/shatfield4) in[#4931](https://github.com/Mintplex-Labs/anything-llm/pull/4931)
- fix: GitLab connector infinite loop and rate limit crash for large repos by[@angelplusultra](https://github.com/angelplusultra) in[#5021](https://github.com/Mintplex-Labs/anything-llm/pull/5021)
- fix: add password character validation to onboarding single-user setup by[@angelplusultra](https://github.com/angelplusultra) in[#5037](https://github.com/Mintplex-Labs/anything-llm/pull/5037)
- Native Tool calling by[@timothycarambat](https://github.com/timothycarambat) in[#5071](https://github.com/Mintplex-Labs/anything-llm/pull/5071)
- fix: resolve Gemini agent 400 error on tool call responses by[@angelplusultra](https://github.com/angelplusultra) in[#5054](https://github.com/Mintplex-Labs/anything-llm/pull/5054)
- fix: prevent CMD/CTRL+Arrow scroll from overriding textarea cursor movement by[@angelplusultra](https://github.com/angelplusultra) in[#5053](https://github.com/Mintplex-Labs/anything-llm/pull/5053)
- Normalize scraper runtimeargs for bulk-scraper by[@timothycarambat](https://github.com/timothycarambat) in[#5083](https://github.com/Mintplex-Labs/anything-llm/pull/5083)
- Lemonade integration by[@timothycarambat](https://github.com/timothycarambat) in[#5077](https://github.com/Mintplex-Labs/anything-llm/pull/5077)


## New Contributors


- [@dipanshurdev](https://github.com/dipanshurdev) made their first contribution in[#5010](https://github.com/Mintplex-Labs/anything-llm/pull/5010)


**Full Changelog** :[v1.11.0...v1.11.1](https://github.com/Mintplex-Labs/anything-llm/compare/v1.11.0...v1.11.1)
