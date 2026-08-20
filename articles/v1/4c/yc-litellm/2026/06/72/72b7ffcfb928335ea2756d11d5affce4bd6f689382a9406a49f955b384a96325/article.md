---
schema_version: "1.0.0"
document_id: "72b7ffcfb928335ea2756d11d5affce4bd6f689382a9406a49f955b384a96325"
company_key: "yc-litellm"
company: "LiteLLM"
source_id: "yc-litellm-news-import-8bfeaefbc2ad"
canonical_url: "https://docs.litellm.ai/blog/claude_fable_5"
published_at: "2026-06-10T10:00:00+00:00"
first_seen_at: "2026-07-22T02:33:29.495129+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:83fee9b71200a002246b32f8cb49786b9defc497e682c265c9b27b89f8e5b6c9"
---

# Day 0 Support: Claude Fable 5

LiteLLM now supports[Claude Fable 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) on Day 0. Use it across Anthropic, Azure, Vertex AI, and Bedrock through the LiteLLM AI Gateway. Call it with the same OpenAI-compatible request you already use, and track spend, rate limits, and logging in one place.


## What's new in Fable 5​


Fable 5 is Anthropic's first publicly available Mythos-class model, priced at 2x Opus 4.8. A few things stand out for teams running it through a gateway:


- **The frontier, now public.** Anthropic reports Fable 5 is state-of-the-art on nearly all tested benchmarks, and the highest scorer among frontier models on Cognition's frontier coding benchmark, even at medium reasoning effort. ([details from Anthropic](https://www.anthropic.com/news/claude-fable-5-mythos-5) )
- **Built for long-running work.** A 1M-token context window and up to 128K output tokens, with focus that holds across millions of tokens in long-horizon agentic tasks.
- **Adaptive thinking only.** Fable 5 decides how deeply to think on its own. You steer it per request with` reasoning_effort` or` output_config.effort` ; fixed thinking budgets,` temperature` ,` top_p` , and assistant message prefill are not supported by the model.
- **$10 / MTok input and $50 / MTok output** , with prompt caching at $1.00 / MTok (read) and $12.50 / MTok (write). On Bedrock, the` us.` and` eu.` inference profiles carry the usual 10% regional premium while` global.` stays at base price; LiteLLM tracks every variant automatically.
- **A fallback you might notice.** On flagged cybersecurity and biology requests (under 5% of sessions, per Anthropic), the response is served by Opus 4.8 instead.
- **One gateway, every surface.** Vision, PDF input, computer use, tool calling, prompt caching, adaptive thinking, and structured output, all available across Anthropic, Azure, Vertex AI, and Bedrock with unified spend tracking, logging, and fallbacks.


## Before you flip it on: provider opt-ins​


Fable 5 requires a data sharing opt-in on some clouds; prompts are shared with Anthropic and retained for up to 30 days.


- **Bedrock** : set your account's data retention mode to` provider_data_share` , and invoke through an inference profile (` us.` ,` eu.` , or` global.` prefix); direct model ID invocation is not supported.
- **Vertex AI** : enable Anthropic data sharing for your project and accept the Fable 5 terms in Model Garden.
- **Azure AI Foundry** : create a` claude-fable-5` deployment; the model's TPM quota meter starts at 0 on some subscriptions, so you may need a quota request first.


## Enabling Fable 5​


Fable 5 ships in the **` v1.89.0-rc.2`** image (and every release after it). How you pick it up depends on where your proxy reads pricing from:


-


**Default (remote cost map): no upgrade needed.** In the LiteLLM UI, open the **Price Data** tab under **Models + Endpoints** and click **Reload Price Data** (or, as a proxy admin,` POST /reload/model_cost_map` ). This refetches the latest pricing from LiteLLM's cost map **and** re-registers provider routing in one step, so` claude-fable-5` becomes available across Anthropic, Azure, Vertex AI, and Bedrock, even if you're on an older proxy version.


-


