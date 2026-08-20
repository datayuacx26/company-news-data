---
schema_version: "1.0.0"
document_id: "0842b5c154cfd80e0563645da37cc16c603e9295a636182739f720f2094a1e59"
company_key: "yc-afterquery"
company: "AfterQuery"
source_id: "yc-afterquery-news-import-ad1daf482411"
canonical_url: "https://www.afterquery.com/blog/on-policy-distillation-gdpval"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-21T05:04:33.106205+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:074dd3207179531c8069eeca46952ab32b8453fe9c61cf26ffc3bbeda59258b4"
---

# How we achieved a net win-loss margin of +21.4% on GDPval with on-policy distillation

AfterQuery researchers trained NVIDIA’s[Nemotron-3-Nano-30B-A3B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16) with on-policy reverse-KL distillation from a frozen[Nemotron-3-Super-120B-A12B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Super-120B-A12B-BF16) teacher, using[Tinker](https://thinkingmachines.ai/tinker/) . We then evaluated the result on[GDPval](https://openai.com/index/gdpval/) , OpenAI’s benchmark for professional knowledge-work tasks. After only 50 steps of training, the Nemotron student’s **mean GDPval rubric score increased by 5.1 points** , yielding a **+20.9% net win-loss margin** against the base model. The training split contained only tasks from AfterQuery’s[Off-The-Shelf Office Agent Training Dataset](https://www.afterquery.com/contact) and no examples from the public GDPval eval set.


We then reproduced the result with a different model family. The same recipe applied to[Qwen3.5-9B](https://huggingface.co/Qwen/Qwen3.5-9B) , distilled from a[Qwen3.5-397B-A17B](https://huggingface.co/Qwen/Qwen3.5-397B-A17B) teacher, produced a comparable **+21.4% net win-loss margin** against the base model.


Nano samples each token, the frozen Nemotron-3-Super teacher scores them, and training shifts Nano’s token choices toward the teacher’s distribution.


## What is on-policy distillation?


In on-policy distillation, a student model learns by comparing its own generations against those of a stronger teacher.


For each task, the student samples a rollout token by token. For every token the student produced, the teacher asks how likely it would have been to produce that same token, given everything that came before it. Ordinary off-policy distillation trains the student on excellent rollouts it would never produce on its own; on-policy distillation trains it on the messy ones it actually generates. You can think of it like the difference between memorizing flashcards of correct answers and having a tutor grade your own homework: the flashcards only ever show the right answer, while the tutor catches the exact step where you went wrong. This matters especially for the GDPval benchmark, where an intermediate mistake tends to corrupt every action that follows. An agent that opens the wrong file or starts from a wrong assumption must then recover from the resulting situation.


The signal is a per-token reverse KL in the student-to-teacher direction.¹


Each term is the student’s log-probability for a sampled token minus the teacher’s for the same token. Minimizing it pushes the student toward tokens the teacher finds more likely and away from tokens it finds less likely.


## Configuration


In our experiment, we used Nemotron-3-Super as the teacher because it is substantially stronger than Nano while staying in the same model family, and Nemotron-3-Nano as the student because it starts from a much lower professional-work baseline, as reflected on the public[GDPval-AA leaderboard](https://artificialanalysis.ai/evaluations/gdpval-aa) . Keeping both in the same family helps since they share a common tokenizer, chat template, and response style. As a result, less of training is spent correcting format mismatches. The loop follows the[Tinker](https://github.com/thinking-machines-lab/tinker-cookbook/blob/main/tinker_cookbook/distillation/train_on_policy.py) recipe for on-policy distillation. Nano samples the task rollout. Super is queried for log-probabilities on Nano’s sampled tokens. Tinker turns the log-probability difference into a negative reverse-KL token signal, then updates only the Nano LoRA adapter through its importance-sampling loss.


### Training configuration


Setting Value


Student nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16


Teacher nvidia/NVIDIA-Nemotron-3-Super-120B-A12B-BF16


Renderer / chat template nemotron3


Loss importance_sampling with per-token reverse KL


Dataset size 800 AfterQuery OTS Professional-Work Tasks


KL coefficient 1 on teacher KL


Learning rate 1e-4


LoRA rank 32


Batch size 16


Temperature 1


## Results


### Training curves


Teacher reverse KL, per-token entropy, efficiency rate, and any-deliverable rate across the 50-step distillation run. Falling entropy under a mode-seeking reverse-KL objective⁴ could in principle signal mode collapse; the capability benchmarks below confirm it does not.


As Nano’s token distribution moves toward Super’s, its per-token entropy falls and the model grows more decisive: it produces a usable deliverable more often, and does so more efficiently.


### Improvement on the official GDPval eval


After only 50 steps of training, our distilled model improves on both aggregate scoring and paired comparisons across all 220 tasks in OpenAI’s official GDPval eval set. Mean weighted rubric score is up 5.1 points, and the distilled model wins 123 of the 220 task pairs, ties 20, and loses 77 — a 55.9% win rate and a +20.9% net win-loss margin against the base model. A two-sided sign test on the decisive task pairs confirms the margin is statistically significant (p ≈ 0.001).


Is this specific to Nemotron, or is on-policy distillation a general lever for professional work? We ran the identical recipe on a completely different family to find out:[Qwen3.5-9B](https://huggingface.co/Qwen/Qwen3.5-9B) as the student, distilled from the far larger[Qwen3.5-397B-A17B](https://huggingface.co/Qwen/Qwen3.5-397B-A17B) teacher. Same on-policy reverse-KL signal, the same Tinker recipe, the same AfterQuery Off-The-Shelf GDPval Training Dataset. The gain carried over, and was if anything larger. The distilled Qwen student improved its mean weighted rubric score by 7.9 points over the base model, achieving a 57.3% win rate and a +21.4% net win–loss margin.


*(Win rate = wins / 220; net win-loss margin (wins − losses) / 220.)*


## What improvement looks like in practice


Below, the base model’s deliverable beside the distilled model’s on the same task, for both model families. Use the arrows to page through each file.


### Example #1


Nemotron-3-Nano


— Task` c44e9b62` asks for a briefing note, FTE spreadsheet, and revised org chart for a government staffing plan.


Task excerpt.


“Create an information package on FTE reductions for your branch. The package should include a revised organizational chart as a PDF, an updated FTE report in Excel, and a briefing note describing the background, proposed reductions, and fit with budget planning principles.”


BASE MODEL


1 page


DISTILLED MODEL


1 / 2


### Example #2


Nemotron-3-Nano


— Task` a95a5829` asks for a police training-request policy and companion Excel log.


Task excerpt.


“Create a comprehensive general order in Word for how training requests are submitted, reviewed, approved, tracked, and documented. Include eligibility, required request information, evaluation criteria, timelines, final approval authority, required sign-offs, Excel logging, participation tracking, and record maintenance.”


BASE MODEL


1 page


DISTILLED MODEL


1 / 4


### Example #3


Qwen3.5-9B


— Task` 2fa8e956` asks for a shareable Word guide to wineries within a one-hour drive of a Napa Valley hotel.


Task excerpt.


“Create a shareable Microsoft Word document listing wineries within a one-hour drive that offer tasting experiences and a variety of grape types. Keep it to no more than four pages, add a footer titled Napa Valley Wineries, include a royalty-free photo of Napa Valley vineyards, and for each winery give name, grape varieties, a description, visiting hours, address, phone number, distance in miles, and estimated drive time.”


BASE MODEL


1 / 12


DISTILLED MODEL


1 / 3


### Example #4


Qwen3.5-9B


— Task` 1a78e076` asks for an evidence-based literature review on hypertension treatment adherence in older adults.


Task excerpt.


“Complete an evidence-based literature review determining the factors that contribute to or affect hypertension treatment adherence in older adults, as a Word document between 10 to 15 pages examining prevalence data, how adherence varies across older age groups, morbidity and mortality rates associated with poor adherence, and financial impact of hypertension management.”


BASE MODEL


1 / 12


DISTILLED MODEL


1 / 12


### Example #5


Qwen3.5-9B


— Task` be830ca0` asks for an Analyze tollgate presentation with project charter, five Minitab charts, and A3 summary for a Lean Six Sigma project.


Task excerpt.


“Develop an Analyze tollgate presentation in PowerPoint whose first slide includes a Project Charter, and create a One-Way ANOVA interval plot, an I-MR Control Chart, a Linear Regression Analysis, 1-Sample Hypothesis Test, and a Process Capability Analysis using Minitab. Also include an A3 Summary with background, project purpose, current conditions, goals, analysis results, and follow-up.”


BASE MODEL


1 page


DISTILLED MODEL


1 page


1 / 5


### Qualitative patterns


- **Substance over scaffolding.** Base outputs often had the right shell with placeholders. Distilled outputs more often filled sections with specific policy text, analysis, rows, and supporting details.
- **Artifacts that actually work.** The distilled model builds artifacts that actually function: Excel sheets that use live formulas instead of hard-coded values, working tables and cross-references, and document structure that holds together rather than breaking under its own formatting.
- **Follows the formatting it’s told to.** Tasks showcase stronger instruction following, specifically with formatting-related requests like highlights, date blocks, and signature blocks.
- **Expert depth, not surface answers.** The distilled model goes a level deeper, giving the specific details an expert would rather than a surface answer. In the hypertension literature review above (Example #4), the distilled model engages the clinical specifics the task calls for, like prevalence across older age groups, the morbidity and mortality tied to poor adherence, and the financial impact, where the base model stays general.


### Measuring the shift outside the targeted capability


When a model is pushed hard on one kind of work, it often loses ground on everything else. Rather than measuring generalization across many benchmarks that target the same capability, we looked for regressions on work the model never practiced. We compared the distilled Nemotron model with the base on graduate-level reasoning, broad knowledge, competition math, and code. The scores hold. Across GPQA, MMLU-Pro, AIME, LiveCodeBench, and SciCode, every score stays within run-to-run noise of the published base. The GDPval improvement did not come at the expense of general ability.


Each benchmark’s change after distillation, shown with its 95% sampling interval. Every interval includes zero, so none of the shifts are statistically distinguishable from the base model: the capability the student gained on GDPval did not come at a measurable cost elsewhere.


Baseline results are NVIDIA’s published Nemotron-3 Nano numbers, produced with[NVIDIA’s NeMo Evaluator](https://huggingface.co/blog/nvidia/nemotron-3-nano-evaluation-recipe) . The distilled results are our 50-step checkpoint, evaluated with NeMo Evaluator and[NeMo Skills](https://github.com/NVIDIA/NeMo-Skills) under the sampling parameters from NVIDIA’s official[evaluation config](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16/blob/main/nemo-evaluator-launcher-configs/local_nvidia_nemotron_3_nano_30b_a3b.yaml) .


## Human data as a lever


The mechanism improving Nano is the distillation process, but it only works so well because the student is practicing on tasks that mimic real work. Our gains in performance were only possible because the AfterQuery[Off-The-Shelf Office Agent Training Dataset](https://www.afterquery.com/contact) used well-seeded workspaces, messy inputs, specific deliverables, and realistic constraints, like the structure seen in the official GDPval eval.


This same AfterQuery data was used by NVIDIA in the development of Nemotron 3 Ultra to hillclimb GDPval, as cited in their[technical report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Ultra-Technical-Report.pdf) .


## Our evaluation harness


AfterQuery’s GDPval evaluation harness follows the same broad shape as Artificial Analysis’s GDPval-AA methodology, which is built on OpenAI’s original GDPval benchmark. It runs in two stages.


The execution stage is built directly on Stirrup, the open-source agent framework Artificial Analysis developed and used for GDPval-AA. Stirrup runs a minimal Reason-Act-Observe loop, gives the model a small set of tools, and lets the model choose its own approach. The tools include shell and code execution, web search, web fetch, image viewing, and a finish tool.


The grading stage is where our setup differs. Artificial Analysis uses Gemini 3.1 Pro for pairwise comparisons between anonymized model submissions, then turns those comparisons into an Elo-style ranking. We grade responses directly against OpenAI’s public GDPval rubrics from the[Hugging Face release](https://huggingface.co/datasets/openai/gdpval) . Three Gemini 3.1 Pro graders independently mark each criterion, and majority vote gives the final weighted score.


Get in touch[here](https://www.afterquery.com/contact) to access our off-the-shelf GDPval, Office Agent, and agentic post-training datasets, or reach out to us directly atresearch@afterquery.com .


AfterQuery is an applied research lab curating data solutions to accelerate foundation model development.


## References


1. Lu, Kevin, and Thinking Machines Lab. *On-Policy Distillation.* *Thinking Machines Lab: Connectionism* , Oct. 2025.[thinkingmachines.ai/blog/on-policy-distillation](https://thinkingmachines.ai/blog/on-policy-distillation/) .[doi:10.64434/tml.20251026](https://doi.org/10.64434/tml.20251026) .
2. Agarwal, R., Vieillard, N., Zhou, Y., Stanczyk, P., Ramos, S., Geist, M., Bachem, O. *On-Policy Distillation of Language Models, Learning from Self-Generated Mistakes.* ICLR 2024. The modern reference for on-policy distillation of language models; introduces the Generalized Knowledge Distillation (GKD) framework.[arxiv.org/abs/2306.13649](https://arxiv.org/abs/2306.13649)
3. Ross, S., Gordon, G. & Bagnell, J. A. *A Reduction of Imitation Learning and Structured Prediction to No-Regret Online Learning.* AISTATS 2011. The DAgger paper, origin of the on-policy framing as a way to avoid the exposure-bias failure mode of pure behavior cloning.[arxiv.org/abs/1011.0686](https://arxiv.org/abs/1011.0686)
4. Minka, T. *Divergence measures and message passing.* Microsoft Research Technical Report MSR-TR-2005-173. The canonical reference for the mode-seeking behavior of reverse-KL vs. the mode-covering behavior of forward-KL.[microsoft.com/en-us/research/publication/divergence-measures-and-message-passing](https://www.microsoft.com/en-us/research/publication/divergence-measures-and-message-passing/)
5. NVIDIA. *NVIDIA Nemotron 3, Efficient and Open Intelligence* (white paper,[arXiv 2512.20856](https://arxiv.org/abs/2512.20856) ); and *Nemotron 3 Nano, Open, Efficient Mixture-of-Experts Hybrid Mamba-Transformer Model for Agentic Reasoning* ([arXiv 2512.20848](https://arxiv.org/abs/2512.20848) ). Model cards are[Nemotron-3-Nano-30B-A3B (HF)](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16) and[Nemotron-3-Super-120B-A12B (HF)](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Super-120B-A12B-BF16) .
6. OpenAI. *GDPval, an economically-grounded agent benchmark.*[openai.com/index/gdpval](https://openai.com/index/gdpval/) ·[dataset (HF)](https://huggingface.co/datasets/openai/gdpval)
7. Artificial Analysis. *Intelligence benchmarking methodology.*[artificialanalysis.ai/methodology/intelligence-benchmarking](https://artificialanalysis.ai/methodology/intelligence-benchmarking)
