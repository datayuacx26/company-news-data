---
schema_version: "1.0.0"
document_id: "61237dad8309a26727096dc0dff58a5db12fc6d1b7f640ab09c62756569bc75e"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/claude-opus-5-launch"
published_at: "2026-07-27T22:40:00+00:00"
first_seen_at: "2026-07-27T22:01:15.428635+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:88910552c7477e495ba38a4fa372ef31d13eeeaf51679ffeded468c5e164a256"
---

# Claude Opus 5 Is Live: Benchmarks, Pricing, and What's New

Anthropic released Claude Opus 5 on July 24, 2026. The model approaches[Fable 5](https://emergent.sh/learn/what-is-claude-fable-5) on most major benchmarks, costs half as much per token, and is now the default model on Claude Max.


For anyone building with AI tools, the practical story is simple: the model most people will use every day just got significantly stronger without getting more expensive.


## What Claude Opus 5 Costs and Where to Find It


Opus 5 is priced at $5 per million input tokens and $25 per million output tokens, the same as its predecessor Opus 4.8. Developers can access it through the API using the model string claude-opus-5.


On the consumer side, Opus 5 is the new default on Claude Max and the strongest model available on Claude Pro. There are no data retention requirements for general access, which was also the case with Opus 4.8.


Anthropic is also offering a Fast mode that runs at roughly 2.5 times the default speed. Fast mode costs twice the base price and is available on the Claude Platform and through usage credits in Claude Code.


Two beta features ship alongside the model: mid-conversation tool changes on the Claude Platform (developers can now swap tools without breaking the prompt cache) and automatic fallbacks on the API (requests flagged by safety classifiers can route to another model instead of being blocked).


## Claude Opus 5 Benchmark Performance


The headline numbers are strong. All benchmarks below are reported by Anthropic from their[official announcement](https://www.anthropic.com/news/claude-opus-5) , with scores at the highest effort level unless noted otherwise.


On **Frontier-Bench v0.1** , a third-party agentic coding benchmark, Opus 5 scores 43.3%. That is more than double Opus 4.8's 21.1% and comfortably ahead of Fable 5's 33.7%.


It also beats[GPT-5.6](https://emergent.sh/news/gpt-5-6-launch) Sol (34.4%) by a wide margin. On a cost-per-attempt basis, Opus 5 reaches its peak performance at roughly half the cost of Fable 5 reaching its peak.


On **ARC-AGI-3** , a benchmark that tests novel problem-solving, Opus 5 scores 30.2%. Opus 4.8 managed just 1.5%, and GPT-5.6 Sol scored 7.8%. Fable 5 has no reported score. This is one of the largest generational jumps in the entire table.


On **AutomationBench** , which measures whether models can complete real business workflows from start to finish, Opus 5 scores 26.0%. The next closest model is GPT-5.6 Sol at 18.1%, followed by Fable 5 at 17.4% and Opus 4.8 at 17.0%. Even at its lowest effort setting, Opus 5 passes more tasks than any other model.


A few other standout numbers: Opus 5 hits 70.6% on **OSWorld 2.0** (computer use), up from 55.7% for Opus 4.8. It leads knowledge work on **GDPval-AA v2** with a score of 1861, ahead of Fable 5's 1747. And on **BrowseComp** (agentic search), it scores 90.8%, slightly ahead of GPT-5.6 Sol's 90.4%.


It is not the top model everywhere. GPT-5.6 Sol leads on **DeepSWE v1.1** (72.7% vs Opus 5's 68.8%), and Fable 5 edges ahead on **FrontierCode v1.1** (53.5% vs 53.4%) and the **Legal Agent Benchmark** (13.3% vs 11.7%). On **HealthBench Professional** , the top score belongs to Mythos 5 (66.0%), with Opus 5 at 59.8%.


The overall picture: Opus 5 leads or ties the field on the majority of evaluations, and on the ones where it trails, the gap is narrow, except against Mythos 5 on specialized safety and security tasks.


## How Opus 5 Works Differently


Benchmarks tell part of the story. The other part is how the model behaves during extended, autonomous work.


Anthropic highlights several examples from evaluations and early-access testing. In one Frontier-Bench task, Opus 5 was given a drawing of a machine part and asked to rebuild it as a 3D model, but was intentionally given no way to view the drawing directly. The model wrote its own computer vision pipeline to extract the geometry from raw pixels, then reconstructed the full part. No competing model given the same setup solved it in five attempts.


In another case, given a real bug in a popular open-source package manager, Opus 5 found the root cause and fixed an edge case that the community's own patch had missed. A competing model fixed only the surface symptom and reported the bug resolved.


Early-access customers echo this pattern. Zapier said Opus 5 topped its AutomationBench leaderboard and completed a full churn-prevention sequence end to end, including flagging at-risk accounts, alerting the right owner, and summarizing for retention ops. Box found Opus 5 outperformed Opus 4.8 by 8% overall, with 11% improvement on data analysis and 17% on due diligence workflows.


Anthropic also notes that Opus 5 produces stronger visual outputs, with improved artifacts, animations, and interactive elements.


## Safety and Alignment


Opus 5 is Anthropic's most aligned model to date, scoring 2.3 on their automated behavioral audit for overall misaligned behavior (the lowest of any recent model, per Anthropic). It follows Claude's Constitution more closely than Opus 4.8,[Sonnet 5](https://emergent.sh/news/claude-sonnet-5-launch) , or Fable 5 and shows the lowest rates of deceptive behavior.


On the dual-use capability front, Opus 5 does not advance the frontier. It remains behind Mythos 5 in both biology research and offensive cybersecurity. Anthropic notes that while Opus 5 approaches Mythos 5 at identifying software vulnerabilities, it falls substantially behind on actually exploiting them.


The model was not trained on cybersecurity tasks. Its improvements in that area are a side effect of becoming more generally capable.


## What This Means for Builders


Claude Opus 5 shifts the practical calculus for anyone building with AI. The model that most people will actually use daily is now performing at or near Fable 5 levels on coding, reasoning, and business automation, at the same $5/$25 price point that Opus 4.8 had.


The AutomationBench results are especially relevant. A model that can complete business workflows end to end, from flagging at-risk accounts to alerting the right team member, is not just a better chatbot. It is a better foundation for the tools and automations that non-technical builders rely on.


For builders on platforms like Emergent, where[Opus 5 is already available](https://x.com/emergentlabs/status/2080705257538207781?s=20) , this upgrade flows downstream. Stronger models produce better code, more reliable backends, and more polished outputs. And the cost to get that performance did not go up.


If you have been using Opus 4.8, Opus 5 is a direct upgrade at the same price. If you have been paying for Fable 5 on every task, Opus 5 may handle 90% of those tasks just as well for half the cost. Either way, the gap between what AI can do and what most people are paying for it just got narrower.


*For more coverage of AI launches and practical builder takeaways, follow*[Emergent News](https://emergent.sh/news) *.*
