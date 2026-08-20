---
schema_version: "1.0.0"
document_id: "aeebfd7f558ec3fbf5ec2f1de05187417a4e98e95d4193d2afbe23e587b5036c"
company_key: "yc-helicone"
company: "Helicone"
source_id: "yc-helicone-news-import-836d1e549638"
canonical_url: "https://www.helicone.ai/blog/migration-openrouter"
published_at: "2025-09-12T00:00:00+00:00"
first_seen_at: "2026-07-21T22:44:44.297475+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:a89c70035d8d74d3c8c5798b14932d7fd0d4edc0d89a456a69deae5a549960b4"
---

# How to Migrate from OpenRouter to Helicone AI Gateway

# How to Migrate from OpenRouter to Helicone AI Gateway


September 12, 2025


· 2 minute read


Juliette Chevalier


· September 12, 2025


## Table of Contents


- Why Switch to Helicone AI Gateway
- Migration Guide: OpenRouter → Helicone AI Gateway


## Why Switch to Helicone AI Gateway


Let's be honest: OpenRouter is a great tool. It's simple to use, has a great UI, and routes you to any model using the OpenAI API.


So we thought: **why not build an open-source alternative, with Helicone's observability built in and charge 0% markup fees?**


That's what we did. The Helicone AI Gateway is an OpenRouter alternative - with all the features you love, but with 0% markup fees.


- **0% markup fees** - only pay exactly what providers charge
- **Automatic fallbacks** - when one provider is down, route to another instantly
- **Built-in observability** - logs, traces, and metrics by default without extra setup
- **Cost optimization** - automatically route to the cheapest, most reliable provider for each model, always rate-limit aware
- **Passthrough billing & BYOK support** - let us handle auth for you ([request access here!](https://helicone.ai/credits) ) or bring your own keys


## Migration Guide: OpenRouter → Helicone AI Gateway


### Step 1: Set up your API keys


Set up your provider[API keys](https://helicone.ai/providers) and get your Helicone API key from the[Helicone dashboard](https://helicone.ai/api-keys) .


### Step 2: Edit your code


Replace OpenRouter's endpoint with Helicone AI Gateway:


```text
// ❌ Before: OpenRouter
const   client =  new    OpenAI  ({
baseURL  :  "https://openrouter.ai/api/v1"  ,
apiKey  :  "sk-or-v1-..."    // OpenRouter key
});


// ✅ After: Helicone AI Gateway
const   client =  new    OpenAI  ({
baseURL  :  "https://ai-gateway.helicone.ai"  ,
apiKey  : process. env  . HELICONE_API_KEY    // Helicone key
});


// Query the model
const   response =  await   client. chat  . completions  . create  ({
model  :  "claude-3.5-sonnet"  ,  // or find 100+ other models in https://helicone.ai/models
messages  : [{  role  :  "user"  ,  content  :  "Hello!"   }]
});


```


That's it. No other code changes needed.


### Step 3: Review your requests in the Helicone dashboard


Go to[Helicone](https://helicone.ai/requests) and see your requests logged automatically.


### Migration Gotchas to Watch For


- **Different model names:** Some models use slightly different naming conventions. Helicone has a[model registry](https://helicone.ai/models) to help you find the exact model name.
- **Model Providers:** Helicone routes you to the cheapest, most reliable provider for each model, which means we don't require you to name the provider when making the request like OpenRouter does (i.e. OpenRouter:` anthropic/claude-3.5-sonnet` vs Helicone:` claude-3.5-sonnet` ).
- **Provider keys:** You can use Helicone's auth ([request access here!](https://helicone.ai/credits) ) or bring your own keys. To BYOK, you can set up your provider keys to the[Helicone dashboard](https://helicone.ai/providers) .


### The Bottom Line


As your AI usage grows, cost becomes more and more important.


The Helicone AI Gateway gives you the control you need to optimize your costs while keeping your infrastructure simple and reliable. With observability built in, you never miss a request or an error again!


**Ready to migrate?**


- [Sign up for Helicone (free tier available)](https://helicone.ai/signup) and get your API key
- Update the` baseURL` in your OpenAI request to` https://ai-gateway.helicone.ai`
- Deploy and watch your costs (and errors) drop!


Need help? Join our[Discord](https://discord.gg/7aSCGCGUeu) where engineers share about building with AI.


**Want to contribute?** The Helicone AI Gateway is open-source on[GitHub](https://github.com/helicone/helicone) . Help us build the future of AI infrastructure.
