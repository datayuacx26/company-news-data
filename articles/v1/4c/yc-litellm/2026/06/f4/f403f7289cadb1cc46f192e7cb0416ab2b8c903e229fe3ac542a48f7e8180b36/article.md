---
schema_version: "1.0.0"
document_id: "f403f7289cadb1cc46f192e7cb0416ab2b8c903e229fe3ac542a48f7e8180b36"
company_key: "yc-litellm"
company: "LiteLLM"
source_id: "yc-litellm-news-import-8bfeaefbc2ad"
canonical_url: "https://docs.litellm.ai/blog/lite-harness-sdk"
published_at: "2026-06-02T09:00:00+00:00"
first_seen_at: "2026-07-22T02:33:29.495129+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:eaaa8415a61540903a06639c3150a43074c157704768f1d98292a0209c18eded"
---

# LiteLLM Labs: Announcing Lite-Harness SDK — Unified API for Claude Code, Codex, and Pi AI

Harnesses are the next frontier of vendor lock-in. LiteLLM was built to swap across model providers easily. However, as the models get saturated, the next area for competition becomes the harnesses and managed agents. To make it easy to go across vendors at the harness layer, we're launching the Lite-Harness SDK. This is a simple TypeScript+Python SDK which allows developers to change harnesses, like they change models.


It exposes harnesses in a unified Claude Agents SDK spec. This means that if you wrote your app with the Claude Agents SDK, and want to try another harness (Pi AI, Hermes, Codex, OpenCode), you can do so without rewriting your code.


Today, it supports 3 harnesses - Claude Code, Codex, and Pi AI. Please file an issue[here](https://github.com/LiteLLM-Labs/lite-harness/issues) , if you want us to add another harness.


Here's how it works:


**TypeScript Example**


```text
import     {   query   }     from     "@lite-harness/sdk"  ;         const   prompt   =     "Fix the failing test"  ;         // Claude Code harness      for     await     (  const   message   of     query  (  {       prompt  ,       options  :     {   harness  :     "claude-code"  ,   model  :     "claude-opus-4-8"     }  ,      }  )  )     {         console  .  log  (  message  )  ;      }         // Codex harness      for     await     (  const   message   of     query  (  {       prompt  ,       options  :     {   harness  :     "codex"  ,   model  :     "gpt-5.5"     }  ,      }  )  )     {         console  .  log  (  message  )  ;      }
```


**Python Example**


```text
from   lite_harness   import   query  ,   AgentOptions       prompt   =     "Fix the failing test"         # Claude Code harness      async     for   message   in   query  (         prompt  =  prompt  ,         options  =  AgentOptions  (  harness  =  "claude-code"  ,   model  =  "claude-opus-4-8"  )  ,      )  :           print  (  message  )         # Codex harness      async     for   message   in   query  (         prompt  =  prompt  ,         options  =  AgentOptions  (  harness  =  "codex"  ,   model  =  "gpt-5.5"  )  ,      )  :           print  (  message  )
```


## LiteLLM AI Gateway​


Lite-Harness supports proxy'ing harnesses via LiteLLM AI Gateway. This enables easy model swapping, cost controls and logging.


Point Lite-Harness at your gateway by setting two environment variables:


```text
export LITELLM_API_BASE=https://litellm.your-company.com/v1    export LITELLM_API_KEY=sk-litellm-...
```


Then call as usual — every underlying model request routes through the gateway:


```text
```


---


### Frequently Asked Questions​


### Do I have to use the LiteLLM AI Gateway?​


No.` lite-harness` works standalone — point it at provider APIs with native keys. AI Gateway integration is opt-in for teams that want central key management, budgets, fallbacks, and a single audit log across every model call.


### Does swapping harnesses change agent behavior?​


Yes — that's the point. Each harness keeps its native loop, tool-calling semantics, and prompt format.` lite-harness` unifies how you *invoke* them, not how they run internally. Run the same prompt across all three to see which combo lands the task best.


### Is this ready for production?​


` lite-harness` is an early, experimental project. This is in public beta. Please join our[discord](https://discord.gg/Nkxw3rm3EE) , to help design it to your preference.


### Is this available in LiteLLM OSS?​


Yes.` lite-harness` is MIT-licensed at[github.com/LiteLLM-Labs/lite-harness](https://github.com/LiteLLM-Labs/lite-harness) .[LiteLLM Enterprise](https://litellm.ai/enterprise) adds SSO/SCIM, air-gapped deployment, 24/7 SLA, and advanced guardrails on top of the AI Gateway it pairs with.


## Recommended Reading​


- [LiteLLM AI Gateway — full feature overview](https://docs.litellm.ai/docs/simple_proxy)
- [LiteLLM Managed Agents Platform — Alpha](https://docs.litellm.ai/blog/agent-platform-alpha)
- [Load balancing and routing across 100+ LLM providers](https://docs.litellm.ai/docs/routing)
