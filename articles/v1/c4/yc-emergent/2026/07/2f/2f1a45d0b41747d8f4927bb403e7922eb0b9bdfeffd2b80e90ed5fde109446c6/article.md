---
schema_version: "1.0.0"
document_id: "2f1a45d0b41747d8f4927bb403e7922eb0b9bdfeffd2b80e90ed5fde109446c6"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/claude-opus-5-release-date"
published_at: "2026-07-24T21:41:00+00:00"
first_seen_at: "2026-07-24T18:25:00.224632+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:d27b294b475a32f3f8cb6604adff7b503967082d7eedbdf1102b415f85ca1a66"
---

# Claude Opus 5 Release Date: What We Know So Far

**Update (July 27, 2026):** Claude Opus 5 officially launched on July 24. For the full breakdown of benchmarks, pricing, and what it means for builders, read our[launch coverage](https://emergent.sh/news/claude-opus-5-launch) .


Claude Opus 5 launched on July 24, 2026. It is priced at $5 per million input tokens and $25 per million output tokens, the same as Opus 4.8, and is available on the API as claude-opus-5. It is the new default model on Claude Max and the strongest model on Claude Pro.


Before the official announcement, the model had one of the more visible pre-launch trails in recent AI history. A codename, two Cursor sightings, and deployment tracker activity all preceded the launch by weeks. Here is how it unfolded.


## What Is Claude Opus 5?


Claude Opus 5 is the latest model in Anthropic's Opus tier, the company's premium line for complex reasoning, coding, and autonomous agent work. It replaced Claude Opus 4.8 (released May 28, 2026) as the Opus flagship.


Anthropic's model lineup breaks into distinct tiers. At the top sits the Mythos class, which includes[Claude Fable 5](https://emergent.sh/learn/what-is-claude-fable-5) (launched June 9, 2026) and the restricted Claude Mythos 5. Below that is the Opus tier, now led by Opus 5. Then comes[Sonnet 5](https://emergent.sh/news/claude-sonnet-5-launch) (June 30, 2026), Anthropic's high-value workhorse, followed by Haiku 4.5 for lightweight, high-volume tasks.


Opus 5 slots exactly where pre-launch speculation placed it: between the Mythos tier and Opus 4.8, offering stronger performance than 4.8 at a more accessible price point than Fable 5's $10/$50 per million tokens.


## How the Launch Unfolded


### The Honeycomb leak (July 8)


On July 8-9, a model called "Claude Honeycomb EAP" appeared inside[Cursor's model picker](https://x.com/chetaslua/status/2075053167121973413) and was pulled within hours. It was a real listing in a shipping product, seen by multiple people and[reported by The New Stack](https://www.techtimes.com/articles/320265/20260712/fable-5-free-through-july-19-anthropic-blinks-again-opus-5-leak-surfaces-cursor.htm) before it disappeared.


The listing showed a 1M-token context window, an "extra high effort" reasoning mode (a step above the "high" default on Opus 4.8), per-turn controls with safety fallbacks that route to Opus 4.8, and placement alongside other frontier models in Cursor's picker.


Two things stood out. The fallback chain stepping down to Opus 4.8 implies Honeycomb sits above 4.8 in capability. Fallbacks conventionally step down, not up. And Cursor has been a reliable pre-launch venue before. A "claude-sonnet-5" identifier surfaced in Google Vertex AI logs before Sonnet 5 launched, following the same pattern.


The listing did not include pricing, benchmarks, an official name, or a release date. "Honeycomb" was a codename.


### Deployment signals (July 23-24)


On July 23,[deployment trackers reported](https://x.com/M1Astra/status/2080246754104971356) Opus 5 preparations and described the model as "rolling out across providers." Some users on X claimed they were[served improved responses under the Opus 4.8 label](https://x.com/testingcatalog/status/2080260867946352854) . On July 24, a Cursor error dialog surfaced showing the string claude-opus-5-thinking-high, the first appearance of the literal "claude-opus-5" name in a shipping product.


### Official launch (July 24)


Hours after the Cursor string surfaced, Anthropic published the official announcement. Opus 5 scored 43.3% on Frontier-Bench v0.1 (more than double Opus 4.8's 21.1%) and led on most major evaluations. Anthropic described it as approaching Fable 5 intelligence at half the price.


## What Claude Models Are Available Right Now


**Claude Fable 5** is Anthropic's most capable publicly available model. It excels at the hardest autonomous tasks: long-running code migrations, complex multi-step research, and anything where getting it right on the first pass saves significant time and money. It costs $10/$50 per million input/output tokens and requires 30-day data retention. Think of this as the specialist you bring in for the toughest jobs.


**Claude Opus 5** is the new Opus flagship and the model most builders should default to for complex work. It approaches Fable 5 on most benchmarks at half the token cost ($5/$25 per million tokens), with no data retention requirements. It is the default on Claude Max, the strongest model on Claude Pro, and available on Emergent. For most professional work, Opus 5 is where quality and cost find the best balance.


**Claude Opus 4.8** remains available at the same $5/$25 pricing and continues to serve as the fallback model when Opus 5's safety classifiers flag a request. It supports zero data retention. For builders already using Opus 4.8, Opus 5 is a direct upgrade at the same price.


**Claude Sonnet 5** is the value pick that has been closing the gap fast. At $3/$15 per million tokens (with introductory pricing of $2/$10 through August 31, 2026), it delivers performance close to Opus 4.8 on many tasks. It is the default model on Claude's free and Pro plans and the best starting point for most everyday work.


**Claude Haiku 4.5** is the speed and budget option at $1/$5 per million tokens. It handles simple classification, extraction, and high-volume tasks where cost matters more than depth.


For most builders, the practical strategy is straightforward: start with Sonnet 5 for everyday work, step up to Opus 5 when accuracy on hard tasks matters, and reserve Fable 5 for the work where failure costs more than the token premium.


## What This Means If You Build Without Code


Opus 5 is a meaningful upgrade over Opus 4.8 for the work AI app-building platforms handle behind the scenes: code generation, multi-step logic, backend reliability, and handling complex app requirements. It delivers those improvements at the same price as Opus 4.8, so the cost of building does not go up.


The current model lineup is the strongest it has ever been. Sonnet 5 alone outperforms what was available at the Opus tier just six months ago, and Opus 5 now approaches Fable 5 on most benchmarks at half the cost.


On Emergent, Opus 5, Sonnet 5, Fable 5, and all other major models are already available to build with.


Stay tuned to[Emergent News](https://emergent.sh/news) for more on AI tools, launches, and what they mean for builders.
