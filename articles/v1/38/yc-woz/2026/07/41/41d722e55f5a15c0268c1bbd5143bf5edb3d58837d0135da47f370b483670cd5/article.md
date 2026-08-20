---
schema_version: "1.0.0"
document_id: "41d722e55f5a15c0268c1bbd5143bf5edb3d58837d0135da47f370b483670cd5"
company_key: "yc-woz"
company: "Woz"
source_id: "yc-woz-news-import-ac8bcbd945f9"
canonical_url: "https://wozcode.com/blog/opus-4-7-pricing"
published_at: null
first_seen_at: "2026-07-24T07:30:19.491643+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:feba73188224f824c7f6223d638c0894e5fa549334c81e17824da6aece612e3f"
---

# As Opus gets smarter, WOZCODE's edge gets bigger

[← Blog](https://www.wozcode.com/blog)


Opus 4.6 vs. 4.7


April 28, 2026


· 5 min read


# As Opus gets smarter, WOZCODE's edge gets bigger


Vanilla Opus 4.7 costs **80% more than 4.6** on Claude Code's default settings. With WOZCODE installed, the price only increased 12%. Savings widened from **−41% to −63%** .


## The expectation


The intuition we kept hearing was: "once Claude is smart enough, why do I need a token-saving plugin?" Smarter model picks better tools, takes fewer detours, writes less filler. WOZCODE's edge should compress over time.


It's a reasonable hypothesis. We expected the same thing internally, and treated each Opus release as a stress test for whether the plugin still earned its keep. So when 4.7 dropped this week we re-ran our standard code-editing benchmark on both 4.6 and 4.7, vanilla Claude Code vs Claude Code + WOZCODE.


## The result


Same TypeScript codebase, same prompts, same` leave-defaults` preset (the default config Anthropic shipped with 4.7), just two variables: model (4.6 / 4.7) and plugin (vanilla / WOZCODE). The prompts cover real refactor work: fix a 500 error, split an oversized service class, add JWT typing, set up Jest, and so on.


Here's the four-cell matrix:


Model Vanilla Claude Code + WOZCODE WOZCODE savings


Opus 4.6 $11.62 $6.88 −40.8%


Opus 4.7 $20.92 $7.73 −63.1%


And it's faster, too. WOZCODE clears the suite in less wall-clock time on both models, and the gap widens on 4.7 the same way the cost gap does:


Model Vanilla Claude Code + WOZCODE Time saved


Opus 4.6 28m 31s 24m 43s −13.3%


Opus 4.7 35m 2s 26m 21s −24.8%


Two things jumped out.


**Vanilla cost nearly doubled.** 4.6 to 4.7 went from $11.62 to $20.92 on the same prompts. Per-turn cost rose from roughly $0.05 on 4.6 to $0.13 on 4.7, a 2.6x jump. That's the tokenizer +` xhigh` effort change Anthropic flagged in the 4.7 launch announcement, fully visible at the default config most users will run.


**WOZCODE barely moved.** $6.88 on 4.6, $7.73 on 4.7, a 12% increase. The relative gap between vanilla and WOZCODE went from $4.74 to $13.19 per run. Quality stayed within noise across all four cells.


WOZCODE savings on 4.6


−41%


$6.88 vs $11.62


WOZCODE savings on 4.7


−63%


$7.73 vs $20.92


Vanilla 4.6 → 4.7


+80%


$11.62 → $20.92


## Why a smarter model amplifies WOZCODE


WOZCODE replaces a handful of Claude Code's built-in tools (Read, Grep, Glob, Edit, NotebookEdit) with versions designed to do more work per call:


- **Combined search + read.** One call discovers files *and* returns matched content, instead of grep → read → read.
- **Batched edits.** One call applies many edits across many files, so you stop paying turn-tax on every individual change.
- **AST-aware truncation.** Big TS/JS files come back as signatures during exploration; switch to full content only when you've narrowed down.
- **Live SQL introspection.** One` Sql` tool runs queries against connected databases directly: schema discovery, foreign-key graph, real query results. The vanilla pattern is psql-via-Bash plus several rounds of context-padded output; WOZCODE collapses it to a single call.
- **Fuzzy edit matching.** Levenshtein-based matching tolerates indentation and whitespace drift, so edits land on the first try instead of bouncing back with "old_string not found" errors that burn another turn.


Every one of these tools rewards a planner that thinks *before* acting. Batched edits only save turns if the model can plan ten edits at once instead of stumbling into them one at a time. Combined search + read only helps if the model knows what it's looking for before it asks. Subagent delegation only works if the model recognizes which subtasks are deterministic enough to hand off.


That's exactly the muscle that gets stronger with each Opus release. 4.7 is meaningfully better than 4.6 at recognizing when a single batched call replaces a sequence of small ones, when a signature view is enough vs when to pull full file bodies, when to dispatch a Haiku subagent vs do it inline. WOZCODE on 4.6 took 128 turns to clear the suite. WOZCODE on 4.7 took 52. Same tools, less than half the round trips.


Vanilla Claude Code didn't get that benefit, because the tools it has are turn-by-turn primitives. There's no "plan ten edits" call to plan into. The smarter planner has nothing larger to delegate to, so it just spends more thinking tokens per turn at the new` xhigh` default. That's where the 80% cost increase comes from: 4.7 isn't doing more turns than 4.6, it's doing denser, more expensive turns.


## What about the price?


Anthropic kept Opus 4.7's per-token price identical to 4.6 ($5/$15 per million in/out), but flagged two reasons real spend goes up:


> **Tokenizer update.** The same input may produce more tokens.
>
>
> **Effort levels.** The model tends to think more on harder problems, producing more output tokens at higher effort.
>
>
> At` xhigh` effort (the new Claude Code default), Opus 4.7 can cost roughly **20–30% more** than Opus 4.6 at` max` effort.
>
>
> — Anthropic, Opus 4.7 launch announcement


Our measured number on vanilla Claude Code at` leave-defaults` is +80%, well past Anthropic's stated range. Some of that is workload-specific (these prompts trigger a lot of cross-file reasoning, which 4.7 spends more output tokens on), but the direction is clear: at the default config Anthropic shipped, the price hike is real and bigger than the headline number suggests.


The absolute dollar gap WOZCODE opens up is now $13.19 per run. Run the suite five times in a workweek and that's $66 saved on this one workload, on one developer.


## What this means going forward


The headline finding isn't really about 4.7. It's about the trajectory.


WOZCODE's edge widened by 22 points moving from 4.6 to 4.7. The safe assumption for 4.8, 5.0, and beyond is that the gap keeps widening, not closing. Better planners get more out of better tools. Vanilla Claude Code's tools don't change shape with the model; WOZCODE's do. That asymmetry compounds.


**If you're on Claude Code's flat plan** (Pro / Max / Team), the dollar bill doesn't change but your usage cap fills up faster on 4.7. WOZCODE finishes the same suite in 52 turns vs vanilla's 161, so you burn roughly a third of the budget per task.


**If you're paying per-token** (API or pass-through), upgrading from 4.6 to 4.7 adds about $0.85 per run with WOZCODE installed ($6.88 → $7.73). Without WOZCODE, the same upgrade adds $9.30 per run ($11.62 → $20.92). Installing the plugin and upgrading the model in the same week leaves you ahead on every dimension.


Either way: 4.7 is the smartest Opus yet, and you should use it. Just don't pay full price.


### Cut your Opus 4.7 bill nearly in half


Install WOZCODE in under a minute. No code leaves your machine, no config to manage.


[Install WOZCODE →](https://www.wozcode.com/docs)


Benchmark methodology: identical TypeScript codebase, both runs on April 28, 2026. The` leave-defaults` preset means the runner doesn't override Claude Code's effort or thinking settings, so each model runs with whatever Claude Code ships as its default. For 4.7 that's the new` xhigh` effort level; for 4.6 it's the prior default. This is what most users will see out of the box. Per-prompt breakdowns and raw run logs available on request.
