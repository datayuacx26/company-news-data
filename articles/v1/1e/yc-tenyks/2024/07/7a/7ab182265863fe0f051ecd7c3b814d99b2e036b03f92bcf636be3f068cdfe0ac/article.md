---
schema_version: "1.0.0"
document_id: "7ab182265863fe0f051ecd7c3b814d99b2e036b03f92bcf636be3f068cdfe0ac"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-rss-b52d09f64a8c"
canonical_url: "https://medium.com/@tenyks_blogger/cvpr-2024-foundation-models-visual-prompting-are-about-to-disrupt-computer-vision-026f2c1c3a2f"
published_at: "2024-07-11T21:55:40+00:00"
first_seen_at: "2026-07-27T06:27:15.777070+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:3a3915dcba8c3950041d18044155708a362f707f04e1af9d8eb97c2e3029057b"
---

# CVPR 2024: Foundation Models + Visual Prompting Are About to Disrupt Computer Vision

Artificial Intelligence


Computer Vision


Foundation Models


Machine Learning


Data Science


# CVPR 2024: Foundation Models + Visual Prompting Are About to Disrupt Computer Vision


## Discover 5 groundbreaking advances in Visual Prompting from CVPR 2024.


[The Tenyks Blogger](https://medium.com/@tenyks_blogger?source=post_page---byline--026f2c1c3a2f---------------------------------------)


8 min read


·


Jul 11, 2024


--


Press enter or click to view image in full size


Visual Prompting in action: a vision system using predictions as visual prompts for a promptable model


> **TL;DR** 🔥 5 Visual Prompting advances in Computer Vision
>
>
> 1. Image understanding: Simple visual markers can help[foundation models](https://medium.com/@tenyks_blogger/the-foundation-models-reshaping-computer-vision-b299a91527fb) to better understand specific parts of an image.
>
>
> 2.[MLLMs](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) : Using scene graphs to enable Multimodal Large Language Models (MLLMs) to describe images better without needing extra training data.
>
>
> 3. Foundation Models: Improving vision foundation models (e.g., SAM) through strategic visual prompting.
>
>
> 4. Improved generalization: Teaching AI to recognize unseen (novel) objects quickly while remembering the old ones (base knowledge).
>
>
> 5. Visual Prompting meets Active Learning: AI can learn new visual tasks smartly, using fewer examples and without forgetting what it already knows.


In this article, we explore Visual Prompting, a technique that enables the adaptation of large vision models to new tasks.


Alongside defining this approach and offering a systems perspective on how promptable models are revolutionizing the field, we also highlight five of the most significant advances in Visual Prompting introduced at CVPR.


🔥 You won’t believe what CVPR 2024 unveiled! Check the top takeaways here:


- [Image and Video Search & Understanding](https://medium.com/@tenyks_blogger/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more-59dad7568b80) (RAG, Multimodal, Embeddings, and more).
- [Top Highlights You Must Know](https://medium.com/@tenyks_blogger/top-4-highlights-of-cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding-a808d4bb331e) — Embodied AI, GenAI, Foundation Models, and Video Understanding.


🔥 Learn more about **Segment Anything Model 2** (SAM 2):


- 🆕[SAM 2 + GPT-4o](https://medium.com/@tenyks_blogger/segment-anything-model-2-sam-2-gpt-4o-cascading-foundation-models-via-visual-prompting-76158ff0b9f4) — Cascading Foundation Models via Visual Prompting — Part 1


> **“Prompting is an interface to model editing that everyone can use”**
>
>
> [Phillip Isola](http://web.mit.edu/phillipi/) *, AI researcher who coined the term “Visual Prompting” \[1\]*


## Table of contents


1. What is Visual Prompting
2. Visual Prompting: A Systems Perspective
3. Visual Prompting Advances That Stole the Show at CVPR 2024
4. What’s next


## 1. What is Visual Prompting


### 1.1 The origins of Visual Prompting


Prompting for vision can be traced back to Image Analogies \[2\], a work published in 2001, where researchers developed a strategy to process images by example, in which both a prompt and a query are required to retrieve a response.


In the Generative AI era, what’s **new** about prompting (in Vision)?


1. Prompting refers to adapting models to do things they were not purposely trained to do. In technical terms, prompting makes possible to adapt pre-trained models to unseen distributions.
2. Prompting was popularized in Language Models \[3\], where large pre-trained models (i.e., GPT-4) are adapted to new tasks.
3. *Visual Prompting* \[1\] refers to the process of adapting large-scale vision models to perform (unseen) vision tasks.


### 1.2 Understanding Prompting for Vision


To understand prompting in the vision domain, it might be useful to establish the differences between prompting and fine-tuning (a standard adaptation method).


Press enter or click to view image in full size


Figure 1. Main differences between Visual Prompting and Fine-tuning


Figure 1 illustrates the key differences between visual prompting and fine-tuning in the context of[foundation models for computer vision](https://medium.com/@tenyks_blogger/the-foundation-models-reshaping-computer-vision-b299a91527fb) . Visual Prompting employs visual cues or examples to guide the model without modifying its parameters. This approach offers flexibility, and lower computational requirements. On the hand, fine-tuning involves retraining the model on specific datasets, modifying its parameters to achieve better task-specific performance at the cost of higher computational resources.


However, these two approaches aren’t binary choices. They both represent a continuum of potential adaptation strategies for foundation models as shown in Figure 2.


Press enter or click to view image in full size


Figure 2. Visual Prompting provides maximum flexibility and and quick adaptation


The diagram above reveals that when **flexibility** and **speed** are required, Visual Prompting might be the best approach to leverage large-scale vision models for certain applications such as visual search and retrieval, or rapid prototyping and experimentation.


## 2. Visual Prompting: A Systems Perspective


Perhaps the key insight into the strengths of Visual Prompting lies in understanding this technique from a systems perspective, particularly within a multi-stage vision system.


A promptable model can be seamlessly integrated with other systems, allowing it to perform specific tasks during inference as part of a larger AI system.


Press enter or click to view image in full size


Figure 3. Integration of Visual Prompting in a Multi-Stage Vision System: SegmentAnything , a foundation model, can utilize visual prompts generated by a preceding object detection model to perform precise image segmentation \[4\]


Figure 3 shows a system that employs to a promptable[foundation model](https://medium.com/@tenyks_blogger/the-foundation-models-reshaping-computer-vision-b299a91527fb) as a component of a larger system:


- **Input Image:** The system starts with an input image, which in this case shows a group of horses running in a field.
- **Object Detection:** The input image is processed by an object detector (e.g., YOLO-World \[6\]). This step identifies and localizes objects in the image, producing bounding boxes around detected objects. The output shows bounding boxes around each horse, as well as smaller boxes for clouds in the sky.
- **Segmentation:** The detected boxes are then used as **visual prompts** for a promptable segmentation model (e.g., Segment Anything \[5\]). This model generates precise masks for each detected object, resulting in a more detailed segmentation of the image.


## 3. Visual Prompting Advances That Stole the Show at CVPR 2024


### 3.1 Intuitive Visual Prompting for Large Multimodal Models


Press enter or click to view image in full size


Figure 4. CVPR 2024 poster of ViP-LLaVA


**Paper** :[cvpr open access](https://openaccess.thecvf.com/content/CVPR2024/papers/Cai_ViP-LLaVA_Making_Large_Multimodal_Models_Understand_Arbitrary_Visual_Prompts_CVPR_2024_paper.pdf)


💻 **Run it** :[https://vip-llava.github.io/](https://vip-llava.github.io/)


💡 **Main novelty:** The introduction of a multimodal model capable of decoding arbitrary (free-form) visual prompts, allowing users to intuitively interact with the model by marking images with natural cues like “red bounding box” or “pointed arrow” without needing complex region encodings.


**Potential applications:**


- **a) Healthcare Imaging** : Allowing medical professionals to highlight specific areas in medical images (e.g., X-rays, MRIs) for more accurate diagnosis and analysis.
- **b) E-commerce Product Search** : Enabling users to mark specific parts of product images (e.g., highlighting a shoe’s heel) to find similar items or detailed product information.


### 3.2 Zero-Shot Visual Prompting to Enhance AI’s Understanding of Images


Press enter or click to view image in full size


Figure 5. CCoT full prompt example: First, generate a scene graph using the image and task prompt. Then, extract the answer by prompting the LMM with the image, scene graph, and question.


**Paper** :[https://arxiv.org/pdf/2311.17076](https://arxiv.org/pdf/2311.17076)


💻 **Run it** :[https://github.com/chancharikmitra/CCoT](https://github.com/chancharikmitra/CCoT)


💡 **Main novelty:** The development of the Compositional Chain-of-Thought (CCoT) method, which involves a two-step zero-shot prompting process. First, a[Multimodal Large Language Model](https://medium.com/@tenyks_blogger/multimodal-large-language-models-mllms-transforming-computer-vision-76d3c5dd267f) (MLLM) generates a scene graph from an image based on a task prompt. Then, this scene graph is used to provide context for generating a detailed and accurate response, leveraging the compositional information **without needing annotated data or fine-tuning** .


**Potential applications:**


- **a) Visual Question Answering** : Providing precise answers to questions about an image by comprehensively understanding the visual content and its composition.
- **b) Surveillance** : Identifying objects in an image and understanding the relationships between them, which is useful for surveillance applications.


### 3.3 Cost-Effective Segmentation in Foundation Models


Press enter or click to view image in full size


Figure 6. CVPR 2024 poster of Semantic-aware SAM for Point-Prompted Instance Segmentation


**(Highlight) Paper** :[https://arxiv.org/abs/2312.15895](https://arxiv.org/abs/2312.15895)


💻 **Run it** :[https://github.com/zhaoyangwei123/SAPNet](https://github.com/zhaoyangwei123/SAPNet)


💡 **Main novelty:** The development of the Semantic-Aware Instance Segmentation Network (SAPNet), which integrates Multiple Instance Learning (MIL) with visual foundation models like SAM \[5\] using point prompts. SAPNet enhances category-specific segmentation by strategically selecting representative mask proposals and addressing segmentation challenges with Point Distance Guidance and Box Mining Strategy.


**Potential applications:**


- **a) Autonomous Driving** : Improving object detection and categorization in autonomous vehicle systems, leading to better decision-making and safety.
- **b) Agricultural Monitoring** : Providing precise segmentation of specific crops or plants in aerial or satellite imagery for better agricultural management and yield prediction.


### 3.4 Using Visual Prompts in Foundation Models for Better Image Segmentation


Press enter or click to view image in full size


Figure 7. CVPR 2024 poster of Visual Prompting for Generalized Few-Shot Segmentation


**Paper** :[https://arxiv.org/pdf/2404.11732](https://arxiv.org/pdf/2404.11732) .


💻 **Run it** :[https://github.com/rayat137/VisualPromptGFSS](https://github.com/rayat137/VisualPromptGFSS)


💡 **Main novelty:** The use of learned visual prompts with a transformer decoder for generalized few-shot segmentation (GFSS). Specifically, they introduce a unidirectional causal attention mechanism between novel prompts (learned from limited examples) and base prompts (learned from abundant data).


**Potential applications:**


- **a) Autonomous Vehicles:** Quickly adapting to recognize and segment new objects or road conditions with minimal examples, while retaining performance on common road elements.
- **b) Satellite Imagery Analysis:** Identifying and segmenting new types of land use or environmental changes with few examples, while maintaining accuracy for well-known geographical features.


### 3.5 Active Learning meets Prompting in Vision Language Models (VLMs)


Press enter or click to view image in full size


Figure 8. CVPR 2024 poster of Active Prompt in Visual Language Models


**Paper** :[https://arxiv.org/pdf/2311.11178](https://arxiv.org/pdf/2311.11178)


💻 **Run it** :[https://github.com/kaist-dmlab/pcb](https://github.com/kaist-dmlab/pcb)


💡 **Main novelty:** The development of a novel active learning framework called PCB, specifically designed for pre-trained Vision Language Models (VLMs). This approach addresses the challenges of adapting VLMs to new tasks while minimizing the need for expensive labelling.


**Potential applications:**


- **a) Medical Imaging:** Quickly adapting VLMs to identify new disease patterns or anomalies with *minimal* expert labeling.
- **b) E-commerce:** Improving product categorization and search capabilities by adapting VLMs to new product lines with limited manual input.


## 4. What’s next


As we discussed in this article, Visual Prompting makes it possible to adapt foundation models in input space. This is important because this input serves as a universal interface for both humans and models \[4\].


Promptable models in the field of Vision are likely to redefine how the traditional Computer Vision pipeline operates. Many of these models can be seen as the building blocks that will replace some of the common stages in a traditional pipeline (e.g., labelling).


At Tenyks, we believe that this disruption is closer than we expect. In our article[Computer Vision Pipeline 2.0](https://medium.com/p/3cd0e63e805b) , we break down some of the key insights into why this change is **inevitable** .


Get up to speed with **the best of CVPR 2024** with the ⭐ highlights of the premier conference in the field of Vision here:


🔥 Learn more about **Segment Anything Model 2** (SAM 2):


## References


\[[1](https://arxiv.org/pdf/2203.17274) \] Exploring Visual Prompts for Adapting Large-Scale Models


\[[2](https://mrl.cs.nyu.edu/publications/image-analogies/analogies-72dpi.pdf) \] Image Analogies


\[[3](https://openai.com/index/better-language-models/) \] Language Models are Unsupervised Multitask Learners


\[[4](https://prompting-in-vision.github.io/slides/cvpr23/lecture-3.pdf) \] Visual Prompting


\[[5](https://arxiv.org/abs/2304.02643) \] Segment Anything


\[[6](https://arxiv.org/pdf/2401.17270) \] YOLO-World: Real-Time Open-Vocabulary Object Detection


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan.


*If you’d like to know more about* ***Tenyks*** *, explore*[sandbox](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*
