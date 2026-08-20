---
schema_version: "1.0.0"
document_id: "dea6adbb31f78554f35a9f7f56a53a926dfcba2f572cfe9f991cb2e6e96a3fde"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/smr-launch-gamebench-pilot"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-08-15T23:36:18.505766+00:00"
fetched_at: "2026-08-15T23:36:20.179971+00:00"
content_hash: "sha256:260668e111dfbebbb108c367ae5d4fe6872055fcc6619bcd76bf6a2eea6649c3"
---

# Evaluating Managed Research | Synth Blog

> **Archived:** The Open Research routes referenced in this launch snapshot were retired on July 25, 2026.


Managed Research is built around one promise: a research run should leave behind a receipt you can inspect, forward, and evaluate. GameBench is one of the ways we make that promise concrete.


Each GameBench task gives the run a bounded environment, an executable interface, a reward path, and artifacts a reviewer can replay. That makes the difference between "the agent worked on the benchmark" and "here is the policy, here is the rollout, here is the score, and here is where it failed."


## Candidate Receipts


The core object in GameBench is the candidate receipt. A receipt should make a candidate inspectable without reading a transcript: how many tokens were spent to produce it, what reward it reached, what code changed, what rollout it produced, and what the verifier saw.


The figure below plots real candidate receipts for the Craftax and Rogue DEO code-policy tasks: each point is a scored candidate, positioned by the cumulative tokens spent producing it, comparing a **standalone Codex hillclimb** against the **same task run through Managed Research (SMR)** . Click any point to open its receipt — code path, rollout, reward breakdown, and (for SMR) the run id.


These are genuine auto-research traces, not illustrations. On Craftax the managed run authored a single candidate (` hillclimb_v1` ) that beat the best standalone candidate (achievement score **0.152** vs **0.127** ) — it learned to bank iron, place a furnace, craft a sword, and fight back. The standalone curve also shows search honestly: it climbs to a peak at` attempt3` , then` attempt4` explores and regresses. Rogue's stair objective is already saturated by the shipped baseline, so its panel tracks **scout score** (exploration coverage) instead; the managed run's` scout_detour_v1` lifts it from 45 to 50 by adding opportunistic item detours, and the standalone search nudges its own baseline from 55 to 57. Each lane is measured against its own baseline, so the absolute offsets differ; the point is the trajectory and the inspectable receipt behind every dot.


Loading candidate curves…


## What GameBench Covers


GameBench is not one task. It is four task families over the same environments.


Subtask What the run has to do What we inspect


Engine dev / NEV Rebuild or verify a game environment from specs Event-log parity, legal actions, rewards, checkpoints, terminal state


Code policy opt Write or improve an executable policy Score lift, candidate artifacts, train/heldout behavior


Cybernetic opt Combine symbolic code with bounded steering Reward above code-only baseline, call count, token ledger


Policy puzzles Diagnose a hidden flawed policy from traces` diagnosis.json` , flaw label, trace-backed evidence


The environments range from small contract checks to longer-horizon and multi-agent settings: TicTacToe, Sokoban, Crafter, Craftax, MiniHack/Rogue, Frogs, Overcooked v2, and DungeonGrid. Coverage across these families is still filling in; this post focuses on the Craftax and Rogue DEO receipts above rather than a full scorecard.


## Policy Puzzles


Policy-puzzle lanes invert the task. The hidden policy is intentionally flawed; the run sees behavior traces and must produce a diagnosis with the flaw label and trace-backed evidence.


### Policy Puzzle Failure Modes


Rogue / door blind


## Harbor Task Boundary


GameBench tasks are packaged as normal Harbor tasks:


text


```text
task.toml
instruction.md
environment/Dockerfile
solution/solve.sh
tests/test.sh
```


That matters because Harbor gives the run a clean task boundary. The task has instructions, a container, an oracle path, and a verifier that writes a reward. Managed Research then has to operate inside that boundary and return a receipt with the lane, terminal state, score when available, duration, cost, candidate artifacts, and rollout links.


## What Good Looks Like


For GameBench, quality is mostly coverage, hardness, and evidence.


Coverage means every shipped environment has clear local and platform paths for the task families that make sense. Hardness means the agent cannot win by copying pre-baked strong policies, reading stale hillclimb reports, or matching filenames from leaked puzzle metadata. Evidence means the receipt contains enough state for another reviewer to reproduce the claim or understand the failure.


The cleanup work is therefore straightforward:


- keep Harbor bundles clean and agent-facing workspaces free of strong reference candidates;
- keep task families aligned through the same registry;
- make all launch tables receipt-based, not narrative-based;
- keep failed and stale lanes visible; and
- reserve leaderboard language for repeated, heldout, multi-model studies.


The next step is broader coverage: repeated seeds, more model families, and correlation work against ReportBench. That is intentionally outside this post.


Docs:[GameBench](https://docs.usesynth.ai/managed-research/gamebench)


## Minor Updates


Synth Tag is a small beta surface for starting one bounded Managed Research task from SDK or MCP, steering it while active, and retrieving the same run receipt you would inspect in the product.


python


```text
from   synth_ai   import   SynthClient


client   =   SynthClient()
session   =   client.research.tag.create_session(
"Investigate a failing benchmark lane and summarize the smallest fix."  ,
definition_of_done  =  "Return a root-cause note with evidence and next action."  ,
)


receipt   =   client.research.tag.get_session(session.session_id).receipt
```


Install:


bash


```text
pip   install   "synth-ai[research]==0.12.0"
```


Beta scope is deliberately small: SDK and MCP only. Tag is not Slack, access bundles, team memory, routines, or automatic Factory linking.


Research Factory also has a small control-plane update: create a Factory, create or link Efforts, inspect runs, and publish selected work when it is meant to be public. It is not yet the default self-serve first session, and this post should not imply Gardener digests, Seraph briefs, cross-run memory, or unattended programme scheduling are launched.


The Factory proof run was retired with the Open Research product.


## Try It


Start with one bounded Managed Research task. Use Synth Tag or the web app, inspect the receipt, and forward the run instead of a summary.


[Start Managed Research](https://www.usesynth.ai/signup?product=managed-research&utm_source=blog&utm_campaign=smr-launch-gamebench)
