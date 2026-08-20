---
schema_version: "1.0.0"
document_id: "413e1e39225ed862a1621b8c3decf59a6c93da70a0e466491ec6ce8350c7167d"
company_key: "yc-afterquery"
company: "AfterQuery"
source_id: "yc-afterquery-news-import-ad1daf482411"
canonical_url: "https://www.afterquery.com/blog/how-afterquery-helped-nvidia-hill-climb-gdpval"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-21T05:04:33.106205+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:1f69865410bb2978b50b5b0b073d8f93703e57255d43e17ec2424a5c1a74a59c"
---

# How AfterQuery Helped NVIDIA Hill-Climb GDPval

NVIDIA publicly used AfterQuery’s[Off-The-Shelf Office Agent Training Dataset](https://www.afterquery.com/contact) to improve Nemotron 3 Ultra on[GDPval](https://openai.com/index/gdpval/) . Nemotron 3 Ultra is a fully open 550B-A55B LatentMoE model with open weights, training data, and recipes. Ultra runs at up to ~6× the throughput of comparable open models (5.9× vs GLM-5.1, 4.8× vs Kimi K2.6) on long-horizon agentic tasks at the same accuracy, and supports a context length of up to 1M tokens.


AfterQuery is the only data partner named in the Nemotron 3 Ultra[technical report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Ultra-Technical-Report.pdf) :


> “We then constructed a training distribution from AfterQuery (AQ) tasks that share important latent structure with GDPval, including file-grounded reasoning, professional deliverables, multi-step analysis, and judged final outputs. For each AQ task, we used a strong model to generate multiple full trajectory rollouts. These rollouts were used in two stages. First, before pivot RL, we performed light SFT directly on the student Ultra model. The goal of this step was to transfer the strong model’s workflow priors for GDPval-like tasks to the student. Second, after this MOPD warmup, we proceeded with pivot RL in the MOPD stage, distilling the SFT-trained teacher into the student Ultra model using pivots derived from the strong model’s AQ rollouts.”


## What is GDPval


GDPval is OpenAI’s benchmark for real-world professional tasks. It spans 44 occupations across the nine largest sectors of US GDP, with 1,320 tasks (220 of them open-sourced) drawn from the real work of industry professionals who average 14 years of experience. Each task gives the model a prompt, often with reference files, and asks for a finished deliverable: a spreadsheet, slide deck, document, diagram, etc.


Artificial Analysis maintains a public leaderboard version,[GDPval-AA v2](https://artificialanalysis.ai/evaluations/gdpval-aa) , that scores models on the open tasks and is now the highest-weighted evaluation in their Intelligence Index. Models solve the tasks agentically, working in a sandbox with shell + web access via the Stirrup harness. The resulting deliverables are compared in blind pairwise matchups, each graded by a judge sampled from a rotating panel of three frontier LLMs, and those results are fit to an Elo scale anchored to human expert work at 1,000 Elo.


AfterQuery’s Office Agent tasks mirror GDPval task structure, with file-grounded inputs, multi-step analysis, and rubrics.


## PivotRL


[PivotRL](https://arxiv.org/abs/2603.21383) (Yi et al., 2026) is a turn-level RL method for agent training. It starts from existing SFT trajectories and treats each assistant turn as a possible training state. For each candidate turn, it samples several next actions from the reference or initial policy and scores them with a verifier. It keeps only the turns where the sampled actions produce mixed outcomes—some pass, some fail—and discards turns that are already uniformly solved or uniformly failed. RL is then run locally at those retained “pivot” turns, using verifier rewards for functionally valid actions rather than exact matches to the demonstration. The intended benefit is lower rollout cost: on SWE-Bench, the paper reports accuracy comparable to end-to-end RL with about 4× fewer rollout turns.


## Impact


The practical use case for PivotRL shows up in NVIDIA’s Nemotron 3 Ultra training recipe. For GDPval-like office tasks, NVIDIA first used a strong model to generate full AfterQuery trajectories, then reused intermediate decision points from those trajectories as pivots during the MOPD stage. In other words: PivotRL supplies the local “where should we train?” states, while MOPD (Multi-teacher On-Policy Distillation) supplies the teacher-student learning signal at those states.


NVIDIA trained specialized teachers by domain and then distilled them into Ultra through MOPD. For the office/workplace teacher, the AfterQuery tasks were chosen because they resemble GDPval: file-grounded reasoning, multi-step analysis, professional deliverables, and judged final outputs. The report says the AQ rollouts were used in two stages: a light SFT warmup to transfer the strong model’s workflow priors, followed by pivot RL in MOPD using pivots from those same strong-model rollouts.


Domain Student No warmup Warmup Teacher


GDPval 28.9 35.3 46.7 49.5


BrowseComp 31.0 33.0 44.4 51.0


HLE (no tools) 25.6 26.3 26.7 32.1


NVIDIA’s warmup ablation. Student is the starting checkpoint, Teacher the specialized model being distilled toward.


On GDPval, warmup raises the MOPD result from 35.3 to 46.7, leaving Ultra only 2.8 points behind the office/workplace teacher. BrowseComp shows the same pattern, rising from 33.0 to 44.4. HLE barely moves, from 26.3 to 26.7. AfterQuery has[similarly validated](https://www.afterquery.com/blog/on-policy-distillation-gdpval) that on-policy distillation works well for improving models on GDPval-style tasks, reaching a +20.9% net win-loss margin over base with a Nemotron 3 Nano student with pure OPD.


Get in touch[here](https://www.afterquery.com/contact) to access our off-the-shelf GDPval, Office Agent, and agentic post-training datasets, or reach out to us directly atresearch@afterquery.com .


AfterQuery is an applied research lab curating data solutions to accelerate foundation model development.


Sources: NVIDIA Nemotron 3 Ultra technical report (Tables 4–5, Office & Workplace Task Teacher); PivotRL, Yi et al., 2026 (arXiv 2603.21383v1); GDPval-AA v2 leaderboard.
