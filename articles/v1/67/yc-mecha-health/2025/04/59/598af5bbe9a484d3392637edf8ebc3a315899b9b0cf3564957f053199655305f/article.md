---
schema_version: "1.0.0"
document_id: "598af5bbe9a484d3392637edf8ebc3a315899b9b0cf3564957f053199655305f"
company_key: "yc-mecha-health"
company: "Mecha Health"
source_id: "yc-mecha-health-news-import-dc619b8bf4eb"
canonical_url: "https://www.mecha-health.ai/blog/Interpreting-a-radiological-foundation-model"
published_at: "2025-04-15T00:00:00+00:00"
first_seen_at: "2026-08-10T00:30:28.783990+00:00"
fetched_at: "2026-08-10T00:30:30.164114+00:00"
content_hash: "sha256:54ce609dc627b1e4a66899166842f7164aa366e629a557274b6fcc143e6336e1"
---

# Interpreting a radiological foundation model

Last year, Anthropic demonstrated that it was possible to[extract individual and human-interpretable features from a general-purpose foundation model](https://transformer-circuits.pub/2024/scaling-monosemanticity/) . Much of this work focussed on text, and the team was able to discover specific features that corresponded to human-interpretable concepts such as the Golden Gate Bridge, coding errors, the immune system, gender bias awareness, and more.


We were curious whether similar techniques could be applied to a radiological foundation model. In this article, we will explore the results of our experiments and the implications for the field of radiology.


To begin, we will introduce the concept of a radiological foundation model and the method we used to extract features. Next, we will present our findings and discuss their significance. Finally, we will conclude with some thoughts on the future of radiological foundation models.


## What is a radiological foundation model?


This is an increasingly common term in the field of radiology without a standard definition. There are at least three common 'flavours' of radiological foundation models:


1. **Foundation models as vision encoders** : These models are trained to extract features from radiological images, such as CT scans or X-rays. They are typically used as a starting point for other tasks, such as image classification or segmentation.


Rendering diagram…


1. **Foundation models as vision-language systems** : These models are trained to understand the relationship between radiological images and associated text, such as radiology reports. They are typically used for tasks such as image captioning or report generation.


Rendering diagram…


1. **Foundation models as multimodal systems** : These models are trained to understand the relationship between radiological images, associated text, and other modalities, such as patient metadata. They can be used to produce multimodal outputs and can thus be used for image classification, segmentation, and report generation.


Rendering diagram…


In our work \[1\], we created a "1.5" system, which can use human-interpretable features (extracted from a foundation model vision encoder) to produce a radiology report. In order to do this, we first extracted interpretable features from a foundation model vision encoder. This was achieved by using a sparse autoencoder.


## Sparse autoencoders


A sparse autoencoder (SAE) is a type of neural network that is trained to reconstruct its input while also enforcing a sparsity constraint on the hidden layer. This means that only a small number of neurons in the hidden layer are allowed to be active for a given input. This can be useful for feature extraction, as it encourages the model to learn a small number of important features that can be used to reconstruct the input.


Rendering diagram…


The diagram above illustrates the key components of an SAE:


1. The encoder transforms the input image into a sparse hidden representation where only a small subset of neurons are active.
2. Sparsity is enforced through regularization techniques like L1 regularization.
3. The decoder attempts to reconstruct the original input from this sparse representation.
4. The network is trained with two objectives: 1. Minimize reconstruction error; 2. Maintain sparsity in the hidden layer.
5. This process leads to the discovery of human-interpretable features.


### SAEs in radiology


We trained an SAE on a foundation model vision encoder \[1\]. The effect of this was to learn a set of features that were human-interpretable. For each image, a small number of features were activated in our SAE, and we found that often, they corresponded to human-interpretable concepts. In the following sections we will explore some of these features.


## Visualizing the feature space in a foundation model


This interactive visualization shows the feature space of our radiological foundation model. Each point represents a learned feature, and you can filter by a small number of illustrative categories to explore specific aspects of the model's understanding.


Loading...


### High-level feature analysis


Features in the model can be grouped into high-level categories based on their activation patterns. For example, the model has learned a large number of features related to **cardiac devices** (such as pacemakers). This set of cardiac device features (highlighted in orange) is visualized below:


Loading visualization…


Distribution of learned radiological features, with pacemaker-related features highlighted.


Similarly, the model has identified distinct features related to **post-surgical hardware** , which are highlighted in green in the visualization below:


Loading visualization…


Distribution of learned radiological features, with post-surgical hardware features highlighted.


Some categories are very well represented in the model, such as the representation of **clear lung fields** . This category in particular has a large number of features, which suggests that the model has learned to identify this concept well. The visualization below shows the features that correspond to clear lung fields highlighted in red-orange:


Loading visualization…


Distribution of learned radiological features, with clear lung field features highlighted.


Other categories (such as **orthopaedic implants** ) have fewer features. This is likely due to the rarity of these findings in the training data. The model has learned to identify these features, but they are not as well represented as other categories:


Loading visualization…


Distribution of learned radiological features, with orthopaedic implant features highlighted.


We can draw two conclusions from this analysis:


1. **The model has learned a number of features related to specific radiological concepts** , such as cardiac devices and post-surgical hardware.
2. **The model's understanding of these concepts is not uniform** ; some categories are well represented, while others are less so. This suggests that the model's performance may vary depending on the specific concept being considered.


With regards the second point, the most probable reason for this is the distribution of training data. The model was trained on a large dataset of radiological images, but some concepts are much rarer than others. For example, the model has learned to identify features related to clear lung fields well, but it has fewer features related to orthopaedic implants. This finding aligns with recent work by Anthropic which showed that[Oversampling a Topic in the SAE Training Set Results in More Detailed Features Related to that Topic](https://transformer-circuits.pub/2024/september-update/index.html#oversampling) \[2\].


### Highest activating images


For each feature, we can visualize the images that most strongly activate that feature. This allows us to understand what the model has learned and how it interprets different radiological concepts. For instance, here are the highest activating images for **feature 17184** , which clearly corresponds to the presence of a left-sided pacemaker:


Figure: Highest activating images for feature 17184 - this feature captures the presence of a left-sided pacemaker.


**Feature 42033** is another example, which appears to correspond to the presence of median sternotomy wires. The images below show the highest activating images for this feature:


Figure: Highest activating images for feature 42033 - this feature captures the presence of median sternotomy wires.


**Feature 46263** demonstrates the model's ability to identify thoracic spinal hardware. The images below show the highest activating images for this feature:


Figure: Highest activating images for feature 46263 - this feature captures the presence of thoracic spinal hardware.


As can be seen from the highest activating images, the model has learned to identify these features well. This is a promising result for the use of foundation models in radiology, as it shows that they can demonstrably learn clinically relevant features in radiological images.


## Limitations


While our findings are promising, there are several limitations to using SAEs for feature extraction in radiology:


1. **Interpretability** : While SAEs can learn human-interpretable features, the interpretation of these features is not always straightforward. We used automated interpretability techniques to label our features similar to the pipeline described[here](https://transformer-circuits.pub/2023/monosemantic-features/index.html#appendix-automated) \[3\]. Such approaches can fail. For example, looking at a specific region of our feature space reveals a cluster of 'horizontal' chest x-rays -- the feature here is clearly about orientation, but the automated descriptors are wrong. The visualization below zooms in on this region -- clicking on any points in the plot will show the corresponding images:


Loading...


1. **Accuracy** : It has recently been demonstrated that SAEs can under-perform probes and this is important to bear in mind when interpreting the results above. A short description of these findings can be read in the following X thread:


> [View this post on X](https://x.com/i/web/status/1894385838487642346)


## Conclusion


In this article, we explored the use of sparse autoencoders to extract human-interpretable features from a radiological foundation model. We found that the model has learned a number of features related to specific radiological concepts, such as cardiac devices and post-surgical hardware. However, the model's understanding of these concepts is not uniform, and some categories are better represented than others.


It is clear that foundation models have the potential to learn clinically relevant features in radiological images. However, there are still challenges to overcome, such as interpretability and accuracy. At Mecha Health we are committed to addressing these challenges and advancing the field of radiology by developing next generation highly accurate models.


If you would like to cite this work, please use the following BibTeX entry:


```text
@misc{mecha2025interpreting,
author = {Ahmed Abdulaal and Hugo Fry and Ayodeji Ijishakin and Nina Montaña Brown},
title = {Interpreting a radiological foundation model},
year = {2025},
month = {April 18},
url = {https://www.mecha-health.ai/blog/Interpreting-a-radiological-foundation-model},
note = {On understanding and interpreting radiological foundation models}
}


```


## References


1. Abdulaal, Ahmed, et al. "An X-Ray Is Worth 15 Features: Sparse Autoencoders for Interpretable Radiology Report Generation." arXiv preprint arXiv:2410.03334 (2024).
2. Anthropic Interpretability Team. (2024, September). Circuits updates — September 2024. Transformer Circuits Thread.[https://transformer-circuits.pub/2024/september-update/index.html](https://transformer-circuits.pub/2024/september-update/index.html)
3. Bricken, T., McDougall, C., Cunningham, H., Henighan, T., Jermyn, A., Jones, A., Persic, A., Qi, Z., Thompson, T. B., Zimmerman, S., Rivoire, K., Conerly, T., Olah, C., & Batson, J. (2023, October). Towards monosemanticity: Decomposing language models with dictionary learning. Transformer Circuits Thread.[https://transformer-circuits.pub/2023/monosemantic-features/index.html​](https://transformer-circuits.pub/2023/monosemantic-features/index.html%E2%80%8B)


Talk to us


## Interested in next-generation AI for radiology?


[Talk to a founder](https://cal.com/team/mecha-health/30mins)


From the journal


## Continue reading


[Research Healthcare Evaluations Research · May 28, 2025 · 15 min read Why radiologists make mistakes On the reasons radiologists make mistakes and how to improve report quality. Read article](https://www.mecha-health.ai/blog/Why-radiologists-make-mistakes)[Research Healthcare Evaluations Research · January 29, 2025 · 15 min read Evaluating automated radiology reports On assessing the quality and accuracy of automatically generated radiology reports. Read article](https://www.mecha-health.ai/blog/Evaluating-automated-radiology-reports)
