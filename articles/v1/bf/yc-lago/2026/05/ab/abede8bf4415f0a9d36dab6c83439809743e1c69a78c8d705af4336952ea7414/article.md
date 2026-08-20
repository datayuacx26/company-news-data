---
schema_version: "1.0.0"
document_id: "abede8bf4415f0a9d36dab6c83439809743e1c69a78c8d705af4336952ea7414"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/ai-billing-is-mostly-token-plumbing"
published_at: "2026-05-26T21:00:43+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:42:46.458609+00:00"
content_hash: "sha256:35136644093ee752298a2e4a543177b25b9f4d4992678541d832e368d7e64e60"
---

# AI Billing is (mostly) token plumbing

*Why we built the Lago Agent SDK, and what we're shipping next.*


We just released the Lago Agent SDK. Two libraries, Python and TypeScript. They wrap your LLM client and send token usage to Lago for billing. That's the surface.


The point is what you stop doing.


## The token plumbing


Every team that shipped an AI feature in the last 18 months built the same thing. Smart search, inbox triage, meeting summaries, coding agents, vibe-coded apps. All of them ended up writing token-extraction middleware.


The middleware is the same job, repeated everywhere. Call an LLM. Parse the response for token counts. Attribute the call to a customer. Send the count to a[billing system](https://getlago.com/blog/understanding-billing-system-software) . Repeat for every provider, every model family, every streaming response, every retry, every cached call.


Every provider returns usage in a different shape:


```text
openai_resp  .  usage  .  prompt_tokens
anthropic_resp  .  usage  .  input_tokens            # plus cache_creation_input_tokens, cache_read_input_tokens
bedrock_resp  [  "usage"  ]  [  "inputTokens"  ]           # camelCase, dict access, no cache fields at this level


```


Cache tokens have sub-types. Streaming responses bury usage in the last event, sometimes. Reasoning tokens are folded into output on some models, broken out on others. The schemas change every quarter.


This is the token plumbing. Not differentiating, not what your AI feature is for, and it breaks every time a provider ships an update.


## Two audiences, same plumbing


The B2B SaaS team adding AI to an existing product. Intercom shipping Fin on top of seat-based pricing. Notion layering AI as a per-seat add-on. Atlassian Intelligence rolling out across Jira and Confluence. The team has billed per-seat for years and now needs to charge for inference-backed features without rewriting the engine. Product wants AI live in two weeks. Engineering owns a sidecar nobody wants to maintain. The CFO wants to know if the feature has positive margin. Nobody can answer cleanly because token data lives in logs, not invoices.


The AI-native team building on top of LLMs. Cursor, Lovable, Replit, voice and browser agents. They pay a per-token rate to a model provider and bill the user with margin on top. Cost-plus, end to end. Every point of margin matters because COGS is variable per-customer and tracked in real time. Under-count and they bleed margin. Over-count and they lose trust. The middleware has to be exact, every release, for every model they add.


Both groups built the same plumbing. We're tired of building it.


## Wrap once


Before, billing an LLM call looked something like this.


```text
resp   =   client  .  converse  (  modelId  =  "..."  ,   messages  =  [  .  .  .  ]  )


usage   =   resp  [  "usage"  ]
billing  .  send_event  (  customer_id  ,     "llm_input_tokens"  ,    usage  [  "inputTokens"  ]  )
billing  .  send_event  (  customer_id  ,     "llm_output_tokens"  ,   usage  [  "outputTokens"  ]  )
billing  .  send_event  (  customer_id  ,     "llm_cache_read"  ,      usage  .  get  (  "cacheReadInputTokens"  ,     0  )  )
# ... repeat for cache writes, tool calls, reasoning tokens, streaming chunks
# ... then write it all again, differently, for the next provider you add


```


After, you wrap the client once.


```text
# OpenAI
client   =   sdk  .  wrap  (  OpenAI  (  )  )
client  .  chat  .  completions  .  create  (  model  =  "gpt-4o"  ,   messages  =  [  .  .  .  ]  )


# Anthropic
client   =   sdk  .  wrap  (  Anthropic  (  )  )
client  .  messages  .  create  (  model  =  "claude-sonnet-4-5"  ,   messages  =  [  .  .  .  ]  )


# Bedrock
client   =   sdk  .  wrap  (  boto3  .  client  (  "bedrock-runtime"  )  )
client  .  converse  (  modelId  =  "..."  ,   messages  =  [  .  .  .  ]  )


# token attribution happens automatically, per customer, across every provider


```


What lands in billing tells the story.


Old world. Anthropic returns one shape:


```text
{
"model"  :     "claude-sonnet-4-5"  ,
"usage"  :     {
"input_tokens"  :     1200  ,
"output_tokens"  :     340  ,
"cache_creation_input_tokens"  :     800  ,
"cache_read_input_tokens"  :     4000
}
}


```


OpenAI returns another:


```text
{
"model"  :     "gpt-4o"  ,
"usage"  :     {
"prompt_tokens"  :     1200  ,
"completion_tokens"  :     340  ,
"prompt_tokens_details"  :     {     "cached_tokens"  :     4000     }
}
}


```


Different field names, different nesting, different cache semantics. You write one extractor per provider, map the fields, send one event per dimension. Then a model adds a new field and you do it again. New world. The SDK normalizes both into the same canonical shape and batches them to Lago:


```text
{
"external_subscription_id"  :     "sub_acme"  ,
"events"  :     [
{     "code"  :     "llm_input_tokens"  ,             "properties"  :     {     "value"  :     1200     }     }  ,
{     "code"  :     "llm_output_tokens"  ,            "properties"  :     {     "value"  :     340      }     }  ,
{     "code"  :     "llm_cached_input_tokens"  ,      "properties"  :     {     "value"  :     4000     }     }  ,
{     "code"  :     "llm_cache_creation_tokens"  ,  "properties"  :     {     "value"  :     800      }     }
]
}


```


Same event shape regardless of provider. Customer attribution is automatic. Cache fields populate when the provider returns them, stay absent when it doesn't.


The wrapped client behaves identically to the original. Same arguments, same return shape, same exceptions. The SDK extracts usage from every response, normalizes it across providers, attributes it to a customer subscription, and streams events to Lago in batches. Overhead in the low milliseconds. If anything in the SDK fails, the LLM call still returns.


No migration. The application calls the model the same way it did yesterday.


## Two layers, two jobs


Most teams have infrastructure around their LLM calls. Edge proxies for caching repeated prompts. AI gateways for fallback routing and rate limits. Observability layers for latency and error tracking. Edge inference hosts for region-locality. These layers protect margin and user experience.


The SDK composes with them. It runs in your application process, alongside whatever you already use. If your stack runs through Cloudflare AI Gateway, the Gateway keeps doing its job and the SDK reads the response that comes back through it. Same for Bedrock with API Gateway in front, an edge setup on Workers AI, or a self-hosted LiteLLM proxy.


Two layers, two jobs. Your existing stack knows about your traffic: what got cached, what got retried, what was slow. The SDK knows about your customers: which subscription this call belongs to, what feature it was billed against, what margin tier the customer is on. Caching savings show up in your cost line. Token counts show up on the customer's invoice. Both layers see the same response, so the math agrees.


## The pricing-table promise


The SDK gets tokens out of the response and into billing. It does not yet tell you what those tokens cost.


If you're billing cost-plus today, you maintain your own pricing table. Per-model input rate. Per-model output rate. Cache read and cache write with separate TTL tiers. Long-context surcharges. Reasoning tokens. The table moves every time a provider posts a blog. You're updating a YAML file in your repo and hoping nobody forgot the last change.


The next thing we're shipping is the table itself. Lago maintains current per-model pricing for every major provider. You set a markup. We compute cost from the token counts the SDK already captures, apply your margin, and charge the customer. You stop tracking provider price changes. You stop reconciling cost-plus math at month-end.


For AI-native teams, that's pass-through cost with a clean markup, kept honest by infrastructure that updates when the providers update. For B2B SaaS adding AI features, the same table answers the margin question the CFO keeps asking, without anyone maintaining a spreadsheet.


## The gap


The gap between "the LLM returned tokens" and "the customer got billed for tokens." Every customer-facing team building AI owns it. Most have a half-finished plan to extend it for the next provider. We wrote[the full breakdown of AI billing infrastructure](https://getlago.com/blog/ai-billing-infrastructure) if you're building this yourself.


It's the most code per dollar of value of anything in your stack. Someone has to own it. It should not be every team in the industry, in parallel, separately, forever.


## Try it


The libraries are on GitHub today.


- Python:[getlago/lago-agent-sdk-python](https://github.com/getlago/lago-agent-sdk-python)
- TypeScript:[getlago/lago-agent-sdk-js](https://github.com/getlago/lago-agent-sdk-js)
- Docs:[docs.getlago.com/guide/ai-agents/agent-sdk](https://docs.getlago.com/guide/ai-agents/agent-sdk)
