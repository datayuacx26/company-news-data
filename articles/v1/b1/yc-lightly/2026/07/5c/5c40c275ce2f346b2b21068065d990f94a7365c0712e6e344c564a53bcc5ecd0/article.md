---
schema_version: "1.0.0"
document_id: "5c40c275ce2f346b2b21068065d990f94a7365c0712e6e344c564a53bcc5ecd0"
company_key: "yc-lightly"
company: "Lightly"
source_id: "yc-lightly-news-import-b9754ffe9935"
canonical_url: "https://lightly.ai/blog/efficient-vlm-training"
published_at: null
first_seen_at: "2026-07-24T02:26:12.148445+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:df85ce055d15cb3fc8591a57efa94f6502cbca4b21af0fbc31c7c6dbf82c36f9"
---

# Efficient Training for Multimodal Vision Models: Techniques and Trade-offs

‍


Recently, Vision Language Models have increased in popularity and industry adoption. This has largely been driven by the development of open LLMs, which enable researchers to leverage unimodal pre-trained models to create VLMs. Moreover, the literature reveals different disparate design choices.


This article examines the key techniques and trade-offs in effectively training multimodal vision models, focusing on the model performance, training time, and resource utilization trade-off.


> **💡Pro Tip:** If you want to deepen your understanding of transformer based generative architectures, our[Diffusion Transformers](https://www.lightly.ai/blog/diffusion-transformers-dit) guide explains how diffusion processes integrate with modern transformer designs.


## Multimodal Design Choices


Most early works in Multimodal models were inspired by LLMs, which have shown to be good few-shot learners (through prompting). Flamingo ([Alayrac et al. 2022](https://arxiv.org/abs/2204.14198) ) was one of the first methods that showed image or video tasks can be cast as text prediction problems with visual input conditioning provided the model can ingest a multimodal input prompt wherein images and/or videos are interleaved with text. Flamingo ([Alayrac et al. 2022](https://arxiv.org/abs/2204.14198) ) could ingest high-resolution images or videos thanks to a Perceiver-based architecture that could produce a **small fixed number of visual tokens per image/video** , given a large and variable number of visual input features. This made it natural for Flamingo to be used for in-context few-shot learning.


Since the introduction of Frozen ([Tsimpoukelli et al., 2021](https://arxiv.org/abs/2106.13884) ) and Flamingo ([Alayrac et al. 2022](https://arxiv.org/abs/2204.14198) ), most VLMs have been built on top of unimodal pre-trained backbones rather than training entirely new models from scratch. This approach involves initializing new parameters to bridge pre-trained vision and text backbones, which are fine-tuned during the pre-training phase. While effective, this strategy inherits certain limitations from the underlying LLMs, such as:


- **Hallucinations** : Generating plausible but incorrect information.
- **Poor Generalization** : Struggles with handling long input sequences, a common challenge for downstream applications.


### Training Trade-offs in Vision Language Models


When developing VLMs, researchers must balance multiple competing factors:


1. **Model Performance** :


- Leveraging pre-trained backbones ensures strong baseline performance but introduces constraints tied to the original models' capabilities.
- Fine-tuning multimodal-specific parameters can enhance adaptation but requires extensive data.


2. **Training Efficiency** :


- Building on pre-trained LLMs accelerates model development, avoiding the resource-intensive process of training from scratch.
- However, fine-tuning large-scale models is computationally expensive and demands significant GPU resources.


3. **Resource Utilization** :


- The integration of vision and text modalities increases complexity, requiring efficient memory management during training.
- Methods like the Perceiver architecture in Flamingo help optimize resource use, but scaling remains challenging.


> **💡Pro Tip:** If you are bottlenecked by GPU memory or compute during multimodal training, the[NVIDIA B200 vs H100](https://www.lightly.ai/blog/nvidia-b200-vs-h100) comparison shows how hardware choices affect scaling efficiency and cost trade-offs.


### Cross-Attention: A Critical Component


**Figure:** Flamingo architecture overview., one of the first multimodal models to use a cross-attention mechanism for fusing various modalities. **Source:**[Alayrac et al. (2022)](https://arxiv.org/abs/2204.14198)


A key mechanism in many VLMs is cross-attention, which enables interaction between vision and language modalities.[Alayrac et al. (2022)](https://arxiv.org/abs/2204.14198) introduced the cross-attention architecture, wherein the image-hidden states encoded by the vision backbone are used to condition the frozen language model using freshly initialised cross-attention layers interleaved between the pre-trained language model layers. **The keys and values in these layers are obtained from the vision features, while the queries are derived from the language inputs** . This form of in-context learning has significant advantages over gradient-based few-shot learning methods.


Practically, for cross-attention-based models, changing the backbones to a better one (in their own respective modality) leads to a performance boost under a fixed size of pre-trained backbones. However, switching to a better-performing language model under a given budget leads to the most significant improvement for the combined system.


### Self-Attention


In the self-attention architecture introduced by FROMAGe ([Koh et al., 2023](https://proceedings.mlr.press/v202/koh23a/koh23a.pdf) ) and BLIP2 ([Li et al., 2023](https://proceedings.mlr.press/v202/li23q/li23q.pdf) ), the output of the vision encoder is treated as tokens and concatenated to the sequence of text tokens. The entire sequence is then passed as input to the language model (the input to the ***language*** model is ***language*** and ***visual*** tokens.). The layers that map the vision-hidden space to the text-hidden space are known as ***modality projection layers*** .


> **💡Pro tip:** When integrating visual encoders into multimodal pipelines, our[Segment Anything Model and Friends](https://www.lightly.ai/blog/segment-anything-model-and-friends) article highlights how robust segmentation models improve visual token quality.


**Figure:** Idefics2 model architecture. **Source:**[Laurençon et al. (2024)](https://arxiv.org/abs/2405.02246)


‍


The Idefics model family is a prominent class of VLMs that use a fully auto-regressive architecture.[Laurençon et al. (2024)](https://arxiv.org/abs/2405.02246) report that when attempting to train unimodal backbones and new parameters, the loss often diverges and leads to unstable training runs. They report that using LoRA to adapt the parameters of the unimodal backbones while using standard fine-tuning for the new parameters yields more stable training runs. In fact, cross-attention-based models perform better under frozen backbones than fully autoregressive backbones, but autoregressive backbones perform better with more degrees of freedom. However, LoRA adaptation can be done at a fraction of the GPU cost of pre-training and can be merged back at no additional inference cost.


## Boosting Model Performance


Over the years, many papers have analysed model training and identified certain tips and tricks to help model performance. Let’s look at some of them:


- Because vision encoders are often trained on different datasets and optimised for various tasks, some models, like SPHINX ([Lin et al., 2023](https://arxiv.org/abs/2311.07575) ), combine representations from multiple encoders, such as DINOv2 ([Oquab et al., 2023](https://arxiv.org/abs/2304.07193) ) and CLIP ([Radford et al., 2021](https://arxiv.org/abs/2103.00020) ), to create a **richer sequence of visual embeddings.** However, this comes at the expense of computational efficiency.
- [Li et al. (2022)](https://arxiv.org/abs/2212.00794) , inspired by the sparse computation of Masked Auto-encoders (MAEs), propose to randomly remove a large portion of image patches during CLIP-based contrastive image-text pre-training. This allows models to learn from more image-text pairs given the same wall-clock time and contrast more samples per iteration with a similar memory footprint.


**Figure:** Accuracy vs. training time trade-off of the FLIP method. With a higher masking ratio of 50% or 75% models train faster and are more accurate than its CLIP counterpart. **Source:**[Li et al. (2022)](https://arxiv.org/abs/2212.00794)


- [Sun et al. (2023)](https://arxiv.org/abs/2303.15389) use pre-trained EVA models, which combine the high-level semantics of image-text contrastive learning with geometric and structural capture from masked image modelling, to improve feature representation and expedite the convergence of CLIP models.
- [Chen and Wang 2022](https://research.google/blog/pali-scaling-language-image-learning-in-100-languages/) report a stronger increase in performance by **scaling the size of the vision encoder compared to scaling the size of the language model,** even though scaling the vision encoder leads to a smaller parameter count increase.
- Vision encoders are typically trained on fixed-size square images. Resizing an image before encoding changes its aspect resolution and reduces quality, affecting downstream performance (changing the aspect ratio of an image with text would hinder a VQA task). Therefore,[Laurençon et al. (2024)](https://arxiv.org/abs/2405.02246) interpolate the pre-trained positional embeddings to allow for a higher resolution and train the vision encoder with LoRA parameters to adapt to these modifications.


### Training Multimodal Models


**Figure:** Different stages of VLM training and the types of datasets used. **Source:**[Laurençon et al. (2024)](https://arxiv.org/abs/2408.12637)


‍


Multimodal training typically occurs in multiple stages due to the following reasons:


- Limited availability of high-quality data
- Memory constraints for efficient training
- Stability issues


During these stages, progressively higher-quality data is introduced, the maximum image resolution is gradually increased, and more model parts are unfrozen.


### 1. Pre-Training


The primary goal of pre-training is to align the backbone models and train the newly initialised parameters in the model. To efficiently train on a large number of images, the image resolution is typically kept low at the start of training and gradually increased over time. Once the resolution is sufficiently high, datasets containing large images, such as PDFs, can be incorporated into the training data.


> **💡Pro Tip:** Many multimodal systems start from pretrained models. See our Blog on[Transfer learning](https://www.lightly.ai/blog/transfer-learning) on how to strengthen these pipelines.


### 2. Supervised Fine-Tuning (SFT)


Having trained a general-purpose vision-language representation model during the pre-training phase, we now perform supervised fine-tuning to train the model for several tasks.


### 3. Alignment


But do we need to align the model after SFT further?


- Align the model’s output with human preferences, making it more intuitive and better at following complex instructions.
- Effectively reduces hallucinations, where the model might describe objects or details not actually present in the image.
- Enhances model safety by minimising the risk of generating harmful content.


## Best Practices for Efficient VLM Training


Training a VLM is a multi-stage process requiring lots of training data and is computationally expensive. Moreover, even fine-tuning on specific domains is memory intensive and can only be deployed on high-resource systems. Various previous works have identified certain tips to train VLMs efficiently:


- Flamingo ([Alayrac et al. 2022](https://arxiv.org/abs/2204.14198) ) was the first to use a **small fixed number of visual tokens per image.**
- [Laurençon et al. (2024)](https://arxiv.org/abs/2405.02246) propose using LoRA ([Hu et al. 2021](https://arxiv.org/abs/2106.09685) ) fine-tuning rather than full fine-tuning. This decreases GPU consumption and the number of parameters needed to be tuned with no additional inference latency.
- More recently, models such as SmolVLM ([Marafioti et al. 2024](https://huggingface.co/blog/smolvlm) ) have aggressively used the pixel shuffle strategy ([Chen et al. 2024](https://arxiv.org/pdf/2404.16821) ) to compress the patched visual information. This enables the model to adapt effectively to the input image's varying resolutions and aspect ratios.


An LLM with a long context window and a vision backbone with an efficient resizing method enables their use for basic video analysis tasks.


## Adapting VLMs for Open Domain Tasks


While Vision Language Models can be fine-tuned for improved performance this negatively impacts their performance on Out-Of-Distribution (OOD) classes and open-domain tasks where the desired output classes are not known. This causes major safety risks in situations that require capabilities of OOD detection and/or accurate identification of **both** novel and seen classes. While there are existing methods that boost VLM performance on both In-Distribution (ID) and Out-Of-Distribution (OOD) tasks,[Zang et al. (2024)](https://arxiv.org/pdf/2401.15914) show that after long enough fine-tuning without proper regularization, VLMs tend to overfit the known classes in the given dataset, with degraded performance on unknown classes.


**Figure:** Comparative Analysis of ID vs OOD


Based on this observation they propose a method to improve OOD generalisation without hurting the ID performance of fine-tuned models. They propose to:


- Generate features for unknown classes using a lightweight attention module, with an “extrapolating bias” on the unknown classes.
- An adaptive self-distillation mechanism that regularizes these features to further reduce overfitting during joint optimisation.


The goal is to use Knowledge Distillation to train a teacher model, based on past training stages, that overfits less. This teacher model then helps guide the current training stage (student model), which usually overfits more.


> For more details about Knowledge Distillation refer to our blogpost on[Knowledge Distillation Trends](https://www.lightly.ai/post/knowledge-distillation-trends)


## Conclusion


The landscape of Vision Language Models represents a fascinating intersection of architectural choices, training strategies, and performance optimization techniques. The evolution from early approaches like Flamingo to more recent implementations has revealed several critical insights about effectively building and training these systems. The trade-offs between cross-attention and self-attention architectures demonstrate that there's no one-size-fits-all solution – while cross-attention models perform better with frozen backbones, autoregressive approaches show superior results with greater degrees of freedom when properly stabilized through techniques like LoRA.


The multi-stage training process has emerged as a practical necessity, driven by data quality constraints, memory limitations, and stability considerations. This staged approach, incorporating pre-training, supervised fine-tuning, and alignment, allows for progressive refinement of model capabilities while managing computational resources effectively. The introduction of efficiency-focused techniques, such as Flamingo's fixed visual tokens and SmolVLM's pixel shuffle strategy, has made these models more practical to train and deploy.


However, challenges remain, particularly in the realm of out-of-distribution generalization. The tendency of VLMs to overfit known classes during fine-tuning highlights the delicate balance between optimizing for specific tasks and maintaining broad applicability. Solutions like adaptive self-distillation and feature generation for unknown classes show promise in addressing these limitations while preserving in-distribution performance.


The development of VLMs exemplifies the intricate relationship between architectural decisions, training methodologies, and practical constraints. Success in this domain requires careful consideration of these various elements, from choosing appropriate backbone models and attention mechanisms to implementing efficient training strategies and addressing generalization challenges. This holistic approach to model development has been crucial in advancing the capabilities of vision-language systems while making them more practical to deploy and use.


Saurav


Developer Advocate, LightlyAI


## References


1. Multimodal Few-Shot Learning with Frozen Language Models ([Tsimpoukelli et al., 2021](https://arxiv.org/abs/2106.13884) )
2. Flamingo: a Visual Language Model for Few-Shot Learning ([Alayrac et al. 2022](https://arxiv.org/abs/2204.14198) )[‍](https://arxiv.org/abs/2212.00794)
3. Scaling Language-Image Pre-training via Masking ([Li et al. 2022](https://arxiv.org/abs/2212.00794) )
4. BLIP-2: Bootstrapping Language-Image Pre-trainingwith Frozen Image Encoders and Large Language Models ([Li et al., 2023](https://proceedings.mlr.press/v202/li23q/li23q.pdf) )
5. Grounding Language Models to Images for Multimodal Inputs and Outputs ([Koh et al., 2023](https://proceedings.mlr.press/v202/koh23a/koh23a.pdf) )[‍](https://arxiv.org/abs/2405.02246)
6. What matters when building vision-language models? ([Laurençon et al. 2024](https://arxiv.org/abs/2405.02246) )[‍](https://arxiv.org/pdf/2401.15914)
7. Overcoming the Pitfalls of Vision-Language Model Finetuning for OOD Generalization ([Zang et al. 2024](https://arxiv.org/pdf/2401.15914.pdf) )
