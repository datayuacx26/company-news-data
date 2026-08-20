---
schema_version: "1.0.0"
document_id: "dc248c0eba1b152931ad7b166d26788a69adfd0ad1ea1c6b2f10719058ba39cc"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-engineering-or-fine-tuning-an-empirical-assessment-of-large-language-models-in-automated-software-engineering-tasks"
published_at: "2023-10-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:6ad5e3bd012d224ee1e96d2d237758ae004cbba8c3951f660285a12c3d4c6d37"
---

# Prompt Engineering or Fine Tuning: An Empirical Assessment of Large Language Models in Automated Software Engineering Tasks

Do not index


Original Paper


[https://arxiv.org/abs/2310.10508](https://arxiv.org/abs/2310.10508)


Blog URL


[blog.athina.ai /prompt-...ng-tasks](https://blog.athina.ai/prompt-engineering-or-fine-tuning-an-empirical-assessment-of-large-language-models-in-automated-software-engineering-tasks)


**Original Paper:**[https://arxiv.org/abs/2310.10508](https://arxiv.org/abs/2310.10508)


**By:**[Jiho Shin](https://arxiv.org/search/cs?searchtype=author&query=Shin%2C%20J) ,[Clark Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20C) ,[Tahmineh Mohati](https://arxiv.org/search/cs?searchtype=author&query=Mohati%2C%20T) ,[Maleknaz Nayebi](https://arxiv.org/search/cs?searchtype=author&query=Nayebi%2C%20M) ,[Song Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20S) ,[Hadi Hemmati](https://arxiv.org/search/cs?searchtype=author&query=Hemmati%2C%20H)


**Abstract:**


> In this paper, we investigate the effectiveness of state-of-the-art LLM, i.e., GPT-4, with three different prompting engineering techniques (i.e., basic prompting, in-context learning, and task-specific prompting) against 18 fine-tuned LLMs on three typical ASE tasks, i.e., code generation, code summarization, and code translation. Our quantitative analysis of these prompting strategies suggests that prompt engineering GPT-4 cannot necessarily and significantly outperform fine-tuning smaller/older LLMs in all three tasks. For comment generation, GPT-4 with the best prompting strategy (i.e., task-specific prompt) had outperformed the first-ranked fine-tuned model by 8.33% points on average in BLEU. However, for code generation, the first-ranked fine-tuned model outperforms GPT-4 with best prompting by 16.61% and 28.3% points, on average in BLEU. For code translation, GPT-4 and fine-tuned baselines tie as they outperform each other on different translation tasks. To explore the impact of different prompting strategies, we conducted a user study with 27 graduate students and 10 industry practitioners. From our qualitative analysis, we find that the GPT-4 with conversational prompts (i.e., when a human provides feedback and instructions back and forth with a model to achieve best results) showed drastic improvement compared to GPT-4 with automatic prompting strategies. Moreover, we observe that participants tend to request improvements, add more context, or give specific instructions as conversational prompts, which goes beyond typical and generic prompting strategies. Our study suggests that, at its current state, GPT-4 with conversational prompting has great potential for ASE tasks, but fully automated prompt engineering with no human in the loop requires more study and improvement.


---


###


Summary Notes


####


Unveiling the Potential of GPT-4 in Software Engineering


The introduction of Large Language Models (LLMs) like GPT-4 is transforming the landscape of software engineering by automating complex tasks.


These models hold the promise of revolutionizing code generation, summarization, and translation, but their success depends on tailoring them to specific Automated Software Engineering (ASE) tasks.


This blog post explores how prompt engineering and fine-tuning play crucial roles in harnessing the power of GPT-4, sharing key insights for AI Engineers in the industry.


####


Exploring Methods: Quantitative and Qualitative


The study under discussion undertook a mixed-methods approach:


####


Quantitative Analysis Highlights


- **Tasks:** Focused on code generation, summarization, and translation.


- **Comparison:** GPT-4's adaptability and performance were measured against 18 fine-tuned models.


- **Metrics:** Performance was quantified using BLEU scores, a standard accuracy metric in machine translation.


####


Qualitative Analysis Highlights


- **Participants:** Included 27 graduate students and 10 industry experts.


- **Approach:** Used conversational prompts with GPT-4, providing insights into the model's strengths and weaknesses.


####


Discoveries and Insights


####


Quantitative Findings


- **Code Generation:** GPT-4 matched fine-tuned models in performance.


- **Code Summarization:** GPT-4 surpassed the top fine-tuned model by 8.33% in BLEU score with effective prompts.


- **Code Translation:** GPT-4 and fine-tuned models performed comparably, each excelling in different areas.


####


Qualitative Observations


- **Conversational Prompts:** GPT-4 showed improved performance with nuanced prompting.


- **Prompt Optimization:** Participants were able to enhance GPT-4's output by refining prompts.


- **Challenges:** The study highlights the need for research into automating prompt engineering to reduce the need for manual intervention.


####


Conclusions and Practical Implications


GPT-4 shows promise in ASE when used with conversational prompts, yet the journey to fully optimize LLMs for software engineering continues. The findings suggest:


- **Prompt Engineering's Role:** Effective in some cases, but not always superior to fine-tuning.


- **The Value of a Hybrid Approach:** Combining human insight with machine learning could be the best strategy for leveraging LLMs in software development.


####


Future Directions


Research should focus on:


- **Automating Prompt Engineering:** Minimizing human intervention in crafting prompts.


- **Identifying Effective Prompts:** Understanding what makes prompts successful in ASE tasks.


####


Contribution to Software Engineering


This research provides a detailed comparison between prompt engineering and fine-tuning within ASE, offering insights into optimizing LLM interactions for more effective software development processes.


In summary, as AI continues to advance, understanding how to best integrate tools like GPT-4 into software development practices is invaluable. This study not only sheds light on current capabilities but also challenges us to innovate further in AI integration.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
