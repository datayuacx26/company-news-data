---
schema_version: "1.0.0"
document_id: "10d892f258d6ae271361225a79f478bbfc3a2b564eff67f7d349d5e31e4a1159"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/segment-any-anomaly-without-training-via-hybrid-prompt-regularization"
published_at: "2023-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:59dfbff438200ff81cfd3cca876843f1984bdbb4c2787cf7aa2ecfb0c0e0e86f"
---

# Segment Any Anomaly without Training via Hybrid Prompt Regularization

Do not index


Original Paper


[https://arxiv.org/abs/2305.10724](https://arxiv.org/abs/2305.10724)


Blog URL


[blog.athina.ai /segment...rization](https://blog.athina.ai/segment-any-anomaly-without-training-via-hybrid-prompt-regularization)


**Original Paper:**[https://arxiv.org/abs/2305.10724](https://arxiv.org/abs/2305.10724)


**By:**[Yunkang Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20Y) ,[Xiaohao Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20X) ,[Chen Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20C) ,[Yuqi Cheng](https://arxiv.org/search/cs?searchtype=author&query=Cheng%2C%20Y) ,[Zongwei Du](https://arxiv.org/search/cs?searchtype=author&query=Du%2C%20Z) ,[Liang Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20L) ,[Weiming Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20W)


**Abstract:**


> We present a novel framework, i.e., Segment Any Anomaly + (SAA+), for zero-shot anomaly segmentation with hybrid prompt regularization to improve the adaptability of modern foundation models. Existing anomaly segmentation models typically rely on domain-specific fine-tuning, limiting their generalization across countless anomaly patterns. In this work, inspired by the great zero-shot generalization ability of foundation models like Segment Anything, we first explore their assembly to leverage diverse multi-modal prior knowledge for anomaly localization. For non-parameter foundation model adaptation to anomaly segmentation, we further introduce hybrid prompts derived from domain expert knowledge and target image context as regularization. Our proposed SAA+ model achieves state-of-the-art performance on several anomaly segmentation benchmarks, including VisA, MVTec-AD, MTD, and KSDD2, in the zero-shot setting. We will release the code at \\href{
>
>
> [this https URL](https://github.com/caoyunkang/Segment-Any-Anomaly)
>
>
> [this https URL](https://github.com/caoyunkang/Segment-Any-Anomaly)


---


###


Summary Notes


####


SAA+ for Zero-Shot Anomaly Segmentation


In the fast-paced world of artificial intelligence, identifying anomalies in images is crucial for areas like industrial inspections and medical imaging.


Traditional methods, which train on normal data to spot anomalies, often fall short due to the wide variety of anomalies and the challenge of collecting enough training data.


This is where the innovative approach of zero-shot anomaly segmentation (ZSAS) comes into play, using foundation models without needing specific training data for each domain.


####


**Understanding the SAA Framework**


The Segment Any Anomaly (SAA) framework is at the forefront of this approach. It uses a combination of anomaly detection and refinement processes, guided by basic language prompts like "Anomaly".


However, this method sometimes misidentifies normal areas as anomalies due to vague prompts and differences in the foundational model's initial training.


####


**Improving with SAA+**


To overcome these issues, the Segment Any Anomaly Plus (SAA+) framework was developed. SAA+ improves accuracy by:


- **Hybrid Prompts** : Combining detailed anomaly descriptions with image-specific information.


- **Domain Expert Knowledge** : Using accurate descriptions of anomalies as prompts.


- **Image Context** : Employing tools to highlight and refine the focus on actual anomalies.


####


**How SAA+ Works**


SAA+ uses a sophisticated technique called hybrid prompt regularization. This approach guides foundation models by blending textual descriptions, object properties, and image-specific prompts.


Tested on various datasets, SAA+ has shown impressive results in identifying anomalies without prior training on them, setting new benchmarks in the field.


####


**Exploring the Methodology and Results**


SAA+ builds on previous research into unsupervised learning and prompt engineering. Its methodology revolves around zero-shot anomaly segmentation, focusing on generating and refining anomaly regions with the help of hybrid prompts.


The effectiveness of this method has been proven across several datasets, demonstrating SAA+'s superiority in anomaly segmentation.


####


**Looking Forward**


The SAA+ framework marks a significant advance, offering a robust solution for zero-shot anomaly detection. Future efforts will focus on using larger foundation models and improving prompt engineering to address more anomaly types.


While challenges remain in scalability and computational demands, the potential for SAA+ in diverse applications is expansive.


In summary, SAA+ introduces a groundbreaking approach to anomaly detection, promising more efficient and accurate segmentation across various fields.


This development opens new avenues for research and practical applications, bringing us closer to solving the complex challenge of anomaly detection in AI.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
