---
schema_version: "1.0.0"
document_id: "295ffcd5245a451071bc6d052931a685128d9f34880edd01fcdd867b4c8e33ad"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/claude-fable-browser-agent-benchmark"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:1a436e67549a86580f405e0da3886aab3139010c33de9fd943ce94e11531e7ba"
---

# Claude Fable Sets High Score on BU Bench

Anthropic's new Claude Fable 5 model scored **80.0%** on BU Bench V1 with the open-source[Browser Use library](https://github.com/browser-use/browser-use) . Setting a high score, it bested the next-highest model, GPT 5.5, by 12 points.


The result came at an exorbitant price: **$580.87** in API cost for 100 tasks.


BU Bench tests whether browser agents can complete real web tasks. It requires agents to handle multi-step navigation, search, information extraction, form filling, dynamic UI interactions, iframes, PDFs, downloaded files, and synthesis across live websites. Its tasks, code, and model scores are available[here](https://github.com/browser-use/benchmark) .


Below is Claude Fable 5 compared to other open- and closed-source models.


## Result


Fable passed **80 of 100** tasks. Of the 20 failures, 16 were judged incorrect, and four hit the 30-minute task timeout. Tasks took **6m 53s on average** .


That makes Fable slower per task than GPT 5.5, Opus 4.7, and BU 2.0, but faster than Gemini 3 Pro, Qwen 3.6 Plus, Kimi K2.6, and DeepSeek V4 Pro.


The cost comparison makes the tradeoff clear.


Fable completed many of the long research and browsing tasks that usually separate stronger web agents from weaker ones. These are tasks that require following many steps of reasoning, checking constraints, and returning a structured answer rather than extracting one from a page.


## Failure pattern


Fable's failures mostly emerged from three problems: it extracted the wrong fact or chose the wrong entity, it could not reach or search a source well enough, or it gave an answer that the trace did not fully support.


That is different from GPT 5.5's failure pattern. GPT 5.5 failed 32 tasks, often by leaving multi-part tasks unfinished. Fable failed nine of those same tasks and passed the other 23. The shared failures tended to involve fragile source access, many-step research tasks, or pages that were hard to search.


Fable also had failures of its own. In a few cases, the final answer looked plausible from the trace but still disagreed with the benchmark answer.


Fable is clearly better at finishing complex Browser Use tasks, but its remaining failures are ones you would expect from any model.


## Final thoughts


From our initial testing of Fable, it appears the model is better at continuing complex tasks for longer. It is better at keeping track of constraints, moving between sources, and turning messy web pages into a final answer.


Browser agents often fail in boring ways. They click the wrong result, miss one condition, lose a page, or answer before they have enough proof. Better models reduce these small failures, making web agents feel more reliable and deterministic.


The cost is still hard to ignore. Fable is far more expensive than any other model. But it also failed in fewer dumb ways.
