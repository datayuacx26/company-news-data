---
schema_version: "1.0.0"
document_id: "cdb5479ea1f87c40a28954c43bbd9c1fa858b05eb32c3ee050bb8f321b68f117"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/scaling-train-time-compute-for-gepa"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-22T15:27:15.216799+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:8cabca055270ee06eebf492a8a7ab0a45629bdeaa2e33bbc7923647e27f3b508"
---

# Scaling Train Time Compute for Gepa

The prompt-optimization loop is simple to describe: propose a candidate, run it against a task, score the result, and use the evidence to propose again. The hard part is keeping the task boundary stable while teams change languages, scoring code, datasets, and model providers underneath it. Without that boundary, every optimizer integration becomes a custom harness, and every result is harder to reproduce.


Synth Containers make that boundary an HTTP contract. The optimizer does not import a task package, read local dataset files, or learn how a harness happens to be implemented. It calls a small set of routes, receives rows and rewards, and treats the container as the source of truth for the task. The task can be a classifier, a coding agent, a QA system, or a long-horizon environment; the optimizer sees the same surface.


GEPA is the first optimizer we are shipping on top of this interface. We started with public cookbook tasks because the contract needs to be inspectable: the container code is public, the chart rows are generated from recorded run artifacts, and publication requires an exact evidence commit for the generated chart rows.


In this post, we describe the container contract and report the same-container comparison against gepa-ai on the current four-container comparison set: HealthBench Pro, tau2-bench retail, Banking77, and HotpotQA. A proposer-model run group on HealthBench Pro and tau2-bench retail is reported in the Proposer Scaling section as a sanity check, not as a full scaling law.


---


## Policy Scaling


**Thesis:** Same-container comparisons are useful only when the task boundary, policy model, split shape, and evaluator are held constant.


The initial-post scope is four containers in the current comparison set: HealthBench Pro, tau2-bench retail, Banking77, and HotpotQA. Rows with evidence were run through the same container boundary for Synth GEPA and gepa-ai, then re-scored posthoc on the same heldout seeds. Train split metadata is shown only where the producer summary exposes a train denominator. Candidate counts, rollout calls, and wall-clock time vary by run, so this is same-container evidence rather than an equal-compute benchmark.


### Same-container head-to-head


Synth GEPA vs gepa-ai on tau2-bench retail, HealthBench Pro, HotpotQA, and Banking77. Each row uses the same container boundary, policy model, train split, and heldout split.


Task Best


heldout


Heldout


pareto K


Train


pareto K


Joint


pareto


tau2-bench retail


heldout → Synth


gepa


0.400


Synth


0.430


+0.030


gepa


62/100


Synth


63/100


+1


gepa


22/30


Synth


26/30


+4


gepa


1 · 50%


Synth


1 · 50%


0


HealthBench Pro


heldout → Synth


gepa


0.353


Synth


0.361


+0.008


gepa


157/200


Synth


158/200


+1


gepa


86/100


Synth


77/100


-9


gepa


3 · 50%


Synth


3 · 50%


0


HotpotQA


heldout → gepa-ai


gepa


0.748


Synth


0.707


-0.042


gepa


145/200


Synth


142/200


-3


gepa


—


Synth


—


—


gepa


5 · 83%


Synth


1 · 17%


-4


Banking77


heldout tie


gepa


0.785


Synth


0.785


tie


gepa


182/200


Synth


177/200


-5


gepa


100/100


Synth


97/100


-3


gepa


3 · 60%


Synth


2 · 40%


-1


Read:


best heldout is the strongest single candidate; heldout/train pareto are cumulative rows solved by any candidate through K; joint pareto is the 3-objective frontier (heldout, cost, time).


