---
schema_version: "1.0.0"
document_id: "9cf1fd454869ae48ee4746a8145e4f110ff59f5340bfc1c1e91de27b5c8a6690"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/grok-4-5-launch"
published_at: "2026-07-10T17:17:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:3d28e23f59c7463ad09cd5e95dd9fefb4f522a06262d222081f28323a9a27048"
---

# Grok 4.5: SpaceXAI's New AI Model Bets on Speed and Cost Over Raw Power

Most AI model launches compete on one thing: benchmark scores. SpaceXAI is trying something different. Grok 4.5, the company's newest model[launched on July 8, 2026](https://x.ai/news/grok-4-5) , doesn't claim to be the best-performing model you can buy. Instead, it makes a pointed argument about efficiency: what if you could get close to frontier-level performance at a fraction of the cost and speed?


It's a pitch that matters if you're building with AI on a budget, and it lands during one of the busiest weeks in AI this year.


## What Is Grok 4.5?


Grok 4.5 is a large language model built by SpaceXAI, the entity formed after[SpaceX acquired Elon Musk's AI company xAI earlier this year](https://www.axios.com/2026/07/08/spacexai-grok-new-model) .[Cursor describes it](https://cursor.com/blog/grok-4-5) as a mixture-of-experts model, and it's the first model SpaceXAI developed jointly with Cursor, the popular AI coding editor whose parent company, Anysphere, SpaceX has agreed to acquire in a reported $60 billion all-stock deal expected to close later this year.


Unlike earlier Grok versions that leaned toward general chatbot use, Grok 4.5 is built for coding, agentic tasks (where AI handles multi-step workflows with minimal human input), and what SpaceXAI broadly calls "knowledge work." The model was trained on tens of thousands of Nvidia GB300 GPUs, with trillions of tokens of real developer coding data from Cursor sessions mixed in alongside STEM, research, and engineering material.


[Elon Musk described it on X](https://x.com/elonmusk/status/2074740539874775163?s=20) as "an Opus-class model, but faster, more token-efficient and lower cost," referencing Anthropic's high-end Claude model line.


## The Benchmarks: Competitive, Not Dominant


SpaceXAI was unusually transparent about where Grok 4.5 stands. According to the company's own published benchmark data, Grok 4.5 sits in the middle of the pack among top models, not at the top.


On DeepSWE 1.0, a coding evaluation run by Datacurve and Artificial Analysis, Grok 4.5 scored 62.0%, behind Anthropic's Fable (66.1%) and OpenAI's GPT 5.5 (64.31%), according to SpaceXAI's published figures. On SWE Bench Pro, it resolved 64.7% of tasks, trailing Fable at 80.4% and Opus 4.8 at 69.2%, per the same source.


Where Grok 4.5 stood out in SpaceXAI's benchmarks was SWE Marathon, where it topped the field at 29.0% resolution rate, and Terminal Bench 2.1, where it scored 83.3%, essentially matching GPT 5.5 at 83.4%.


SpaceXAI also claims Grok 4.5 ranks first on[Harvey's Legal Agent Benchmark](https://www.vals.ai/benchmarks/hlab) , a test for legal work in agentic settings. It's worth noting that the independent leaderboard run by Vals.ai does not list Grok 4.5 and shows Anthropic models in the top positions. SpaceXAI may be referencing different evaluation conditions or metrics. As with all self-reported benchmarks, the numbers should be taken as directional rather than definitive until independent testing confirms them.


## The Real Story: Token Efficiency and Pricing


The benchmark scores tell one story. The cost story is arguably more interesting for anyone actually paying to run AI.


Grok 4.5 is priced at **$2 per million input tokens** and **$6 per million output tokens** . For comparison, Anthropic's Claude Opus 4.8 runs $5 input / $25 output per million tokens. OpenAI's GPT-5.6 Luna tier sits at $1 input / $6 output. That puts Grok 4.5 in a competitive middle ground: cheaper than most frontier models, though not the absolute cheapest option available.


But pricing alone doesn't capture the full picture. According to SpaceXAI's own data, Grok 4.5 resolves SWE Bench Pro tasks using an average of about 15,954 output tokens, compared to roughly 67,020 for Opus 4.8 on the same benchmark. That's about 4.2 times fewer tokens to complete the same work. If that efficiency holds up in real-world use, the effective cost difference is much larger than the per-token price alone suggests.


The model also runs at roughly 80 tokens per second, which SpaceXAI describes as "fast-model speed." Early independent testing from[TryAI](https://www.tryai.dev/blog/grok-4.5-vs-gpt-5.5-vs-claude-build-off) found Grok 4.5 streaming at approximately 110 tokens per second, about double the speed of comparable models in their test setup.


## Beyond Code: Office Work and App Building


Grok 4.5 isn't just a coding model. SpaceXAI is positioning it as a general knowledge-work tool through Grok Build, its AI coding and productivity agent where Grok 4.5 is now the default model.


Through Grok Build, the model can assemble multi-sheet Excel spreadsheets with working formulas and web-sourced data, create PowerPoint presentations using native shapes and diagrams (not just flat images), and draft long-form documents in Word. SpaceXAI has also rolled out Microsoft Office plugins for all three tools.


For the builder-curious crowd, SpaceXAI highlighted Grok 4.5's ability to generate complete, functional apps from a single prompt. Their demo showed a working Three.js solar system simulation built from one natural-language description. Independent testing from TryAI confirmed the model produces polished, functional apps from one-shot prompts, going toe-to-toe with GPT 5.5 and Claude Opus 4.8 in a head-to-head build-off.


## Where to Try It


Grok 4.5 is available now through three channels:[Grok Build](https://x.ai/cli) (SpaceXAI's terminal-based coding agent), Cursor on all plans, and the SpaceXAI API console. SpaceXAI is offering free usage for a limited time in both Grok Build and Cursor, with Cursor doubling included usage for the first week.


One important caveat: Grok 4.5 is **not yet available in the EU** across any SpaceXAI product or API. The company says EU access is expected in mid-July.


## What This Means for Builders


The AI model market is getting crowded, and that's good news if you're building products. Grok 4.5 signals a shift where labs compete on cost efficiency, not just raw capability. For solo creators and small teams running AI-powered features, a model that finishes the same task in a quarter of the tokens at a lower per-token price can meaningfully change the economics of what you can build and ship.


The free trial window in Grok Build and Cursor makes this an easy one to test without commitment. If you're currently paying per-token for coding assistance, document generation, or agent workflows, Grok 4.5 is worth benchmarking against whatever you're running today.


Stay tuned to[Emergent News](https://emergent.sh/news) for more on AI tools, launches, and what they mean for builders.
