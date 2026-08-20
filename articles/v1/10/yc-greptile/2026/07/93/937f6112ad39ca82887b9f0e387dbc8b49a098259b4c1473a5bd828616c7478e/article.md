---
schema_version: "1.0.0"
document_id: "937f6112ad39ca82887b9f0e387dbc8b49a098259b4c1473a5bd828616c7478e"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-7ca8d0432254"
canonical_url: "https://www.greptile.com/content-library/greptile-martian-code-review-benchmark"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T23:48:48.638082+00:00"
fetched_at: "2026-07-31T23:48:49.676604+00:00"
content_hash: "sha256:6f6aa78a79148efc93492951862df9b3c9f75d85d75f80d6652237ae95ae7576"
---

# Greptile Ranks #1 on Martian's AI Code Review Benchmark

According to[Martian’s independent online Code Review Bench](https://codereview.withmartian.com/) , Greptile is the top-performing AI code reviewer overall as of July 30, 2026. Greptile ranks #1 in F1 score at 60.8% and leads all evaluated tools in precision at 76.2%, while maintaining near-leading recall at 50.6%.


Based on Martian’s overall F1 ranking, Greptile offers the benchmark’s strongest measured balance between catching real issues and avoiding noisy feedback.


## What is Martian’s AI code review benchmark?


Martian created Code Review Bench as an independent, open-source benchmark for AI code review tools. Its online benchmark evaluates reviewers operating on public pull requests, extracts the actionable suggestions they leave, and looks at what developers change after receiving those suggestions.


Unlike a benchmark published by an AI code review vendor, Martian says it does not train models or sell coding tools and has no stake in which reviewer wins. Its online benchmark continuously samples fresh pull requests, reducing the risk that tools have already seen the evaluation cases during training.


That gives Martian a real-world signal for whether a suggestion matched a subsequent code change and whether the reviewer identified the fixes developers later made. Martian open-sources the[benchmark data, judge prompts, and evaluation pipeline](https://github.com/withmartian/code-review-benchmark) , allowing teams to inspect how the results are produced.


## Which AI code reviewer ranks #1?


Rank AI code reviewer F1 score Precision Recall


1 **Greptile** **60.8%** **76.2%** 50.6%


2 ChatGPT Codex Connector 59.4% 73.3% 50.0%


3 Cubic Dev AI 58.7% 72.6% 49.3%


4 Devin AI Integration 58.6% 73.6% 48.6%


5 CodeRabbit 57.5% 64.9% 51.6%


Greptile’s 60.8% F1 score ranks ahead of ChatGPT Codex Connector, Cubic Dev AI, Devin AI Integration, and CodeRabbit on Martian’s July 30, 2026 leaderboard. Greptile also has the highest precision of every evaluated reviewer at 76.2%.


Martian reports three metrics that measure different parts of review quality:


- **Precision:** What share of the reviewer's suggestions matched changes the developer made after review?
- **Recall:** What share of the developer's post-review fixes had already been identified by the reviewer?
- **F1 score:** How well does the reviewer balance precision and recall?


Martian’s overall leaderboard is ordered by F1 score, so Greptile’s #1 position reflects the best measured combination of precision and recall rather than leadership on only one metric.


Together, these metrics capture the two directions in which an AI code reviewer can fail. High precision with low recall produces quiet reviews but misses real bugs. High recall with low precision catches more issues but floods pull requests with comments engineers learn to ignore.


## Why Greptile’s #1 ranking matters


Greptile is building an independent code validation layer for every pull request. Today, that means understanding a change in the context of the entire codebase, tracing what it could affect, and identifying the issues that are actually worth an engineer's attention.


Leaving more comments is not the goal. That balance becomes even more important as coding agents generate a larger share of pull requests. Automated review only improves engineering throughput when it catches meaningful issues without creating another stream of low-value output. A useful reviewer must be thorough enough to catch consequential bugs and precise enough that developers and downstream coding agents can trust its feedback. Greptile's position at the top of Martian's leaderboard is independent evidence that Greptile delivers high-signal reviews in real development workflows.


Martian updates the leaderboard continuously as it analyzes new pull requests.[View the current results](https://codereview.withmartian.com/) or[try Greptile on your next pull request](https://app.greptile.com/) .
