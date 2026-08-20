---
schema_version: "1.0.0"
document_id: "ad880c940bc6133503e0986bd79bd7341b29c77473158a1e8ff91a833cf967a6"
company_key: "yc-prolific"
company: "Prolific"
source_id: "yc-prolific-news-import-3a1392a67315"
canonical_url: "https://www.prolific.com/resources/post-training-an-llm-with-human-data-an-end-to-end-sft-and-dpo-walkthrough"
published_at: null
first_seen_at: "2026-08-11T00:25:59.632542+00:00"
fetched_at: "2026-08-11T00:26:00.293676+00:00"
content_hash: "sha256:07f6ea4fecba59aaa6f8c514f173eba6377f2936bcc751e6f0b7674730685e75"
---

# Post-training an LLM with human data: an end-to-end SFT and DPO walkthrough

Most post-training write-ups treat human data as a given. The


[TRL documentation](https://huggingface.co/docs/trl/dpo_trainer) tells you what format your preference pairs need to be in. The


[DPO paper](https://arxiv.org/abs/2305.18290) tells you what to do with them once you have them. Very little in the public literature walks through the step in between: how the demonstrations and preference judgments actually get collected, what that costs, how long it takes, and which quality decisions along the way determine whether your training signal is clean or noisy.


This post walks through a complete human-in-the-loop post-training pipeline, from base model to SFT to DPO, where both human data collection stages run through the


[Prolific API](https://docs.prolific.com/documentation/get-started/overview) . Everything here is open and reproducible: the


[full repository](https://github.com/prolific-oss/hitl-llm-post-training) is on GitHub, and there is a


[35-minute video tutorial](https://www.youtube.com/watch?v=vh6jUmuXLhU) in which Viviana Márquez, who built the pipeline during her time as a developer relations engineer at Prolific, runs every notebook end to end, including the parts most tutorials cut: waiting for participants, inspecting raw submissions, and deciding which preference pairs to keep.


The training target is deliberately silly. We take


[Qwen3-8B-Base](https://arxiv.org/abs/2505.09388) and teach it to answer like a helpful pirate. The underlying workflow is the same one you would use for brand voice adaptation, domain-specific assistants in medicine or law, safety fine-tuning against human judgments, structured output adherence, or aligning a model with native-speaker preferences in a low-resource language. A toy target keeps every pipeline decision visible without the domain complexity getting in the way, and it makes the before/after comparisons unambiguous.


## Why two stages


If you are already deep in post-training you can skip this section. For everyone else, the short version: a pretrained base model is a next-token predictor, and no amount of prompting turns it into an assistant. Ask a base model to redact PII from a support ticket and it will happily continue writing the support ticket, inventing new PII as it goes.


[Supervised fine-tuning](https://arxiv.org/abs/2203.02155) fixes instruction following by training on demonstration pairs: here is an input, here is what a good response looks like. Preference optimization then handles the qualities that demonstrations alone can't pin down, such as helpfulness, tone, and honesty, by training on comparisons between outputs rather than single gold answers.


Both stages have the same dependency. SFT needs human demonstrations, and DPO needs human preference judgments. The quality of the resulting model is bounded by the quality of that data: a thousand carefully curated demonstrations can outperform far larger, noisier datasets. Which is why the collection step deserves more attention than it usually gets.


## The pipeline at a glance


The repository is organized as eight self-contained notebooks. Prolific handles both human data stages (notebooks 2 and 6).


[Tinker](https://thinkingmachines.ai/tinker/) , the training API from Thinking Machines, handles sampling and fine-tuning; any training stack with an SFT and DPO trainer, such as


[TRL](https://huggingface.co/docs/trl/dpo_trainer) , slots into the same positions.


#


Notebook


What it does


Human data?


1


1_tinker_generate_base.ipynb


Sample base model responses for 100 SFT prompts


2


2_prolific_sft_collection.ipynb


Collect human rewrites via the Prolific API


Free-text demonstrations


3


3_tinker_sft_training.ipynb


LoRA SFT on the human rewrites


4


4_tinker_compare_base_sft.ipynb


Sanity check: base vs SFT outputs


5


5_tinker_generate_sft_pairs.ipynb


Sample response pairs from the SFT model for 1,000 prompts


6


6_prolific_dpo_collection.ipynb


Collect pairwise preferences via the Prolific API


Preference judgments


7


7_tinker_dpo_training.ipynb


DPO on the SFT model


8


8_tinker_compare_all.ipynb


Three-way comparison: base / SFT / DPO


Every study configuration lives in version-controlled YAML rather than in notebook cells, so task instructions, payment, participant counts, and screeners are all diffable and reusable. API tokens are created from the Prolific workspace sidebar; workspace and project IDs can be pulled


[from the API](https://docs.prolific.com/documentation/get-started/overview) or read straight out of the app URLs.


## Stage 1: collecting SFT demonstrations


The design decision that shapes this stage: rather than asking participants to write pirate responses from a blank page, notebook 1 first samples a base-model response for each of the 100 prompts, and participants are shown that draft as a reference to rewrite. Editing is a cognitively cheaper task than authoring, and it anchors responses to the content of the original while participants concentrate on the style transformation. The output of notebook 1 is a plain JSONL of


{prompt_id, prompt, response}


rows that becomes the upload file for the study.


Collection runs through


[AI Task Builder](https://github.com/prolific-oss/prolific-ai-task-builder-getting-started) , Prolific's workflow for structured annotation and data collection tasks. The study definition is a YAML file, and it is worth reading closely because task instructions are the highest-leverage input in the whole pipeline. The instructions here don't say "respond like a pirate" and leave it there; they define the task, walk through the steps, and set expectations for authenticity and completeness. Sloppy instructions produce sloppy demonstrations, and no amount of downstream filtering fully recovers from that. This is the same lesson the annotation literature keeps relearning: much of what looks like annotator noise is


[underspecified task design](https://arxiv.org/abs/2211.02570) .


The screener choices are similarly deliberate. Residence in the US or UK, first language English, broad age range. Pirate register is a culturally specific, linguistically demanding style task, so first-language fluency was screened for on quality grounds rather than by habit. Prolific reported roughly 165,000 active participants matching those criteria at launch, out of a pool of more than 300,000 people active on the platform in the previous 90 days. One participant per prompt, since a demonstration needs to exist once, and redundancy budget is better spent later where disagreement carries signal.


The economics, because tutorials never publish them: each participant handled 10 prompts at an estimated two minutes per rewrite, so the study was set at 20 minutes with a £5 reward, roughly £15 per hour. The platform shows the effective hourly rate at setup, and the right way to read it is as a quality parameter as much as an ethics one. Fair pay recruits fast and keeps effort high; domain experts price higher. In this run, 10 participants completed all 100 rewrites in 1 hour and 4 minutes end to end, with a median completion time close to the 20-minute estimate.


The raw submissions export with full audit trail and optional participant demographics, and a short processing step reshapes them into standard chat-format SFT data. Everything in this stage, dataset creation, batch definition, study creation, publishing, and result retrieval, happens through API calls from the notebook. The video shows the corresponding UI state after each call, which is a useful way to learn the object model, but nothing requires clicking through the app. That matters if the endgame is human data collection as a step inside a larger training pipeline rather than a manual side process.


## SFT training, briefly


Notebook 3 runs LoRA fine-tuning through Tinker with rank 16. The rationale for a low rank is worth stating since it generalizes:


[LoRA rank](https://arxiv.org/abs/2106.09685) scales with how much new capability you are trying to add, and this task adds none. It nudges an 8B model toward a response style, so a small, cheap, constrained adapter is the right tool. Over training, mean negative log-likelihood on the target tokens drops from 2.82 to 0.60, meaning the model becomes progressively less surprised by human-written pirate responses. Notebook 4 is a plain sanity check, sampling the base and SFT models side by side on held-out prompts. The SFT model reliably answers in pirate register. It is not yet reliably helpful, which is the point of the second stage.


## Stage 2: collecting DPO preferences


Notebook 5 uses the SFT model to generate a response pair for each of 1,000 prompts. Notebook 6 then puts those pairs in front of humans.


Two design decisions here separate a usable preference dataset from an expensive pile of coin flips.


First, the instructions never ask "which response do you prefer?" Unconstrained preference is exactly the kind of underspecified question that manufactures disagreement, because annotators fall back on private and inconsistent criteria. The task instead operationalizes the training target: select the response that is more helpful, meaning more detailed, more practical, more directly useful. If you are optimizing for helpfulness, ask about helpfulness. The gap between the preference you want to train on and the question you actually asked is a data quality failure that no optimizer can see, and it is invisible in every downstream metric until the model behaves strangely.


Second, every pair was judged by five independent annotators rather than one. That multiplies cost by five and buys the single most valuable artifact in the pipeline: an agreement distribution.


## The agreement filter


With five judgments per pair, every pair lands somewhere between 5-0 unanimity and a 3-2 split. In this run the distribution looked the way preference data usually does: a large mass of unanimous or near-unanimous pairs, a smaller band of 4-1s, and a tail of 3-2 splits where the two responses were genuinely close, or the criterion genuinely ambiguous, or both.


The pipeline trains only on the 4-1 and 5-0 pairs and drops the rest.


That is a real trade-off, not a free win. Discarding split decisions shrinks the dataset and throws away pairs that may contain fine-grained signal about hard cases. What it buys is a clean margin:


[DPO's objective](https://arxiv.org/abs/2305.18290) pushes the policy to widen the likelihood gap between chosen and rejected responses, and when the "chosen" label is only weakly better than the rejected one, that gradient is teaching the model a distinction humans themselves don't reliably make. For a small dataset and a sharply defined target, filtering to high-agreement pairs is the defensible call. For subjective or pluralistic targets, the disagreement itself is data, and there is a growing research thread arguing it should be


[modeled rather than discarded](https://arxiv.org/abs/2211.02570) . Either way, you can only make that decision if you collected redundant judgments in the first place. Single-annotator preference datasets don't give you the choice; they just silently mix both regimes into your training data.


The operational numbers again, since they are the part you can't get from a paper: 250 participants provided the five-way judgments across all 1,000 pairs, averaging 11 minutes each on a selection task paid proportionally less than the writing task. The full preference dataset was complete in 2 hours and 22 minutes. Recruitment was not the bottleneck at any point in either study; participants began arriving within minutes of publishing, and total wall-clock time was dominated by task length. The high-agreement pairs export directly into the comparison format DPO trainers expect, with the raw five-way votes preserved alongside as an audit trail.


## DPO training and an honest read of the metrics


Notebook 7 runs DPO on the SFT checkpoint. The headline metric, DPO classification loss, falls from 0.65 to roughly 1.2e-5, with training accuracy reaching 100%: the model ends training able to perfectly separate chosen from rejected responses in its training batches. On a filtered, high-margin dataset of this size that is at least partly a warning sign, and the tutorial says so on camera. Perfect separation of a small preference set is consistent with memorization, and the honest verdict has to come from held-out behavior rather than the loss curve.


Notebook 8 provides it, sampling base, SFT, and DPO checkpoints side by side. On "How do I apologize properly?", the base model wanders into meta-commentary about the question instead of answering it. The SFT model answers in confident pirate register but thinly. The DPO model answers in pirate register with structure, detail, and actionable steps. The style came from stage one; the helpfulness came from stage two; and the separation between the two failure modes is exactly why the stages are separate.


## What transfers to real workloads


Strip out the pirate and the pipeline shape is unchanged for production use cases: participants rewriting model outputs into your brand voice, clinicians rewriting responses for a medical assistant and then adjudicating pairs, safety raters producing the judgments behind a harmlessness objective, native speakers aligning a model in a language your team doesn't have in-house. The pieces that carry over directly are the ones this post has dwelt on, because they are the ones that determine data quality regardless of domain: reference-anchored writing tasks over blank-page authoring, operationalized preference criteria over vague ones, screeners chosen for the task rather than by default, fair pay treated as a quality parameter, redundant judgments, and an explicit, recorded policy for disagreement.


A caveat that belongs in any credible version of this post: not every model change needs this. Regression checks, format validation, and cases with encodable success criteria are well served by automated evaluation, and


[LLM-as-judge](https://arxiv.org/abs/2306.05685) scales where humans can't. Human data earns its cost where the target is judgment itself: preference, trust, domain correctness, cultural context, or how an output actually lands with a person. Post-training targets sit squarely in that category, which is why every serious post-training pipeline since


[InstructGPT](https://arxiv.org/abs/2203.02155) has had humans in the loop somewhere.


The loop also doesn't end at training. The same infrastructure that collects training data collects evaluation data, and evaluating with humans at scale is its own discipline; that is the motivation behind


[HUMAINE](https://huggingface.co/spaces/ProlificAI/humaine-leaderboard) , Prolific's benchmark for how models perform with real, demographically representative humans, with the methodology, dataset, and paper public. If you want to see how practitioner teams combine automated and human evaluation in practice,


[this write-up from teams at Microsoft, Amazon, and Braintrust](https://www.prolific.com/resources/how-the-best-ai-teams-evaluate-at-speed-lessons-from-microsoft-amazon-and-braintrust) is a good companion piece.


## Run it yourself


The


[repository](https://github.com/prolific-oss/hitl-llm-post-training) contains every notebook, config, and output schema described here, and the


[video walkthrough](https://www.youtube.com/watch?v=vh6jUmuXLhU) runs it live. It is published as an educational, beta-status resource, so treat it as a reference implementation to adapt rather than a production dependency. Swap the prompt files and task YAMLs for your own use case; the pipeline shape holds. If you are starting from zero, the


[survey getting-started repo](https://github.com/prolific-oss/prolific-survey-getting-started) is the fastest first API call, the


[AI Task Builder getting-started repo](https://github.com/prolific-oss/prolific-ai-task-builder-getting-started) covers the collection workflow used here in isolation, and the


[API documentation](https://docs.prolific.com/documentation/get-started/overview) is the canonical reference. A


[CLI](https://github.com/prolific-oss/cli) and an


[MCP server](https://github.com/prolific-oss/prolific-mcp) exist for teams that want Prolific inside scripted or agentic workflows.
