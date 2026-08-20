---
schema_version: "1.0.0"
document_id: "ec27d2e7319bdb1e69d01a1f368b101909db33d73f26e90bfb72761b78a94f9d"
company_key: "yc-cerebrium"
company: "Cerebrium"
source_id: "yc-cerebrium-news-import-eb465eef12b4"
canonical_url: "https://cerebrium.ai/blog/a-low-latency-architecture-for-voice-agents-with-real-time-web-search"
published_at: "2026-07-15T14:13:03+00:00"
first_seen_at: "2026-07-21T13:06:54.599340+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:22f904cc15e6fb89f6df8775c47162396babff1d1f9311e6a95ae5bd0b3a0498"
---

# A Low-Latency Architecture for Voice Agents with Real-time Web Search

A Low-Latency Architecture for Voice Agents with Real-time Web Search


**Cerebrium + Linkup + LiveKit**


## The hidden tradeoff in production voice agents


[Building a real-time voice agent](https://cerebrium.ai/blog/deploying-a-global-scale-ai-voice-agent-with-500ms-latency) forces teams into a difficult constraint: **latency vs. intelligence.**


Unlike text chat, voice interactions operate within a strict latency budget. If responses take too long, the conversation breaks down. Production systems generally target **sub-second time-to-first-audio** from the end of a user's utterance — beyond ~1.5–2s of silence, the interaction feels broken.


[A typical voice pipeline](https://cerebrium.ai/docs/v4/examples/realtime-voice-agents) looks like this:


```text
User Speech → STT → LLM Reasoning → TTS → Audio Response
```


Each stage adds latency:


Stage Typical latency


Streaming STT 100–150 ms


LLM time-to-first-token 300 ms – 1.2 s


TTS first audio 100–150 ms


This leaves little headroom. To stay in budget, teams often reach for small, fast models (Gemini Flash, GPT-4.1-mini, Llama 3.1 8B, Ultravox). They respond quickly, but bring real limitations:


-


Outdated knowledge (frozen training cutoff)


-


Weaker reasoning


-


Higher hallucination rates on factual queries


For anything beyond narrow use cases, teams face a subtle tradeoff: tolerate hallucinations, or accept the slower responses of larger models.


## Two levers, not one: fast MoE inference *and* Web Search retrieval


This architecture attacks the tradeoff from both sides.


**1. A fast MoE model instead of a small dense model.** We serve[Qwen/Qwen3.6-35B-A3B](https://huggingface.co/Qwen/Qwen3.6-35B-A3B) — a 35B-parameter Mixture-of-Experts model with only ~3B active parameters per token. You get large-model quality at small-model latency, especially with DFLASH speculative decoding on top. In production we measure **60–115 tokens/sec** and **300–510 ms TTFT** on a[single NVIDIA B200](https://cerebrium.ai/docs/getting-started/introduction) .


**2. Conditional retrieval for freshness.** No model, however large, knows today's stock price. When a query needs live facts, the model[calls a search API](https://www.linkup.so/) , retrieves grounded context, and answers from it. The catch: most search APIs add 800–1500 ms.[Linkup's /Search (fast) endpoint](https://docs.linkup.so/pages/changelog/fast-depth) is optimized for AI web search retrieval and returns in **sub-second** time (~0.7 s measured) — and we still hide it behind a spoken filler (below).


## Architecture: Cerebrium + Linkup + LiveKit


```text
User Speech
↓
STT (Deepgram nova-3)
↓
LLM tool decision (Qwen3.6-35B-A3B on Cerebrium B200, via SGLang)
↓
Conditional tool call ──► Linkup Web Search API ──► Retrieved context
↓
LLM grounded response
↓
TTS (Cartesia sonic-2)
↓
Audio Response
```


The key design decision is **conditional retrieval** : the LLM only calls Linkup when a query needs external knowledge (current events, prices, company data, docs). For conversational turns it answers immediately, with no search.


## Measured latency (live console session)


Stage Measured


Streaming STT (transcription delay) 120–230 ms


End-of-utterance detection ~580–600 ms


LLM #1 — tool decision (TTFT) 330–510 ms


Linkup` fast` search (` searchResults` ) ~0.7 s


LLM #2 — grounded response (TTFT) 330–360 ms


TTS first audio (TTFB) 130–190 ms


Perceived time-to-first-audio (filler) ~1.0 s


Time to grounded spoken answer ~2–3 s


The honest takeaway: retrieval still costs real time (~0.7s), but[Linkup's fast depth](https://docs.linkup.so/pages/documentation/endpoints/search/reference?utm_source=chatgpt.com) keeps it sub-second. The system stays conversational by **speaking a filler the moment a search starts** , so the user hears a response at ~1s while the search completes in the background.


### The latency-masking technique


When the model calls[search_web](https://docs.livekit.io/agents/logic/tools/definition/) , the agent immediately says a short filler ("I'm checking the web for you right now") before awaiting Linkup. This is the single most important UX trick in the system — it converts the retrieval gap into a natural, responsive-feeling turn.


We use[outputType: "searchResults"](https://docs.linkup.so/pages/sdk/python/python) rather than` "sourcedAnswer"` : Linkup returns raw ranked snippets and skips its own answer-composition step, which trims latency (and cost) since our LLM already synthesizes the spoken reply from the retrieved context.


```text
@function_tool  ()
async   def   search_web  (self, ctx: RunContext, query:   str  ) ->   str  :
"""Search the web for real-time information using Linkup fast search."""
ctx.session.say(  "I'm checking the web for you right now."  ,
allow_interruptions  =  True  )
data   =   await   self  ._linkup_request(  "search"  , {
"q"  : query,
"depth"  :   "fast"  ,
"outputType"  :   "searchResults"  ,    # raw snippets; our LLM composes the reply
"maxResults"  :   3  ,
"includeImages"  :   False  ,
})
return   _format_sourced_answer(data)   if   data   else   "NO_RESULTS: ..."
```


## Implementing the pipeline


You can access the fine Github repository[here](https://github.com/CerebriumAI/examples/tree/master/6-voice/19-voice-webagent)


### 1. Deploy the LLM on Cerebrium


Qwen3.6-35B-A3B is served with[SGLang](https://cerebrium.ai/docs/v4/examples/deploy-a-vision-language-model-with-sglang) + DFLASH speculative decoding on a B200,[using a custom runtime](https://cerebrium.ai/docs/toml-reference/toml-reference) . Key settings from` sglang-llm/cerebrium.toml` :


```text
[  cerebrium  .  deployment  ]
docker_base_image_url =   "modalresearch/sglang:nightly-dev-cu13-20260619-patched"


[  cerebrium  .  hardware  ]
compute =   "BLACKWELL_B200"
gpu_count =   1


[  cerebrium  .  scaling  ]
min_replicas =   1            # avoid cold starts on a large MoE
replica_concurrency =   16


[  cerebrium  .  runtime  .  custom  ]
port =   8000
healthcheck_endpoint =   "/health"
entrypoint = [  "python3"  ,   "main.py"  ]
```


Core SGLang server args:


```text
--speculative-algorithm DFLASH
--speculative-draft-model-path z-lab/Qwen3.6-35B-A3B-DFlash
--speculative-num-draft-tokens 16
--attention-backend trtllm_mha
--speculative-draft-attention-backend fa4
--mem-fraction-static 0.75
--reasoning-parser qwen3
--tool-call-parser qwen3_coder
```


> **Note on constrained decoding:** DFLASH speculative decoding does not yet support grammar / JSON-schema constrained output. If you need structured JSON, disable DFLASH for those requests or post-process plain text.


### 2. Point the voice agent at your endpoint


The LiveKit agent uses an OpenAI-compatible client against the[Cerebrium endpoint](https://cerebrium.ai/docs/endpoints/openai-compatible-endpoints) . Disabling "thinking" tokens is a real latency lever:


```text
openai.LLM(
model  =  "Qwen/Qwen3.6-35B-A3B"  ,
base_url  =  os.getenv(  "LLM_BASE_URL"  ),     # <https://api.cerebrium.ai/v4/><project>/sglang-llm/v1
api_key  =  os.getenv(  "CEREBRIUM_API_KEY"  ),
temperature  =  0.4  ,
extra_body  =  {  "chat_template_kwargs"  : {  "enable_thinking"  :   False  }},
)
```


### 3. Register Linkup as a tool


The LLM is given a` search_web` tool (via LiveKit's` @function_tool` ); when it detects a factual query it emits a tool call, Linkup returns ranked search results, and the snippets are injected into context for the final response.


## Before / after


Metric GPT-4o-mini baseline Cerebrium Qwen3.6 + Linkup


STT ~0 ms (streamed) ~0 ms (streamed)


LLM #1 TTFT (tool decision) ~1.9 s ~0.3–0.5 s


Web search ~2.0 s ~0.7 s


LLM #2 TTFT (response) ~1.0 s ~0.3 s


TTS TTFB ~0.3 s ~0.1–0.2 s


Perceived time-to-first-audio ~5.3 s ~1.0 s (filler) / ~2–3 s answer


## The result: fast *and* accurate voice agents


Voice latency constraints do not require sacrificing intelligence. By pairing a fast MoE model (Qwen3.6 35B-A3B + DFLASH on Cerebrium B200) with[low-latency retrieval](https://cerebrium.ai/blog/creating-a-realtime-rag-voice-agent) (Linkup) and a filler-masked conditional tool call, voice agents stay inside the conversational window while grounding answers in the live web — with transparent, reproducible latency numbers rather than best-case estimates.
