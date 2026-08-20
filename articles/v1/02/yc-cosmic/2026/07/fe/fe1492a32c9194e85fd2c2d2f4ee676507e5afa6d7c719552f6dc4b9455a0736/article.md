---
schema_version: "1.0.0"
document_id: "fe1492a32c9194e85fd2c2d2f4ee676507e5afa6d7c719552f6dc4b9455a0736"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/deepseek-v4-flash-benchmarks-pricing"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T23:21:18.245884+00:00"
fetched_at: "2026-07-31T23:21:20.497958+00:00"
content_hash: "sha256:36d7dea4de4dd5e5388a2c8775afe8a8ca2b001c4daba18e8f6404b352ffd025"
---

# DeepSeek V4 Flash: Benchmarks, Pricing, and What It Means for Content Agents

DeepSeek released **V4-Flash** today, July 31, 2026, and pushed it to public beta on the API under the model name . It landed on the front page of Hacker News twice within hours.


The headline is easy to summarize: near the top of the intelligence rankings, at the bottom of the price rankings. The details are more interesting than the headline, and one of them changes the cost math in a way the per-token price does not show.


Here is what shipped, what the numbers say, and where a model like this fits in a content pipeline.


## What actually shipped


According to DeepSeek's official changelog, V4-Flash uses the **same architecture and the same parameter size** as the earlier V4-Flash-Preview. The only change is post-training. DeepSeek re-post-trained the model and reports scores that represent a significant jump over the V4-Pro-Preview.


A few practical notes from the release:


- The model is in **public beta** on the API.
- It **natively supports the Responses API format** and has been adapted for Codex.
- The V4-Pro API, app, and web experiences are **unchanged** . An official V4-Pro release is still to come.
- Independent measurement puts it at **284B parameters** , with weights not publicly released.
- It is **text in, text out** . No image, audio, or video input.


That last point matters if you were hoping to drop it into a workflow that processes screenshots or PDFs. You cannot, at least not directly.


## The benchmark numbers


DeepSeek published the following scores, evaluated with its own DeepSeek Harness in minimal mode at max effort, 0.95, 1.0:


Benchmark Score


Terminal Bench 2.1 82.7


Cybergym 76.7


Toolathlon (verified) 70.3


DSBench-FullStack 68.7


DSBench-Hard 59.6


DeepSWE 54.4


NL2Repo 54.2


Agent Last Exam 25.2


Automation Bench (Public) 25.1


Read the shape of that table rather than any single row. The strongest results cluster around **terminal use, tool calling, and multi-step engineering work** : Terminal Bench at 82.7 and Toolathlon at 70.3 are agentic evaluations, not trivia recall. This is a model tuned to operate software.


The bottom two rows deserve an honest caveat. Agent Last Exam and Automation Bench are deliberately brutal, long-horizon evaluations where scores in the twenties are normal across the frontier right now. A 25.2 is not a failure. It is a reminder that fully autonomous, hours-long task completion remains unsolved by everyone.


One more caveat worth stating plainly: these are vendor-published numbers, produced with the vendor's own harness at max effort settings. Treat them as a strong signal and verify on your own evals before you migrate anything that matters.


Independent measurement from Artificial Analysis puts V4-Flash at an **Intelligence Index of 50** , ranking it **2nd out of 162** models in its class, against a class median of 17.


## The price story, and the part people will miss


On raw API pricing, V4-Flash ranks **1st out of 162** in its class:


- **$0.14** per 1M input tokens
- **$0.28** per 1M output tokens
- **$0.003** per 1M cached input tokens, a **98% discount** on a cache hit
- **$0.06** per 1M tokens blended
- **1M token context window**


That cache-hit price is the number to pay attention to, and it is the one that gets skimmed past. For any agent that runs repeatedly against the same system prompt, the same schema definitions, and the same style guide, the fixed prefix of every request is identical. Caching turns that prefix from a recurring cost into a rounding error.


Now the catch.


Artificial Analysis also measured **verbosity** , and V4-Flash consumed **210M output tokens** to complete the Intelligence Index, against a median of **62M** . That ranks it 58th of 162 on output efficiency. It produced roughly three and a half times the median number of tokens to reach its score.


