---
schema_version: "1.0.0"
document_id: "a1a75093e5b00d7ba7e1037b11191ead6ee1e9fec920a101bd6a23e02b646e6e"
company_key: "yc-fly-io"
company: "Fly.io"
source_id: "yc-fly-io-news-import-54d37e81cc45"
canonical_url: "https://fly.io/blog/build-better-agents-with-morphllm/"
published_at: "2025-08-25T00:00:00+00:00"
first_seen_at: "2026-07-21T20:38:55.214316+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:f28f6a30125fd8a6b5dab4f9a1921c0b6d513506bc00054f29ae2694458098dc"
---

# Build Better Agents With MorphLLM

Author Name Daniel Botha Image by


[Annie Ruygt](https://annieruygtillustration.com/)


I’m an audiophile, which is a nice way to describe someone who spends their children’s college fund on equipment that yields no audible improvement in sound quality. As such, I refused to use wireless headphones for the longest time. The fun thing about wired headphones is when you forget they’re on and you stand up, you simultaneously cause irreparable neck injuries and extensive property damage. This eventually prompted me to buy good wireless headphones and, you know what, I break fewer things now. I can also stand up from my desk and not be exposed to the aural horrors of the real world.


This is all to say, sometimes you don’t know how big a problem is until you solve it. This week, I chatted to the fine people building[MorphLLM](https://morphllm.com/) , which is exactly that kind of solution for AI agent builders.


## Slow, Wasteful and Expensive AI Code Changes


If you’re building AI agents that write or edit code, you’re probably accepting the following as “the way it is”: Your agent needs to correct a single line of code, but rewrites an entire file to do it. Search-and-replace right? It’s fragile, breaks formatting, silently fails, or straight up leaves important functions out. The result is slow, inaccurate code changes, excessive token use, and an agent feels incompetent and unreliable.


Full file rewrites are context-blind and prone to hallucinations, especially when editing that 3000+ line file that you’ve been meaning to refactor. And every failure and iteration is wasted compute, wasted money and worst of all, wasted time.


## Why We Aren’t Thinking About This (or why I wasn’t)


AI workflows are still new to everyone. Best practices are still just opinions and most tooling is focused on model quality, not developer velocity or cost. This is a big part of why we feel that slow, wasteful code edits are just the price of admission for AI-powered development.


In reality, these inefficiencies become a real bottleneck for coding agent tools. The hidden tax on every code edit adds up and your users pay with their time, especially as teams scale and projects grow more complex.


## Better, Faster AI Code Edits with Morph Fast Apply


MorphLLM’s core innovation is Morph Fast Apply. It’s an edit merge tool that is semantic, structure-aware and designed specifically for code. Those are big words to describe a tool that will empower your agents to make single line changes without rewriting whole files or relying on brittle search-and-replace. Instead, your agent applies precise, context-aware edits and it does it ridiculously fast.


It works like this:


- You add an ‘edit_file’ tool to your agents tools.
- Your agent outputs tiny` edit_file` snippets, using` //...existing code...` placeholders to indicate unchanged code.
- Your backend calls Morph’s Apply API, which merges the changes semantically. It doesn’t just replace raw text, it makes targeted merges with the code base as context.
- You write back the precisely edited file. No manual patching, no painful conflict resolution, no context lost.


## The Numbers


MorphLLM’s Apply API processes over 4,500 tokens per second and their benchmark results are nuts. We’re talking 98% accuracy in ~6 seconds per file. Compare this to 35s (with error corrections) at 86% accuracy for traditional search-and-replace systems. Files up to 9k tokens in size take ~4 seconds to process.


Just look at the damn[graph](https://morphllm.com/benchmarks) :


These are game-changing numbers for agent builders. Real-time code UIs become possible. Dynamic codebases can self-adapt in seconds, not minutes. Scale to multi-file edits, documentation, and even large asset transformations without sacrificing speed or accuracy.


## How to Get in on the MorphLLM Action


Integration with your project is easy peasy. MorphLLM is API-compatible with OpenAI, Vercel AI SDK, MCP, and OpenRouter. You can run it in the cloud, self-host, or go on-prem with enterprise-grade guarantees.


I want to cloud host mine, if only I could think of somewhere I could quickly and easily deploy wherever I want and only pay for when I’m using the infra 😉.


## Get Morphed


MorphLLM feels like a plug-in upgrade for code agent projects that will instantly make them faster and more accurate. Check out the docs, benchmarks, and integration guides at[docs.morphllm.com](https://docs.morphllm.com/) . Get started for free at[https://morphllm.com/dashboard](https://morphllm.com/dashboard)


Next post ↑[Litestream v0.5.0 is Here](https://fly.io/blog/litestream-v050-is-here/) Previous post ↓[Trust Calibration for AI Software Builders](https://fly.io/blog/trust-calibration-for-ai-software-builders/)
