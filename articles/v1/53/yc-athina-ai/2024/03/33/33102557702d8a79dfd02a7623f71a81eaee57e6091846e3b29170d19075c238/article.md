---
schema_version: "1.0.0"
document_id: "33102557702d8a79dfd02a7623f71a81eaee57e6091846e3b29170d19075c238"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/automated-black-box-prompt-engineering-for-personalized-text-to-image-generation"
published_at: "2024-03-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:05.625622+00:00"
content_hash: "sha256:0d027f3dfa4324b85d73b307d4654462609a7d5fb47c1cb01347a81c42ab1453"
---

# Automated Black-box Prompt Engineering for Personalized Text-to-Image Generation

Do not index


Original Paper


[https://arxiv.org/abs/2403.1910](https://arxiv.org/abs/2403.19103)


Blog URL


[blog.athina.ai /automat...neration](https://blog.athina.ai/automated-black-box-prompt-engineering-for-personalized-text-to-image-generation)


**Original Paper:**[https://arxiv.org/abs/2403.19103](https://arxiv.org/abs/2403.19103)


**By:**[Yutong He](https://arxiv.org/search/cs?searchtype=author&query=He%2C%20Y) ,[Alexander Robey](https://arxiv.org/search/cs?searchtype=author&query=Robey%2C%20A) ,[Naoki Murata](https://arxiv.org/search/cs?searchtype=author&query=Murata%2C%20N) ,[Yiding Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20Y) ,[Joshua Williams](https://arxiv.org/search/cs?searchtype=author&query=Williams%2C%20J) ,[George J. Pappas](https://arxiv.org/search/cs?searchtype=author&query=Pappas%2C%20G%20J) ,[Hamed Hassani](https://arxiv.org/search/cs?searchtype=author&query=Hassani%2C%20H) ,[Yuki Mitsufuji](https://arxiv.org/search/cs?searchtype=author&query=Mitsufuji%2C%20Y) ,[Ruslan Salakhutdinov](https://arxiv.org/search/cs?searchtype=author&query=Salakhutdinov%2C%20R) ,[J. Zico Kolter](https://arxiv.org/search/cs?searchtype=author&query=Kolter%2C%20J%20Z)


**Abstract:**


> Prompt engineering is effective for controlling the output of text-to-image (T2I) generative models, but it is also laborious due to the need for manually crafted prompts. This challenge has spurred the development of algorithms for automated prompt generation. However, these methods often struggle with transferability across T2I models, require white-box access to the underlying model, and produce non-intuitive prompts. In this work, we introduce PRISM, an algorithm that automatically identifies human-interpretable and transferable prompts that can effectively generate desired concepts given only black-box access to T2I models. Inspired by large language model (LLM) jailbreaking, PRISM leverages the in-context learning ability of LLMs to iteratively refine the candidate prompts distribution for given reference images. Our experiments demonstrate the versatility and effectiveness of PRISM in generating accurate prompts for objects, styles and images across multiple T2I models, including Stable Diffusion, DALL-E, and Midjourney.


---


###


Summary Notes


####


Automated Black-box Prompt Engineering for Personalized Text-to-Image Generation


Transforming text descriptions into captivating images is a complex challenge in artificial intelligence. Traditionally, creating these text-to-image (T2I) translations required manual prompt engineering, a time-consuming and expert-driven process.


However, the emergence of automated techniques, particularly PRISM (Prompt Refinement and Iterative Sampling Mechanism), is revolutionizing this process by using large language models (LLMs) for more efficient prompt refinement.


This post explores PRISM's methodology, its experiments, and its potential to transform personalized T2I generation.


###


Introduction


Moving from manual to automated prompt engineering represents a significant evolution in T2I generation. Unlike the manual approach, which is straightforward but laborious and requires deep model knowledge, automated methods like PRISM facilitate prompt generation with minimal human input, making T2I generation more accessible and efficient across various models.


###


Background


####


Controllable T2I Generation Techniques


Efforts to achieve controllable T2I generation have led to several methods:


- **Training-free methods** : Use of pre-trained diffusion models.


- **Fine-tuning approaches** : Techniques like Dreambooth.


- **Prompt tuning methods** : Including Textual Inversion.


These methods, however, often lack in interpretability and generalizability.


####


Prompt Engineering Approaches


Prompt engineering is split between manual and automated techniques. Manual engineering is widespread for its simplicity but requires significant effort and expertise. Automated methods promise efficiency but face challenges in achieving comparable interpretability and generalizability.


###


PRISM Methodology


PRISM addresses these challenges by generating prompts that direct a T2I model to produce images aligned with the concepts in reference images.


Through iterative refinement with a multimodal LLM, PRISM fine-tunes prompts based on visual similarities between generated and reference images, without needing model retraining. This approach streamlines the creation of personalized T2I content.


###


Experiments


####


Implementation


PRISM utilizes GPT-4V for prompt generation and image scoring, with SDXL-Turbo as the T2I generator, demonstrating adaptability and efficiency.


####


Evaluation


PRISM is benchmarked against methods like Textual Inversion and BLIP-2, using metrics such as CLIP image similarity. Results show PRISM's superior interpretability, visual accuracy, and model transferability.


####


Findings


PRISM outperforms existing methods, offering enhanced interpretability, visual accuracy, and adaptability across T2I models. This underscores its potential to revolutionize personalized T2I generation.


###


Conclusion


PRISM represents a breakthrough in automated prompt engineering, using LLMs for iterative refinement to enable efficient T2I generation across different models.


This approach democratizes T2I generation and opens new possibilities for rapid, diverse image creation, marking a significant advancement for AI-driven visual content creation.


####


Acknowledgments


Supported by entities including ONR, NSF, and Sony AI, PRISM's development highlights collaborative innovation in AI research.


Its versatility across T2I models underlines the strength of its methodology and sets a benchmark for automated visual content generation from text descriptions, promising exciting future developments in the field.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