Cheap tokens and cheap tasks are two different measurements. A model at a third of the per-token price that emits three times the tokens has spent your savings. The total cost to evaluate V4-Flash on the Intelligence Index came to **$72.02** , which is genuinely low, so the economics still work out well here. The lesson is that you should benchmark **cost per completed task** on your own workload rather than reading the price-per-million column and stopping there.


If your workload is latency-sensitive, the verbosity number is also a throughput warning. More tokens means more time to last token.


## Where this fits in a content pipeline


Strong tool-calling scores, a 1M token context window, and a 98% cache discount describe a specific kind of useful: **long-running agents that read and write structured content** .


That is a real workload. Translating a content library into six locales. Auditing every post for a deprecated product claim. Regenerating meta descriptions across a thousand objects. Keeping a changelog in sync with release notes. These jobs are repetitive, schema-bound, and expensive at frontier prices, which is exactly the profile where a cheap, cache-friendly, tool-capable model earns its place.


The 1M context window matters here for a specific reason. Content operations tend to need **broad but shallow** context: a large number of records, each fairly small. You can put an entire content model and hundreds of objects in a single request and let the model reason across all of it, instead of building a retrieval layer to work around a smaller window.


With Cosmic, the read side of that loop is a few lines. Pull the objects and the fields you need:


```text


```


Because limits the payload to the fields you actually asked for, you control your own token bill at the source. Sending 200 slugs and meta descriptions costs a fraction of sending 200 full post bodies.


Then hand the batch to the model. The API is OpenAI-compatible, so the client you already have works:


```text


```


Keep byte-identical across runs so it stays cacheable. Put the variable content in the user message. That one discipline is where the 98% cache discount actually shows up on your invoice.


Writing results back through the SDK closes the loop, and because every change lands as a versioned object rather than a direct database write, you keep an audit trail of what the agent changed.


## Should you switch?


A reasonable read as of today:


**Good fit for V4-Flash**


- High-volume, repetitive content operations where per-task cost dominates
- Agentic and terminal-oriented work, which is where the benchmarks are strongest
- Workloads with a large stable prompt prefix that will hit the cache
- Jobs that need broad context across many records


**Look elsewhere**


- Anything requiring image, audio, or video input, since the model is text-only
- Latency-critical, user-facing interactions, given the verbosity numbers
- Production systems with no tolerance for a public beta API
- Deployments that require open weights, which are not available here


The honest summary: this is a strong, cheap, tool-capable text model that is well suited to background content work and not yet the right choice for multimodal or latency-sensitive paths. Run it against your own evals before you commit.


## Build the pipeline the model plugs into


Model leaderboards will keep churning. The thing that determines whether you can take advantage of a release like this one is whether your content layer is API-first, so swapping the model behind it is a config change rather than a migration.


That is what Cosmic is for. Your content lives in a headless CMS with a REST API and a TypeScript SDK, so any model, agent, or framework can read and write it. When a cheaper or smarter model ships next month, you point at it and move on.


- **[Start building free](https://www.cosmicjs.com/signup?utm_source=blog&utm_medium=content&utm_campaign=deepseek-v4-flash-benchmarks-pricing)** and connect a bucket in minutes
- Read the **[AI features overview](https://www.cosmicjs.com/ai?utm_source=blog&utm_medium=content&utm_campaign=deepseek-v4-flash-benchmarks-pricing)** to see what is built in
- New to the model? Start with **[headless CMS explained](https://www.cosmicjs.com/headless-cms?utm_source=blog&utm_medium=content&utm_campaign=deepseek-v4-flash-benchmarks-pricing)**
- Comparing coding agents too? See our **[Claude Code vs GitHub Copilot vs Cursor breakdown](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026?utm_source=blog&utm_medium=content&utm_campaign=deepseek-v4-flash-benchmarks-pricing)**
- Running this at team scale? **[Book 15 minutes with our CEO](https://calendly.com/tonyspiro/cosmic-intro?utm_source=blog&utm_medium=content&utm_campaign=deepseek-v4-flash-benchmarks-pricing)**


*Benchmark scores in this post are from DeepSeek's official changelog dated July 31, 2026. Pricing, Intelligence Index, parameter count, and verbosity figures are from Artificial Analysis. Vendor-published benchmarks were produced with DeepSeek's own harness at max effort settings, so verify against your own workload.*
