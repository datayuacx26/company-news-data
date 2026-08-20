---
schema_version: "1.0.0"
document_id: "13c2f3c01dcd2455b3973a6ac89bbe4ba107e57bde0d8e0d9ded1fd2040d99bb"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/managed-research"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-22T15:27:15.216799+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:2c06691248188d20d61caaddcaa38847a997eed63a115919a42fc802937ef208"
---

# Managed Research | Synth Blog

## What Managed Research Is


Managed Research runs repeatable, inspectable research workflows against real repos. You give it a project — code, notes, credentials, research goals — and it works through the task, keeps you in the loop via a shared message queue, and hands back a structured artifact bundle you can inspect, diff, and continue from.


## The Interface: Message Queue


The core primitive is a **message queue** shared between you, the orchestrating agent, and any sub-agents it spawns. Every question the agent needs answered, every approval it requests, every status update it emits goes through this queue. You can watch it live or review it afterward. You can post a steering message at any point during a run and the agent will pick it up at its next turn.


This is the human/agent/agent interface for the whole system. It is not a chat thread. It is a structured, ordered log of intents and responses that persists across the full run.


### What You Upload


Input What it is


Repo URL or local path The codebase the agent works against


Notes Free-text context: goals, constraints, prior findings, things to avoid


Credentials API keys, provider bindings — injected into the agent's environment at runtime


Files Any additional context files (specs, logs, previous run artifacts)


### What You Get Back


Output What it is


Pull request A real PR against the target repo if the run produces code changes


Artifact bundle Reports, eval summaries, scored outputs, run metadata


Message log Full record of every queue event during the run


Usage Plan allowance, flex-credit drawdown, and charged usage


Run state Structured result manifest you can query via SDK or MCP


### Agent Tools Today


The agent has two tool categories at launch:


- **Tinker** — training-oriented compute: SFT, DPO, RLVR, FBC. Used when the task involves improving a model, not just running code.
- **OpenRouter** — model inference across providers. Used for evals, comparisons, and any task that needs model output without training.


## Runbooks


Managed Research launches with two runbooks.` lite` is the default low-overhead collaboration posture for focused tasks.` heavy` is the higher-coordination posture for longer, multi-actor research pushes.


## Evals


On ReportBench, the` lite` runbook scores 1.00 on the launch spine (` readme_smoke` ) and reaches **+0.507** uplift on a Tinker-gold RLVR transfer task. We are not claiming` lite` beats Codex or opencode: on the launch spine the standalone comparison is not meaningful, because the task requires the SMR runtime and standalones fall back to packaging prior artifacts; on the +0.507 task the standalone baseline is still pending a runtime fix. Where a clean head-to-head exists today, SMR and Codex standalone are comparable. Full methodology, per-task results, token spend, and standalone baselines are in the system card.


## How to Use Managed Research


Three surfaces. Pick the one that fits your workflow. All use the same **` synth-ai\[research\]`** install — there is not a separate Managed Research package.


### MCP


Add the hosted Managed Research MCP server to your Claude, Cursor, or Codex setup. You get tools for launching runs, posting queue messages, checking run state, and pulling artifacts — all from inside your editor.


bash


```text
uv   add   "synth-ai[research]"
export   SYNTH_API_KEY  =  "sk_..."
codex   mcp   add   managed-research   --url   https://api.usesynth.ai/mcp
```


For local stdio:


bash


```text
uv   tool   install   "synth-ai[research]"
synth-ai-managed-research-mcp
```


Add it to your Claude Desktop config:


json


```text
{
"mcpServers"  : {
"managed-research"  : {
"command"  :   "synth-ai-managed-research-mcp"  ,
"env"  : {
"SYNTH_API_KEY"  :   "your-key-here"
}
}
}
}
```


The MCP surface exposes the same run lifecycle the frontend and SDK do — 222 launch-target tools across projects, runs, artifacts, checkpoints, approvals, GitHub setup, and project economics. You can launch a run from Claude, post to the message queue, and pull artifacts when it completes.


### Python SDK


python


```text
from   synth_ai   import   SynthClient


client   =   SynthClient()
research   =   client.research


run   =   research.runs.start(
"Improve the eval harness for the banking77 transfer task."  ,
work_mode  =  "directed_effort"  ,
providers  =  [{  "provider"  :   "openrouter"  }],
runbook  =  "lite"  ,
)


result   =   run.wait(  timeout  =  60   *   60  ,   poll_interval  =  15  )
print  (result.public_state)
print  (run.report_text())
```


