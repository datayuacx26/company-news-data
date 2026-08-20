---
schema_version: "1.0.0"
document_id: "e6b42d151a0e445d08f10c010a700d0d1c3b3be1ef75d4f284eaebb9105aa8ff"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/muse-spark-1-1-launch"
published_at: "2026-07-11T01:00:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:d8fdb5d33db38a1abd5d064193faa9ccc541778c9cf4fdb5605cdd05bc3993dd"
---

# Meta Launches Muse Spark 1.1 and Its First Paid AI Model API

Three months ago, Meta shipped[Muse Spark](https://about.fb.com/news/2026/04/introducing-muse-spark-meta-superintelligence-labs/) , its first AI model from the newly formed Meta Superintelligence Labs. It was fast, multimodal, and notably closed-source, a sharp break from Meta's open-weight Llama era. Now Meta is back with[Muse Spark 1.1](https://ai.meta.com/blog/introducing-muse-spark-meta-model-api/) , a major upgrade tuned for agentic tasks, coding, and tool use.


But the model itself is only half the story. Alongside it, Meta launched the[Meta Model API in public preview](https://developer.meta.com/ai/products/meta-model-api/) , the first time Meta has ever charged developers for direct access to one of its own AI models. That makes this a two-part announcement: a more capable model, and a new business model. Both matter.


## What Muse Spark 1.1 Actually Does


Muse Spark 1.1 is a multimodal reasoning model built for agentic tasks. In plain terms, that means it can process images and video, reason through multi-step problems, and use external tools and software to complete tasks on its own, not just answer questions in a chat window.


According to[Meta's announcement](https://ai.meta.com/blog/introducing-muse-spark-meta-model-api/) , Muse Spark 1.1 delivers substantial gains over the original Muse Spark across four areas: tool use, computer use, coding, and multimodal understanding.


A few headline capabilities stand out. The model can manage a context window of 1 million tokens, meaning it can hold long documents, extended conversations, and multi-step workflows in memory without losing track. It supports multi-agent orchestration, meaning it can act as both a lead agent (planning and delegating tasks across parallel subagents) and a subagent (executing a specific job and reporting back to the coordinator).


On computer use, Meta says the model can navigate desktop applications, adapt to unfamiliar interfaces, and decide when to write a script versus when to click through a UI directly. For multimodal tasks, it can take smartphone video, extract useful images, and reason about the content to complete a task, like creating a Facebook Marketplace listing from a product walkthrough video.


## Benchmarks: Strong on Agents, Competitive on Coding


Meta published a detailed benchmark table comparing Muse Spark 1.1 against Claude Opus 4.8,[GPT-5.5](https://emergent.sh/news/gpt-5-5-launch) , and Gemini 3.1 Pro. The pattern is clear: Muse Spark 1.1 leads on agent-focused evaluations, but trails the top models on pure coding tasks.


[Source](https://ai.meta.com/blog/introducing-muse-spark-meta-model-api/)


On the agentic side, the numbers are strong. According to[Meta's own benchmark data](https://ai.meta.com/blog/introducing-muse-spark-meta-model-api/) , Muse Spark 1.1 scores 88.1 on MCP Atlas (a test of scaled tool use), ahead of both Opus 4.8 and GPT-5.5. On JobBench, which measures professional tool use, it scores 54.7 versus Opus 4.8's 48.4 and GPT-5.5's 38.3. It also leads on Humanity's Last Exam with tools (62.1) and Finance Agent v2 (57.2).


Coding is where the picture gets more nuanced. On Terminal-Bench 2.1, Muse Spark 1.1 scores 80.0, behind GPT-5.5 (83.4) and Opus 4.8 (82.7). On SWE-Bench Pro, Opus 4.8 leads at 69.2 versus Muse Spark 1.1's 61.5. On DeepSWE 1.1, a long-horizon coding benchmark, GPT-5.5 leads at 67.0, with Muse Spark 1.1 at 53.3. That last number still represents a massive jump from the original Muse Spark's score of 10.0 on the same test,[according to OfficeChai's analysis](https://officechai.com/ai/muse-spark-1-1-benchmarks/) .


On Meta's internal coding benchmark, Muse Spark 1.1 scores 68.3, just behind Opus 4.8's 69.0 and just ahead of GPT-5.5's 67.1.


**Worth noting** : these benchmarks are self-reported by Meta. Independent evaluations from third parties will paint a fuller picture as the model sees wider use.


## The Meta Model API: Pricing and Access


This is Meta's first paid AI model offering. The Meta Model API is now in[public preview](https://developer.meta.com/ai/products/meta-model-api/) for U.S.-based developers, with pay-as-you-go pricing at $1.25 per million input tokens and $4.25 per million output tokens. New accounts get $20 in free credits. The API uses an OpenAI-compatible format, meaning developers with existing OpenAI integrations can switch with minimal code changes.


For context,[according to CNBC](https://www.cnbc.com/2026/07/09/meta-jumps-into-ai-coding-market-to-chase-anthropic-and-openai.html) , Alexandr Wang, head of Meta Superintelligence Labs, described the pricing as "very aggressive and attractive" compared to similar offerings. Meta CEO Mark Zuckerberg framed the pricing at roughly 25% of what Anthropic and OpenAI charge for comparable models. The pricing sits below premium options like Anthropic's Claude Sonnet 4.6 while above entry-level models like Claude Haiku 4.5 and GPT-5 mini.


One technical detail worth tracking: Muse Spark 1.1 is a reasoning model, so its internal "thinking" tokens are billed at the output rate. You can control reasoning depth with a reasoning_effort parameter, which means matching effort to task complexity is the main lever on cost.


Early API partners include Replit, Cline, and Box. The model is also available now in "Thinking" mode in the Meta AI app and on meta.ai for consumer use.


## Why This Is a Strategic Shift for Meta


This is more than a model launch. For years, Meta built its AI reputation on open-source Llama models distributed for free. Muse Spark 1.1 and the paid Meta Model API represent a deliberate pivot toward commercial AI products.


[According to Quartz](https://qz.com/meta-muse-spark-api-developers-paid-anthropic-openai-070926) , the move comes as Zuckerberg faces pressure from Wall Street to show returns on Meta's massive AI infrastructure investment ($125 to $145 billion in planned spending).


An open-source version of the model is reportedly in development, though no timeline has been given. Meta is also training a more powerful next-generation model, internally code-named Watermelon. Muse Spark 1.1 itself was developed under the code name Avocado, according to[CNBC's reporting](https://www.cnbc.com/2026/07/09/meta-jumps-into-ai-coding-market-to-chase-anthropic-and-openai.html) .


The strategic bet is clear: compete on price and OpenAI-compatibility to win developer adoption, then leverage Meta's massive consumer distribution across WhatsApp, Instagram, Facebook, and Ray-Ban smart glasses to scale usage.


## What This Means for Builders


For non-technical founders and solo builders, the key takeaway is straightforward: the AI models powering the tools you use are getting better and cheaper at the same time. Muse Spark 1.1's aggressive pricing puts pressure on every other AI provider to lower costs, which means the platforms and tools built on top of these models will keep getting more accessible.


The agentic capabilities are the most relevant part of this launch for anyone building products. AI models that can plan tasks, use tools, and manage long workflows are the foundation of the no-code and low-code tools that turn ideas into working software. Every improvement in agent reliability trickles down to better experiences for the people using platforms like[Emergent](https://emergent.sh/) to build real products without writing code.


Stay tuned to[Emergent News](https://emergent.sh/news) for more coverage of the AI tools and models shaping how products get built.
