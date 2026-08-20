---
schema_version: "1.0.0"
document_id: "2be15e11df3742feafefb8ef6aeeb227f1cbc20ab90e644dc04dfe178a86568a"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/sakana-ai-launches-fugu"
published_at: "2026-06-30T03:30:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:336c530f12e4642c9fc4cb488b7af8a7c610ccddfb14bb5cc7735495de3a7cd1"
---

# Sakana AI Launches Fugu, a Multi-Agent System That Orchestrates Frontier AI Models

What if instead of picking one AI model and hoping it handles everything, you could have a system that automatically picks the right model for every part of your task, then stitches the results together into one answer?


That's the pitch behind[Sakana Fugu](https://sakana.ai/fugu/) , a new product from Tokyo-based Sakana AI that launched on June 22. Fugu is not a single large language model. It's a multi-agent orchestration system that coordinates a pool of frontier AI models behind a single API. You send one request, and Fugu figures out internally which specialists to call, in what order, and how to combine their work. The Sakana AI team described Fugu as "an LLM trained to call various LLMs in an agent pool, including instances of itself recursively."


The timing is hard to ignore. Fugu launched just ten days after the[US shut down international access to Claude Fable 5 and Mythos Preview](https://emergent.sh/news/claude-fable-5-banned) under a new export control directive, restricting Anthropic's most powerful AI models to approved regions. For teams around the world who had built workflows on those models, access disappeared overnight. Sakana's answer: build a coordination layer that can route around any single provider going offline.


## What Sakana Fugu Actually Does


Most AI products give you one model behind one endpoint. Fugu gives you a team of models behind one endpoint, with an orchestrator deciding how to deploy them.


Sakana Fugu is a language model trained to act as an orchestrator: it receives a request, decides whether to handle it directly or delegate to specialist models in its agent pool, manages verification and synthesis, and returns a single response.


Think of it like a general contractor for AI tasks. You describe the job, and the system figures out which specialists to pull in, in what order, and how to check their work. If you send a request like "write test code from this spec," Fugu internally breaks the task down, has a model good at design draft the plan, has another model write the implementation, and has a verifier model check for mistakes. You only see the final output.


The system ships in two tiers. Fugu is designed for everyday, latency-sensitive work, while Fugu Ultra is built for complex, multi-step tasks. Both are accessible through the same OpenAI-compatible API, meaning if you're already using a tool like[GPT-5.5](https://emergent.sh/news/gpt-5-5-launch) or Claude via an OpenAI-style SDK, switching to Fugu is mostly just swapping the endpoint.


## The Research Behind It


This isn't a slick wrapper slapped on a few API calls. Sakana Fugu is grounded in two peer-reviewed papers presented at ICLR 2026, one of the top machine learning conferences in the world.


The first paper, TRINITY, describes an evolved LLM coordinator that manages multiple models over multiple turns, assigning each one a "Thinker," "Worker," or "Verifier" role. The second, Conductor, uses reinforcement learning to discover natural-language coordination strategies. Both were peer-reviewed and accepted at the conference, which means other researchers validated the approach before Sakana turned it into a product.


TRINITY is a roughly 0.6-billion-parameter coordinator, evolved with CMA-ES, that assigns roles across a pool of much larger worker models. Conductor is a 7-billion-parameter model trained with reinforcement learning to discover coordination strategies. Both are small relative to the frontier models they orchestrate, which is part of the design: the heavy lifting gets delegated to the specialists.


## The Benchmark Story (And Its Caveats)


On paper, the numbers are strong. On SWE-Bench Pro, a coding benchmark, Fugu Ultra scores 73.7%, ahead of Claude Opus 4.8 at 69.2%, GPT-5.5 at 58.6%, and Gemini 3.1 Pro at 54.2%. Across[Sakana's published table](https://github.com/SakanaAI/fugu) , the orchestrator posts the top score on 10 of 11 benchmark rows, spanning coding, reasoning, science, and long-context tasks.


Sakana also claims Fugu Ultra "stands shoulder-to-shoulder" with Anthropic's[Fable 5](https://emergent.sh/learn/what-is-claude-fable-5) and[Mythos](https://emergent.sh/news/weeks-after-claude-mythos) Preview. But there's a catch worth knowing: all benchmark numbers are Sakana-reported and have not yet been independently reproduced by third-party labs. An[independent analysis notes](https://techsy.io/en/blog/sakana-fugu) that Fable 5 scores about 80.0 on SWE-Bench Pro against Fugu Ultra's 73.7, so the "matches Fable 5" claim needs an asterisk.


Fable 5 and Mythos Preview are not publicly accessible due to export controls, which means they cannot be included in Fugu's agent pool. Sakana is essentially saying a coordinated team of publicly available models can rival the performance of restricted frontier models. That's a more interesting (and more honest) framing than simply claiming equivalence.


## What Early Testers Are Saying


The launch drew immediate testing from some of the most closely watched voices in the AI community, and the reactions have been mixed.


**Ethan Mollick** , a Wharton professor whose shader and creative-coding experiments have become an informal benchmark for AI coding ability, posted his verdict within 24 hours. He wrote on X: "I have been trying Sakana Fugu Ultra-high and, first, it is incredibly slow: my typical coding tests (shaders, interactive scenes) take 30 minutes to run. And the results are... fine. It does not match Fable in real use."


ML engineer **Hamel Husain** described the system as solid for code reviews but weaker on frontend work, calling it "a bit jagged in its abilities."


[Nicholas Thompson](https://www.linkedin.com/posts/nicholasxthompson_the-most-interesting-thing-in-tech-sakana-ugcPost-7474958415585230849-t1Im/?utm_source=share&utm_medium=member_desktop&rcm=ACoAABx6oiIBApSoN-YGcRMkdPxyUUkKMU634SA) , CEO of The Atlantic and former Editor in Chief of Wired, called Sakana Fugu "the most interesting thing in tech," describing it as a clever new way of using AI to route queries between different models, and noting it's clearly a response to what happened to Anthropic the week prior.


The emerging consensus: Fugu Ultra is a specialist tool, not a daily workhorse. It shines on complex, multi-step problems where depth matters more than speed.


## What This Means for Builders


Sakana Fugu represents something genuinely new in the AI landscape: the idea that *coordinating* existing models can rival training bigger ones. For non-technical builders and small teams, two things matter here.


First, the "outputs vs. products" gap is widening. Even with strong orchestration, raw AI output still needs refinement, context, and product thinking before it becomes something you can ship. Tools that help you turn model outputs into real, deployable products are becoming more, not less, important as the model layer gets more complex.


Second, the vendor lock-in problem is real. The Fable 5 export ban showed that access to AI models can vanish overnight. If you're building products on top of AI, it's worth thinking about how dependent you are on any single provider, and whether platforms that abstract away that dependency can protect your workflow.


Fugu may not be ready to replace your daily AI tool today. But the approach it represents, collective intelligence over brute-force scale, is a direction the entire industry is watching closely.


Stay tuned to[Emergent News](https://emergent.sh/news) for more updates like this from the world of AI and vibe coding.
