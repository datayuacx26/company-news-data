---
schema_version: "1.0.0"
document_id: "6b9ddc23b6c5a681ce540ea508d824e0952ff81371e923ac441564d312c825eb"
company_key: "yc-tenyks"
company: "Tenyks"
source_id: "yc-tenyks-news-import-0db3935b63be"
canonical_url: "https://www.tenyks.ai/blog/multimodal-large-language-models-mllms-transforming-computer-vision"
published_at: "2024-07-01T00:00:00+00:00"
first_seen_at: "2026-07-22T16:03:37.171335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:ae11533aef9c8a3c26b805a3dc3b9f4212b8e47bdfc5cf915e7239564b6e10d0"
---

# Multimodal Large Language Models (MLLMs) transforming Computer Vision

This article introduces what is a Multimodal Large Language Model (MLLM) \[1\], their applications using challenging prompts, and the top models reshaping Computer Vision as we speak.


‍


### Table of Contents


1. What is a Multimodal Large Language Model (MLLM)?
2. Applications and use cases of MLLMs in Computer Vision
3. Top Multimodal Large Language Models
4. What’s next


‍


## 1. What is a Multimodal Large Language Model (MLLM)?


In layman terms, a Multimodal Large Language Model (MLLM) is a model that merges the reasoning capabilities of Large Language Models (LLMs), for instance GPT-3 \[2\] or LLaMA-3 \[3\], with the ability to receive, reason, and output with multimodal information.


‍


Figure 1 illustrates a multimodal AI system in healthcare \[4\]. It receives two inputs: 1) a medical image and 2) a query in text: “ *Is pleural effusion present in this image* ?”. The system output consists of an answer (i.e., a prediction) to the given query.


‍


Figure 1. A multimodal medical system created by aligning a radiology Vision encoder and a LLM \[4\]


‍


👉 In this article, we might use the term **Multimodal Model** as a shortcut for MLLM.


‍


### 1.1 The rise of multimodality in Artificial Intelligence


Over the past few years, there has been a significant transformation in Artificial Intelligence, largely driven by the rise of Transformers \[5\] in Language Models \[6\]. It’s no breaking news that the adoption of this architecture, invented by Google in 2017, has also impacted the domain of Computer Vision.


‍


One of the earliest examples was Vision Transformers (ViT) \[7\], which uses Transformers to segment images into multiple patches, treating them as individual visual tokens for input representation.


Figure 2. Some of the Multimodal Large Language Models (MLLMs) developed between 2022 and 2024


‍


With the raise of Large Language Models (LLMs), a new type of generative model, Multimodal Large Language Models (MLLMs) naturally emerged.


‍


As shown in Figure 2, in 2023 most big tech companies developed at least one MLLM. In 2024, OpenAI’s GPT-4o made the headlines during its launch in May.


‍


### 1.2 MLLMs vs VLMs vs Foundation Models


Some consider MLLMs to be really Foundation Models. For instance, Google’s Vertex AI shows Multimodal Large Language Models such as Claude 3, PaliGemma or Gemini 1.5 as Foundation Models 🤔.


‍


👉 Learn more about Foundation Models in Computer Vision in this post.


‍


On the other hand, Vision Language Models (VLMs) \[8\] are a specialized category of Multimodal Models that integrate text and image inputs and generate text outputs.


‍


The main difference between Multimodal Models and VLMs lies in (1) the capacity of MLLMs to work with more modalities, not only text and images as VLMs, and (2) VLMs are less performant in reasoning skills.


‍


### 1.2 Architecture


As illustrated in Figure 3, the architecture of a MLLM is divided in three parts:


- Modality encoder: The encoding components condense raw data formats like visuals or sound into a more streamlined representation. Instead of beginning the training process from the ground up, a prevalent strategy involves utilizing a pre-trained encoder (e.g., CLIP) that has been calibrated to other modalities.
- LLM backbone: A language model is required to output responses in text. It acts as the “brain” of the MLLM. The encoder is fed with images, audio or video and generates features, which are processed by a connector (or modality interface).
- Modality interface (i.e., conector): This functions as an intermediary or link between the encoder and the LLM. Since LLMs can only interpret text, it’s crucial to connect text with other modalities effectively.


‍


Figure 3. Multimodal Understanding: the components of the first stage of multimodality


‍


## 2. Applications and use cases of Multimodal Models in Computer Vision


Instead of providing a list of the different use cases where these models excel, we spun a couple of GPUs to test three the top MLLMs using challenging queries (no more cats 😺 and dogs 🐶 examples).


