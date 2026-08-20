---
schema_version: "1.0.0"
document_id: "e3970a5ba4ae9d76898039bad0cc13527467df742a401c46e113f98a00877096"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/jesterky-dynamic-workflows"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-24T04:58:53.081142+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:62bd32faaa14537427817ec864fa45df1bb0860284768eba54249a074c85dc7e"
---

# jesterky runs agent workflows as replayable programs

Dynamic model workflows fan out, call tools, mutate state, and become difficult to inspect after the run. **jesterky** records the specification, runtime events, and sandbox outputs so research engineers can inspect the path a workflow took and check a replay against that record.


Headline results


### Three workloads, run live, replayed


Every number is read from a committed run manifest or score artifact. Each card names its evidence and its boundary.


actor: codex · gpt-5.5


proxy: gemini / deepseek


replay: addr + kind + payload


#### Blog quality scan


replay ok


2 SOUND / 6 FRAGILE


gpt-5.5 audits 8 production posts, live


[quality_scan_blogs.live.manifest.json](https://github.com/synth-laboratories/jesterky/blob/main/proof/quality_scan_blogs.live.manifest.json)


One blog corpus and one recorded run; this is a scan, not a quality benchmark.


#### Dev-port bench


per-cell score artifacts


20/20 faithful → 4/27 cliff


models port Python game engines to Rust in a seeded sandbox


[dev_port_refactor_bench.md](https://github.com/synth-laboratories/jesterky/blob/main/proof/dev_port_refactor_bench.md)


One run per cell; repeat attempts swing mid-ladder cells, so no model ranking is claimed.


#### SMR ReportBench


replay ok


agreement 1.0 · score 0.926


workflow verdicts vs. autograde over real graded runs


[smr_reportbench_trace_evaluate.md](https://github.com/synth-laboratories/jesterky/blob/main/proof/smr_reportbench_trace_evaluate.md)


Four graded lanes in the captured packet; this does not establish general evaluator reliability.


Manifests and score files live in the repo's[proof/ tree](https://github.com/synth-laboratories/jesterky/tree/main/proof) ; replay re-executes the orchestration against recorded outputs and must land identically.


Workflow record


### What jesterky captures from a run


01


spec


workflow inputs


→


02


fan out


parallel tasks


→


03


sandbox


environment + files


→


04


manifest


events + outputs


→


05


replay


check the recorded run


The manifest captures the specification, runtime events, sandbox artifacts, and outputs. Replay runs the scheduler against that record.


### Read the evidence at the right level


The claims here are about workflow execution and artifacts, not deterministic model behavior. A replay verifies the recorded orchestration path against recorded outputs. It does not guarantee that a future call to a hosted model will make the same choice unless that call is recorded or otherwise controlled.


## The quality scan, live


jesterky run … --actor codex --model gemini/gemini-3.1-flash-lite --follow


A real --follow


run over the current published-blog corpus. This capture completed 57 events with 10 recorded outputs in 20 seconds; the button reloads the GIF on demand. It is a workflow demo, not a benchmark.


expand → map → reduce, live


### One run audited every published post on this blog


The workflow expands the blog corpus into per-post audit jobs, maps a gpt-5.5 auditor over them concurrently, and reduces the verdicts into a report. These are its real findings — including about this blog.


events 52 · recorded 9


managed-factory


SOUND 7


strong launch proof; routing metadata mostly absent


synth-tag-v1


SOUND 7


clear beta post with smoke proof; routing metadata missing


go-explore


FRAGILE 7.1


measured benchmark claims lack required proof metadata


scaling-train-time-compute-for-gepa


FRAGILE 7


required routing metadata largely absent


stack-handoffs


FRAGILE 6


measured claims lack reproducible proof


smr-launch-gamebench-pilot


FRAGILE 6


measured lifts lack visible proof metadata


managed-research


FRAGILE 6


launch metadata and proof links incomplete


launching-synth-managed-research-mcp-first


FRAGILE 5


launch metadata and release custody missing


Committed manifest:[quality_scan_blogs.live.manifest.json](https://github.com/synth-laboratories/jesterky/blob/main/proof/quality_scan_blogs.live.manifest.json) . Replay re-runs the fan-out against the recorded verdicts: replay ok: events=52 recorded=9


.


Same spec, two actors: the[fake actor](https://github.com/synth-laboratories/jesterky/blob/main/crates/jesterky-actor/src/lib.rs) echoes deterministically — what the tests run on. Swap in the[codex actor](https://github.com/synth-laboratories/jesterky/blob/main/crates/jesterky-model/src/lib.rs) and the identical workflow drives gpt-5.5 on ChatGPT-bundle auth, no API key in the process. The spec doesn't change; the actor behind the seam does.


## Models run a real environment


sandbox → port → verify → capture → score


### Five engines, three models, one cliff


Each model ports a working Python game engine to a Rust crate inside a seeded workspace, iterating against a 4-scenario train oracle. The captured crate is scored on ALL scenarios — the tick marks the train subset: bars that stop there memorized the diff and generalized zero.


all-scenario pass rate · 1 run/cell


Cliff = mechanic complexity, not lines of code: minihack (1,036 LOC) defeats every model; earthborne (1,916 LOC) is fully ported twice. Repeat attempts swing mid-ladder cells (gpt-5.5 sokoban 1.0 → 0.33 → 0.33), so no per-model ranking is claimed. Per-cell artifacts:[gamebench](https://github.com/synth-laboratories/gamebench) score.sandbox.*.json


.


workflow and single-session runs


### Two models, two execution modes


Sokoban (15 scenarios), identical seeded files, porter brief, and 10-minute cap. The comparison is Gemini Flash Lite and GPT-5.6 Luna at low effort, each run through the workflow and a single Codex session.


sokoban · 600s cap · 1 run/completed cell


#### gemini-3.1-flash-lite


Workflow


4/15


0:56 · 303k tok · $0.087 est.


Single harness


0/15


5:48


#### gpt-5.6-luna · low


Workflow


4/15


1:47


Single harness


2/15


3:49 · 59.6k tok


Flash Lite scored 4/15 in the workflow at 0:56, using 303k reported tokens (294k input, 8.8k output) with a $0.087 rate-card estimate. Its bare session scored 0/15 in 5:48. The bare crate depended on train_eventlogs.json


outside the capture allowlist, so the rebuilt scorer could not open that file. Luna at low effort scored 4/15 in the workflow and 2/15 in the bare session (1:47 vs 3:49). This is one run per cell, not a model ranking.


The workspace is seeded by[jesterky-sandbox](https://crates.io/crates/jesterky-sandbox) (local or Docker); capture globs pull the built crate back into the run manifest. Gemini and DeepSeek drive the same loop through[jesterky-proxy](https://crates.io/crates/jesterky-proxy) — Responses⇄chat tool-call translation plus Gemini's thought-signature round-trip — so one agentic harness runs any model. The ports above are manifests you can replay.


## Workflows against deep standards


engineering soundness, continuously


### 57 modules graded against 7 house standards


The same expand → map → reduce shape pointed at code: modules across five repos, each graded 1–10 on typed seams, no-fallbacks, error legibility, exhaustive enums, unique nouns, earned abstraction, and explicit-over-implicit — with caps-at-hold verdicts and file-level evidence.


57 modules · 8 capped at hold


unique_nouns


7.6 ·1 flagged


typed_seams


7.4 ·7 flagged


earned_abstraction


7.3 ·0 flagged


explicit_over_implicit


7.2 ·5 flagged


exhaustive_enums


6.9 ·12 flagged


no_fallbacks


6.5 ·14 flagged


error_legibility


6.3 ·9 flagged


containers:src/synth_containers


typed_seams


HttpObject = dict\[str, object\] … ConfigDict(extra="allow") — open JSON contracts at a public seam


optimizers:code/src/tools


explicit_over_implicit


runtime tests bypass seams with casts: \`tools: tools as never\`, \`let mockCurrentWorktreeSession: any = null\`


synth-ai:managed_research/models


typed_seams


typed StrEnums exist, but escape hatches remain: \`schedule: dict\[str, object\] = field(default_factory=dict)\`


Low bars are the point: the scan names the debt ( no_fallbacks


is the weakest house dimension, 14/57 modules flagged) and every grade carries a quoted file-level violation. Run on a schedule, the workflow turns a written standard into a maintained one.


A standards document only matters when a workflow can apply it consistently. The scan above runs the[engineering-soundness rubric](https://github.com/synth-laboratories/jesterky/blob/main/crates/jesterky-quality/src/lib.rs) against each module, records quoted evidence, and applies a caps-at-hold verdict. The manifest gives a reviewer the inputs, outputs, and evidence behind each grade.


## Verify a captured run


replay verification


### Check a captured run against the runtime


Events use a logical address: run_id, node_path, iteration, and local_seq. During replay, the runtime must reproduce the recorded address, kind, and payload.


spec + args


workflow inputs


quality_scan_blogs.json


→


live run


model and tool calls


events=52 · recorded=9


→


manifest


captured event data


*.live.manifest.json


→


replay check


runtime vs. recorded data


replay ok: 52/9


Timestamps and log order stay as metadata. Parallel workers can emit in a different order, but the logical event address remains stable.


Replay tests the workflow runtime separately from a new model call. It reads a captured manifest, re-runs the scheduler against recorded outputs, and fails when an event's logical address, kind, or payload changes. It does not claim that a future hosted-model call will return the same answer.


bash


```text
jesterky   run   examples/quality_scan_blogs.json   --actor   codex   --out   run.manifest.json
jesterky   replay   run.manifest.json   --spec   examples/quality_scan_blogs.json
# replay ok: events=52 recorded=9
```


## In the products


- **[Optimizers](https://github.com/synth-laboratories/optimizers) :** GELO and GEPA take a` \[jesterky_workflow\]` config block — traces export, a workflow clusters failure themes, and the themes materialize into the proposer before each propose. GELO's arm with the workflow improved Craftax reward over baseline by[+0.977 vs +0.339 without](https://github.com/synth-laboratories/jesterky/blob/main/proof/gelo_jesterky_workflow_ablation.md) . The same hook inside GEPA is[wired but showed no uplift](https://github.com/synth-laboratories/jesterky/blob/main/proof/gepa_jesterky_workflow_ablation.md) , and we report it that way.
- **Stack:** agent workers get five MCP verbs —` stack_jesterky_register,launch,inspect,replay,compare` — verified end-to-end against the[published binary](https://crates.io/crates/jesterky-cli) , including a full Stack effort eval — launch → inspect → replay → compare with` node_diff_count=0` and A0–A3 acceptance recorded pass.
- **Cloud:** SMR ReportBench runs are graded through the same trace-evaluate workflow; gpt-5.5's verdicts[agreed with autograde on all four graded lanes](https://github.com/synth-laboratories/jesterky/blob/main/proof/smr_reportbench_trace_evaluate.md) (report score 0.926, replay ok).


## Where it falls short


A flaky hosted model still produces a flaky run — jesterky classifies the failure, it can't prevent it. Mid-ladder dev-port cells swing between runs, so the[bench packet](https://github.com/synth-laboratories/jesterky/blob/main/proof/dev_port_refactor_bench.md) claims the cliff, not model rankings. GEPA's workflow arm has no measured uplift. The ReportBench A/B ran as a[post-hoc revision pass](https://github.com/synth-laboratories/jesterky/blob/main/proof/reportbench_ab.md) (+0.294 on the one non-ceiling lane, with two checks recovered by rubric gaming — the packet says which); in-run guidance is the next rung.


## Resources


- [jesterky repository](https://github.com/synth-laboratories/jesterky) — source, examples, CLI, and the runtime contract.
- [Proof artifacts](https://github.com/synth-laboratories/jesterky/tree/main/proof) — committed manifests, score files, ablations, and run notes behind the claims above.
- [jesterky on crates.io](https://crates.io/crates/jesterky-cli) and[jesterky on PyPI](https://pypi.org/project/jesterky/) — install surfaces for the 0.1.1 release.
- [Claude Code auto mode](https://www.anthropic.com/engineering/claude-code-auto-mode) — the workflow-orchestration pattern that motivated this substrate.
- [GELO: Go-Explore for long-horizon agents](https://www.usesynth.ai/blog/go-explore) — the adjacent Synth optimizer work that consumes workflow evidence.
- [GEPA platform](https://www.usesynth.ai/blog/introducing-gepa-platform) — the optimizer surface where the workflow hook is wired, including the negative result reported here.


## Citation


If you use the workflow substrate or the replayable-agent-workflow pattern in research, cite the post and repository:


bibtex


```text
@misc  {  jesterky2026  ,
title          =   {  jesterky runs dynamic agent workflows  }  ,
author         =   {  {Synth Team}  }  ,
year           =   {  2026  }  ,
howpublished   =   {  \url{https://www.usesynth.ai/blog/jesterky-dynamic-workflows}  }  ,
note           =   {  Open-source Rust workflow substrate  }  ,
url            =   {  https://github.com/synth-laboratories/jesterky  }
}
```


**Start with the recorded quality-scan example.** cargo install jesterky-cli


, run the scan, then inspect the manifest and replay report. The evidence linked above lives in the[proof/ tree](https://github.com/synth-laboratories/jesterky/tree/main/proof) .


[View jesterky on GitHub](https://github.com/synth-laboratories/jesterky?utm_source=blog&utm_medium=inline_cta&utm_campaign=jesterky-dynamic-workflows)[Read the proof artifacts](https://github.com/synth-laboratories/jesterky/tree/main/proof?utm_source=blog&utm_medium=inline_cta&utm_campaign=jesterky-dynamic-workflows)
