---
schema_version: "1.0.0"
document_id: "91506acd0186dbfe8627eb372e5abbf5e36f9267574dfca5bf672685a3ac3663"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/re-imagine-the-negative-prompt-algorithm-transform-2d-diffusion-into-3d-alleviate-janus-problem-and-beyond"
published_at: "2023-04-26T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:ef9e78ed28c8ef2d8b03e93335d64b295fdfc04e04fed5b9a5a9093fdc45e10a"
---

# Re-imagine the Negative Prompt Algorithm: Transform 2D Diffusion into 3D, alleviate Janus problem and Beyond

Do not index


Original Paper


[https://arxiv.org/abs/2304.04968](https://arxiv.org/abs/2304.04968)


Blog URL


[blog.athina.ai /re-imag...d-beyond](https://blog.athina.ai/re-imagine-the-negative-prompt-algorithm-transform-2d-diffusion-into-3d-alleviate-janus-problem-and-beyond)


**Original Paper:**[https://arxiv.org/abs/2304.04968](https://arxiv.org/abs/2304.04968)


**By:**[Mohammadreza Armandpour](https://arxiv.org/search/cs?searchtype=author&query=Armandpour%2C%20M) ,[Ali Sadeghian](https://arxiv.org/search/cs?searchtype=author&query=Sadeghian%2C%20A) ,[Huangjie Zheng](https://arxiv.org/search/cs?searchtype=author&query=Zheng%2C%20H) ,[Amir Sadeghian](https://arxiv.org/search/cs?searchtype=author&query=Sadeghian%2C%20A) ,[Mingyuan Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20M)


**Abstract:**


> Although text-to-image diffusion models have made significant strides in generating images from text, they are sometimes more inclined to generate images like the data on which the model was trained rather than the provided text. This limitation has hindered their usage in both 2D and 3D applications. To address this problem, we explored the use of negative prompts but found that the current implementation fails to produce desired results, particularly when there is an overlap between the main and negative prompts. To overcome this issue, we propose Perp-Neg, a new algorithm that leverages the geometrical properties of the score space to address the shortcomings of the current negative prompts algorithm. Perp-Neg does not require any training or fine-tuning of the model. Moreover, we experimentally demonstrate that Perp-Neg provides greater flexibility in generating images by enabling users to edit out unwanted concepts from the initially generated images in 2D cases. Furthermore, to extend the application of Perp-Neg to 3D, we conducted a thorough exploration of how Perp-Neg can be used in 2D to condition the diffusion model to generate desired views, rather than being biased toward the canonical views. Finally, we applied our 2D intuition to integrate Perp-Neg with the state-of-the-art text-to-3D (DreamFusion) method, effectively addressing its Janus (multi-head) problem. Our project page is available at
>
>
> [this https URL](https://perp-neg.github.io/)


---


###


Summary Notes


####


Perp-Neg: Elevating AI-Generated Images and 3D Models


In the exciting world of AI and image generation, diffusion models have emerged as key players, transforming text prompts into stunning visuals.


Yet, turning complex text instructions into accurate visual representations poses challenges, especially when the instructions include what not to include in the image.


This is where most traditional models struggle, leading to limitations in creativity and precision.


Perp-Neg, or the Perpendicular Negative Prompt Algorithm, introduces a novel solution to enhance how diffusion models interpret and act on text prompts.


This innovative approach ensures that generated images or 3D models embody the desired features while effectively excluding unwanted elements, all without the hassle of retraining.


####


Introducing the Perp-Neg Algorithm


Perp-Neg is based on a simple, yet profound concept: using the geometry of score space to better handle negative prompts.


This method ensures the path to remove noise from images runs perpendicular to the desired concept, cleverly avoiding unwanted features while keeping the intended design intact.


####


Key Features:


- **Geometric Precision:** Utilizes perpendicular gradient sampling for clear separation of positive and negative prompt influences.


- **No Retraining Needed:** Easily integrates with existing models, eliminating the need for retraining.


- **Improved 3D Generation:** Addresses the Janus problem in text-to-3D tasks, ensuring accurate viewpoint representation.


####


Revolutionizing 2D-to-3D Conversion


Converting 2D models to 3D is challenging, particularly due to the Janus problem, where models display conflicting views.


Perp-Neg's view conditioning technique guarantees that each view accurately reflects the text prompt, enhancing 3D model fidelity.


####


Proven Effectiveness


Extensive testing confirms Perp-Neg's success in enhancing the accuracy of 3D views and aligning 2D images with text prompts, outperforming existing methods.


####


Highlights:


- **Enhanced View Accuracy:** Better 2D view generation for 3D models when used with DreamFusion.


- **Effective Negative Prompt Handling:** Images adhere to negative prompt constraints while preserving the main concept.


- **Consistent View Interpolation:** Offers smoother transitions between different angles.


####


Conclusion: Advancing Diffusion Model Capabilities


Perp-Neg marks a significant leap forward in diffusion models, providing a robust tool for AI Engineers to improve the creativity and precision of both image and 3D model generation.


By effectively managing negative prompts, Perp-Neg enhances output quality and broadens creative possibilities, setting a new standard in visual generation.


####


Visual Aids and Further Reading:


- **Figures 1 to 3:** Illustrate improvements in fidelity, handling of negative prompts, and view interpolation.


- **Table 1:** Shows Perp-Neg's effectiveness in generating accurate views.


For a deeper understanding of Perp-Neg and its impact, the research paper offers comprehensive insights into its development, application, and potential to shape future AI-driven visual generation efforts.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