Full SDK reference:[docs.usesynth.ai/managed-research/sdk](https://docs.usesynth.ai/managed-research/sdk)


### Frontend


The Managed Research frontend gives you a project dashboard, a live message queue view, run history, artifact inspector, and usage breakdown. No code required to launch a run or review what the agent did.


[Apply for Managed Research Beta Access →](https://www.usesynth.ai/signup?product=managed-research&utm_source=blog&utm_campaign=smr-launch)


[Read the Docs](https://docs.usesynth.ai/managed-research/quickstart)


## Plans and Beta Access


Managed Research uses the same plan and credit model across standalone SMR and Managed Factory. Free, Standard ($20/month), and Max ($200/month) include org-level premium and value usage windows with visible reset times. Flex credits are used after included windows are exhausted, and manual promo or make-good grants are explicit audit events rather than automatic resets. Premium models consume allowance faster; value models stretch the same allowance further. Beta Access remains an overlay on top of the plan when it unlocks experimental work modes such as` open_ended_discovery` ,` heavy` , and all-model access.


[Apply for the Free Beta →](https://www.usesynth.ai/signup?product=managed-research&utm_source=blog&utm_campaign=smr-launch)


---


## NanoHorizon: The Launch Walkthrough


The candidate was` 22b80c9549` , nicknamed **Dispatch Fix E2E** . Its hypothesis was narrow: preserve a compact todo scratchpad and end-to-end dispatch rule inside the prompt-opt lane so the agent keeps the current highest-priority subgoal coherent across a short Craftax action batch. The point was not to rewrite the whole harness. The point was to make one reviewable change, run it honestly, and keep the result inspectable.


### Run Snapshot


Field Value


Candidate 22b80c9549 — **Dispatch Fix E2E**


Managed Research project 4479f3a0-2342-4098-9682-65a8ac2db4a4


Managed Research run 137f0a5b-a1c2-412d-a28c-870fed26db45


Pull request[synth-laboratories/nanohorizon#6 — Preserve Craftax todo dispatch contract](https://github.com/synth-laboratories/nanohorizon/pull/6)


PR scope 278 additions, 28 deletions, 37 files, head 2b8671e


Final score **1.25** mean unique Craftax achievements per rollout


Evaluation size 20 rollouts, 0 eval errors


Successful rollouts 14 / 20 rollouts achieved at least one reward-bearing event


Most frequent achievements collect_sapling 55%, place_plant 55%, collect_wood 15%


Artifacts replay video, eval_summary.json, scored checkout, public PR diff


### Before, After, Baseline


Run What it proved Score Rollouts with achievements Eval errors


Broken pre-launch attempt 44715c7533 The idea was not enough if the end-to-end path was still broken 0.00 0 / 20 20 / 20


Launch run 22b80c9549 The repaired Managed Research loop can take a candidate to PR, score it, and publish inspectable evidence 1.25 14 / 20 0 / 20


Current reference baseline gpt-4-1-nano-baseline Useful quality bar for the same evaluation surface 1.25 15 / 20 0 / 20


**Honest read**


The NanoHorizon launch run did *not* beat the current baseline. What it did prove was that the Managed Research loop is real: a broken attempt became a clean scored run with a reviewable PR, durable artifacts, and enough evidence to reason about what to try next.


### What The Managed Research Experiment Actually Changed


Surface Change Why it mattered


Prompt-opt source contract Extended the shared todo scratchpad requirements to preserve the compact scratchpad wording and the first-item dispatch rule This kept reflection and rollout feedback aligned with the candidate instead of letting the contract drift


Candidate config Added a narrow prompt-opt config for the dispatch-fix experiment The change stayed small and measurable instead of turning into a broad harness rewrite


Record bundle Packaged the matching command, metadata, notes, metrics, and run config The experiment stayed reproducible after the run ended


Verification Targeted validation passed on the candidate-specific prompt-opt surfaces We had evidence the candidate was structurally coherent before scoring it


Protected Craftax harness Left the shared runtime and eval harness files alone The run tested a scoped intervention rather than muddying the result with a larger systems change


The PR reflects that scope. It stayed in the prompt-opt lane and candidate packaging rather than editing the protected Craftax runtime surfaces directly.


### What Improved


- The pipeline stopped failing catastrophically. The pre-launch attempt ended with` 20 / 20` eval errors; the launch run completed all` 20` rollouts cleanly.
- The agent produced a real PR against a public repo instead of stopping at an internal note or a local patch.
- The candidate reliably reached early-game progress states, especially` collect_sapling` and` place_plant` .
- The output bundle was durable enough to inspect afterward: video, summary JSON, diff, repo state, and run metadata.


### What Did Not Improve


- The run matched the current baseline instead of beating it.
- Mid- and late-game Craftax progress stayed flat. Mining, crafting, dungeon entry, and later combat achievements remained at zero in this evaluation.
- The locally cached` diff.patch` artifact was empty even though the upstream PR diff is intact.
- Some internal seams between run-level and submission-level state are still rougher than we want.


### Why This Matters


The important thing here is not that we found a miraculous one-shot improvement. The important thing is that we now have a real, inspectable loop:


1. propose a candidate
2. run it through Managed Research
3. get a real repo change
4. score it on the benchmark surface
5. inspect the artifacts and decide what to try next


That is the difference between "an agent did something interesting once" and "we can run research against this surface on purpose."


## ProjectBench: NanoHorizon


NanoHorizon is the first live project in ProjectBench, our long-horizon benchmark that scores multi-milestone research campaigns end-to-end rather than one task at a time. Each campaign runs through a fixed milestone sequence — scope, GEPA seed, SFT uplift, PivotRL, RLVR refinement, final package — and is scored on milestone completion, trajectory coherence, and final artifact quality together. NanoResearch is the public face: anyone can submit a candidate, run it through Managed Research, and see where it lands.


The results below are pulled live from NanoResearch. Every scored run — including the one described above — is visible there with its PR, replay video, and full artifact bundle.


[View full leaderboard ↗](https://usesynth.ai/nanoresearch)


Live results from NanoResearch — start your own run and compare.


The top fully agent-authored submission on the board today is the same run described above: candidate 22b80c9549 — *Dispatch Fix E2E* , PR #6, score` 1.25` . We are naming that explicitly so the provenance is faithful: the hypothesis, the code change, and the PR all came from the Managed Research loop, not a human-authored strategy the agent executed. Pushing past that row with more agent-authored candidates is follow-on work we are planning, not a claim we are making at launch.


## ProjectBench: RuneBench Hillclimbing


The second live ProjectBench project is a hillclimbing campaign on RuneBench — an AI agent benchmark where agents write TypeScript snippets against a headless RuneScape server at 8x speed and are scored on peak XP rate (best XP/min in any 15-second window).


The campaign question: can Managed Research take a Gemini 2.5 Pro baseline on RuneBench and improve it using only prompt, tool description, and wiki context changes — no SFT, no RL, no model weight updates?


The three-milestone arc is: establish a clean Gemini baseline on representative skill tasks → hillclimb one change at a time (system prompt strategy cues, wiki context filtering, tool description tightening, planning step prefix) accepting changes that move peak XP by ≥5% → run the improved config head-to-head against baseline on held-out tasks and report percentage lift honestly.


This is deliberately the kind of improvement loop most teams already do informally. The ProjectBench version makes each iteration scored and artifact-backed: change log, before/after XP numbers, and an explicit rejected-change list so the final comparison report reflects what actually happened rather than what was hoped.


The RuneBench campaign is follow-on work for the next proof packet; we are not using it as a launch claim.


## Launch Scorecard


The launch is not resting on one appealing screenshot or one internal benchmark row. At a glance, the current public-safe evidence stack looks like this:


Surface Headline claim Evidence quality Status


NanoResearch / NanoHorizon real candidate → PR → scored artifact bundle public end-to-end ready


External control surfaces MCP, SDK, docs quickstart, and launch-facing quality checks stayed in scope external surface readiness ready with limits


ReportBench runtime spine readme_smoke completed with reward 1.0 launch smoke ready


ReportBench headline tasks Tinker-backed Banking77 SFT +0.093; Tinker-backed GSM8K DPO +0.030 benchmark evidence ready


MLEBench Plus three strict 1.0 wins strict benchmark proof ready


RLVR open-ended research Banking77 transfer signal is strong; GSM8K remains mixed discovery proof active


This launch is supported by one public submission example, one external-surface readiness pass, one clean runtime smoke, one strict benchmark family, and one active open-ended research thread.


## What Managed Research Actually Is


Managed Research is a product for running repeatable, inspectable research workflows against real repos. It gives teams a control plane for runs, an MCP and Python SDK surface for launching and inspecting them, and application surfaces that consume the same system.


Product surface What the user gets


Project context repos, files, notes, credentials, and project knowledge attached before launch


Resources and usage provider readiness, launch preflight, and attributable usage across OpenRouter and Tinker


Launch control setup preparation, capacity preview, launch preflight, and explicit run trigger


Runtime control truthful run state, pause/stop/resume, and clear runtime intent acknowledgement


Human review questions, approvals, queued steering messages, and an operator console


External surfaces MCP, Python SDK, and quickstart docs that describe the same launch and review path


Evidence artifacts, timelines, traces, logs, experiments, and PRs


Results workspace archives, outputs, usage, and structured result manifests


NanoResearch is the clearest public front door for this launch. It gives people a place to inspect the run path, compare hillclimbing outcomes, and see what the product actually leaves behind when it works.


At launch, the product is strongest when the work involves:


- verification
- eval execution
- data assembly
- careful context optimization
- evidence-backed iteration


## What We Guarantee At Launch


The guarantee at launch is not that every run wins. The guarantee is that the workflow is legible enough to operate on purpose.


Guarantee What that means publicly


Real project context Managed Research starts from repos, files, notes, and project material instead of a blank prompt


Explicit launch authority setup, capacity preview, and launch preflight exist to make the launch decision inspectable before spend happens


Human steering is first-class runtime messages, approvals, and operator review are part of the product path, not cleanup after the fact


External surfaces stay aligned docs, SDK/MCP surfaces, and launch copy should describe the same product nouns and launch path


Outputs are reviewable PRs, artifacts, workspace archives, usage, and result manifests are meant to be inspected after the run


Limits are stated plainly we would rather show a baseline match, a scorer caveat, or a mixed research result than flatten it into marketing copy


Those are workflow guarantees. They are different from promising that every benchmark is already solved.


## The Proof Stack Behind This Launch


The scorecard above is the short version. Underneath it, the broader case for Managed Research is that the system now has multiple layers of evidence:


Surface Public-safe read today Why it matters


External control surfaces MCP and SDK are part of the shipped product story, the public quickstart exists, and the launch-facing content/type/metadata/link checks stayed coherent makes the product easier to approach and trust from outside the company


ReportBench launch spine readme_smoke completed end to end with reward 1.0, $0.04 cost, and 94.6s wall time on the shipped runtime spine proves the launch path can actually create a runnable project, launch work, score it, and mirror artifacts


ReportBench headline tasks Tinker-backed Banking77 SFT moved 0.879 → 0.971 (+0.093); Tinker-backed GSM8K DPO moved 0.020 → 0.050 (+0.030) shows the system can support measurable quality improvements on real training-oriented tasks, not only smoke checks


MLEBench Plus three strict tasks scored 1.0 gives us clean harness-routed proof on a separate benchmark family


Open-ended RLVR work Banking77 transfer runs already show clear source signal; GSM8K RLVR is still mixed enough that we treat it as active research rather than a polished headline shows that Managed Research is not limited to directed task completion


**Why this matters**


The point of this launch is not that every surface is already perfect. The point is that Managed Research now has a stack of evidence: one public submission example, one external-surface readiness pass, one clean runtime smoke, one strict benchmark family, and one open-ended research thread already underway.


## Directed-Effort Benchmark Proof


NanoHorizon is the most legible public example, but the launch is backed by more than one workflow. The directed-effort evidence matters because it shows Managed Research can support concrete, scored tasks across multiple benchmark surfaces.


### ReportBench


ReportBench is the most direct product-adjacent proof surface for this launch. It gives us both the runtime spine and a small set of measurable quality-improvement tasks.


Task Baseline Final Delta Why it matters


readme_smoke n/a reward 1.0 n/a proves the shipped launch spine can create a project, launch work, and return mirrored artifacts


banking77_sft_qwen3_4b_1 0.879 0.971 +0.093 strongest clean headline uplift in the current launch packet


gsm8k_dpo_llama32_1b_1 0.020 0.050 +0.030 small but real quality movement on a separate task family


The point of these rows is not that prompt or training optimization is magically solved. The point is that Managed Research now has a proof surface where concrete interventions can be run, scored, and compared instead of narrated after the fact.


### How Tinker Shows Up In This Launch


The easiest honest way to understand Tinker in this launch is as the training substrate behind several of the strongest ReportBench rows. The public packet is not claiming generic "Tinker support" as the hero story. It is showing something narrower and more useful: Managed Research can route training-oriented work through a real provider path, keep that provider story visible in launch/readiness surfaces, and then surface the resulting before-and-after evidence in a reviewable benchmark packet.


That is why so many of the named ReportBench rows carry` tinker` in the task id. Banking77 SFT, GSM8K DPO, and the RLVR transfer slices are all part of the same story: training-backed work happened on a real substrate, the result was scored honestly, and the usage/provider story was explicit enough to inspect afterward.


### ReportBench Model Snapshot


The current launch-safe ReportBench packet is also a small model comparison. Different model families are doing materially different things on different task shapes:


Model Confirmed task surface Best current read Honest interpretation


Qwen3-4B-Instruct-2507 Banking77 SFT 0.879 → 0.971 (+0.093) on banking77_sft_qwen3_4b_1 strongest clean launch uplift in the current packet


Llama-3.2-1B GSM8K DPO 0.020 → 0.050 (+0.030) on gsm8k_dpo_llama32_1b_1 smaller model, smaller but still real movement


Llama-3.2-3B Banking77 RLVR transfer 0.0714 → 0.5786 (+0.507) on the 7-class transfer slice strongest open-ended positive signal, but on a narrower task family


Llama-3.2-3B GSM8K RLVR 0.125 → 0.105 (-0.020) on the current pipelinerl slice the RLVR story is still mixed enough that we treat it as research, not marketing


That is exactly why the post stays limitation-forward. The current packet does not say one model family is universally best. It says Qwen looks strongest on the clean SFT headline, the smaller Llama DPO path still moves GSM8K, and the RLVR path is promising on Banking77 but not settled on GSM8K.


### MLEBench Plus


MLEBench Plus is the cleanest strict benchmark surface in the current launch packet. That matters because it gives us one proof family where the verdict path is already crisp.


Task Domain Score Status


mlebench_kaggle_new_york_city_taxi_fare_prediction regression 1.0 strict v1 success


mlebench_kaggle_spooky_author_identification text classification 1.0 strict v1 success


mlebench_kaggle_us_patent_phrase_to_phrase_matching text similarity 1.0 strict v1 success


Together, these rows give the launch a stronger shape than a single demo run. They show runtime correctness, role coverage, and strict benchmark proof across distinct task families.


## Open-Ended Discovery: RLVR Pattern Analysis


Managed Research is not only about directed tasks with one target metric. We also care about open-ended discovery: cases where the useful output is a pattern, a relationship, or a next research question rather than one winning benchmark row.


The RLVR work is the clearest example of that mode in the current launch packet. The question is simple and useful: do the slices of training data we choose for RLVR show up later in downstream transfer behavior?


Source task Current read Why it matters


banking77_rlvr_7class_llama32_3b_1 clear signal, uplift +0.507, rubric reward 0.76 strongest current evidence that the product can support open-ended analysis with real artifacts behind it


gsm8k_rlvr_pipelinerl_llama32_3b_1 mixed enough that we are treating it as active research rather than a launch brag line honest evidence that the open-ended story is real work, not post-hoc packaging


We are intentionally not pretending this section is a finished paper. What it already proves is that Managed Research can accumulate the artifacts, runs, and review trail needed for this kind of analysis. That is enough for the launch story. A deeper RLVR memo can come next.


## Early Work: Recursive Self-Improvement Bench


The next benchmark we are designing — not yet running, but scoped — is` recursive_self_improvement_bench` (` rsi_bench` ). The premise: give Managed Research a frozen snapshot of an earlier version of itself and a small budget to inspect failures and make changes, then measure whether the edited system scores better on heldout tasks from the same theme. The score is not raw task performance on the visible set. It is heldout transfer — how much of the visible-set improvement generalizes to tasks the improving agent never saw.


This sits between ordinary task-solving and open-ended autonomy claims, which is exactly why it is worth building carefully. We are not claiming this at launch. We are stating it as the honest next thing.


## How We Use Managed Research on Itself


We run Managed Research against the Managed Research codebase on a recurring schedule. The project pulls failure summaries from recent benchmark runs, proposes targeted prompt, planner, or verifier changes, and submits them as improvement candidates for the next evaluation cycle. No one presses a button. The system runs overnight and on weekends and leaves behind the same artifact trail — PRs, run records, scored bundles — that any other Managed Research project would.


That is the informal version of what RSI Bench is eventually meant to measure formally.


The organizing unit that makes this possible is the **project** . A project holds the context that persists across runs: repos, files, notes, credentials, research goals, and provider bindings. Runs are always children of a project. One-off work — a quick eval, a single hillclimbing pass — goes against a project with no schedule attached. Recurring work lives in a scheduled project, where the system throws compute at it automatically.


Two schedule modes exist:


Mode What it does


overnight Triggers runs during a configurable off-peak window (e.g. 22:00–06:00 in the project's timezone), on chosen days


perpetual Runs continuously — as soon as a run completes, a cooldown elapses and the next run starts


Compute schedules are not part of the self-serve launch surface. If you need scheduled projects, reach out and we will scope them against your plan, Beta Access state, and resource limits.


## What This Launch Is Not: Managed Agents


This post is about **Managed Research** — hosted research runs, projects, evidence, MCP, and` SynthClient().research` .


**Managed Agents** is a separate product surface (Anthropic-compatible inference, container pools, and related hosting APIs). It is **not** what we are launching or selling in this announcement. Synth may use internal agent infrastructure to power research workers; that is implementation detail, not the customer contract.


If you want inspectable research workflows against your repos, start with Managed Research. Managed Agents documentation lives elsewhere and is out of scope for this launch story.


## What We Are And Are Not Claiming


We are claiming that Managed Research can take real repo context, launch work safely, preserve operator control, produce real PRs and artifacts, and support multiple kinds of proof.


We are not claiming that every benchmark lane is already green, that NanoHorizon beat the current baseline, or that the open-ended RLVR story is already a finished publication-quality memo.


We are also not claiming that entitlement previews are outcome guarantees, that every public surface is equally mature, that the current proof stack means benchmark dominance across every task family, or that **Managed Agents** is part of this launch.


That is why this post leans so hard on inspectability. We would rather show a real run with real artifacts and real limits than flatten the launch into a generic "agents are here" announcement.


## Who It Is For


Wave 1 is for applied AI teams building vertical AI products, evaluators, and other repo-shaped systems that need more leverage without giving up inspectability.


It is also for builders working on public surfaces like NanoResearch and NanoHorizon, where the workflow is legible enough to show exactly what the product is doing.


## Why Teams Should Care


The value is not just model access. It is turning high-variance AI development into a more structured process:


- run more experiments without turning operators into schedulers
- preserve findings instead of losing them in chat logs and dead branches
- keep ownership over the repo, the PRs, the artifacts, and the decision trail
- build repeatable workflows instead of waiting for one lucky run


The foundation model fallacy is believing that model quality alone replaces process. Better models matter. Better models do not eliminate the need for verification, context management, experiment comparison, or durable evidence. Managed Research exists to make those things operational.


## What We Are Launching Now


We are launching Managed Research now because the core loop is real now, and NanoResearch gives that loop a public front door.


The system already helps operators run inspectable, repeatable research workflows, and NanoHorizon is the clearest way to make that visible. There is a lot more to build around experiment memory, prompt optimization, and broader benchmark coverage, but the wave-1 product is already useful.


## Key Links


- [NanoResearch leaderboard](https://usesynth.ai/nanoresearch)
- Top agent-authored NanoResearch submission —[candidate 22b80c9549, PR #6](https://github.com/synth-laboratories/nanohorizon/pull/6)
- [Managed Research quickstart](https://docs.usesynth.ai/managed-research/quickstart)
- [Apply for Managed Research Beta Access](https://www.usesynth.ai/signup?product=managed-research&utm_source=blog&utm_campaign=smr-launch)
