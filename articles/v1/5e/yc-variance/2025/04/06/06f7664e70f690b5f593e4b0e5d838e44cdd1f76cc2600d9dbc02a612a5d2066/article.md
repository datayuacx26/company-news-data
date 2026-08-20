---
schema_version: "1.0.0"
document_id: "06f7664e70f690b5f593e4b0e5d838e44cdd1f76cc2600d9dbc02a612a5d2066"
company_key: "yc-variance"
company: "Variance"
source_id: "yc-variance-news-import-ff45b1fc661c"
canonical_url: "https://www.variance.com/blog/alignment-is-not-free"
published_at: "2025-04-29T00:00:00+00:00"
first_seen_at: "2026-07-22T18:28:25.562528+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:332e5bcb4c092726c242fc7dec8f2f614fed4404ec61945df184cfb7e0cb85d6"
---

# Alignment Is Not Free: How Model Upgrades Can Silence Your Confidence Signals

## The Flattening Calibration Curve


The post-training process for LLMs can bias behavior for language models when they encounter content that violates their safety post-training guidelines. As mentioned by OpenAI's GPT-4 system card, model calibration rarely survives post-training, resulting in models that are extremely confident even when they're wrong.1 For our use case, we often see this behavior with the side effect of biasing language model outputs towards violations, which can result in wasted review times for human reviewers in an LLM-powered content moderation system.


Pre-training vs. Post-preference optimization calibration curves


## A Working Signal on GPT-4o


Take the below histogram of log probs sampled from a golden dataset of false positives against GPT-4o. We can see that almost all outputs have log p≈0 nats (probability ≈ 1) for outputting "true", indicating a true violation in this dataset.


However, there are a few outliers in this dataset, almost all of which correspond to patterns of behavior we observed in our dataset when our model would stray away from formal grounded policy definitions, or hallucinations in content or policy violations.


The functional confidence signal in GPT-4o


This results in a functional enough ROC curve that's helpful for calibrating our model to ignore these outputs, and perform tasks like flagging the content for review or suppress the output as likely spurious.


## The Upgrade That Vanished Uncertainty


However, what we found is that after switching to **GPT-4.1-mini** , this signal vanishes. Although we're still able to measure log probs for other tokens in our structured outputs, each token was 100% confident that it should return **true** in this dataset, which completely destroyed our signal.


Why does a smaller sibling of the same model family erase so much information? It's possible that due to the heavy distillation that occurs to train 4-1 mini for binary decisions (such as outputting a boolean field in a structured output), the dimension is collapsed entirely: the student is taught to emit the right answer and ignore entropy at all. This results in no usable confidence signal.


We tried several other approaches to recover the lost uncertainty signal, all unsuccessful:


1. **Entropy differential hypothesis** : We measured entropy between content array vs. chain-of-thought mean, with the theory that hallucinated violations would be wordier/less confident. In practice, we were unable to find a signal here
2. **Span consistency check** : We analyzed standard deviation of span log-probs, hoping for variation between true/false cases. In practice, both classes showed σ≈0.018 (identical).
3. **Perplexity analysis** : We calculated token-level perplexity averages across all samples. In practice, we found similar metrics for every sample, safe or unsafe.


Failed attempts to recover uncertainty signals in GPT-4.1-mini


The net result is that we've lost our signal for hallucinations! All of these features rely on local entropy surviving RLHF, and we don't have anywhere to look for these signals, requiring new heuristics for model upgrades to solve these failure cases, to re-introduce some uncertainty measures.


In response to this lost hallucination signal, we've implemented several alternative safeguards. These new methods, such as formally requiring policy explanations to be fully grounded in actual data/quotes, are powering new features in our product towards better explainability and policy iteration, but do show how there's more to model upgrades than simply benchmark upgrades.


Our current approach relies on more explicit controls: **requiring detailed explanations from the model for each policy violation** , **demanding specific policy citations to ground decisions** , and **implementing filtering systems to catch corrupted outputs when policies are hallucinated** .


However, the closed-source nature of these models significantly limits our access to internal signals beyond log probabilities. As models continue to be further distilled for efficiency, even these limited signals are fading, creating a growing challenge for reliable uncertainty detection especially when working with closed-source models.


## Alignment Isn't Free


In our situation, the improvements to steerability and performance upgrades of 4.1 were worth it for customers and our internal workarounds were sufficient to actually increase precision with our latest release. A model upgrade is not merely a drop-in performance bump; it is a distributional shift that can invalidate an entire AI stack. Anyone shipping high-precision systems should log raw logits, tie heuristics to specific model versions, and invest in alternative product safeguards. Alignment makes models safer for users but simultaneously masks their own uncertainty from engineers; the burden of re-exposing that uncertainty falls on us.


> 1. *OpenAI GPT-4 System Card*, §6.2 "Calibration": "We observe that RLHF improves helpfulness but can distort the model's probability estimates; after alignment the model tends to be over-confident on both correct and incorrect answers."
