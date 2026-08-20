---
schema_version: "1.0.0"
document_id: "659162142ea157fd857e1626e7a7a28594255c6ca270bb90458cde15c6e0c82e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/tcp-textual-based-class-aware-prompt-tuning-for-visual-language-model"
published_at: "2024-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:8872ea18ab00e3393150774b4ca8470913191b6a1ad2f7c1c1c7f822d87e012a"
---

# TCP:Textual-based Class-aware Prompt tuning for Visual-Language Model

Do not index


Original Paper


[https://arxiv.org/abs/2311.18231](https://arxiv.org/abs/2311.18231)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2311.18231](https://arxiv.org/abs/2311.18231)


**By:**[Hantao Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C%20H) ,[Rui Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20R) ,[Changsheng Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20C)


**Abstract:**


> Prompt tuning represents a valuable technique for adapting pre-trained visual-language models (VLM) to various downstream tasks. Recent advancements in CoOp-based methods propose a set of learnable domain-shared or image-conditional textual tokens to facilitate the generation of task-specific textual classifiers. However, those textual tokens have a limited generalization ability regarding unseen domains, as they cannot dynamically adjust to the distribution of testing classes. To tackle this issue, we present a novel Textual-based Class-aware Prompt tuning(TCP) that explicitly incorporates prior knowledge about classes to enhance their discriminability. The critical concept of TCP involves leveraging Textual Knowledge Embedding (TKE) to map the high generalizability of class-level textual knowledge into class-aware textual tokens. By seamlessly integrating these class-aware prompts into the Text Encoder, a dynamic class-aware classifier is generated to enhance discriminability for unseen domains. During inference, TKE dynamically generates class-aware prompts related to the unseen classes. Comprehensive evaluations demonstrate that TKE serves as a plug-and-play module effortlessly combinable with existing methods. Furthermore, TCP consistently achieves superior performance while demanding less training time. Code:
>
>
> [this https URL](https://github.com/htyao89/Textual-based_Class-aware_prompt_tuning/)


---


###


Summary Notes


##


Boosting Vision-Language Models with Textual-based Class-aware Prompt Tuning (TCP)


In the fast-moving world of Artificial Intelligence (AI), blending visual and textual data through Vision-Language Models (VLMs) marks a revolutionary step forward.


Despite their advanced capabilities in processing multifaceted data, these models often fall short when tasked with specific applications, limiting their practical use.


Textual-based Class-aware Prompt Tuning (TCP) emerges as an innovative solution, aiming to enhance VLMs by infusing them with class-specific textual insights.


This blog explores how TCP refines the functionality of VLMs, offering insights for AI Engineers in commercial settings.


###


Exploring Vision-Language Models (VLMs) and Their Limitations


VLMs stand out for their ability to digest and merge visual and textual inputs. Models like CLIP, trained on vast sets of image-text pairs, excel at creating rich visual and textual representations. Yet, their application in specialized tasks without bespoke tuning remains a hurdle.


Here, prompt tuning steps in, attempting to guide the model's focus through task-relevant textual cues. Nevertheless, this technique often stumbles in promoting clear class distinction and adaptability, especially concerning unseen classes.


###


Introducing Textual Knowledge Embedding (TKE)


At the core of TCP is Textual Knowledge Embedding (TKE), a mechanism that infuses class-level textual understanding into class-specific prompts.


This not only betters VLMs' performance on familiar classes but also significantly enhances their competence with new, unseen classes. By dynamically incorporating these tailored prompts into the Text Encoder, TKE achieves a deeper, more refined fusion of textual and visual data.


###


How TCP Works


TCP builds upon Context Optimization (CoOp) by integrating TKE, creating class-aware prompts from class descriptions.


These prompts are refined through contrastive loss and knowledge-consistency during training, leading to a model that's adept across various scenarios, including adapting to new classes, transferring knowledge across datasets, and excelling in few-shot learning. TCP's brilliance lies in its compatibility with existing prompt tuning methods, serving as a modular enhancement that introduces learnable, knowledge-rich prompt tokens.


###


Performance Evaluation of TCP


TCP's superiority is demonstrated through its impressive outcomes across different datasets and evaluation standards. Notably, its ability to generalize from known to unseen classes sets a new benchmark, clearly surpassing existing methods.


Its consistent performance in cross-dataset generalization further proves TCP's broad applicability, indicating its potential to reduce training durations while improving class differentiation capabilities.


###


Insights for AI Engineers


For AI professionals in the corporate sector, grasping TCP's mechanics is vital. TCP employs adaptable prompt tokens enriched with prior class knowledge, improving class distinction.


Its modular design ensures seamless integration with current prompt tuning frameworks, positioning it as a valuable tool for refining VLM performance.


Extensive testing validates TCP's efficacy, presenting a promising avenue for those aiming to refine prompt tuning techniques and extend their use cases.


###


Conclusion: A New Milestone for Vision-Language Models


Textual-based Class-aware Prompt Tuning sets a new standard in enhancing the adaptability and efficiency of Vision-Language Models.


By weaving class-specific textual knowledge into the learning fabric, TCP not only uplifts model performance on established benchmarks but also pioneers a novel approach to class-aware learning in multimodal models.


For AI Engineers in enterprise environments, TCP offers a robust solution to the challenge of customizing VLMs for distinct tasks, paving the way for future advancements and broader applications in the AI domain.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
