---
schema_version: "1.0.0"
document_id: "8474489a4fd2abb38e04654cb48f3884f0704bbf4c856599b89b828718fea0f1"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/exploring-the-relationship-between-llm-hallucinations-and-prompt-linguistic-nuances-readability-formality-and-concreteness"
published_at: "2023-09-20T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:f5d4c28f4f22abec50edf9829c51b728d3575a4e6049dea42c5591008972bf06"
---

# Exploring the Relationship between LLM Hallucinations and Prompt Linguistic Nuances: Readability, Formality, and Concreteness

Do not index


Original Paper


[https://arxiv.org/abs/2309.11064](https://arxiv.org/abs/2309.11064)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2309.11064](https://arxiv.org/abs/2309.11064)


**By:**[Vipula Rawte](https://arxiv.org/search/cs?searchtype=author&query=Rawte%2C%20V) ,[Prachi Priya](https://arxiv.org/search/cs?searchtype=author&query=Priya%2C%20P) ,[S.M Towhidul Islam Tonmoy](https://arxiv.org/search/cs?searchtype=author&query=Tonmoy%2C%20S%20T%20I) ,[S M Mehedi Zaman](https://arxiv.org/search/cs?searchtype=author&query=Zaman%2C%20S%20M%20M) ,[Amit Sheth](https://arxiv.org/search/cs?searchtype=author&query=Sheth%2C%20A) ,[Amitava Das](https://arxiv.org/search/cs?searchtype=author&query=Das%2C%20A)


**Abstract:**


> As Large Language Models (LLMs) have advanced, they have brought forth new challenges, with one of the prominent issues being LLM hallucination. While various mitigation techniques are emerging to address hallucination, it is equally crucial to delve into its underlying causes. Consequently, in this preliminary exploratory investigation, we examine how linguistic factors in prompts, specifically readability, formality, and concreteness, influence the occurrence of hallucinations. Our experimental results suggest that prompts characterized by greater formality and concreteness tend to result in reduced hallucination. However, the outcomes pertaining to readability are somewhat inconclusive, showing a mixed pattern.


---


###


Summary Notes


####


Understanding the Impact of Prompt Characteristics on LLM Hallucinations


In the world of artificial intelligence, Large Language Models (LLMs) like GPT-4 are transforming industries with applications ranging from chatbots to automated content creation.


However, LLMs can sometimes "hallucinate," generating incorrect or misleading information. This blog post explores how the features of prompts, such as their readability, formality, and concreteness, affect these hallucinations, offering valuable insights for AI engineers.


####


What Causes LLM Hallucinations?


Hallucinations in LLMs refer to outputs that contain incorrect information, which can significantly impact their reliability. These hallucinations can involve:


- **Person (P):** Creating fictional characters.


- **Location (L):** Mentioning non-existent places.


- **Number (N):** Providing wrong numerical data.


- **Acronym (A):** Generating inaccurate details.


####


Exploring the Impact: Study Methodology


The study employed tweets from New York Times events as accurate prompts to investigate how they influence LLM responses.


It analyzed responses from 15 different LLMs, including GPT-2 to GPT-4, OPT, LLaMA, and BLOOM, by using Amazon Mechanical Turk for detailed annotation.


The goal was to link hallucination instances with the prompts' readability, formality, and concreteness.


####


Key Insights: Readability, Formality, and Concreteness


The findings from this research provide a deeper understanding of how prompt features affect LLM behavior:


- **Readability:** Surprisingly, readability had a variable impact on hallucinations. Both simple and complex prompts could lower hallucination rates if they were formal.


- **Formality:** Prompts with a higher level of formality were less likely to lead to hallucinations. A formal tone seems to guide LLMs more clearly, reducing errors.


- **Concreteness:** Detailed, specific prompts were effective in minimizing hallucinations. In contrast, vague or abstract prompts increased hallucination risks, especially with numbers and acronyms.


####


Implications and Next Steps


This research highlights the importance of prompt design in enhancing LLM reliability, especially for enterprise applications where accuracy is crucial.


Future research could explore how different LLM architectures handle these linguistic nuances and examine other prompt characteristics like emotional tone or cultural context to further reduce hallucinations.


####


Conclusion


The relationship between prompt characteristics and LLM hallucinations is a key focus for AI engineers aiming to improve model reliability. Emphasizing formality and concreteness in prompts and understanding the role of readability can significantly lower the incidence of hallucinations.


As we delve deeper into LLM behavior, these insights will guide the development of more reliable and effective AI applications across various sectors, marking a crucial step forward in the journey towards leveraging the full potential of generative AI technologies.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
