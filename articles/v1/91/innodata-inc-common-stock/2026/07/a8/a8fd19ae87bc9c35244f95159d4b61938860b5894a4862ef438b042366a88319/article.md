---
schema_version: "1.0.0"
document_id: "a8fd19ae87bc9c35244f95159d4b61938860b5894a4862ef438b042366a88319"
company_key: "innodata-inc-common-stock"
company: "Innodata Inc."
source_id: "innodata-inc-common-stock-rss-09b9ff75e5ec"
canonical_url: "https://innodata.com/rethinking-long-context-llm-evaluation/"
published_at: "2026-07-13T19:46:46+00:00"
first_seen_at: "2026-07-20T23:17:35.625386+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:0c61722fc44666e6579aa99eb1a4e43391296d38f40179e8b85aad85f6911f7a"
---

# What’s in a Benchmark? Rethinking “Thinking” in Long Context Conversations

# What's in a Benchmark? Rethinking “Thinking” in Long Context Conversations


#### Caitlin Cisar, Language Data Scientist
Jonathan Steuck, VP of LLM Data Services


#### July 13, 2026


When a frontier model fails in the real world, it rarely does so on a straight-forward, single-turn prompt with a well-defined intent. It fails midway through a long conversation. It forgets the instruction you gave three turns ago. It answers from memory, even though you’ve handed it a document with the answer right there.


Simply put, we


know


a model failure when we see one. However, existing benchmarks don’t seem to truly capture the failure modes that matter. Designing an effective evaluation starts with understanding what the model is contending with. To a model, the interactions are messy. For example:


- User’s needs evolve mid-conversation and can be ambiguous or vague


- Critical details are buried in long documents or images


- Instructions get revised, contradicted, or clarified


Compounding the issue is that these conditions don’t necessarily show up one at a time. They show up together, making the failures they produce emergent:


invisible in any single turn, but obvious in the whole.


To help address this, we introduce Innodata’s Long Context & Complex Interaction (LCCI) Benchmark. LCCI is a structured evaluation dataset built to surface the failure modes that emerge when long context and mixed-modality inputs combine across a conversation. While we are in the preliminary stages of testing, initial results are interesting!


## How the LCCI Benchmark Is Built


LCCI


contains


test cases that are fully specified evaluation units: a realistic


and human-


expert


authored


multi-turn conversation, the


multimodal


artifacts attached to it, structured metadata, and a multi-


component


rubric


with binary pass-fail


criteria


designed


specifically for that case.


Our


initial


set includes text, text + image, text + document, and text


+ mixed media interactions.


The


cases target


5 task


types,


so we cover a broad range of interaction types


rather than clustering dat


a


around the easy


combinations


of variables


.


These


map to model failure modes, which include


“l


ost in the middle


”


errors


(see e.g., Liu et al., 2023)


, grounding drift, instruction forgetting, modality neglect, and


structural collapse.


**Figure 1:** Task Types and Failure Modes


Specific to each case is its evaluation rubric: a list of atomic criteria generated directly from that case’s conversation and artifacts. This makes it difficult to game; a model cannot pass by producing fluent, high-quality text that misses the point. A typical rubric has 10–20 turn-level components plus 11 standardized overall performance components. The following figure shows the distribution of components in our initial dataset.


**Figure 2:** Components for Response Evaluation by Rubric Category


Each model’s response is then scored by a two-judge panel of frontier models, with a third-model to arbitrate any disagreements. To ensure validity, at least 20% of the evaluations are human-reviewed to ensure 95%+ accuracy. The completed evaluations give us two metrics that matter for everything that follows:


Component pass rate is the share of all individual rubric items a model satisfies for a given case.


Case pass rate is stricter: a case is considered a pass only when the model satisfies a threshold of 90% of that case’s rubric components. A case with one or two missed items can still pass; however, a case with broad failure across the rubric cannot.


The interactions LCCI is designed for don’t fit cleanly inside any one existing benchmark. Long-context suites stress the token-window but stay single-turn and text-only. Conversational suites cover dialogue, but lightly — capped turn counts, no attachments, no controlled complexity gradient. Multi-modal suites add one extra input channel and stop there.


**LCCI is the first benchmark we’re aware of that pressure-tests all three dimensions together — multi-turn, long-context, and multi-modal — under a controlled, case-specific rubric. The matrix below shows where each one draws the line.**


