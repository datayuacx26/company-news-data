---
schema_version: "1.0.0"
document_id: "7a939275d5580308d9a4d78f3648b4ae5294a626424db258bb310e6201973b43"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/ai-code-review-benchmarks"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:ea34da8802146c8d9db02536c4cd9557af8f697736e4c3f2e491c11b899a3d08"
---

# Every AI code review vendor benchmarks itself, and wins

Last updated on Feb 26, 2026


Several AI code review tools have published benchmarks. The problem: there's no SWE-bench for code review. No shared yardstick. Each vendor runs their own benchmark, on their own dataset, and wins.


We have skin in this game (we recently published[our own benchmarks](https://deepsource.com/benchmarks) ), so take everything here with that in mind. We'll call out our own limitations too.


## Why benchmarks matter


SWE-bench gave coding agents a shared yardstick.[Flawed](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) , but shared. Teams could compare Devin, SWE-Agent, and OpenHands against the same tasks, same criteria. That alone moved the conversation from "trust our demo" to "show your numbers."


AI code review has no equivalent. You can't compare one vendor's F1 score against another's recall rate when they're measuring different things on different data. So engineering leaders end up buying based on demos and gut feel.


## Why code quality benchmarks are the hard problem


Security has relatively clear ground truth: a CVE either exists or it doesn't. Code quality is much harder. What counts as a "bug risk" versus acceptable code? Is a missing null check a bug or an intentional design choice? Reasonable experts disagree on these questions daily in code reviews.


There's no CVE database for code quality. Building ground truth requires expert annotators across multiple languages who agree on issue categories. You need principled ways to handle disagreement, and thousands of labeled examples for statistical significance.


We've explored automated approaches, using the GitHub API to construct ground truth from historical reviews and bug-fix patterns. The signal is too noisy. Reviewers disagree with each other, teams have different standards, and what one codebase considers critical another treats as acceptable.


Manual dataset preparation is necessary, and that's why nobody has built a credible code quality benchmark yet. The closest analogues are linter rule sets (ESLint, Pylint), but those define what to detect, not a test set to evaluate detection.


## The vendor benchmarks


We surveyed the public benchmarks published by AI code review vendors as of February 2026.


### Greptile


Greptile published one of the earlier vendor benchmarks:[50 PRs from 5 repositories](https://www.greptile.com/benchmarks) . The bugs were reconstructed from real bug-fix commits, which is better than synthetic injection. But 50 PRs across 5 repos is a small dataset, and each PR is reduced to a single known bug. The benchmark measures whether a tool can find one pre-selected issue, not whether it surfaces the full range of problems in a real review.


All 5 repos are large, well-known open-source projects with clean commit histories. There's no stated rationale for why these repos were chosen over other candidates. With 50 data points, a handful of edge cases can swing aggregate results significantly.


Greptile reported 82% recall, with a 24-point gap over the next best tool. On 50 data points, that gap has wide confidence intervals. Greptile designed the benchmark, selected the repos, and ran the evaluation.


Later, Augment Code[ran their own evaluation](https://www.augmentcode.com/blog/we-benchmarked-7-ai-code-review-tools-on-real-world-prs-here-are-the-results) on the exact same 5 repos. Greptile scored 45%, not 82%.


Same repos, wildly different results depending on who runs the benchmark.


### Qodo


Qodo published a["real-world benchmark"](https://www.qodo.ai/blog/how-we-built-a-real-world-benchmark-for-ai-code-review/) covering 100 PRs and 580 issues across 8 repositories, though the bugs are synthetic rather than real.


Qodo collected PRs from open-source repos via the GitHub API, then used an LLM to inject violations and bugs into them. The bugs tools are evaluated against aren't bugs that humans wrote and shipped. They're bugs an LLM invented and inserted. The ground truth is LLM-generated, the evaluation uses LLM-as-a-judge, and there's no evidence that synthetic bugs match the distribution or subtlety of real defects.


The dataset is a[mapping repository](https://github.com/agentic-review-benchmarks/benchmark-pr-mapping) that cross-references PRs to each tool's review, but the ground truth (the injected bugs tools are actually scored against) isn't published. Without it, nobody can reproduce the scoring. Qodo designed and ran the benchmark, reporting 60.1% F1.


### Other vendors


Augment Code published a benchmark using[50 PRs from the same 5 repos as Greptile](https://www.augmentcode.com/blog/we-benchmarked-7-ai-code-review-tools-on-real-world-prs-here-are-the-results) . They described their dataset as "expanded and corrected" from Greptile's original. Augment reported 59% F1.


Propel used Augment's dataset as an["externally-authored" evaluation framework](https://www.propelcode.ai/benchmarks) . Propel reported 64% F1.


Macroscope published a[benchmark covering 118 bugs across 45 repos and 8 languages](https://macroscope.com/blog/code-review-benchmark) , a broader dataset in terms of breadth and diversity. Macroscope reported 48% detection rate.


Entelligence published a[benchmark covering 67 bugs across the same 5 repos as Greptile](https://entelligence.ai/code-review-benchmark-2026) , testing 8 tools. Ground truth was built by having three frontier LLMs independently generate review comments, filtering by majority vote, then validating with in-house reviewers. Entelligence reported 47.2% F1.


Other tools (CodeRabbit, Devin, Cursor BugBot) haven't published benchmarks, and every vendor that has published one wins their own.


### The self-evaluation problem


This isn't necessarily deliberate. When you design a benchmark, you make dozens of small decisions: repo selection, what counts as a "real" bug, how to score partial matches. Designers naturally understand their own criteria and optimize for them, consciously or not. The Greptile/Augment case makes this concrete: same repos, different evaluation rules, completely different rankings.


Pharmaceutical companies don't design and run their own clinical trials. Self-evaluation is biased, even in good faith. Independent evaluation, published datasets, reproducible methodology. None of that exists for AI code review yet.


## What makes a benchmark trustworthy


- Use real ground truth. Bugs from CVE databases, bug-fix commits, or human-validated defects, not synthetic ones. If the ground truth was generated by a model, it's unclear what the benchmark is actually measuring.
- Publish downloadable datasets. The minimum bar is a centralized, versioned artifact anyone can download and run.
- Blind the evaluation. The judge, human or LLM, shouldn't see which tool produced which results. If the evaluator knows which output belongs to the benchmark designer's tool, bias is inevitable.
- Run it independently. Ideally, someone with no financial interest runs the benchmark. If self-evaluated, publish raw outputs, judge prompts, scoring logic, and individual verdicts.
- Get a real sample size. 50 PRs is not enough. Statistical noise dominates and a few edge cases can swing scores by 10+ points. Anything under 100 entries should be treated with skepticism.


## Our own benchmarks, and their limits


We'd be hypocrites if we didn't apply this framework to[our own benchmarks](https://deepsource.com/benchmarks) .


We used 165 real CVEs from the[OpenSSF dataset](https://github.com/ossf-cve-benchmark/ossf-cve-benchmark) , not synthetic injections. The broader dataset is larger, but we filtered to entries where:


- Both pre-patch and post-patch commits exist, so we can actually construct the vulnerable and fixed versions of the code.
- The` weaknesses` field isn't empty, since we need CWE identifiers to know what the ground truth is.
- Affected files are under 1,000 lines, to keep execution time practical across 165 entries.


A handful of entries got cut because the diff couldn't be reconstructed, probably force-pushed commits that wiped the original history. We'd rather have 165 solid entries than 300 questionable ones.


All judged results are published as JSONL in our[benchmarks repo](https://github.com/DeepSourceCorp/benchmarks) , and our LLM judge (Claude Opus 4.5) doesn't see tool names, so it can't play favorites.


Where we fall short: we ran the benchmark ourselves. The data is published for anyone to verify, but no third party has done so yet. 165 entries is more than most vendor benchmarks, but it's still not large. And we only cover security. No code quality benchmark yet. The page says "Security Benchmarks," not "AI Code Review Benchmarks," because that distinction matters.


## What's missing


AI code review needs what SWE-bench built for coding agents: a community-maintained benchmark no single vendor controls. The[OpenSSF CVE dataset](https://github.com/ossf-cve-benchmark/ossf-cve-benchmark) is the closest shared foundation for security. For code quality, the work hasn't been done yet, and no single vendor has the incentive or credibility to do it alone.


Until then, treat all vendor benchmarks, including ours, with skepticism. Look for published data, reproducible methodology, and honest scope claims. Be especially wary of any benchmark where the designer also happens to win.
