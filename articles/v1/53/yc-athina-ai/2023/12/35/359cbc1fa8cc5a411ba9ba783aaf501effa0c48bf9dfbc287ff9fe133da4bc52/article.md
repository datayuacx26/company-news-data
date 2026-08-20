---
schema_version: "1.0.0"
document_id: "359cbc1fa8cc5a411ba9ba783aaf501effa0c48bf9dfbc287ff9fe133da4bc52"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-in-prompt-learning-for-universal-image-restoration"
published_at: "2023-12-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:515693b6a17213c2d635dbd4cc38c803c7595391d8d9af2615ef77980d2bac0e"
---

# Prompt-In-Prompt Learning for Universal Image Restoration

Do not index


Original Paper


[https://arxiv.org/abs/2312.05038](https://arxiv.org/abs/2312.05038)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2312.05038](https://arxiv.org/abs/2312.05038)


**By:**[Zilong Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Z) ,[Yiming Lei](https://arxiv.org/search/cs?searchtype=author&query=Lei%2C%20Y) ,[Chenglong Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20C) ,[Junping Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20J) ,[Hongming Shan](https://arxiv.org/search/cs?searchtype=author&query=Shan%2C%20H)


**Abstract:**


> Image restoration, which aims to retrieve and enhance degraded images, is fundamental across a wide range of applications. While conventional deep learning approaches have notably improved the image quality across various tasks, they still suffer from (i) the high storage cost needed for various task-specific models and (ii) the lack of interactivity and flexibility, hindering their wider application. Drawing inspiration from the pronounced success of prompts in both linguistic and visual domains, we propose novel Prompt-In-Prompt learning for universal image restoration, named PIP. First, we present two novel prompts, a degradation-aware prompt to encode high-level degradation knowledge and a basic restoration prompt to provide essential low-level information. Second, we devise a novel prompt-to-prompt interaction module to fuse these two prompts into a universal restoration prompt. Third, we introduce a selective prompt-to-feature interaction module to modulate the degradation-related feature. By doing so, the resultant PIP works as a plug-and-play module to enhance existing restoration models for universal image restoration. Extensive experimental results demonstrate the superior performance of PIP on multiple restoration tasks, including image denoising, deraining, dehazing, deblurring, and low-light enhancement. Remarkably, PIP is interpretable, flexible, efficient, and easy-to-use, showing promising potential for real-world applications. The code is available at
>
>
> [this https URL](https://github.com/longzilicart/pip_universal)


---


###


Summary Notes


##


Simplifying Universal Image Restoration with Prompt-In-Prompt Learning


In the field of image processing, improving image quality through restoration is key for various applications, from surveillance to medical imaging.


The main challenge is dealing with different types of image degradation, such as noise, rain, haze, blur, and poor lighting, which traditionally required a separate model for each type.


This method isn't very efficient or scalable. Universal models attempted to offer a one-size-fits-all solution but often compromised on performance.


This is where the innovative approach of prompt learning comes in, aiming to enhance adaptability and precision in universal image restoration tasks.


###


The Challenge with Multi-Task Networks


The goal of universal image restoration is to create a method that can easily switch between tasks like denoising, deraining, dehazing, deblurring, and enhancing under low-light conditions without needing multiple specialized models.


While multi-task networks showed promise, they were often too rigid, affecting efficiency and performance for specific tasks. Prompt learning, with its roots in linguistic and visual fields, offers a dynamic and effective solution to these challenges.


###


Introducing Prompt-In-Prompt (PIP) Learning


Prompt-In-Prompt (PIP) learning stands out by offering a new way to approach universal image restoration.


It combines high-level degradation-aware prompts with low-level restoration prompts through a sophisticated interaction module, achieving a balance that ensures the model understands both the type of degradation and the details needed for restoration within a single framework.


####


Key Components of PIP


- **Degradation-Aware Prompt** : Identifies the type of degradation, allowing the model to focus on a targeted restoration approach.


- **Basic Restoration Prompt** : Focuses on the detailed textures and structures needed for effective restoration.


- **Prompt-to-Prompt Interaction** : The heart of PIP, which merges high-level and low-level prompts into a unified restoration guide.


- **Prompt-to-Feature Interaction** : Enhances restoration outcomes by adjusting image features based on the degradation type, making the restoration more precise.


###


Testing PIP: Experiments and Results


PIP learning was thoroughly tested across various datasets and compared with the latest methods. The results clearly show PIP's superior performance, especially in complex situations with multiple degradations.


Further studies highlighted the importance of each component in PIP, confirming its adaptability and robustness.


###


The Future of Image Restoration


PIP learning represents a significant step forward in image restoration. By combining understanding of degradation types with restoration details, PIP improves the performance of universal models and opens up new possibilities for innovation.


Its versatility and efficiency make it a valuable tool for AI engineers, offering a scalable solution to image degradation challenges.


Looking forward, PIP learning's potential goes beyond current applications, with the promise to revolutionize other areas of image processing.


Its implementation is openly available on GitHub, encouraging collaboration to further enhance its capabilities, ushering in a new era of image restoration that is more adaptable, precise, and efficient.


In summary, Prompt-In-Prompt learning is a game-changer in universal image restoration, merging high-level and low-level insights to set a new benchmark in image processing.


As we explore PIP learning's full potential, the future of image restoration is indeed promising.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
