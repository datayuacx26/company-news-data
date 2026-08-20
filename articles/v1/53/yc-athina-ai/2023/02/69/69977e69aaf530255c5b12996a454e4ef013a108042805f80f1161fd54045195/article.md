---
schema_version: "1.0.0"
document_id: "69977e69aaf530255c5b12996a454e4ef013a108042805f80f1161fd54045195"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompting-gpt-3-to-be-reliable"
published_at: "2023-02-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:737be2f1e72f86e4cc96be9063231115c17298bfa6fe912142f6efaf87f54228"
---

# Prompting GPT-3 To Be Reliable

Do not index


Original Paper


[https://arxiv.org/abs/2210.09150](https://arxiv.org/abs/2210.09150)


Blog URL


[blog.athina.ai /prompti...reliable](https://blog.athina.ai/prompting-gpt-3-to-be-reliable)


**Original Paper:**[https://arxiv.org/abs/2210.09150](https://arxiv.org/abs/2210.09150)


**By:**[Chenglei Si](https://arxiv.org/search/cs?searchtype=author&query=Si%2C%20C) ,[Zhe Gan](https://arxiv.org/search/cs?searchtype=author&query=Gan%2C%20Z) ,[Zhengyuan Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Z) ,[Shuohang Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20S) ,[Jianfeng Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20J) ,[Jordan Boyd-Graber](https://arxiv.org/search/cs?searchtype=author&query=Boyd-Graber%2C%20J) ,[Lijuan Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20L)


**Abstract:**


> Large language models (LLMs) show impressive abilities via few-shot prompting. Commercialized APIs such as OpenAI GPT-3 further increase their use in real-world language applications. However, the crucial problem of how to improve the reliability of GPT-3 is still under-explored. While reliability is a broad and vaguely defined term, we decompose reliability into four main facets that correspond to the existing framework of ML safety and are well-recognized to be important: generalizability, social biases, calibration, and factuality. Our core contribution is to establish simple and effective prompts that improve GPT-3's reliability as it: 1) generalizes out-of-distribution, 2) balances demographic distribution and uses natural language instructions to reduce social biases, 3) calibrates output probabilities, and 4) updates the LLM's factual knowledge and reasoning chains. With appropriate prompts, GPT-3 is more reliable than smaller-scale supervised models on all these facets. We release all processed datasets, evaluation scripts, and model predictions. Our systematic empirical study not only sheds new insights on the reliability of prompting LLMs, but more importantly, our prompting strategies can help practitioners more reliably use LLMs like GPT-3.


---


###


Summary Notes


####


Enhancing GPT-3 Reliability: A Guide for AI Engineers


The advent of Large Language Models (LLMs) like GPT-3 has revolutionized the ability of machines to understand and generate text that closely resembles human writing.


Despite their capabilities, deploying these models, especially in business contexts, raises questions about their reliability. This blog post breaks down the strategies to improve GPT-3's reliability, drawing from the research paper "Prompting GPT-3 to Be Reliable."


####


Understanding Reliability in GPT-3


Reliability in GPT-3 can be seen from multiple angles:


- **Generalizability** : GPT-3 excels in adapting to new, unseen data distributions, making it superior in handling domain shifts and adversarial inputs.


- **Social Bias and Fairness** : Mitigating social bias is vital. Research shows that prompts with balanced demographic representations can reduce GPT-3's bias.


- **Calibration** : GPT-3 can give accurate confidence estimates for its predictions, often outperforming supervised models.


- **Factuality** : Prompt updates with fresh factual information allow GPT-3 to correct its inaccuracies, offering more accurate and current responses.


####


Research Insights and Practical Strategies


The paper details experiments across these reliability facets, yielding actionable insights:


- **Generalizability** : GPT-3's adaptability is confirmed, highlighting its robustness against domain shifts.


- **Bias Reduction** : Demographically balanced prompts are effective in lowering bias, emphasizing the importance of fair AI.


- **Calibration Analysis** : Certain prompting techniques lead to better confidence estimates from GPT-3, making it more reliable.


- **Factuality Updates** : GPT-3 can update its outputs with new information, stressing the role of prompt design in ensuring accuracy.


####


Conclusion and Future Directions


The research provides AI Engineers with strategies to enhance GPT-3's reliability in enterprise settings by focusing on generalizability, bias reduction, calibration, and factuality. As research progresses, these strategies will become more refined, improving GPT-3's utility.


####


Ethical Considerations


Improving GPT-3's reliability is also an ethical imperative. Efforts to reduce biases, enhance prediction confidence, and ensure accuracy can minimize potential harms and increase trust in AI technologies.


Ongoing research is crucial for overcoming challenges like adversarial attacks and preventing harmful outputs.


####


Further Exploration


For those interested in deeper insights, the research paper "Prompting GPT-3 to Be Reliable" is an invaluable resource. It offers detailed methodologies and findings for AI practitioners looking to apply these strategies in real-world applications.


To sum up, making GPT-3 more reliable is a complex but fruitful endeavor. By leveraging research insights and applying them practically, AI Engineers can lead the way in creating more reliable, fair, and effective AI solutions for business use.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