- **GPT-4o** \[9\]: The most powerful multimodal model from OpenAI released in May 2024. We accessed this model using OpenAI’s API[vision capabilities](https://platform.openai.com/docs/guides/vision) .
- **LLaVA 7b** \[10\]: A multimodal model, derived from the open-source LlaMa model, that integrates a vision encoder and[Vicuna](https://lmsys.org/blog/2023-03-30-vicuna/) for general-purpose visual and language understanding, achieving impressive performance sometimes on par to GPT-4. We accessed this model launching a[Ollama](https://jarvislabs.ai/blogs/ollama_deploy) instance on Jarvislab.
- **Apple Ferret 7b** \[11\]: An open-source Multimodal Large Language Model (MLLM) developed by Apple. It enables spatial understanding by using referring and grounding, which enables the model to recognize and describe any shape in an image, offering precise understanding, especially of smaller image regions. To access the model, we also launched a[Ferret](https://jarvislabs.ai/templates/apple_ferret) instance on[JarvisLab.](https://jarvislabs.ai/)


‍


### 2.1 Counting objects in presence of occlusion


Figure 4 shows how these three top models performed when given an image and a challenging prompt that requested them to count hard hats.


Figure 4. Apple’s Ferret model was the only one that correctly identified the bounding boxes’ location (including the occluded one)


‍


Despite providing a very rich description of the scene (see Figure 4), GPT-4o yielded incorrect coordinates to locate the required hard hats: some of them lie outside of the dimensions of the current image, which is *why* we only see one bounding box on the bottom right corner.


‍


The open-source model, LLaVA, was incapable to detect all the four hard hats (it missed the occluded one on the left side) and provided the wrong location for the bounding boxes.


‍


Surprisingly, Apple’s Ferret, was able to detect the four objects on the image: even the one on the left that is **occluded** ! ⭐️


‍


### 2.2 Autonomous driving: understanding and planning for risk


First, we picked this scene from an autonomous driving[dataset](http://bdd-data.berkeley.edu/) . Second, we increased the difficulty of the prompt: it requires the models to evaluate the risks from the self-driving car perspective while detecting two separate classes, vehicles and pedestrians (see Figure 5).


Figure 5. A challenging prompt requiring the models to detect objects and evaluate risks: Apple Ferret’s model performed better than GPT-4o.


‍


The results show how **LLaVA** performs quite poorly: it hallucinates by not identifying the big truck in front of the autonomous car. Are open-source models really that bad when subjected to challenging tasks? 🤔


‍


While **GPT-4o** shines to return reasoned detailed responses in text, it again performs poorly when it comes to clearly detecting bounding boxes. In contrast, **Apple’s Ferret** is *the only model* that detects the majority of the objects with accurate bounding box coordinates ✅.


‍


### 2.3 Sports analytics: detecting objects and scene understanding


Until now, at least one of the models, Apple’s Ferret, has shown high performance in counting and detecting objects. Let’s turn our attention to a more challenging scenario: sports analytics ⚽️.


‍


Often, unimodal fine-tuned architectures, such as YOLO, tend to perform really well for detecting players in a soccer match: Ccn MLLMs perform good too?


‍


Figure 6. A scene from a soccer match that was tested on the three MLLMs in this article


‍


**Ex 3. Question/Prompt** : *As an AI system that is an expert in sports, particularly in soccer, you’ll be given a scene of a soccer match. Please, (1) describe the scene, (2) count the number of players in each team, (3) provide the bounding box coordinates of the ball and of the goalkeeper, (4) estimate the likelihood of a goal and say what team is likely to score it.*


‍


As shown in Figure 7, detecting the players and the ball broke the three models we analyzed! None of the models is capable to identify two teams and their players.


Figure 7. None of the MLLMs in this article was capable to detect the objects requested in the prompt


‍


So, Multimodal Large Language Models (MLLMs) are good on average, but apparently they aren’t ready to solve computer vision tasks for more demanding use-cases. Even a YOLOv8 model does better in such specific (niche) tasks, 🔎 see[our article on the subject](https://medium.com/@tenyks_blogger/sports-analytics-a-data-centric-approach-to-computer-vision-246f19cf8a04) .


‍


Is fine-tuning MLLMs the way to go instead? 🤔


‍


## 3. Top Multimodal Large Language Models


Now, we list some of the most important MLLMs redefining computer vision:


‍


### GPT-4o (2024, OpenAI)


- **Inputs:** text, images, audio (beta), video (beta).
- **Outputs:** text, images.
- **What is it** : GPT-4o stands for “GPT-4 Omni”, with “Omni” referring to its multimodal capabilities across text, vision, and audio modalities. It is a single unified model that can understand and generate any combination of text, images, audio, and video inputs/outputs.
- **Try it here:**[https://chatgpt.com/](https://chatgpt.com/)
- 🥂 **Little known fact** : GPT-4o employs a “multi-modal chain of thought” approach, where it first reasons about how to break down a problem into a series of steps across different modalities (text, vision, audio), and then executes those steps to arrive at the final solution.


‍


### Claude 3.5 Sonnet (2024, Anthropic)


- **Inputs:** text, images.
- **Output:** text, images.
- **What is it:** With a 200K token context window, Claude 3.5 Sonnet is a multimodal AI system that can understand and generate text, images, audio, and other data formats. Excels at in-depth analysis, research, hypothesis generation, and task automation across various domains like finance, life sciences, and software engineering.
- **Try it here:**[https://claude.ai](https://claude.ai/)
- 🥂 **Little known fact:** Anthropic employs a technique called “recursive reward modeling” which involves using an earlier version of Claude to provide feedback and rewards for the model’s outputs.


‍


### LLaVA (2023, University of Wisconsin-Madison)


- **Inputs:** text, images.
- **Output:** text.
- **What is it** : LLaVA (Large Language and Vision Assistant) is an open-source multimodal AI model that can process and generate both text and visual data as inputs and outputs. It matches GPT-4’s chat abilities and sets a new record on Science QA, showcasing advanced visual-linguistic understanding.
- **Try it here:**[https://llava-vl.github.io](https://llava-vl.github.io/)
- 🥂 **Little known fact** : LLaVA was trained using a technique called “instruction tuning”, where GPT-4 was used to generate synthetic multimodal tasks involving text and images (novel in 2023). LLaVA learned from these diverse examples generated by GPT-4 without direct human supervision.


‍


### **Gemini 1.5** (2024, Google)


- **Inputs:** text, images,
- **Output:** text, images.
- **What is it** : Gemini is a family of large language models developed by Google that can understand and operate across multiple modalities like text, images, audio (beta) and video (beta). It was first unveiled in December 2023 and is available in three optimized variants — Gemini Ultra (largest), Gemini Pro (for scaling), and Gemini Nano (for on-device tasks).
- **Try it here:**[https://gemini.google.com/](https://gemini.google.com/)
- 🥂 **(Obvious) little known fact** : Gemini’s name is a nod to the Gemini zodiac sign, which represents the “Twins” in Greek mythology. This is fitting given Gemini’s dual nature as a highly capable language model that can also process and generate multimodal data like images, audio, and video.


‍


### Qwen-VL (2024, Alibaba Cloud)


- **Inputs:** text, images,
- **Output:** text, images.
- **What is it** : Qwen-VL is an open-sourced multimodal AI model that combines language and vision capabilities. It’s an extension of the Qwen language model, designed to overcome limitations in multimodal generalization. Recently upgraded versions (Qwen-VL-Plus and Qwen-VL-Max) feature improved image reasoning, better detail analysis in images and text, and support for high-resolution images with varied aspect ratios.
- **Try it here:**[https://qwenlm.github.io/blog/qwen-vl/](https://qwenlm.github.io/blog/qwen-vl/)
- 🥂 **(Fun) little known fact** : After launch, Qwen-VL quickly rose to the top of the OpenVLM leaderboard but was surpassed by other more powerful models, especially GPT-4o.


‍


## 4. What’s next?


Multimodal models are definitively transforming computer vision. As an[ML/MLOps Engineer](https://medium.com/@tenyks_blogger/ml-vs-mlops-engineer-key-differences-similarities-43d612bacdd9) , how can you best leverage them when building robust AI pipelines?


‍


Moreover, how do these models, some of them also known as foundation models, impact a traditional computer vision pipeline? 🤔 At[Tenyks](https://tenyks.ai/) , we believe that these models are paving the way for a new kind of pipeline:[Computer Vision Pipeline 2.0](https://medium.com/@tenyks_blogger/computer-vision-is-already-evolving-3cd0e63e805b) .


‍


Learn more about the cutting edge of multimodality and foundation models in our brand-new CVPR 2024 series:


- [CVPR 2024: Image and Video Search & Understanding (RAG, Multimodal, Embeddings, and more)](https://medium.com/@tenyks_blogger/cvpr-2024-image-and-video-search-understanding-rag-multimodal-embeddings-and-more-59dad7568b80)
- [Top 4 Highlights of CVPR 2024: Embodied AI, GenAI, Foundation Models, and Video Understanding](https://medium.com/@tenyks_blogger/top-4-highlights-of-cvpr-2024-embodied-ai-genai-foundation-models-and-video-understanding-a808d4bb331e)


‍


## References


\[1 \] A Survey on Multimodal Large Language Models


‍


\[[2](https://arxiv.org/pdf/2005.14165) \] Language Models are Few-Shot Learners


‍


\[[3](https://ai.meta.com/blog/meta-llama-3/) \] Introducing Meta Llama-3: The most capable openly available LLM to date


‍


\[[4](https://research.google/blog/multimodal-medical-ai/) \] Multimodal medical AI


‍


\[[5](https://arxiv.org/pdf/1706.03762) \] Attention is all you need


‍


\[[6](https://openai.com/index/better-language-models/) \] Language Models are Unsupervised Multitask Learners


‍


\[[7](https://arxiv.org/pdf/2010.11929) \] An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale


‍


\[[8](https://arxiv.org/pdf/2405.17247) \] An Introduction to Vision-Language Modeling


‍


\[[9](https://openai.com/index/hello-gpt-4o/) \] GPT-4o


‍


\[[10](https://llava-vl.github.io/) \] LLaVA: Large Language and Vision Assistant


‍


\[[11](https://machinelearning.apple.com/research/ferret) \] FERRET: Refer and Ground Anything Anywhere at Any Granularity


‍


**Authors** : Jose Gabriel Islas Montero, Dmitry Kazhdan.


‍


*If you’d like to know more about* ***Tenyks*** *, explore*[sandbox](https://tenyks.docsend.com/view/cyzf9j64wytwgtaj) *.*


‍
