---
schema_version: "1.0.0"
document_id: "2999d55e5059b4a86e214fdb630d3a06d73482d4fd0e3c10bc5ea64f6884576e"
company_key: "yc-sourceress"
company: "Sourceress"
source_id: "yc-sourceress-news-import-457a07c39d0c"
canonical_url: "https://imbue.com/blog/2026-04-29-how-ai-code-review-can-make-correct-code-worse"
published_at: "2026-04-29T00:00:00+00:00"
first_seen_at: "2026-07-26T01:22:31.918155+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:a6183862deb098adad53f707b0e5d8915d9ea7e6035db3c6acf497a1c50486b2"
---

# How AI code review can make correct code worse

Code review has been essential in software development, at least while humans drove the changes. But what happens when you replace both the PR author and the reviewer with AI agents? We built such an agent pipeline and tested it on SWE-bench Pro Python tasks. An **implementer** wrote code, a **reviewer** generated PR comments, and a **fixer** addressed the comments.


A typical human code review pipeline, replaced by agents.


The mean score did not move statistically significantly, but we learned something interesting to share.


## Key learnings


AI reviewers flag legitimate issues. But when a weaker fixer agent addresses them, it sometimes makes changes beyond the scope of the review. We call this *overreach* , and it’s how automated code review can make correct code worse. We found that softening the instructions to the fixer reduces regressions.


Read on for the experimental setup, what we mean by “softening instructions”, and how code review delivers improvements beyond what benchmarks can measure.


## How code review broke a correct bug fix


