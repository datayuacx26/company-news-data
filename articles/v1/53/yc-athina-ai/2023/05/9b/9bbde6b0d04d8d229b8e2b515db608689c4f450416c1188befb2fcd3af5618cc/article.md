---
schema_version: "1.0.0"
document_id: "9bbde6b0d04d8d229b8e2b515db608689c4f450416c1188befb2fcd3af5618cc"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/self-contradictory-hallucinations-of-large-language-models-evaluation-detection-and-mitigation"
published_at: "2023-05-25T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:594798859ee581b96130b9879d0056c9b1d580c7e60971ca810497c256fa90c5"
---

# Self-contradictory Hallucinations of Large Language Models: Evaluation, Detection and Mitigation

Do not index


Original Paper


[https://arxiv.org/abs/2305.15852](https://arxiv.org/abs/2305.15852)


Blog URL


[blog.athina.ai /self-co...tigation](https://blog.athina.ai/self-contradictory-hallucinations-of-large-language-models-evaluation-detection-and-mitigation)


**Original Paper:**[https://arxiv.org/abs/2305.15852](https://arxiv.org/abs/2305.15852)


**By:**[Niels Mündler](https://arxiv.org/search/cs?searchtype=author&query=M%C3%BCndler%2C%20N) ,[Jingxuan He](https://arxiv.org/search/cs?searchtype=author&query=He%2C%20J) ,[Slobodan Jenko](https://arxiv.org/search/cs?searchtype=author&query=Jenko%2C%20S) ,[Martin Vechev](https://arxiv.org/search/cs?searchtype=author&query=Vechev%2C%20M)


**Abstract:**


> Large language models (large LMs) are susceptible to producing text that contains hallucinated content. An important instance of this problem is self-contradiction, where the LM generates two contradictory sentences within the same context. In this work, we present a comprehensive investigation into self-contradiction for various instruction-tuned LMs, covering evaluation, detection, and mitigation. Our primary evaluation task is open-domain text generation, but we also demonstrate the applicability of our approach to shorter question answering. Our analysis reveals the prevalence of self-contradictions, e.g., in 17.7% of all sentences produced by ChatGPT. We then propose a novel prompting-based framework designed to effectively detect and mitigate self-contradictions. Our detector achieves high accuracy, e.g., around 80% F1 score when prompting ChatGPT. The mitigation algorithm iteratively refines the generated text to remove contradictory information while preserving text fluency and informativeness. Importantly, our entire framework is applicable to black-box LMs and does not require retrieval of external knowledge. Rather, our method complements retrieval-based methods, as a large portion of self-contradictions (e.g., 35.2% for ChatGPT) cannot be verified using online text. Our approach is practically effective and has been released as a push-button tool to benefit the public at
>
>
> [this https URL](https://chatprotect.ai/)


---


###


Summary Notes


####


Addressing Self-Contradiction in Large Language Models for Better AI Solutions


As artificial intelligence (AI) continues to grow, Large Language Models (LLMs) like ChatGPT are becoming essential tools in various industries for tasks such as problem-solving, customer service, and content creation.


Despite their benefits, these models often face a challenge that can hinder their effectiveness: producing self-contradictory text. This issue not only reduces trust in AI but also limits its practical use in business settings.


####


Understanding the Self-Contradiction Problem


Self-contradiction occurs when an LLM generates text with statements that logically conflict with each other. This issue affects the content's coherence and consistency, with a notable occurrence rate of 17.7% in models like ChatGPT. Such contradictions can negatively impact user experience and the decision-making process in critical business operations.


####


A Novel Approach: Prompting-Based Framework


Researchers Niels Mündler, Jingxuan He, Slobodan Jenko, and Martin Vechev from ETH Zurich have developed a promising strategy to address this issue.


Their prompting-based framework detects and reduces self-contradictions in LLM outputs without relying on external knowledge sources. This technique maintains the text's integrity while significantly lowering contradiction instances.


####


**Study Highlights:**


- **Widespread Issue:** The research identifies a high rate of self-contradictions across different models, emphasizing the need for effective solutions.


- **Successful Mitigation:** The framework can decrease self-contradictions by up to 89.5%, ensuring the content remains informative and coherent.


- **Availability of Tools:** The researchers have made their tool,[chatprotect.ai](http://chatprotect.ai/) , and the codebase public on GitHub to facilitate practical application.


####


Implementing the Framework: Tips for AI Engineers


####


**Managing Contradictions**


- **Triggering:** Create prompts that might lead to contradictory responses to test the model's reasoning.


- **Detecting:** Use the framework to find and analyze contradictions in the content.


- **Mitigating:** Refine the text by removing or adjusting contradictory parts to improve coherence.


####


**Testing and Evaluation**


- **Model Variety:** Test the framework on different models (e.g., GPT-4, ChatGPT) to gauge its effectiveness.


- **Application to Tasks:** Explore the framework's use in specific tasks like text generation and question-answering for tailored enterprise solutions.


- **Performance Measurement:** Track the reduction in contradictions and assess other quality metrics like informativeness and fluency.


####


Broadening Framework Use


This framework is not limited to text generation; its application in question-answering tasks suggests potential for wider use in AI solutions for businesses.


By customizing the framework's steps, AI engineers can enhance various applications, from customer support bots to automated content creation tools.


####


Conclusion: Boosting AI Trust and Reliability


The work of Mündler et al. significantly contributes to improving LLM reliability by offering a solid method to tackle self-contradictions.


This advancement not only deepens our understanding of LLM limitations but also provides AI engineers with effective tools to enhance AI-generated content's quality and trustworthiness.


Reliable, coherent, and contradiction-free content is crucial as AI integrates more into business operations. With this prompting-based framework and the open-source tool[chatprotect.ai](http://chatprotect.ai/) ,


AI engineers have essential resources to overcome the challenge of self-contradiction in LLMs.


This development marks a step forward in creating more sophisticated and reliable AI solutions for the business sector.


---


####


**How Athina AI can help**
