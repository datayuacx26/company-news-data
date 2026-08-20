---
schema_version: "1.0.0"
document_id: "580c540fa6fe21c94aec919fc66ea655ce661f753f009e2b921bf8e402a4085e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/hd-painter-high-resolution-and-prompt-faithful-text-guided-image-inpainting-with-diffusion-models"
published_at: "2024-03-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:07.231499+00:00"
content_hash: "sha256:e71920bca4d535d7df92811acb363e20167b241534418a1f1b7d8f381a48bc42"
---

# HD-Painter: High-Resolution and Prompt-Faithful Text-Guided Image Inpainting with Diffusion Models

Do not index


Original Paper


[https://arxiv.org/abs/2312.14091](https://arxiv.org/abs/2312.14091)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2312.14091](https://arxiv.org/abs/2312.14091)


**By:**[Hayk Manukyan](https://arxiv.org/search/cs?searchtype=author&query=Manukyan%2C%20H) ,[Andranik Sargsyan](https://arxiv.org/search/cs?searchtype=author&query=Sargsyan%2C%20A) ,[Barsegh Atanyan](https://arxiv.org/search/cs?searchtype=author&query=Atanyan%2C%20B) ,[Zhangyang Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Z) ,[Shant Navasardyan](https://arxiv.org/search/cs?searchtype=author&query=Navasardyan%2C%20S) ,[Humphrey Shi](https://arxiv.org/search/cs?searchtype=author&query=Shi%2C%20H)


**Abstract:**


> Recent progress in text-guided image inpainting, based on the unprecedented success of text-to-image diffusion models, has led to exceptionally realistic and visually plausible results. However, there is still significant potential for improvement in current text-to-image inpainting models, particularly in better aligning the inpainted area with user prompts and performing high-resolution inpainting. Therefore, we introduce HD-Painter, a training free approach that accurately follows prompts and coherently scales to high resolution image inpainting. To this end, we design the Prompt-Aware Introverted Attention (PAIntA) layer enhancing self-attention scores by prompt information resulting in better text aligned generations. To further improve the prompt coherence we introduce the Reweighting Attention Score Guidance (RASG) mechanism seamlessly integrating a post-hoc sampling strategy into the general form of DDIM to prevent out-of-distribution latent shifts. Moreover, HD-Painter allows extension to larger scales by introducing a specialized super-resolution technique customized for inpainting, enabling the completion of missing regions in images of up to 2K resolution. Our experiments demonstrate that HD-Painter surpasses existing state-of-the-art approaches quantitatively and qualitatively across multiple metrics and a user study. Code is publicly available at:
>
>
> [this https URL](https://github.com/Picsart-AI-Research/HD-Painter)


---


###


Summary Notes


####


Enhancing Text-Guided Image Inpainting with HD-Painter


In the evolving field of AI, text-guided image inpainting combines natural language and visual creativity to regenerate parts of an image based on textual descriptions.


Despite the progress, aligning images with text and producing high-resolution outputs remain significant challenges. HD-Painter offers a promising approach to overcome these issues, improving prompt alignment and supporting high-resolution image generation without the need for additional training.


####


Simplifying Text-Guided Image Inpainting


Text-guided image inpainting has advanced with diffusion models, but often struggles with prompt alignment and high-resolution creation.


HD-Painter aims to address these issues by introducing two innovative components and a specialized super-resolution technique, making it possible to generate images up to 2K resolution that closely align with textual prompts.


####


Key Features of HD-Painter


- **Prompt-Aware Introverted Attention (PAIntA):** This component enhances the self-attention mechanism in diffusion models, making the content more relevant to the text prompt by minimizing the influence of non-prompt-related information.


- **Reweighting Attention Score Guidance (RASG):** RASG helps the image generation stay true to the text prompt by adjusting the diffusion process, ensuring both alignment and natural image statistics are maintained.


- **Inpainting-Specific Super-Resolution:** Unlike traditional techniques, this approach improves the resolution of inpainted areas by incorporating high-frequency details from the original image, ensuring a seamless and detailed result.


####


Performance and Results


HD-Painter shines when compared to current state-of-the-art methods, excelling in prompt alignment and high-quality image generation.


Evaluations using CLIP score, aesthetic score, and user feedback highlight its effectiveness.


####


Conclusion


HD-Painter significantly advances text-guided image inpainting by solving key issues of prompt alignment and high-resolution image generation. It offers a new tool for AI engineers, enhancing the potential for creative and practical AI applications.


With components like PAIntA and RASG, HD-Painter can produce images that are both high-quality and true to textual descriptions, marking a notable innovation in the AI field.


For a closer look at HD-Painter and its capabilities, the implementation is publicly available, signaling a step forward in text-guided image inpainting technology.


[HD-Painter GitHub Repository](https://github.com/Picsart-AI-Research/HD-Painter)


HD-Painter represents the forward-thinking achievements of AI engineers, showcasing the potential to merge visual and textual creativity through AI, paving the way for future advancements.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
