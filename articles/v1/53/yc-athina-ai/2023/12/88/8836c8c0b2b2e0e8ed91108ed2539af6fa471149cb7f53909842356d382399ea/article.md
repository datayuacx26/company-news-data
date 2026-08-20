---
schema_version: "1.0.0"
document_id: "8836c8c0b2b2e0e8ed91108ed2539af6fa471149cb7f53909842356d382399ea"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/edgesam-prompt-in-the-loop-distillation-for-on-device-deployment-of-sam"
published_at: "2023-12-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:930fff40c28bd10ba1617b1bcfc2b9a6ddd9f2235e6a942e36be41772d464b7f"
---

# EdgeSAM: Prompt-In-the-Loop Distillation for On-Device Deployment of SAM

Do not index


Original Paper


[https://arxiv.org/abs/2312.06660](https://arxiv.org/abs/2312.06660)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2312.06660](https://arxiv.org/abs/2312.06660)


**By:**[Chong Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20C) ,[Xiangtai Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20X) ,[Chen Change Loy](https://arxiv.org/search/cs?searchtype=author&query=Loy%2C%20C%20C) ,[Bo Dai](https://arxiv.org/search/cs?searchtype=author&query=Dai%2C%20B)


**Abstract:**


> This paper presents EdgeSAM, an accelerated variant of the Segment Anything Model (SAM), optimized for efficient execution on edge devices with minimal compromise in performance. Our approach involves distilling the original ViT-based SAM image encoder into a purely CNN-based architecture, better suited for edge devices. We carefully benchmark various distillation strategies and demonstrate that task-agnostic encoder distillation fails to capture the full knowledge embodied in SAM. To overcome this bottleneck, we include both the prompt encoder and mask decoder in the distillation process, with box and point prompts in the loop, so that the distilled model can accurately capture the intricate dynamics between user input and mask generation. To mitigate dataset bias issues stemming from point prompt distillation, we incorporate a lightweight module within the encoder. EdgeSAM achieves a 40-fold speed increase compared to the original SAM, and it also outperforms MobileSAM, being 14 times as fast when deployed on edge devices while enhancing the mIoUs on COCO and LVIS by 2.3 and 3.2 respectively. It is also the first SAM variant that can run at over 30 FPS on an iPhone 14. Code and models are available at
>
>
> [this https URL](https://github.com/chongzhou96/EdgeSAM)


---


###


Summary Notes


##


EdgeSAM: A New Horizon in AI Model Efficiency for Mobile Devices


The quest for mobile-compatible AI solutions is more intense than ever, especially for enterprise AI engineers aiming to deploy advanced models like the Segment Anything Model (SAM) on handheld devices.


However, the high computational needs and size of SAM make it challenging to use on everyday smartphones, such as the iPhone 14. This is where EdgeSAM steps in, offering a tailored solution for these challenges.


###


Introduction to EdgeSAM


EdgeSAM stands out as a game-changer in deploying AI directly onto mobile devices. It achieves this by condensing a Vision Transformer (ViT)-based SAM into a smaller, Convolutional Neural Network (CNN)-based model.


This not only shrinks the model's size but also speeds up processing, making real-time mobile applications both doable and efficient.


At the heart of EdgeSAM is its unique knowledge distillation method, which uses dynamic prompts during training to better guide the learning process.


###


How EdgeSAM Innovates


####


Streamlined Model Architecture


Efficiency is key for AI on mobile devices. EdgeSAM moves from a ViT to a CNN architecture, creating a lighter model without sacrificing accuracy.


This is crucial for bringing AI models like SAM to mobile platforms.


####


Advanced Knowledge Distillation


Where traditional distillation methods might stumble with complex tasks, EdgeSAM excels by introducing task-specific strategies.


Its "Prompt-In-The-Loop Distillation" enhances the focus on challenging areas, preserving essential segmentation abilities.


####


Broadening Segmentation Applications


EdgeSAM extends the use of efficient segmentation models beyond niche applications, like automotive, to interactive mobile use, expanding the scope of mobile AI applications.


###


Testing EdgeSAM: Performance Insights


EdgeSAM underwent thorough testing to prove its worth:


- **Datasets** : It was trained on the SA-1B dataset and tested on COCO and LVIS datasets for a well-rounded evaluation.


- **Speed and Accuracy** : EdgeSAM showed a 40x speed boost over SAM on mobile, achieving over 30 FPS on an iPhone 14 without losing accuracy, a significant achievement for real-time mobile AI.


- **Component Efficacy** : In-depth studies confirmed that EdgeSAM's design and training improvements were key to its performance.


###


EdgeSAM's Impact and Future


EdgeSAM marks a significant advancement in deploying interactive segmentation models on edge devices. Its innovative approach to knowledge distillation has made AI on mobile devices both practical and efficient, setting new standards in the field.


For enterprise AI engineers, EdgeSAM provides a powerful tool for bringing sophisticated AI models to edge devices, promising new possibilities for on-device AI.


###


Appreciation


This breakthrough was supported by grants like the RIE2020 Industry Alignment Fund and collaborations with industry partners, whose contributions have been crucial in advancing on-device AI technology.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
