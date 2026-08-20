---
schema_version: "1.0.0"
document_id: "ac824d922dec80929072e0bb1d4478fa8113ec683d422de9b17bd5b3241cc45c"
company_key: "yc-greptile"
company: "Greptile"
source_id: "yc-greptile-news-import-f703c271ba19"
canonical_url: "https://www.greptile.com/blog/nvidia-nemotron-super-in-code-review"
published_at: "2026-03-11T12:00:00+00:00"
first_seen_at: "2026-07-21T22:07:23.426026+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:cd9fa12f191119a66d17d7b34faacb49dea94e5601512154ff76aa2b30305981"
---

# NVIDIA Nemotron 3 Super in Code Reviews | Greptile

At Greptile, we're working on agents that review and test code changes. We work closely with multiple frontier AI labs to determine the perfect models for each of the dozens of tasks that Greptile's agent completes in order to thoroughly validate a pull request. This might include tracing imports, writing tests, scanning for anti patterns and so on.


Recently, we started working with the NVIDIA Nemotron team on their newest model - Nemotron 3 Super. This is an open weight hybrid MoE model with 120B total parameters and 12B active parameters. Nemotron 3 Super is recommended for multi agent workflows because of its high accuracy for agentic tasks and a 1M token context window. We were given access to an 80% post-trained checkpoint to evaluate using our harness on our dataset of buggy code changes.


**TL;DR** Nemotron 3 Super punches far above its weight class. In spite of being less than a tenth of the size of frontier models, it was able to fluently use the harness, call tools, navigate the codebase using Bash and identify some surprising bugs in the code.


Here is a deep look into how we tested this model checkpoint, and what we learned.


## Methodology


We've been building an internal eval harness that runs models on real PRs to test quality. Large, multi-file refactors are a better test than small bug-fix PRs. They create enough churn to hide real behavior changes, and they force the model to separate cleanup from regressions.


Our harness keeps the setup fixed. Each model gets the same PR, the same prompt, and the same toolset. We then judge the review on four things:


1. Whether it found real bugs
2. Whether the comments were actionable
3. Whether the output stayed high-signal
4. Whether the model pulled the right amount of context for the issues it raised.


We look at the trace as closely as the final review, because a useful comment only matters if the model got there in a way we can trust.


## Nemotron 3 Super at a glance


We were particularly impressed by how much Nemotron 3 Super outperformed what we would typically expect from a model of this size. In our evals, it returned a useful review in 12.5 seconds with just 2 tool calls.


The trace was compact. Nemotron read the diff stat, pulled the full diff, and produced its review directly from the patch. In that pass, it surfaced five findings, three of them substantive.


The clearest example was a CORS regression. During the refactor, an origin check disappeared before the route set its CORS headers. That is exactly the kind of issue that can slip through when a PR mostly reads like cleanup.


Nemotron also identified two smaller regressions in shared utility code: one where a refresh flag's type no longer matched how it was used, and another where negative duration inputs could still produce bad output.


The example PR touched 19 files and produced a 134KB diff, which made it a good test of whether the model could separate cleanup from real regressions. This pass was not exhaustive, but it was well calibrated. It found a real behavioral issue quickly, and it did it with much less exploration than we usually expect.


## More details


Beyond the CORS regression, Nemotron 3 Super also caught a few smaller behavior issues in helpers and edge-case handling. These were smaller issues, but they were legitimate review comments. They showed that the model was tracking behavior changes, not just surface-level edits.


Nemotron model was strongest on issues legible from the patch itself. When a comment depended on more surrounding context, coverage was thinner. That matched the trace, since the model mostly stayed close to the diff.


There were also a few lighter-weight comments mixed in. Some skewed more toward cleanup than correctness, but that did not dominate the review.


Our takeaway was that Nemotron 3 Super is a strong first-pass reviewer, especially on PRs where latency and signal both matter. That combination of speed, efficiency, and reasonable judgment made it worth paying attention to.


We're excited to keep working closely with the NVIDIA Nemotron team as we evaluate new models across the frontier and build the universal validation layer for code.
