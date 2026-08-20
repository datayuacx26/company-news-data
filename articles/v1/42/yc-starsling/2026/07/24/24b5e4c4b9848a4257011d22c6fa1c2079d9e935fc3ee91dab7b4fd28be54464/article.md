---
schema_version: "1.0.0"
document_id: "24b5e4c4b9848a4257011d22c6fa1c2079d9e935fc3ee91dab7b4fd28be54464"
company_key: "yc-starsling"
company: "StarSling"
source_id: "yc-starsling-atom-ebb22850b8ad"
canonical_url: "https://starsling.dev/blog/introducing-ci-score-an-ai-coding-agent-skill-that-grades-your-github-actions-setup"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T21:56:01.282477+00:00"
fetched_at: "2026-07-31T21:56:02.525587+00:00"
content_hash: "sha256:80601eec195a63a3251cbc56318f0742d100bcfb4d02c211939896e0c6d0987a"
---

# Introducing ci-score: an AI coding agent skill that grades your GitHub Actions setup

Today we're releasing[ci-score](https://starsling.dev/ci-score) , a free and[open-source](https://github.com/starslingdev/skills) agent skill that grades your GitHub Actions configuration against a published rubric of best practices and hands your coding agent a ranked fix for every gap it finds. It works with all[73 coding agents currently supported by Vercel's Skills CLI](https://github.com/vercel-labs/skills#supported-agents) , including OpenCode, Claude Code, Codex, and Cursor.


Point it at a local checkout and it reads your workflow files, checks eleven configuration facts, and writes a report with your score, the evidence behind every check, and a copyable prompt for each fix. It runs entirely on your machine. No network calls, no telemetry, nothing sent to StarSling.


## What ci-score does


It works in four steps:


- **Reads your workflows.** It opens the Actions workflow files in a clean checkout and extracts the configuration facts the rubric asks about. No API calls, no credentials, no run history required.
- **Checks[eleven practices](https://starsling.dev/ci-score#best-practices) .** Each one is pass, fail, or not applicable, across caching, checkout depth, build reuse, parallelism, trigger scope, gate hygiene, and the two security practices in scope. The full list is below.
- **Scores what applies.** Your score is passed checks over applicable checks, rounded. A check that cannot apply to your repo leaves the denominator rather than counting against you.
- **Ranks the fixes.** Every failed check ends in a ready-to-paste prompt carrying the fact that failed, where in your YAML it failed, and what to change. Your agent writes the diff for you to review.


You review the diff. Nothing merges that you did not approve.


## It reads facts, it does not guess


Every check is a configuration fact: a thing that is present or absent in your own files. There is no LLM in the scoring path, and no measured magnitude gates a score. That constraint is deliberate, and we got it wrong first. An earlier rubric deducted points based on measured thresholds, which produced wrongful deductions faster than review could remove them, and a score a maintainer cannot debug fails both of us. So the rule now is that you must be able to verify any check against your own YAML in under a minute.


The same principle governs what happens when the rubric does not apply. Rather than publish a confident-looking number from thin evidence, ci-score refuses to score at all in three cases: when a repo has no workflow files, when a scan predates the current rubric, and when the visible workflows are only automation. That last one exists because a repository whose workflows are all release and triage bots once scored a perfectly real-looking number off machinery that was never its CI.


## The eleven checks


Several have a full guide on this site explaining the fix, with good and bad YAML side by side:


- [Cache dependencies in GitHub Actions](https://starsling.dev/best-practices/github-actions/cache-dependencies)
- [Set actions/checkout fetch-depth: 0 vs 1 vs 2](https://starsling.dev/best-practices/github-actions/shallow-checkout)
- [Shard tests across parallel jobs](https://starsling.dev/best-practices/github-actions/shard-tests)
- [Build and test only what changed](https://starsling.dev/best-practices/github-actions/build-only-affected)
- [Cut CI queue time](https://starsling.dev/best-practices/github-actions/cut-queue-time)
- [Cancel superseded runs](https://starsling.dev/best-practices/github-actions/cancel-superseded-runs)
- [Filter workflows with paths and paths-ignore](https://starsling.dev/best-practices/github-actions/path-filter-workflows)
- [Scope id-token: write](https://starsling.dev/best-practices/github-actions/scope-id-token-per-job)
- [Pin GitHub Actions to a commit SHA](https://starsling.dev/best-practices/github-actions/pin-action-shas)


The[methodology page](https://starsling.dev/ci-score/methodology) publishes all eleven with the exact fact each one asks about, the formula, and the cases where the skill refuses to score at all.


## What the score is not


It is not a speed measurement, and it does not predict how fast your CI runs. A repo can follow every practice here and still be slow because one test suite is enormous, and a repo can score badly and still merge quickly. Configuration adherence and wall-clock time are different questions, which is why they are different tools: if wall-clock time is your question,[ci-speedup](https://starsling.dev/ci-speedup) answers it by sampling your real runs.


## Real repositories, really scored


We ran ci-score against a set of well-known open-source repositories and published every result, including the ones that are not flattering:


Every one of those numbers is committed to this site as data and re-derived in our CI from the same scorer the skill runs, so the board cannot drift from what the tool would tell you. See the full[leaderboard](https://starsling.dev/ci-score) .


## Try it


Install the skill and run it in any repo:


```text
npx   skills   add   starslingdev/skills
```


Then invoke` /ci-score` in your agent. Prefer not to install it? Paste this into your coding agent instead and it will fetch and run the skill for you:


```text
Run `npx skills use "https://github.com/starslingdev/skills" --skill "ci-score"` and follow the generated skill instructions now.
```


It writes` ci-score-report.md` in your repo and nothing else. It is open source under the MIT license. Read[the skill](https://github.com/starslingdev/skills/tree/main/skills/ci-score) in the repo, or see how it scores on the[ci-score page](https://starsling.dev/ci-score) .


## Let StarSling run this for you automatically


The skill is you checking the configuration once, with your own agent. Configuration drifts, though: a new workflow lands without a cache, a checkout quietly goes back to full history, an action gets pinned to a moving tag. On paid plans, once optimization PRs are enabled,[StarSling Runners](https://starsling.dev/) keep these practices in place as your workflows change, and open every fix as a PR your team reviews and merges.


If your team runs CI on GitHub Actions,[install the StarSling GitHub App](https://github.com/apps/starslingdev) to keep it in shape automatically.


Happy Slinging 💫


Yonas