**Notes** *Multi-turn conversations: MT-Bench is capped at 2 turns; LMSYS Arena is open-ended but uncontrolled. Multi-modal inputs: VQA v2 and MMMU support image + text only — not document, audio, or video.*


**Figure 3.** Benchmark Feature Matrix, as of April 2026


## What The Numbers Show


Two flagship models, Claude Opus 4.6 and Gemini 3.1 Pro Preview, and one lightweight model, GPT-5.4 mini, have been scored so far. Across Claude and Gemini, LCCI returns


roughly a 79% case pass rate and a 92% component pass rate compared to 65% and 88%, respectively, for GPT-5.4 mini, drawn from 14,550 binary rubric components. We should note, however, that given the differences in model weights, comparing flagship models with lightweight models is not an apples-to-apples comparison. Rather, we recommend that models be compared within their weight class.


#### **Component Pass vs. Case Pass**


A 92% component pass rate achieved by flagship models sounds like a near-solved evaluation. A 79% case pass rate, on the same data, does not. The gap is the point; both numbers tell us something different.


Averaging across components means individual misses and catastrophic failures dissolve into the mean. However, not all failures are the same. Categories with component pass rates above 90% routinely show case pass rates in the 60s. For deployment, this matters. A user does not experience an average component pass rate. Tthey experience whether their specific request worked (or not) and usually to a degree of magnitude.


For diagnostics, the component-level number matters to understand the texture of failure. Each rubric defines the steps a model needs to take for the response to count as successful. When a step is missed, a failure’s


*trajectory* is then captured, and any catastrophic failures can be traced back to the point of origin, step-by-step.


**Figure 4** **.**


Rubric Component Sample for a Turn Level Component


#### **Context Length**


As the word count of user turns and artifacts increases, our hypothesis is that case pass rates should fall. While we observe this tendency most markedly with the lightweight model GPT-5-4 mini, larger models perform more consistently. In contexts up to 50K, pass rates stay above 50% for all three models but quickly fall for GPT-5.4 mini as the context length approaches 100K, the current upper bound in our dataset.i


i We are in the process of collecting and analyzing conversations with context lengths in the hundreds of thousands to 1M+ words.


**Figure 5.** Pass Rate by Context LengthA less expected result is


*what kind* of mistake dominates at each length. The data shows that Artifact Comprehension and Source Fidelity, or how well the model interprets and grounds a response in an artifact, almost always accounts for the largest proportion of failures, irrespective of length. However, the distribution of other failure modes shifts: Reasoning, Synthesis, and Problem Solving tends to account for a larger proportion as context length expands.


This means thatong context is not simply a harder version of short context. When combined across multi-turn conversations, it is a recipe for specific failure modes becoming the dominant risk.


#### **Thinking Harder, Not Better**


The intuition runs like this: thinking tokens are reasoning made visible. More of them means the model is working a problem harder, decomposing it, and checking its work. Pair that with the long-context intuition above, that bigger inputs need bigger thinking budgets, and you get a clean story that hard cases will burn more thinking tokens, and that burn is the model earning its answer.


The data, however, presents a different


story.


Across all three models, thinking-token spend is the strongest negative predictor of whether a case passes (significant after controlling for prompt length, turn count, and context band). More reasoning effort does not necessarily fix a hard case. It tracks one. In other words, the model isn’t always “working harder and succeeding”. Rather, it is working harder and failing. Two cases with the same prompt length, the same turn count, and the same context band can produce wildly different thinking-token counts, and the longer trace is the one more likely to fail.


The practical implication is that token consumption deserves a seat next to pass rate in the evaluation suite. A model that hits the same accuracy at materially lower thinking-token spend is signaling something about the ease of data. A model whose thinking-token spend is climbing on a particular workload is flagging the data that needs a second look.


Token spend, in either direction, reads as a signal of difficulty, not capability, which holds


*whether or not* the model’s top-line score in a leaderboard has moved.


**Figure 6.** Pass Rate by Thinking Tokens (Binned).


Cases with thinking-token spend above zero, binned into deciles per model. Pass rate falls in the upper deciles across all three models.


#### **Explore the Data**


The public preview dashboard makes all three views above interactive — by rubric category (component view and case view), by context length (pass rate curve and failure-mode mix), and by token spend (thinking-token bins, prompt-token bins, and the underlying logistic regression).