**Running` LITELLM_LOCAL_MODEL_COST_MAP=true` ?** The cost map is baked into the image, so the Reload button won't reach it. Pull` v1.89.0-rc.2` or later to get the bundled Fable 5 metadata:


```text
docker pull ghcr.io/berriai/litellm:v1.89.0-rc.2
```


## Usage - Anthropic​


- LiteLLM Proxy


**1. Setup config.yaml**


```text
model_list  :         -     model_name  :   claude  -  fable  -  5           litellm_params  :             model  :   anthropic/claude  -  fable  -  5             api_key  :   os.environ/ANTHROPIC_API_KEY
```


**2. Start the proxy**


```text
docker run -d \      -p 4000:4000 \      -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \      -v $(pwd)/config.yaml:/app/config.yaml \      ghcr.io/berriai/litellm:v1.89.0-rc.2 \      --config /app/config.yaml
```


**3. Test it!**


```text
curl --location 'http://0.0.0.0:4000/chat/completions' \    --header 'Content-Type: application/json' \    --header 'Authorization: Bearer $LITELLM_KEY' \    --data '{      "model": "claude-fable-5",      "messages": [        {          "role": "user",          "content": "what llm are you"        }      ]    }'
```


## Usage - Azure​


- LiteLLM Proxy


**1. Setup config.yaml**


```text
model_list  :         -     model_name  :   claude  -  fable  -  5           litellm_params  :             model  :   azure_ai/claude  -  fable  -  5             api_key  :   os.environ/AZURE_AI_API_KEY            api_base  :   os.environ/AZURE_AI_API_BASE    # https://<resource>.services.ai.azure.com
```


**2. Start the proxy**


```text
docker run -d \      -p 4000:4000 \      -e AZURE_AI_API_KEY=$AZURE_AI_API_KEY \      -e AZURE_AI_API_BASE=$AZURE_AI_API_BASE \      -v $(pwd)/config.yaml:/app/config.yaml \      ghcr.io/berriai/litellm:v1.89.0-rc.2 \      --config /app/config.yaml
```


**3. Test it!**


```text
```


## Usage - Vertex AI​


- LiteLLM Proxy


**1. Setup config.yaml**


```text
model_list  :         -     model_name  :   claude  -  fable  -  5           litellm_params  :             model  :   vertex_ai/claude  -  fable  -  5             vertex_project  :   os.environ/VERTEX_PROJECT            vertex_location  :   global
```


**2. Start the proxy**


```text
docker run -d \      -p 4000:4000 \      -e VERTEX_PROJECT=$VERTEX_PROJECT \      -e GOOGLE_APPLICATION_CREDENTIALS=/app/credentials.json \      -v $(pwd)/config.yaml:/app/config.yaml \      -v $(pwd)/credentials.json:/app/credentials.json \      ghcr.io/berriai/litellm:v1.89.0-rc.2 \      --config /app/config.yaml
```


**3. Test it!**


```text
```


## Usage - Bedrock​


note


Bedrock only serves Fable 5 through inference profiles, so the model ID must carry a` us.` ,` eu.` , or` global.` prefix. Invoking the bare` anthropic.claude-fable-5` model ID returns a validation error.


- LiteLLM Proxy


**1. Setup config.yaml**


```text
model_list  :         -     model_name  :   claude  -  fable  -  5           litellm_params  :             model  :   bedrock/converse/us.anthropic.claude  -  fable  -  5             aws_access_key_id  :   os.environ/AWS_ACCESS_KEY_ID            aws_secret_access_key  :   os.environ/AWS_SECRET_ACCESS_KEY            aws_region_name  :   us  -  east  -  1
```


**2. Start the proxy**


```text
docker run -d \      -p 4000:4000 \      -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \      -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \      -v $(pwd)/config.yaml:/app/config.yaml \      ghcr.io/berriai/litellm:v1.89.0-rc.2 \      --config /app/config.yaml
```


**3. Test it!**


```text
```


## Advanced Features​


