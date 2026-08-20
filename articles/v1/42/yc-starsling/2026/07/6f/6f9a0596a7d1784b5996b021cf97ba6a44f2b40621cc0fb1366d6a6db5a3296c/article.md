---
schema_version: "1.0.0"
document_id: "6f9a0596a7d1784b5996b021cf97ba6a44f2b40621cc0fb1366d6a6db5a3296c"
company_key: "yc-starsling"
company: "StarSling"
source_id: "yc-starsling-atom-ebb22850b8ad"
canonical_url: "https://starsling.dev/blog/introducing-ci-speedup-an-ai-coding-agent-skill-that-analyzes-and-fixes-your-slow-github-actions"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-22T08:43:31.756578+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:6195b795273669004bb904810e59f1a01dde72ec514cc3e96fbcf09631f7e44c"
---

# Introducing ci-speedup: an AI coding agent skill that analyzes and fixes your slow GitHub Actions

Today we're releasing[ci-speedup](https://starsling.dev/ci-speedup) , a free and[open-source](https://github.com/starslingdev/skills) agent skill that finds the one thing actually slowing your pull requests down and gives your coding agent the evidence-backed prompt to fix it. It works with all[73 coding agents currently supported by Vercel's Skills CLI](https://github.com/vercel-labs/skills#supported-agents) , including OpenCode, Claude Code, Codex, and Cursor.


Point it at a repo and it reads your real GitHub Actions runs, works out which required check sets how long a PR waits to go green, and produces a ready-to-paste prompt for your coding agent to make that check faster. It runs on your machine, through your own GitHub CLI. Nothing is sent to StarSling.


## What ci-speedup does


It works in four steps:


- **Reads your workflows and code.** It opens your Actions workflow files and the code they build and test, so it knows what your CI is supposed to do and where the time should be going.
- **Samples your real runs.** Through your own` gh` CLI it pulls recent runs, job timings, and logs. Real numbers from your repo, on your machine.
- **Finds the long pole.** It reconstructs which checks actually block a merge and which single one sets the total wait, then digs into that check step by step for the root cause.
- **Hands your agent the fix.** Every finding ends in a ready-to-paste prompt carrying the root cause, the log evidence, and the measured cost. Your agent checks the code and its git history first, then writes the change for you to review.


You review the diff. Nothing merges that you did not approve.


## It measures, it does not guess


Detection, ranking, critical-path analysis, and every measured number are deterministic and reproducible. There is no LLM in the scoring path. An LLM steps in only when a blocking log matches no catalog detector, where it produces a clearly labeled, log-grounded root-cause lead. Your coding agent then uses that evidence, the surrounding code, and the file's history to reason about and draft the safe fix.


There are[73 patterns](https://github.com/starslingdev/skills/blob/main/skills/ci-speedup/references/optimization-patterns.md) in the catalog, from missing caches and unsharded test suites to full-history checkouts and sleep-based readiness waits. But ci-speedup does not lint for all of them and hand you a wall of warnings. It ranks by measured wall-clock lever and elevates only the causes your run data supports, so the report is a short, ordered list of what will actually move your merge time.


## The same fixes we have already shipped


The fixes ci-speedup surfaces are the ones our own agents have been shipping to open-source maintainers:


- [Mastra](https://starsling.dev/customers/mastra) had one unsharded end-to-end suite gating every PR. The fix sharded it across three parallel Playwright jobs ([mastra-ai/mastra#15888](https://github.com/mastra-ai/mastra/pull/15888) ).
- [Better Auth](https://starsling.dev/customers/better-auth) reinstalled Playwright browsers on every run. The fix moved them into a cached composite action ([better-auth/better-auth#8073](https://github.com/better-auth/better-auth/pull/8073) ).


Both are exactly the kind of long-pole fix ci-speedup finds and writes for you.


## Try it


Install the skill and run it in any repo:


```text
npx   skills   add   starslingdev/skills
```


Then invoke` /ci-speedup` in your agent. Prefer not to install it? Paste this into your coding agent instead and it will fetch and run the skill for you:


```text
Run `npx skills use "https://github.com/starslingdev/skills" --skill "ci-speedup"` and follow the generated skill instructions now.
```


It is open source under the MIT license. Read[the skill](https://github.com/starslingdev/skills/tree/main/skills/ci-speedup) in the repo, or watch it work on the[ci-speedup page](https://starsling.dev/ci-speedup) .


## Let StarSling run this for you automatically


The skill is you running the fix once, with your own agent. CI drifts, though: caches go stale, a new suite lands unsharded, a checkout quietly goes back to full history. On paid plans, once optimization PRs are enabled,[StarSling Runners](https://starsling.dev/) do this continuously, keeping the long pole short as your workflows change, and opening every optimization as a PR your team reviews and merges.


If your team runs CI on GitHub Actions,[install the StarSling GitHub App](https://github.com/apps/starslingdev) to keep it fast automatically.


Happy Slinging 💫


Yonas
