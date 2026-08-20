---
schema_version: "1.0.0"
document_id: "2e6de115f14850154094c6b0f19020db36ed223074d5df371c5688b2f150c0a8"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/mvicad2-delays-dilations"
published_at: null
first_seen_at: "2026-08-18T22:05:34.065730+00:00"
fetched_at: "2026-08-18T22:05:36.322330+00:00"
content_hash: "sha256:b958af87995c062b7b16e2d8db00c1cdc550e8d9e725559c9ed3d403aa356503"
---

# MVICAD2: Multi-View Independent Component Analysis with Delays and Dilations

[View publication](https://arxiv.org/abs/2501.07426)


Machine learning techniques in multi-view settings face significant challenges, particularly when integrating heterogeneous data, aligning feature spaces, and managing view-specific biases. These issues are prominent in neuroscience, where data from multiple subjects exposed to the same stimuli are analyzed to uncover brain activity dynamics. In magnetoencephalography (MEG), where signals are captured at the scalp level, estimating the brain’s underlying sources is crucial, especially in group studies where sources are assumed to be similar for all subjects. Common methods, such as Multi-View Independent Component Analysis (MVICA), assume identical sources across subjects, but this assumption is often too restrictive due to individual variability and age-related changes. Multi-View Independent Component Analysis with Delays (MVICAD) addresses this by allowing sources to differ up to a temporal delay. However, temporal dilation effects, particularly in auditory stimuli, are common in brain dynamics, making the estimation of time delays alone insufficient. To address this, we propose Multi-View Independent Component Analysis with Delays and Dilations (MVICAD2), which allows sources to differ across subjects in both temporal delays and dilations. We present a model with identifiable sources, derive an approximation of its likelihood in closed form, and use regularization and optimization techniques to enhance performance. Through simulations, we demonstrate that MVICAD2 outperforms existing multi-view ICA methods. We further validate its effectiveness using the Cam-CAN dataset, and showing how delays and dilations are related to aging.


- † University Paris-Saclay, Inria Saclay, and CEA
- ‡ CREST at ENSAE
