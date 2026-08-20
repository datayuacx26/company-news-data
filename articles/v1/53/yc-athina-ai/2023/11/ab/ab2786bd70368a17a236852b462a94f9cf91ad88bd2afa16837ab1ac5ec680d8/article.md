---
schema_version: "1.0.0"
document_id: "ab2786bd70368a17a236852b462a94f9cf91ad88bd2afa16837ab1ac5ec680d8"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/fine-tuning-language-models-for-factuality"
published_at: "2023-11-14T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:609648513f940dc0c339c44ffd68578bd99008c6d3131460812758d004da6c70"
---

# Fine-tuning Language Models for Factuality

Do not index


Original Paper


[https://arxiv.org/abs/2311.08401](https://arxiv.org/abs/2311.08401)


Blog URL


[blog.athina.ai /fine-tu...ctuality](https://blog.athina.ai/fine-tuning-language-models-for-factuality)


**Original Paper:**[https://arxiv.org/abs/2311.08401](https://arxiv.org/abs/2311.08401)


**By:**[Katherine Tian](https://arxiv.org/search/cs?searchtype=author&query=Tian%2C%20K) ,[Eric Mitchell](https://arxiv.org/search/cs?searchtype=author&query=Mitchell%2C%20E) ,[Huaxiu Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C%20H) ,[Christopher D. Manning](https://arxiv.org/search/cs?searchtype=author&query=Manning%2C%20C%20D) ,[Chelsea Finn](https://arxiv.org/search/cs?searchtype=author&query=Finn%2C%20C)


**Abstract:**


> The fluency and creativity of large pre-trained language models (LLMs) have led to their widespread use, sometimes even as a replacement for traditional search engines. Yet language models are prone to making convincing but factually inaccurate claims, often referred to as 'hallucinations.' These errors can inadvertently spread misinformation or harmfully perpetuate misconceptions. Further, manual fact-checking of model responses is a time-consuming process, making human factuality labels expensive to acquire. In this work, we fine-tune language models to be more factual, without human labeling and targeting more open-ended generation settings than past work. We leverage two key recent innovations in NLP to do so. First, several recent works have proposed methods for judging the factuality of open-ended text by measuring consistency with an external knowledge base or simply a large model's confidence scores. Second, the direct preference optimization algorithm enables straightforward fine-tuning of language models on objectives other than supervised imitation, using a preference ranking over possible model responses. We show that learning from automatically generated factuality preference rankings, generated either through existing retrieval systems or our novel retrieval-free approach, significantly improves the factuality (percent of generated claims that are correct) of Llama-2 on held-out topics compared with RLHF or decoding strategies targeted at factuality. At 7B scale, compared to Llama-2-chat, we observe 58% and 40% reduction in factual error rate when generating biographies and answering medical questions, respectively.


---


###


Summary Notes


####


A Simplified Guide to Boosting the Accuracy of AI Language Models


In the cutting-edge realm of artificial intelligence (AI), language models like GPT-3.5 have transformed the way machines mimic human speech. Yet, these models often stumble by producing facts that aren't accurate, a phenomenon known as "hallucinations."


This issue risks the model's reliability and the spread of false information. A notable study from researchers at Stanford University and UNC Chapel Hill introduces a smart way to enhance the accuracy of these language models, using advanced natural language processing (NLP) techniques without depending heavily on human input.


####


The Accuracy Challenge with Language Models


Language models, despite their advanced capabilities, occasionally falter in generating accurate information. The effort to manually check facts is overwhelming and not feasible for the vast amount of data these models handle.


The researchers pointed out that although these models can indicate their own uncertainty, harnessing this to improve factual accuracy is a complex task.


####


Innovative Fine-Tuning Methods


The researchers proposed a method to refine language models for better accuracy through two main strategies:


- **Automated Factuality Assessment:** This involves checking the accuracy of text using external databases (reference-based) or the model's own confidence levels (reference-free).


- **Direct Preference Optimization (DPO):** This technique adjusts models to favor more accurate information over inaccuracies.


This method aims to cut down on inaccuracies efficiently, without the extensive need for human reviewers.


####


How It Works and What It Achieves


The approach uses preference-based reinforcement learning to automatically determine which responses are more factual. This includes:


- Generating multiple answers from the language model.


- Evaluating these answers for accuracy using external references or the model's confidence.


- Using the most accurate responses to refine the model's training.


Tests on creating biographies and medical Q&A showed a significant drop in inaccuracies—58% and 40% respectively—in a 7B scale model. Both reference-based and reference-free evaluation methods were effective, each offering distinct advantages.


####


Practical Insights for AI Engineers


For AI engineers in businesses, this research is a game-changer. By applying these fine-tuning strategies, engineers can boost the dependability of language models used in customer service bots, content creation, and more. Here are actionable tips:


- **Use the Latest NLP Tools:** Employ current advancements in automated fact-checking and reinforcement learning for your fine-tuning process.


- **Try Both Assessment Methods:** Depending on your needs, both reference-based and reference-free methods can be beneficial and possibly used together for better results.


- **Keep Evaluating and Adjusting:** Always monitor your model's performance and stay ready to adapt your methods with new breakthroughs.


####


Conclusion


The study by Tian and colleagues offers a promising path to making language models not just smarter, but also more reliable by reducing false information.


By adopting automated fact-checking and preference optimization, AI engineers can significantly improve the accuracy of these models. As we move forward, ongoing research and innovation in this field are essential for harnessing the full potential of language models—ensuring they are both intelligent and trustworthy.


For AI professionals, staying updated with these advancements means leading the way in AI development, ensuring the integrity of AI-generated content, and paving the road to a future where technology aligns with accuracy.