[Click here to see our interactive results dashboard](https://innodata.com/benchmarks/long-context-llm/)


## Why This Matters


Aggregate scores can tell us whether a model is good, but a model that scores well on standard benchmarks may still exhibit grounding drift on long documents, lose instruction constraints in conversations beyond three turns, or consume disproportionate compute on the cases that matter most for a given workload. Without a controlled framework that isolates these failure modes, those weaknesses are difficult to see until production exposes them.


[Talk to us about evaluating against your deployment's interaction profile.](https://innodata.com/contact-us)


#### **Citations**


Liu, N. F., Lin, K., Hewitt, J., Paranjape, A., Bevilacqua, M., Petroni, F., & Liang, P. (2023).


*Lost in the middle: How language models use long contexts* (arXiv:2307.03172). arXiv.


[https://doi.org/10.48550/arXiv.2307.03172](https://doi.org/10.48550/arXiv.2307.03172)


**LOFT —** Lee, J., Chen, A., Dai, Z., Dua, D., Sachan, D. S., Boratko, M., Luan, Y., Arnold, S. M. R., Perot, V., Dalmia, S., Hu, H., Lin, X., Pasupat, P., Amini, A., Cole, J. R., Riedel, S., Naim, I., Chang, M.-W., & Guu, K. (2024).


*Can long-context language models subsume retrieval, RAG, SQL, and more?* (arXiv:2406.13121). arXiv.


[https://doi.org/10.48550/arXiv.2406.13121](https://arxiv.org/abs/2406.13121)


**MMMU-PRO–** Yue, X., Zheng, T., Ni, Y., Wang, Y., Zhang, K., Tong, S., Sun, Y., Yu, B., Zhang, G., Sun, H., Su, Y., Chen, W., & Neubig, G. (2025). MMMU-Pro: A more robust multi-discipline multimodal understanding benchmark. In W. Che, J. Nabende, E. Shutova, & M. T. Pilehvar (Eds.),


*Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)* (pp. 15134–15186). Association for Computational Linguistics.


[https://doi.org/10.48550/arXiv.2409.02813](https://doi.org/10.48550/arXiv.2409.02813)


**MT BENCH–** Zheng, L., Chiang, W.-L., Sheng, Y., Zhuang, S., Wu, Z., Zhuang, Y., Lin, Z., Li, Z., Li, D., Xing, E. P., Zhang, H., Gonzalez, J. E., & Stoica, I. (2023). Judging LLM-as-a-judge with MT-Bench and Chatbot Arena. In


*Advances in Neural Information Processing Systems 36 (NeurIPS 2023), Datasets and Benchmarks Track* .


[https://doi.org/10.48550/arXiv.2306.05685](https://doi.org/10.48550/arXiv.2306.05685)


**LMSYS ARENA–** Chiang, W.-L., Zheng, L., Sheng, Y., Angelopoulos, A. N., Li, T., Li, D., Zhu, B., Zhang, H., Jordan, M. I., Gonzalez, J. E., & Stoica, I. (2024). Chatbot Arena: An open platform for evaluating LLMs by human preference. In R. Salakhutdinov, Z. Kolter, K. Heller, A. Weller, N. Oliver, J. Scarlett, & F. Berkenkamp (Eds.),


*Proceedings of the 41st International Conference on Machine Learning* (Vol. 235, pp. 8359–8388). PMLR.


[https://proceedings.mlr.press/v235/chiang24b.html](https://proceedings.mlr.press/v235/chiang24b.html)


**LONG BENCH V2–** Bai, Y., Tu, S., Zhang, J., Peng, H., Wang, X., Lv, X., Cao, S., Xu, J., Hou, L., Dong, Y., Tang, J., & Li, J. (2025). LongBench v2: Towards deeper understanding and reasoning on realistic long-context multitasks. In W. Che, J. Nabende, E. Shutova, & M. T. Pilehvar (Eds.),


*Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)* . Association for Computational Linguistics.


[https://doi.org/10.48550/arXiv.2412.15204](https://doi.org/10.48550/arXiv.2412.15204)


### Bring Intelligence to Your Enterprise Processes with Generative AI.


Innodata provides high-quality data solutions for developing industry-leading generative AI models, including diverse golden datasets, fine-tuning data, human preference optimization, red teaming, model safety, and evaluation.


[Talk to an Expert](https://innodata.com/contact-us/)


Follow Us


[Linkedin-in](https://www.linkedin.com/company/innodata-inc-)


[Facebook-f](https://www.facebook.com/Innodata.Inc/)


[X-twitter](https://twitter.com/innodata)