Tier-1 same-container comparison from` evals/evidence/benchmarks/*/summary.json` .[View head-to-head data builder rebuild-gepa-core-head-to-head-data.py](https://github.com/synth-laboratories/synth-cookbooks-public/blob/1932ee7861b2fdf69814d368014e0106748c594a/frontend/scripts/rebuild-gepa-core-head-to-head-data.py)


Coverage tells a different part of the story than best-candidate score. On HealthBench Pro, Synth and gepa-ai are close on cumulative heldout coverage (158/200 vs 157/200) even though Synth has the best heldout candidate in this final run. On tau2 retail, Synth is slightly ahead on both coverage (63/100 vs 62/100) and best heldout (0.430 vs 0.400) under the fresh same-container rerun. Banking77 ties on best heldout while gepa-ai covers more rows (182/200 vs 177/200); HotpotQA goes the other way, with gepa-ai ahead on both best heldout (0.748 vs 0.707) and coverage (145/200 vs 142/200).


Y-axis · best heldout score


X-axis · candidate index K


Cumulative heldout coverage on the four same-container comparison tasks. HealthBench counts rows with positive rubric credit; tau2 retail, Banking77, and HotpotQA count successful heldout tasks.[View heldout coverage data builder build_heldout_coverage.py](https://github.com/synth-laboratories/synth-cookbooks-public/blob/1932ee7861b2fdf69814d368014e0106748c594a/cookbooks/blogs/oss-containers-and-gepa/charts/chart-c-use-case-coverage/build_heldout_coverage.py)


## Proposer Scaling


**Thesis:** Changing proposer compute should help most when the task gives the optimizer expensive, high-signal rollout feedback.


The launch proposer sweep is deliberately small: HealthBench Pro and tau2-bench retail, each with gpt-5.4-nano, gpt-5.4-mini, and gpt-5.4 proposers. Those are the proposer labels recorded in the run manifests. These Chart D runs skipped heldout scoring by design, so the chart reports observed optimization reward from each Synth GEPA run rather than posthoc heldout.


Observed optimization reward by proposer model. tau2 retail is monotonic (0.600 → 0.600 → 0.667); HealthBench is not (0.347 → 0.314 → 0.339), so treat this as a two-task sanity check rather than a scaling law.[View proposer scaling builder build_chart.py](https://github.com/synth-laboratories/synth-cookbooks-public/blob/1932ee7861b2fdf69814d368014e0106748c594a/cookbooks/blogs/oss-containers-and-gepa/charts/chart-d-proposer-scaling/build_chart.py)


## What Changes


**Thesis:** GEPA changes candidate fields, not the container contract. The container still owns rows, scoring, rollouts, traces, and task secrets.


The mutable field changes by task, but the optimizer contract does not:


- **HealthBench Pro:**` stage1_system`
- **tau2-bench retail:**` domain_policy`
- **Banking77:**` stage2_system`
- **HotpotQA:**` stage1_system`


Those fields are the only task-specific program surfaces the optimizer mutates.


## Open Sourcing GEPA


**Thesis:** Open sourcing GEPA is not just releasing an optimizer. It is releasing the task boundary, evidence format, and runtime interfaces that make optimizer results inspectable.


The systems story is that the environment around the optimizer is part of the product. For GEPA, that environment is the container contract and the durable optimizer runtime. The optimizer should not know whether a task is a Python classifier, a Rust program, a TypeScript service, a game environment, or a medical-rubric evaluator. It should see rows, candidates, rollouts, rewards, traces, and usage through one stable surface.


The diagrams below are organized as subsystem views rather than one monolithic architecture figure. Start with the container boundary, then inspect the GEPA runtime, then the Flash Evolve scheduling layer.


Subsystem focus


### Container boundary


The task stays behind a small HTTP contract. Rows, rewards, scoring, traces, and secrets remain container-owned.


The optimizer owns search and reaches the task only through typed HTTP routes. Mutable modules, dataset splits, policy execution, reward and scoring, and task secrets all stay inside the container. Implementation notes


+


The contract is deliberately small. A container tells the optimizer what mutable modules exist, which dataset splits are available, how to fetch rows, and how to score a candidate on a row. Everything else stays behind the container boundary.


-


A HealthBench rubric task, a tau2 retail workflow, a Banking77 classifier, and a HotpotQA question-answering run should not require four optimizer integrations.


-


They should require four containers that implement the same routes.


Route Method Optimizer use


/metadata GET Confirms GEPA contract version before the run starts


/program GET Reads the mutable modules and seed candidate


/taskset GET Gets the taskset metadata advertised by the container


/taskset/tasks POST Fetches train, minibatch, reflection, and heldout rows


/rollout POST Submits a candidate and row, gets back reward and usage


/task_info GET Optional task context for proposer prompts


## Resources


- [synth-cookbooks-public](https://github.com/synth-laboratories/synth-cookbooks-public)
- [GEPA launch evidence packet](https://github.com/synth-laboratories/synth-cookbooks-public/tree/gepa-blog-public-evidence-20260605/cookbooks/blogs/oss-containers-and-gepa)
- [Prompt Optimization Overview](https://docs.usesynth.ai/prompt-optimization/overview)
- [gepa-ai reference implementation](https://github.com/gepa-ai/gepa)


## Citations


- [GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning](https://arxiv.org/abs/2507.19457)
- [gepa-ai/gepa](https://github.com/gepa-ai/gepa)
- [HealthBench Professional dataset](https://huggingface.co/datasets/openai/healthbench-professional)
- [tau2-bench](https://github.com/sierra-research/tau2-bench)
- [HotpotQA](https://hotpotqa.github.io/)
- [Banking77](https://huggingface.co/datasets/PolyAI/banking77)
