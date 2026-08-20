---
schema_version: "1.0.0"
document_id: "af579399cb05d32160db90fd5d7ba829d63f144be3b1bd15c1aa664d001e53d3"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/claude-opus-5-available-in-cosmic"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T20:33:07.280239+00:00"
content_hash: "sha256:1b4295be13bdceb4e226200f5760053a20ccf4722244c0983a4b0dd43bbfdb78"
---

# Claude Opus 5 Is Now Available in Cosmic

Anthropic launched Claude Opus 5 today, and it is available in Cosmic right now. Select it from the model dropdown in your agent or AI settings, or pass in the AI text generation API.


## What is Claude Opus 5?


Claude Opus 5 is Anthropic's most capable model to date. It sets new state-of-the-art benchmarks on coding, knowledge work, and long-running agent tasks, while matching the intelligence of Claude Fable 5 at roughly half the cost.


Key capabilities verified from the Anthropic announcement:


- *State-of-the-art on coding and knowledge work* (Frontier-Bench, GDPval-AA)
- *Built for long-running agents:* #1 on Zapier's AutomationBench end-to-end, ARC-AGI 3 score 3x the next-best model
- *Judgment and self-verification:* verifies its own work, catches errors during planning, pushes back on bad designs
- *Most aligned model to date:* 2.3 on misaligned behavior, lowest of recent Anthropic releases
- *Same price as Opus 4.8:* $5/M input tokens, $25/M output tokens, so you get more capability at the same cost
- *Fast mode available:* ~2.5x speed increase for latency-sensitive workflows


## Use Claude Opus 5 in the Dashboard


Open any Agent in your Cosmic dashboard, go to the model settings, and select *Claude Opus 5* from the dropdown. No additional configuration needed. The model is available on all plans.


## Use Claude Opus 5 via the API and SDK


Pass as the model parameter in the Cosmic AI text generation API:


```text


```


### Streaming responses


For long-form content generation or agent workflows, use streaming to get results as they arrive:


```text


```


Full API reference:[AI Text Generation](https://www.cosmicjs.com/docs/api/ai#generate-text) ·[Available Models](https://www.cosmicjs.com/docs/api/ai#available-models)


## Why Opus 5 Matters for Agent Teams


If you have been following the[agent-team pattern](https://www.cosmicjs.com/blog/how-to-build-an-ai-agent-team) , Opus 5 is the right model for your orchestrator role. Its self-verification and judgment capabilities map directly to the human-in-the-loop coordination layer: an Opus 5 orchestrator can verify agent outputs, catch errors before they reach review, and push back on off-brief content without a human having to catch every mistake.


For content agents, code agents, and multi-step workflows, Opus 5's strength on long-running tasks means fewer dropped threads and more reliable end-to-end completions.


## Cosmic Is Model-Agnostic


Cosmic supports Claude, Gemini, GPT, Kimi, and other leading models through the same AI text generation API. You choose the model that fits the task. Swap for or any other supported model string without changing your integration.


See the full list of[available models](https://www.cosmicjs.com/docs/api/ai#available-models) .


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)