A strong agent (Claude Opus) fixes a[qutebrowser](https://github.com/qutebrowser/qutebrowser) color parsing bug (HSV hue percentages were[scaled wrong](https://huggingface.co/datasets/ScaleAI/SWE-bench_Pro/viewer/default/test?row=0&views%5B%5D=test&sql=select+%2A+from+test+where+instance_id+%3D+%27instance_qutebrowser__qutebrowser-6b320dc18662580e1313d2548fdd6231d2a97e6d-v363c8a7e5ccdf6968fc7ab84a2053ac78036691d%27) ), passing all tests.


The Opus implementer's patch: a correct fix that passes all tests.


A code reviewer Vet[1](https://imbue.com/research/2026-04-29-how-ai-code-review-can-make-correct-code-worse/#user-content-fn-1) examines the patch:


> “
>
>
> No regression tests added. Add tests covering percentages and boundary values. ”


A weaker agent (Claude Sonnet) addresses the review. It adds the requested tests. But while it’s there, it tries to “improve” the math by rewriting the rounding logic. It then runs pytest. One test fails. The agent logs:


> “
>
>
> There’s one failing test (` rgba(255, 255, 255, 1.0)` ) that’s **unrelated** to our changes ”


It ships the code.


The Sonnet fixer's patch: adds requested tests but also rewrites the rounding math, breaking an existing test.


However, the failing test is caused by the[change](https://gist.github.com/weishi-imbue/6d4e90029678605ad7ddcba6a3d7fe4c) . The fixer agent just couldn’t connect its own math change to the broken test.


## Zooming out: what happens across all tasks?


We ran this pipeline, implementer → reviewer → fixer, on SWE-bench Pro tasks across ansible and qutebrowser. (See[Pipeline](https://imbue.com/research/2026-04-29-how-ai-code-review-can-make-correct-code-worse/#pipeline) below for the full setup.)


### The invisible value: code quality improvements


In more than 70% of trials, the score doesn’t change. But that doesn’t mean the reviewer and fixer did nothing. The reviewer flagged genuine issues, and the fixer made real changes, sometimes substantial ones. They just didn’t affect the specific tests the benchmark evaluates against.


- [qb-01d1d149](https://huggingface.co/datasets/ScaleAI/SWE-bench_Pro/viewer/default/test?row=0&views%5B%5D=test&sql=select+%2A+from+test+where+instance_id+%3D+%27instance_qutebrowser__qutebrowser-01d1d1494411380d97cac14614a829d3a69cecaf-v2ef375ac784985212b1805e1d0431dc8f1b3c171%27) : Vet flagged that` _initialized` is never set to` True` after` _initialize_info()` runs. Every call to` get_version()` redundantly re-initializes. The fixer added` self._initialized = True` . ([diff](https://gist.github.com/weishi-imbue/2719d1efedf8b4c22628ad3213111397) )


These kinds of changes don’t move benchmarks, but the quality improvement will compound in real codebases.


### Actual lifts: review helps fix more


On some tasks, the reviewer discovers real issues the implementer introduced, and the fixer resolves them. This results in a lift in f2p gains (fail-to-pass: tests that verify the bug is fixed).


- [an-be59caa5](https://huggingface.co/datasets/ScaleAI/SWE-bench_Pro/viewer/default/test?row=0&views%5B%5D=test&sql=select+%2A+from+test+where+instance_id+%3D+%27instance_ansible__ansible-be59caa59bf47ca78a4760eb7ff38568372a8260-v1055803c3a812189a1133297f7f5468579283f86%27) : The implementer added ipset match support to Ansible’s iptables module, scoring 22/23 and failing the hidden test. Claude Code Review (CCR) identified two issues. All four fixer configurations, regardless of model or review format, addressed the review issue correctly and scored 23/23. Consistent lift. ([diff](https://gist.github.com/weishi-imbue/2dc94b9fcbddb6b983176bb3f8ed882a) )


### Regressions: rare but real


Like the rgba regression in our opening story, regressions do happen. To understand them better, we designed a controlled experiment: we took 15 tasks where the implementer already passed all tests (fully resolved), ran code review (Vet) on them, and had fixers address the findings.


**What did the reviewer flag on correct code?**


Of 15 resolved tasks, the reviewer found issues in 8. The breakdown:


Issue type Count Description


\`test_coverage\` 6 “Add tests for this new functionality”


\`logic_error\` or \`runtime_error_risk\` 2 Real issues or potential edge cases


**What happened after the fixer addressed them?**


Most tasks stayed the same. The fixer added tests or made minor fixes without breaking anything.


But one task regressed. The fixer made changes beyond the immediate scope of the review finding, and that led to a regression. This is a common pattern among the regressions we observed. We call it **overreach** .


To understand more about this overreach behavior, we ran an experiment varying the review detail level.


#### More detail enables overreach


We tested full Claude Code Review (CCR) comments (with` <details>` reasoning, over 6k chars) versus summary-only comments (stripped to under 1k chars).


[an-4c5ce5a1](https://huggingface.co/datasets/ScaleAI/SWE-bench_Pro/viewer/default/test?row=0&views%5B%5D=test&sql=select+%2A+from+test+where+instance_id+%3D+%27instance_ansible__ansible-4c5ce5a1a9e79a845aff4978cfeb72a0d4ecf7d6-v1055803c3a812189a1133297f7f5468579283f86%27) : The implementer (Opus) refactored Ansible’s selinux integration, wrapping the native C library in a compatibility layer used across 10 files ([impl patch](https://gist.github.com/weishi-imbue/9becb28be1f95c36a9d68a8f93f5c731#file-1-impl-patch-diff) ). It scored 16/17. CCR then flagged[three quality issues](https://gist.github.com/weishi-imbue/9becb28be1f95c36a9d68a8f93f5c731#file-2-ccr-review-full-md) : a memory leak in selinux ctypes (hard), a double-period typo (easy), and dead code (easy). We then fed these to a Sonnet fixer with various detail levels. The reviewer’s` <details>` sections are well-written, but the extended bug analysis gives Sonnet enough context to invent its own approach and get it wrong.


- **Summary only** : Addressed all quality issues. Added` freecon()` for the memory leak, corrected the typo, removed dead code. 4 lines of source changes. Score unchanged at 16/17 (p2p: 7/8, f2p: 9/9). ([diff](https://gist.github.com/weishi-imbue/9becb28be1f95c36a9d68a8f93f5c731#file-3-fixer-summary-diff) )
- **Full details** : Restructured the selinux integration across 8 files with 113 lines, resulting in a score of 8/17; all 9 f2p tests now fail. Δ=-8. ([diff](https://gist.github.com/weishi-imbue/9becb28be1f95c36a9d68a8f93f5c731#file-4-fixer-full-details-diff) )


#### Measuring overreach: detail leads to more changes


We measured how many lines of code each fixer changed relative to the impl across 64 trials across 16 tasks.


Fixer / Format Avg LOC Max LOC


opus / full 11 58


opus / summary 9 52


sonnet / full 46 343


sonnet / summary 19 160


Full details cause Sonnet to make significantly larger changes.


Full review details cause the Sonnet fixer to change more lines.


#### The one-sentence fix


The original fixer prompt says: “Fix the issues identified.” We introduced softer instruction variants for the fixer, such as “Address the issue when confident.” The specific wording didn’t tend to matter. Any permission to skip hard issues was sufficient. All softer variants reduced Sonnet’s overreach and eliminated catastrophic regressions, while Opus was largely unaffected.


Softer instructions eliminate Sonnet's catastrophic regressions; Opus is largely unaffected.


## Practical guidance


1. **Invest in the fixer.** Its quality matters. If you’re auto-fixing review issues with agents, use softer instructions to let the fixer skip hard issues. Let humans focus on what it skips.
2. **Review in the loop, not just at the PR stage.** Review at the PR stage is useful, but it’s even better to add a review step inside the agentic loop, catching issues on small changes before they compound. Stay tuned for a future post on agentic review setup best practices.


## Appendix


### Benchmark and task selection


We selected 175 Python tasks from[SWE-bench Pro](https://huggingface.co/datasets/ScaleAI/SWE-bench_Pro) .


We ran all tasks to establish implementer baselines, then selected subsets for different experiments. Partial-score tasks (some tests passing) give room for both lifts and regressions. Fully-resolved impl patches (all tests passing) make it easier to isolate review-plus-fix damage on working code.


### Pipeline


Each task goes through 3 independent stages, each in its own sandbox with no shared Claude session or conversation context:


1. **Implementer** : Claude Code CLI agent (` max_turns=200` , 1h timeout) attempts the bug fix in a per-task Docker image from SWE-bench Pro.
2. **Reviewer** : Claude Code Review (GitHub-managed, inline PR comments with` <details>` reasoning) or Vet CLI (structured JSON output).
3. **Fixer** : Claude Code CLI agent addresses review findings in a fresh sandbox with the impl patch pre-committed. Tests are hidden from the agent.


### Eval protocol


Each patch (impl and fixer separately) is evaluated in a fresh Docker container with a pristine repo and pre-baked tests.


- **Eval runs per patch** : 4 runs; we use the minimum score. (A flaky impl is considered failing the test.)
- **Metrics** :` passed / total` , decomposed into` f2p` (fail-to-pass: hidden tests verifying the task) and` p2p` (pass-to-pass: pre-existing tests that should still pass).
- **Delta** : fixer score minus impl score. Positive = lift, negative = regression.
- **Statistical test** : Wilcoxon signed-rank.


### Fixer prompt templates


```text
markdown
```


#### Instruction variants


- Baseline:` Fix the issues identified in the review comments.`
- A softer variant:` Address the issues identified in the review comments when you are confident the fix is correct.`
- A softer variant:` Review the issues. For each, decide whether you can fix it safely without breaking existing behavior. Skip any issue where you are not confident.`


## References


1. [Vet](https://github.com/imbue-ai/vet) is an open-source review agent. It supports usage via the terminal, GitHub PR actions, and as an agent skill. For the purpose of our experiments, Vet was configured as a CLI tool.[↩](https://imbue.com/research/2026-04-29-how-ai-code-review-can-make-correct-code-worse/#user-content-fnref-1)
