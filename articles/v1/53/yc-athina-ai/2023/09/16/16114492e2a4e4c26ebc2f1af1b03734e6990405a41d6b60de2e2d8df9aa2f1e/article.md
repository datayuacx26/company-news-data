---
schema_version: "1.0.0"
document_id: "16114492e2a4e4c26ebc2f1af1b03734e6990405a41d6b60de2e2d8df9aa2f1e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/researdesign-guidelines-for-prompt-engineering-text-to-image-generative-models"
published_at: "2023-09-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:e816850e3c11cd158c74eee5d3d7608aa64296661809f390b91aa68c48e7bfdb"
---

# ResearDesign Guidelines for Prompt Engineering Text-to-Image Generative Models

Do not index


Original Paper


[https://arxiv.org/abs/2109.06977](https://arxiv.org/abs/2109.06977)


Blog URL


[blog.athina.ai /researd...e-models](https://blog.athina.ai/researdesign-guidelines-for-prompt-engineering-text-to-image-generative-models)


**Original Paper:**[https://arxiv.org/abs/2109.06977](https://arxiv.org/abs/2109.06977)


**By:**[Vivian Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20V) ,[Lydia B. Chilton](https://arxiv.org/search/cs?searchtype=author&query=Chilton%2C%20L%20B)


**Abstract:**


> Text-to-image generative models are a new and powerful way to generate visual artwork. However, the open-ended nature of text as interaction is double-edged; while users can input anything and have access to an infinite range of generations, they also must engage in brute-force trial and error with the text prompt when the result quality is poor. We conduct a study exploring what prompt keywords and model hyperparameters can help produce coherent outputs. In particular, we study prompts structured to include subject and style keywords and investigate success and failure modes of these prompts. Our evaluation of 5493 generations over the course of five experiments spans 51 abstract and concrete subjects as well as 51 abstract and figurative styles. From this evaluation, we present design guidelines that can help people produce better outcomes from text-to-image generative models.


---


###


Summary Notes


####


Enhancing Text-to-Image Generation with Prompt Engineering Techniques


In the dynamic realm of artificial intelligence, text-to-image generative models stand out as a revolutionary advancement. These models can turn written descriptions into detailed visual art, presenting new opportunities and challenges in prompt engineering.


Crafting well-designed prompts is essential to produce high-quality, relevant images. This blog explores insights from Columbia University's research, offering actionable advice for AI engineers in enterprise environments to refine text-to-image generation.


####


Overview of Experiments


Columbia University's Vivian Liu and Lydia B. Chilton conducted experiments to understand how different prompt parameters influence image quality.


Their research, involving 5493 image generations across 51 themes and styles, shed light on the intricacies of prompt engineering.


Key areas of focus included prompt wording, random starting points, ideal iteration counts, style handling, and the interplay between themes and styles.


####


Detailed Findings


- **Prompt Wording:** The experiments showed that the way a prompt is phrased has a minimal impact on the outcome. The critical factors are the chosen subjects and styles.


- **Random Seeds:** Using various seeds is important, as different starting points can lead to diverse outcomes.


- **Iteration Count:** Achieving desired results doesn't require numerous iterations; an optimal range is between 100 and 500.


- **Artistic Styles:** The effectiveness of models varies with different styles, showing a preference for certain interpretations.


- **Theme and Style Interaction:** The quality of generated images is significantly influenced by the combination of a theme's abstractness and the chosen style, with concrete themes and figurative styles often leading to better results.


####


Key Insights


The research offers valuable lessons for optimizing prompt engineering:


- **Prompt Composition:** Focus on the subject and style rather than the phrasing.


- **Variability of Seeds:** Experiment with multiple seeds to find the best outcomes due to the variability in initial conditions.


- **Efficient Iteration:** Fewer iterations can be sufficient, which can make the generation process faster without sacrificing quality.


- **Style Preferences:** Be aware of model biases towards certain styles.


- **Influence of Theme and Style:** The mix of a theme's nature and the style can greatly affect the generation quality.


####


Practical Guidelines for AI Engineers


To improve text-to-image generation, engineers should:


- **Prioritize Key Elements:** Emphasize subject and style keywords in prompts.


- **Use Multiple Seeds:** Test with various seeds to handle output variability.


- **Match Themes with Styles:** Consider how the abstractness of the subject and the chosen style interact, as concrete subjects paired with figurative styles usually produce better images.


####


Conclusion


The study by Liu and Chilton offers deep insights into prompt engineering for text-to-image models, helping AI engineers use these models more effectively.


By applying the study's guidelines, engineers can enhance control over image generation, leading to superior results.


####


Looking Forward


Future research will be essential for understanding how generative models interpret artistic styles and subjects more deeply.


Exploring complex prompt elements like mood or specific artistic techniques could expand text-to-image generation's creative and commercial potential in AI.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