### Adaptive Thinking​


note


When using` reasoning_effort` with Claude Fable 5, all values are mapped to` thinking: {type: "adaptive"}` . Fable 5 only supports adaptive thinking; explicit budgets via` thinking: {type: "enabled", budget_tokens: ...}` are rejected by the Anthropic API with a 400 error. To control thinking depth, pair adaptive thinking with` output_config.effort` (seeEffort Levels below) rather than a fixed budget.


- /chat/completions
- /v1/messages


LiteLLM supports adaptive thinking through the` reasoning_effort` parameter:


```text
curl --location 'http://0.0.0.0:4000/chat/completions' \    --header 'Content-Type: application/json' \    --header 'Authorization: Bearer $LITELLM_KEY' \    --data '{      "model": "claude-fable-5",      "messages": [        {          "role": "user",          "content": "Solve this complex problem: What is the optimal strategy for..."        }      ],      "reasoning_effort": "high"    }'
```


Use the` thinking` parameter with` type: "adaptive"` to enable adaptive thinking mode:


```text
curl --location 'http://0.0.0.0:4000/v1/messages' \    --header 'x-api-key: sk-12345' \    --header 'content-type: application/json' \    --data '{        "model": "claude-fable-5",        "max_tokens": 16000,        "thinking": {            "type": "adaptive"        },        "messages": [            {                "role": "user",                "content": "Explain why the sum of two even numbers is always even."            }        ]    }'
```


### Effort Levels​


Claude Fable 5 supports the full effort ladder:` low` ,` medium` ,` high` (default),` xhigh` , and` max` . These give you finer-grained control over how much reasoning the model applies to a task. Pass the effort level via the` output_config` parameter.


On Bedrock,` output_config.effort` caps at` xhigh` ; the other providers accept the full ladder up to` max` .


- /chat/completions
- /v1/messages


```text
curl --location 'http://0.0.0.0:4000/chat/completions' \    --header 'Content-Type: application/json' \    --header 'Authorization: Bearer $LITELLM_KEY' \    --data '{      "model": "claude-fable-5",      "messages": [        {          "role": "user",          "content": "Explain quantum computing"        }      ],      "output_config": {        "effort": "max"      }    }'
```


**Using OpenAI SDK:**


```text
import   openai       client   =   openai  .  OpenAI  (         api_key  =  "your-litellm-key"  ,         base_url  =  "http://0.0.0.0:4000"      )        response   =   client  .  chat  .  completions  .  create  (         model  =  "claude-fable-5"  ,         messages  =  [  {  "role"  :     "user"  ,     "content"  :     "Explain quantum computing"  }  ]  ,         extra_body  =  {  "output_config"  :     {  "effort"  :     "max"  }  }      )
```


**Using LiteLLM SDK:**


```text
from   litellm   import   completion       response   =   completion  (         model  =  "anthropic/claude-fable-5"  ,         messages  =  [  {  "role"  :     "user"  ,     "content"  :     "Explain quantum computing"  }  ]  ,         output_config  =  {  "effort"  :     "max"  }  ,      )
```


You can combine` reasoning_effort` with` output_config` for even more fine-grained control over the model's behavior.


```text
curl --location 'http://0.0.0.0:4000/v1/messages' \    --header 'x-api-key: sk-12345' \    --header 'content-type: application/json' \    --data '{        "model": "claude-fable-5",        "max_tokens": 4096,        "messages": [            {                "role": "user",                "content": "Explain quantum computing"            }        ],        "output_config": {            "effort": "max"        }    }'
```


**Effort level guide:**


Effort When to use


` low` Short, fast responses for simple lookups, formatting, and classification


` medium` Balanced tradeoff for everyday Q&A and light reasoning


` high` (default) Complex reasoning, code generation, analysis


` xhigh` Hard problems like multi-step math, deep research, and agentic planning


` max` The hardest tasks where you want maximum reasoning depth regardless of latency (not available on Bedrock)
