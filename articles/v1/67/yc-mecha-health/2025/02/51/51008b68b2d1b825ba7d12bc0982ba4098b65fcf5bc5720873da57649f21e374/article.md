---
schema_version: "1.0.0"
document_id: "51008b68b2d1b825ba7d12bc0982ba4098b65fcf5bc5720873da57649f21e374"
company_key: "yc-mecha-health"
company: "Mecha Health"
source_id: "yc-mecha-health-news-import-dc619b8bf4eb"
canonical_url: "https://www.mecha-health.ai/blog/Mecha-net-v0.1"
published_at: "2025-02-07T00:00:00+00:00"
first_seen_at: "2026-08-10T00:30:28.783990+00:00"
fetched_at: "2026-08-10T00:30:30.164114+00:00"
content_hash: "sha256:b5a0b1f23adc3b77be24ee7d5c259510d33135258ab7bcdfd3e3568cf90b2f6e"
---

# Mecha Net v0.1

## Enter Mecha Net


Today we're pleased to announce Mecha Net v0.1, the first version of our foundation model for Chest X-Ray (CXR) report generation. Most commercially available models for this task today have a narrow scope, detecting one or two abnormalities at a time - detecting 50 abnormalities usually amounts to requiring 50 different models.


Mecha Net, on the other hand, is a generalist system that aims to produce full reports by direct analysis of a scan. This process requires training powerful transformer-based vision encoders on a large number of images, and then using the outputs of such a model to produce full reports in the target language.


## Performance Evaluation


We compare Mecha Net to the current state-of-the-art (SOTA) models for the task of report generation for CXRs. In particular, we compared to finetunes of OpenAI’s GPT-4V model, the MAIRA systems from Microsoft, Google Deepmind’s Med-PaLM M, and Harvard’s Medversa.


We report the CheXbert F1 metrics which are computed on the CheXbert labeller outputs. This labeller recovers 99.13% of the performance of a radiologist on reporting 14 clinically important findings in CXRs \[1\]. Uncertain findings are taken as negative, as per prior work. We discuss CheXbert in greater detail in our[evaluations post](https://www.mecha-health.ai/blog/Evaluating-automated-radiology-reports) .


First, we report the micro-averaged F1 scores across all 14 labels:


Performance comparison


### CheXbert Micro F1 scores


Higher is better


As can be seen, the Mecha Net models outperform all prior models where these metrics are available.


Next, we report the macro-averaged F1 scores across all 14 labels:


Performance comparison


### CheXbert Macro F1 scores


Higher is better


Once more, the Mecha Net models outperform all prior models where these metrics are available.


We additionally report the RadCliQ score (version 0) \[2\] to allow for comparison with MedVersa. Lower scores are better. Mecha Net 0.1 achieves a RadCliQ score of 2.66 \[2.63, 2.68\], outperforming MedVersa (2.71 \[2.66, 2.75\]) and overlapping with MAIRA-2 (2.64 \[2.61, 2.67\]), which had the best score in this evaluation. Given the overlapping bounds between Mecha Net 0.1 and MAIRA-2, their performance is nearly indistinguishable within the margin of uncertainty, suggesting that Mecha Net 0.1 performs at a top-tier level in clinical reasoning.


## A note on auxiliary data


It should be noted that earlier versions of our system (labelled 'Mecha Net 0.0.1 and 0.0.2') are already able to outperform the current SOTA **without the use of any auxiliary information** . This means these models are able to produce reports from a single image, without the use of information such as:


- Patient history.
- Prior studies.
- Lateral scans.
- Segmentation masks.
- Multi-modal imaging for transfer learning.
- Baseline patient demographics data.


Naturally, we are very excited about the potential of our system when these auxiliary data sources are used.


The Mecha Net 0.1 model has access to the *'indication'* of a scan (the clinical reason for which the scan was ordered), as well as up to *two previous written reports* (only the text of the reports is used, no images are used). This model is able to outperform all prior models. However, as mentioned, we are still working on the best auxiliary data sources, and how best to use them.


## Next Steps


In future, we intend to:


- Perform more detailed evaluations of our system.
- Trial the system in a clinical setting, where feedback is accrued directly from radiologists.
- Investigate the optimal strategy for auxiliary data.
- Actively investigate and improve the underlying algorithms of our systems themselves.


You'll note the version number is still only 0.1 - we are very far from version 1.0, and are extremely excited about the journey to get our system to that point. In the end, we will ensure that no one has to wait for their scan results ever again.


## References


1. Smit, Akshay, et al. "CheXbert: combining automatic labelers and expert annotations for accurate radiology report labeling using BERT." arXiv preprint arXiv:2004.09167 (2020).
2. Yu F, Endo M, Krishnan R, et al. Evaluating Progress in Automatic Chest X-Ray Radiology Report Generation. medRxiv 2022.
