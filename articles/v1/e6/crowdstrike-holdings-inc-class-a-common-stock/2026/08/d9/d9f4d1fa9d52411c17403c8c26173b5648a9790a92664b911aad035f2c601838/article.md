---
schema_version: "1.0.0"
document_id: "d9f4d1fa9d52411c17403c8c26173b5648a9790a92664b911aad035f2c601838"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/teaching-ai-to-reason-through-detection-triage/"
published_at: null
first_seen_at: "2026-08-17T23:55:39.454949+00:00"
fetched_at: "2026-08-17T23:55:41.128532+00:00"
content_hash: "sha256:a7f6e20aa207c013c64a0213a16d93dd48603a8c2c771bf53d74260b3f6c393b"
---

# Teaching AI to Reason Through Detection Triage

Every security alert begins with a deceptively simple question: Is this a real threat or just noise? Detection triage sits at the front of the response lifecycle, where answering this question quickly and accurately can determine what gets investigated, prioritized, or safely closed.


CrowdStrike’s NVIDIA Nemotron-powered detection triage models already answer this question at machine speed, reading a detection and producing a true positive (TP) or false positive (FP) verdict with a calibrated confidence score. But experienced analysts do more than arrive at a verdict. They *reason* toward weighing evidence across command lines, behavioral context, and other signals to understand not only what happened, but why it matters.


So we taught our triage model to do the same. Our latest research paper,[“Cybersecurity Detection Classification with Reasoning-enabled Language Models”](https://arxiv.org/pdf/2607.28460) (Khanna *et al.* , July 2026) which supports the[Open Secure AI Alliance](https://blogs.nvidia.com/blog/open-secure-ai-alliance/) , trains a CrowdStrike Charlotte AI™ triage classifier to reason through a detection step by step, and produce a transparent chain of reasoning, before it commits to a verdict. The result is more accurate triage, more detections that can be safely automated, and a rationale that SOC analysts can read, evaluate, and trust.


Below are the broader lessons from our findings:


- **Reasoning makes triage better and more transparent** : Teaching the model to think through a detection improves accuracy while producing an auditable rationale that analysts can evaluate.
- **More automation is done safely** : Large gains in high-confidence recall mean more benign alerts are auto-closed and more real threats can be prioritized for analysts, directly reducing alert fatigue.
- **Specialization beats scale** : A fine-tuned Nemotron 3 Nano 30B-A3B open model outperforms frontier general-purpose models many times its size on this task. (See figure 2)
- **The agentic SOC keeps advancing** : This research, currently focused on Windows endpoint detections, points to where our NVIDIA Nemotron 3 Nano 30B-A3B-powered triage is headed next, with more platforms to follow.


## From Labels to Reasoning


The conventional approach to LLM-based triage asks the model to read a detection and output a label directly. This is fast and scalable, and produces a usable confidence score; however, it treats a reasoning-driven task as an instinctive response.


Chain-of-thought reasoning changes that. Before deciding, the model works through the evidence in the detection: what the process is doing, where it came from, whether the parent-child process chain looks legitimate, and how the pieces fit together. This reasoning trace empowers better verdicts and provides an auditable explanation that an analyst can review, turning an opaque label into a decision they can stand behind.


## How We Trained the Model to Reason


Getting a model to reason well about real detections took a four-stage training recipe, each stage building on the last:


1. **Prompt optimization** : We automatically searched for the strongest possible reasoning prompt rather than hand-writing one. Crucially, we guarded the search with an LLM judge that rewarded genuine multi-field reasoning, preventing the model from collapsing into brittle numeric shortcuts that score well but don't generalize or explain anything useful.
2. **Self-training** : The model learned from its own best work. It generated reasoning traces, kept the ones that reached the correct verdict, and fine-tuned them, concentrating its effort on the hardest detections it hadn't yet mastered.
3. **Reinforcement learning with verifiable rewards** : Because a triage verdict is either right or wrong, we could reward the model directly for correct, well-formed answers. This allows it to discover better reasoning strategies on its own. Notably, it became both *more accurate and more concise* , reasoning in fewer tokens over the course of training.
4. **Confidence calibration** : Reasoning introduces a subtle problem: Once the model has argued its way to a conclusion, its final label token is nearly always near-certain, so the token's probability is no longer a trustworthy confidence signal. We solved this by training a separate calibrator that reads the full reasoning trace and estimates the probability that the verdict is correct, restoring the reliable confidence score that automated triage depends on.


## Results


The payoff shows up where it matters most: at the high-confidence operating point that governs automated triage. At this tier, detections can be auto-closed or prioritized with minimal analyst intervention, so higher recall here translates directly into more workload removed from the queue. Compared to the direct-label approach, the reasoning-enabled system surfaces dramatically more actionable detections at the same high precision.


Table 1. Recall at the high-confidence operating point (98% FP precision, 99% TP precision) on a held-out test set of real Windows endpoint detections **Verdict** **Direct-label Recall** **Reasoning Recall** **Improvement**


**False Positive** 21.8% 64.8% +43.0 percentage points


**True Positive** 34.7% 53.0% +18.3 percentage points


This 43.0 percentage point increase in high-confidence false positive recall means far more benign alerts can be automatically and safely closed, while the 18.3 percentage point gain in true positive recall means more genuine threats can be prioritized for analysts.


Just as striking is *what* delivers this performance. The reasoning system reaches 82.6% overall accuracy, well above every off-the-shelf model we tested, including frontier general-purpose models many times its size. In our comparisons, leading general-purpose models clustered around 55% to 71% accuracy on this task, roughly in line with an untrained NVIDIA Nemotron 3 Nano 30B-A3B model and well below the fine-tuned result.


Figure 2. Comparison of model performance showing overall accuracy on triage detections


The lesson is one we've seen repeatedly: For a specialized domain like triage, targeted training on real detection data can outperform raw scale. This success highlights the power and usefulness of open models. Leveraging highly capable open models provides the flexibility needed to build purpose-driven AI that can outperform much larger, closed alternatives on specialized tasks like detection triage.


#### Additional Resources


- *Learn how[CrowdStrike Falcon AI Detection and Response](https://www.crowdstrike.com/en-us/platform/falcon-aidr-ai-detection-and-response/) secures AI.*
- *[Download our guide](https://www.crowdstrike.com/en-us/resources/white-papers/five-steps-for-frontier-ai-security-readiness/) to explore the five steps for frontier AI security readiness.*
- *Explore cutting-edge cybersecurity research at[Day Zero 2026](https://www.crowdstrike.com/en-us/events/day-zero-2026/) , a summit for the threat research community.*
- *Experience Fal.Con 2026 from anywhere with[Fal.Con Digital](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) , featuring keynote livestreams and on-demand access to 100+ sessions.*
