---
schema_version: "1.0.0"
document_id: "c1e72772e64a13581733eaa3736d953df1790447f0f3ba8d6f12163e8ac73a1a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/antifakeprompt-prompt-tuned-vision-language-models-are-fake-image-detectors"
published_at: "2023-11-03T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:50903153ba8cab815e90b4b4daf5d1df1449fb326b23d42b710c9910629725f1"
---

# AntifakePrompt: Prompt-Tuned Vision-Language Models are Fake Image Detectors

Do not index


Original Paper


[https://arxiv.org/abs/2310.17419](https://arxiv.org/abs/2310.17419)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.17419](https://arxiv.org/abs/2310.17419)


**By:**[You-Ming Chang](https://arxiv.org/search/cs?searchtype=author&query=Chang%2C%20Y) ,[Chen Yeh](https://arxiv.org/search/cs?searchtype=author&query=Yeh%2C%20C) ,[Wei-Chen Chiu](https://arxiv.org/search/cs?searchtype=author&query=Chiu%2C%20W) ,[Ning Yu](https://arxiv.org/search/cs?searchtype=author&query=Yu%2C%20N)


**Abstract:**


> Deep generative models can create remarkably photorealistic fake images while raising concerns about misinformation and copyright infringement, known as deepfake threats. Deepfake detection technique is developed to distinguish between real and fake images, where the existing methods typically train classifiers in the image domain or various feature domains. However, the generalizability of deepfake detection against emerging and more advanced generative models remains challenging. In this paper, inspired by the zero-shot advantages of Vision-Language Models (VLMs), we propose a novel approach using VLMs (e.g. InstructBLIP) and prompt tuning techniques to improve the deepfake detection accuracy over unseen data. We formulate deepfake detection as a visual question answering problem, and tune soft prompts for InstructBLIP to distinguish a query image is real or fake. We conduct full-spectrum experiments on datasets from 3 held-in and 13 held-out generative models, covering modern text-to-image generation, image editing and image attacks. Results demonstrate that (1) the deepfake detection accuracy can be significantly and consistently improved (from 54.6% to 91.31%, in average accuracy over unseen data) using pretrained vision-language models with prompt tuning; (2) our superior performance is at less cost of trainable parameters, resulting in an effective and efficient solution for deepfake detection. Code and models can be found at
>
>
> [this https URL](https://github.com/nctu-eva-lab/AntifakePrompt)


---


###


Summary Notes


####


Revolutionizing Deepfake Detection with AntifakePrompt


The advent of generative AI models has been a double-edged sword, offering unprecedented creative possibilities while also introducing the challenge of deepfakes.


These digitally manipulated media raise significant concerns about misinformation and security. Traditional detection methods have struggled to keep up with the evolving sophistication of deepfakes, leading to the development of more advanced and adaptable solutions. Among these, AntifakePrompt stands out as a promising innovation that utilizes Vision-Language Models (VLMs) for effective deepfake detection.


####


The Current Scenario


To appreciate AntifakePrompt's significance, it's essential to understand the current landscape:


- **Visual Generative Models** : Innovations like StyleGAN and Stable Diffusion have pushed the boundaries of digital creativity, enabling high-quality image generation and text-to-image conversions.


- **Deepfake Detection** : Traditional detection approaches often look for specific flaws in GAN-generated images, but they fall short against new or refined generative techniques.


- **Vision-Language Models (VLMs)** : VLMs, enhanced by training on diverse visual question answering (VQA) tasks, excel in interpreting the complex relationship between visual data and language.


- **Prompt Tuning** : This cutting-edge technique fine-tunes model responses to specific queries with minimal data, proving crucial for adapting VLMs to new tasks.


####


Introducing AntifakePrompt


AntifakePrompt represents a significant advancement in deepfake detection. It transforms this challenge into a VQA task, utilizing the capabilities of pretrained VLMs, particularly InstructBLIP, to accurately identify manipulated images.


####


How It Functions


AntifakePrompt adapts InstructBLIP through soft prompt tuning, effectively asking, "Is this photo real?" to classify images.


This method leverages the model's extensive training, enabling it to detect nuanced indications of authenticity or manipulation.


####


Empirical Evidence


####


Testing and Results


Testing across various datasets, including real and synthetic images, AntifakePrompt outshines traditional methods, showing remarkable accuracy even with unseen generative models.


Its efficiency is further highlighted by its lower requirement for trainable parameters.


####


Ablation Study Insights


Further studies shed light on the influence of prompt positioning on accuracy and identified which model components benefit most from tuning, offering directions for future refinement.


####


Future Directions


AntifakePrompt marks a significant step towards robust deepfake detection. Its adaptability and efficiency present a formidable tool for AI engineers and researchers.


Future work will aim to minimize training data needs and boost the model's resilience against different types of attacks.


####


Conclusion


As generative AI continues to evolve, tools like AntifakePrompt are crucial for maintaining the integrity of digital content.


By leveraging VLMs and prompt tuning, we can better navigate the challenges posed by deepfakes, ensuring a safer and more trustworthy digital environment.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
