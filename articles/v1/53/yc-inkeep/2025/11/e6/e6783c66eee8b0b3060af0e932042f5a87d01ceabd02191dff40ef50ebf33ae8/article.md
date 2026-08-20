---
schema_version: "1.0.0"
document_id: "e6783c66eee8b0b3060af0e932042f5a87d01ceabd02191dff40ef50ebf33ae8"
company_key: "yc-inkeep"
company: "Inkeep"
source_id: "yc-inkeep-rss-006a915c529f"
canonical_url: "https://inkeep.com/blog/composer-1-vs-swe-15-performance-pricing-lock-in"
published_at: "2025-11-04T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:13.852300+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:450a8b4f127e234c35641e6856efc26af65a49b9df6ffd6516a8c02768928f55"
---

# Composer-1 vs SWE-1.5: Performance, Pricing, and Lock-In Explained

The latest frontier in AI-assisted programming is being shaped not by general-purpose models, but by vertically specialized ones. Composer-1 from Cursor and SWE-1.5 from Windsurf represent a new generation of coding models trained directly in agentic environments rather than on static text corpora. These two systems share similarities- and their differences hint at how the next wave of coding intelligence will evolve.


## Reinforcement Learning and Real Engineering Tasks


Both Composer-1 and SWE-1.5 are exclusively available through their respective editors, not via a public API. That limitation is telling: it allows tight integration with real development tools and environments, which are essential for reinforcement learning (RL). Composer was trained through reinforcement learning in a vast number of cloud-based sandboxed coding environments- hundreds of thousands running concurrently. Instead of relying on static datasets or artificial benchmarks, it learned directly from real-world software engineering tasks using real development tools.


This closed-loop RL training marks a shift away from static fine-tuning on GitHub data and toward adaptive, task-driven learning. If you aim to build a truly agentic coding tool, reinforcement learning against your own custom tool stack may be the most effective path forward.


## Vertical Models Over General Purpose Systems


Unlike ChatGPT or other generalist LLMs, these new vertical models focus purely on software engineering. They don’t need to excel at writing essays or summarizing novels- they just need to code well. That specialization simplifies training and allows for focused optimization. The future may lie in proliferation of such domain-limited models rather than large, general ones.


This[post](https://x.com/nickgomez/status/1985424577334550536) from Nick Gomez, Inkeep CEO, captures the broader shift unfolding in the AI software business model.


## Architectures: MoE Efficiency and Open-Weight Foundations


Composer-1 is a Mixture of Experts (MoE) model- an architecture that allows high performance without the costs typically associated with scaling dense transformers. MoE architectures route tasks to specialized “expert” submodels, lowering compute needs while maintaining performance. This approach is detailed in various industry studies and overviews of MoE design theory.


Both Composer-1 and SWE-1.5 are rumored to be trained atop GLM 4.6, an open-weights model originating from China. The fact that[both can communicate fluently in Chinese](https://www.linkedin.com/posts/debarghyadas_both-cursor-and-cognition-windsurf-new-activity-7389857797355536384-9yZy?utm_source=share&utm_medium=member_desktop&rcm=ACoAABQxojABb9DLgQGWwJy2PF7BpWvorJHMaxc) lends some credibility to this claim. Open-weights models differ from fully open-source ones in that their parameters are available for use and fine-tuning, but their training datasets and pipelines are not necessarily public.


This choice also reflects a broader shift: Chinese open-weights models have recently surpassed U.S. equivalents in adoption across many development ecosystems.


## Benchmarking Gaps and Accountability Questions


A concern emerges around Composer’s performance claims. As of now, there are no independent benchmarks or community-verified test results for Composer-1:


- No independent verification
- Potential selection bias toward tasks where the model excels
- No standardized cross-model comparisons


Cursor Bench, an internal benchmark used by the company, remains closed-source and not publicly documented. Without third-party validation, it is difficult to assess whether Composer’s reported gains reflect generalizable performance or highly tailored evaluation settings.


In contrast, SWE-1.5 has been tested on SWE-Bench Pro- a recognized community benchmark designed to measure real-world software engineering competence. This provides greater transparency and comparability, allowing its results to be weighed against other leading coding models and offering stronger evidence of its claimed technical advantages.


## Performance and Ecosystem Lock-In


In raw performance terms, Composer produces roughly 250 tokens per second, while SWE-1.5 operates at up to an impressive 950 tokens per second. Haiku 4.5, by comparison, outputs around 140 tokens per second.


These impressive numbers, however, come with constraints. Without open API access, both models remain tied to their native editors- non-portable to external workflows, difficult to integrate into CI/CD pipelines, and effectively locking developers into the Cursor or Windsurf ecosystems.


Based on my own testing, both models fall short of top-tier frontier models like GPT-5 Codex and Claude Sonnet 4.5. Between the two, SWE-1.5 demonstrates superior performance in both code quality and generation speed. For a visual comparison, Peter Gostev conducted an interesting test in this[post](https://x.com/petergostev/status/1984013740988096528) , asking each model to create a 3D render of the Golden Gate Bridge. As shown in the video below, Windsurf produces output with fewer errors and greater detail.


Your browser does not support the video tag.


## Pricing Parity and Strategic Positioning


Composer-1’s cost is[currently equivalent to GPT-5 Codex](https://cursor.com/docs/models) , raising the question: why not opt for an established model with broader access and verified benchmarks? Windsurf’s SWE-1.5 differentiates mainly through speed, editor integration, and the inclusion of custom RL data pipelines.


Windsurf introduced its first frontier AI model, SWE-1, earlier this year in May. Composer-1 marks Cursor’s first major entry into this competitive segment, and together, they reveal an unmistakable direction for coding AI: faster, narrower, and deeply integrated models trained through real-world feedback loops rather than abstract datasets.
