---
schema_version: "1.0.0"
document_id: "64119ea3d018e5f04ede8258e0073f7a01773755f0889cdd90a88248de94f580"
company_key: "yc-cactus"
company: "Cactus"
source_id: "yc-cactus-news-import-6d2738280b9a"
canonical_url: "https://cactuscompute.com/blog/needle"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-21T11:58:31.633623+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:1a18f43f63b21bdce1c07360e7e7197d010a4de1d00e9766a9c40f6761427f24"
---

# Needle: We Distilled Gemini Tool Calling into a 26M Model

We open-sourced Needle, a 26M parameter function-calling (tool use) model. It runs at 6000 tok/s prefill and 1200 tok/s decode on consumer devices.


We were always frustrated by the little effort made towards building agentic models that run on budget phones, so we conducted investigations that led to an observation: agentic experiences are built upon tool calling, and massive models are overkill for it. Tool calling is fundamentally retrieval-and-assembly (match query to tool name, extract argument values, emit JSON), not reasoning. Cross-attention is the right primitive for this, and FFN parameters are wasted at this scale.


## Simple Attention Networks


The entire model is just attention and gating, no MLPs anywhere. Needle is an experimental run for single-shot function calling for consumer devices (phones, watches, glasses...).


We found that the "no FFN" finding generalizes beyond function calling to any task where the model has access to external structured knowledge (RAG, tool use, retrieval-augmented generation). The model doesn't need to memorize facts in FFN weights if the facts are provided in the input.


Full architecture writeup:[docs/simple_attention_networks.md](https://github.com/cactus-compute/needle/blob/main/docs/simple_attention_networks.md) .


## Training


- Pretrained on 200B tokens across 16 TPU v6e (27 hours)
- Post-trained on 2B tokens of synthesized function-calling data (45 minutes)
- Dataset synthesized via Gemini with 15 tool categories (timers, messaging, navigation, smart home, etc.)


## Performance


While it beats FunctionGemma-270M, Qwen-0.6B, Granite-350M, LFM2.5-350M on single-shot function calling, those models have more scope/capacity and excel in conversational settings. Small models can be finicky — we encourage you to test on your own tools via the playground and finetune accordingly.


## Quickstart


```text
git   clone   https://github.com/cactus-compute/needle.git
cd   needle   &&   source   ./setup
needle   playground
```


Opens a web UI at` http://127.0.0.1:7860` where you can test and finetune on your own tools. Weights are auto-downloaded.


## Usage (Python)


```text
from   needle   import   SimpleAttentionNetwork, load_checkpoint, generate, get_tokenizer


params, config   =   load_checkpoint(  "checkpoints/needle.pkl"  )
model   =   SimpleAttentionNetwork(config)
tokenizer   =   get_tokenizer()


result   =   generate(
model, params, tokenizer,
query  =  "What's the weather in San Francisco?"  ,
tools  =  '[{"name":"get_weather","description":"Get current weather for a city.","parameters":{"location":{"type":"string","description":"City name.","required":true  }}  }]'  ,
stream  =  False  ,
)
print  (result)
# [{"name":"get_weather","arguments":{"location":"San Francisco"}}]
```


## Get the model


Needle is part of our broader work on[Cactus](https://github.com/cactus-compute/cactus) , an inference engine built from scratch for mobile, wearables and custom hardware. Everything is MIT licensed.


- Weights:[huggingface.co/Cactus-Compute/needle](https://huggingface.co/Cactus-Compute/needle)
- GitHub:[github.com/cactus-compute/needle](https://github.com/cactus-compute/needle)
