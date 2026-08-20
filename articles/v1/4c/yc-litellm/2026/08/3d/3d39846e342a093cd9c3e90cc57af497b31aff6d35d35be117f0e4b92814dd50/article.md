---
schema_version: "1.0.0"
document_id: "3d39846e342a093cd9c3e90cc57af497b31aff6d35d35be117f0e4b92814dd50"
company_key: "yc-litellm"
company: "LiteLLM"
source_id: "yc-litellm-news-import-8bfeaefbc2ad"
canonical_url: "https://docs.litellm.ai/blog/gemini_3_7_flash"
published_at: "2026-08-13T10:00:00+00:00"
first_seen_at: "2026-08-14T04:30:08.362666+00:00"
fetched_at: "2026-08-14T04:30:09.430758+00:00"
content_hash: "sha256:bed76098fc1fab907ba84f5d8813d498f6af168b1a56fd77672bc159dc10729b"
---

# Day 0 support: Gemini 3.7 Flash

LiteLLM now supports` gemini-3.7-flash` on day 0, on both Google AI Studio (` gemini/` ) and Vertex AI (` vertex_ai/` ). Google's newest Flash model delivers faster responses with meaningfully better quality than 3.6 Flash and scores higher on complex multi-step agentic, coding, and reasoning benchmarks.


note


**No Docker image upgrade needed.** Gemini 3.7 Flash routes through the existing Gemini configs, so any recent LiteLLM version works out of the box for inference. For cost tracking, hit the **Reload Model Cost Map** button in the Admin UI (or` POST /reload/model_cost_map` ) to pull the latest pricing from GitHub. This is available on` v1.76.0` and above. The` gemini-3.7-flash` pricing and metadata are also bundled starting in` v1.98.0-dev.2` for anyone running with` LITELLM_LOCAL_MODEL_COST_MAP=true` .


## Launch pricing​


Gemini 3.7 Flash launches at a 50% discount that runs through December 31, 2027. LiteLLM tracks cost at the promotional rate.


Promotional Standard


Input $0.75 / 1M tokens $1.50 / 1M tokens


Output $3.75 / 1M tokens $7.50 / 1M tokens


Cache reads, batch, flex, and priority tiers are discounted proportionally.


## Quick Start​


- SDK
- PROXY


```text
from   litellm   import   completion       response   =   completion  (         model  =  "gemini/gemini-3.7-flash"  ,         messages  =  [  {  "role"  :     "user"  ,     "content"  :     "Summarize this article in 3 bullet points."  }  ]  ,      )         print  (  response  .  choices  [  0  ]  .  message  .  content  )
```


**1. Setup config.yaml**


```text
model_list  :         -     model_name  :   gemini  -  3.7  -  flash          litellm_params  :             model  :   gemini/gemini  -  3.7  -  flash            api_key  :   os.environ/GEMINI_API_KEY           # Or use Vertex AI         -     model_name  :   vertex  -  gemini  -  3.7  -  flash          litellm_params  :             model  :   vertex_ai/gemini  -  3.7  -  flash            vertex_project  :   your  -  project  -  id            vertex_location  :   us  -  central1
```


**2. Start proxy**


```text
litellm --config /path/to/config.yaml
```


**3. Make requests**


```text
curl -X POST http://localhost:4000/v1/chat/completions \      -H "Content-Type: application/json" \      -H "Authorization: Bearer <YOUR-LITELLM-KEY>" \      -d '{        "model": "gemini-3.7-flash",        "messages": [{"role": "user", "content": "Hello!"}]      }'
```


## Thinking levels​


Gemini 3.7 Flash is a reasoning model. LiteLLM maps OpenAI` reasoning_effort` to Gemini's` thinkingLevel` , so the same request shape you use for other reasoning models works here.


```text
from   litellm   import   completion       response   =   completion  (         model  =  "gemini/gemini-3.7-flash"  ,         messages  =  [  {  "role"  :     "user"  ,     "content"  :     "What's 2+2?"  }  ]  ,         reasoning_effort  =  "low"  ,      )         print  (  response  .  choices  [  0  ]  .  message  .  content  )
```


Known limitation at launch


The` minimal` thinking level is not yet supported on` gemini-3.7-flash` . The Gemini API returns a 400 (` Thinking level MINIMAL is not supported for this model` ). Google plans minimal thinking support as a fast follow. All other thinking levels work as expected.


## Supported Endpoints​


LiteLLM provides full end-to-end support for Gemini 3.7 Flash on:


- ` /v1/chat/completions` - OpenAI-compatible chat completions endpoint
- ` /v1/responses` - OpenAI Responses API endpoint (streaming and non-streaming)
- [/v1/messages](https://docs.litellm.ai/docs/anthropic_unified) - Anthropic-compatible messages endpoint
- ` /v1/generateContent` -[Google Gemini API](https://docs.litellm.ai/docs/generateContent) compatible endpoint


All endpoints support streaming and non-streaming responses, function calling with thought signatures, multi-turn conversations, and full multimodal input (text, image, audio, video).


## Feedback​


Running Gemini 3.7 Flash through LiteLLM and hitting something unexpected? Share it on[GitHub discussion #36799](https://github.com/BerriAI/litellm/discussions/36799) .
