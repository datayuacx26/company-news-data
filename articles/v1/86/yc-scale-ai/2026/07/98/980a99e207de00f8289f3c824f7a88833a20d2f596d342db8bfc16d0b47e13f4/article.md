---
schema_version: "1.0.0"
document_id: "980a99e207de00f8289f3c824f7a88833a20d2f596d342db8bfc16d0b47e13f4"
company_key: "yc-scale-ai"
company: "Scale AI"
source_id: "yc-scale-ai-news-import-179249eb8ad6"
canonical_url: "https://scale.com/blog/insights-generator"
published_at: "2026-07-16T19:35:00+00:00"
first_seen_at: "2026-07-22T12:47:56.369954+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:9848b72ed79894a0d306b221abd8d138915bba5699eb7c68c0899f7aef6fcdf9"
---

# Insights Generator: Diagnosing Agent Failures

Too many companies are learning the[hard way](https://scale.com/blog/introducing-the-six-percent-report) : successful pilots often do not lead to successful deployments. As engineers take their agents from v1 to v2, diagnosing failure patterns becomes a major engineering cost. We created Insights Generator to increase engineer productivity when doing this work. In one study, engineers using Insights Generator improved their agents nearly twice as much as those using leading tools.


Coming to the[Scale GenAI Platform](https://scale.com/genai-platform) , Insights Generator reads across an agent's full run history at once, catching what no single trace reveals.


*For a closer technical look, read the[paper](https://labs.scale.com/papers/insights-generator) or the full write-up on[Scale Labs](https://labs.scale.com/blog/insights-generator) .*


## Results


We evaluated Insights Generator across four settings, varying who did the judging and what was measured. Two results stand out:


- **Engineers using Insights Generator nearly doubled the improvement of Claude Code with subagents.** In a controlled study, Scale engineers given IG reports lifted agent performance by +30.4 percentage points over baseline in a 90-minute window. The same engineers given Claude Code with subagents reports achieved +16.2 points in the same window. The diagnostic report was the only variable.
- **Insights Generator produced the strongest diagnostic reports under automated evaluation.** In a pairwise tournament using an automated judge, IG's diagnostic reports won 77.9% of comparisons across SpreadsheetBench and Humanity's Last Exam, compared with 62.4% for reports from the next-best system. IG's largest advantages were in explaining the mechanisms behind failures and grounding findings in specific trace evidence, giving engineers a clearer path from diagnosis to fix.


Most agent debugging tools work one trace at a time. Observability platforms let you inspect individual runs, but you still have to do the reading. Other tools look across many runs but flag patterns too vaguely to fix. Insights Generator reads across the full set of an agent's runs, tests theories about what's going wrong, and produces a report that names each pattern, how often it occurs, and which specific runs to look at.


## Failure Compound


Agents often don’t know when they fail. They produce wrong answers with confident self-assessment, and the errors compound across runs. By the time anyone notices, the behavior is baked in. Insights Generator surfaces these failures at scale. A few examples from corpora we analyzed:


- On AppWorld, a benchmark where agents complete multi-step tasks across apps like Spotify and Venmo, 50 of 51 incorrect runs marked themselves as successfully completed, often with celebratory language
- On SpreadsheetBench, 128 of 250 runs generated a self-contained solution that the runtime silently discarded. Agents usually recovered on the next turn, but the bug wasted at least 128 turns.
- IG uncovered a benchmark-contamination channel on Humanity’s Last Exam. Four of 391 runs found copies of the benchmark on Hugging Face that included the answers, and all four were graded correct. One agent downloaded the benchmark’s complete answer key; another copied an exact answer from a search result and immediately returned it.


While a single trace can present an error mode, full corpus analysis identifies and prioritizes the most important error modes for improving agent performance. All of them were obvious once the full corpus was analyzed together. Reading a handful of traces gives you anecdotes, but reading across the corpus gives you the patterns that actually explain how your agent behaves in production.


## Coming to Scale GenAI Platform


Insights Generator is rolling out to[Scale GenAI Platform](https://scale.com/genai-platform) customers as a diagnostic layer for production agents. Instead of engineers manually pulling traces and hunting for patterns when something goes wrong, IG surfaces and quantifies issues across the full trace corpus, so teams spend their time fixing what's breaking rather than finding it.


The result is a shift in how agent improvement works. It stops being a manual, one-off engineering project and starts becoming a continuous loop, where better diagnostics produce better outcomes on both sides, whether the fix comes from a person or a system. That's how enterprises close the gap between a successful pilot and a reliable deployment.
