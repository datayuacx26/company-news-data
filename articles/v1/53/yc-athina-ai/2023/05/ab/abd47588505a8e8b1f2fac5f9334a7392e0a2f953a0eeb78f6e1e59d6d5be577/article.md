---
schema_version: "1.0.0"
document_id: "abd47588505a8e8b1f2fac5f9334a7392e0a2f953a0eeb78f6e1e59d6d5be577"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/are-chatbots-ready-for-privacy-sensitive-applications-an-investigation-into-input-regurgitation-and-prompt-induced-sanitization"
published_at: "2023-05-24T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:b6991a3f2a563640a4880bfb9725131de56b423943f8de42c6e2cf892539385a"
---

# Are Chatbots Ready for Privacy-Sensitive Applications? An Investigation into Input Regurgitation and Prompt-Induced Sanitization

Do not index


Original Paper


[https://arxiv.org/abs/2305.15008](https://arxiv.org/abs/2305.15008)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2305.15008](https://arxiv.org/abs/2305.15008)


**By:**[Aman Priyanshu](https://arxiv.org/search/cs?searchtype=author&query=Priyanshu%2C%20A) ,[Supriti Vijay](https://arxiv.org/search/cs?searchtype=author&query=Vijay%2C%20S) ,[Ayush Kumar](https://arxiv.org/search/cs?searchtype=author&query=Kumar%2C%20A) ,[Rakshit Naidu](https://arxiv.org/search/cs?searchtype=author&query=Naidu%2C%20R) ,[Fatemehsadat Mireshghallah](https://arxiv.org/search/cs?searchtype=author&query=Mireshghallah%2C%20F)


**Abstract:**


> LLM-powered chatbots are becoming widely adopted in applications such as healthcare, personal assistants, industry hiring decisions, etc. In many of these cases, chatbots are fed sensitive, personal information in their prompts, as samples for in-context learning, retrieved records from a database, or as part of the conversation. The information provided in the prompt could directly appear in the output, which might have privacy ramifications if there is sensitive information there. As such, in this paper, we aim to understand the input copying and regurgitation capabilities of these models during inference and how they can be directly instructed to limit this copying by complying with regulations such as HIPAA and GDPR, based on their internal knowledge of them. More specifically, we find that when ChatGPT is prompted to summarize cover letters of a 100 candidates, it would retain personally identifiable information (PII) verbatim in 57.4% of cases, and we find this retention to be non-uniform between different subgroups of people, based on attributes such as gender identity. We then probe ChatGPT's perception of privacy-related policies and privatization mechanisms by directly instructing it to provide compliant outputs and observe a significant omission of PII from output.


---


###


Summary Notes


####


Ensuring Privacy in AI: A Close Look at Chatbots in Sensitive Applications


As artificial intelligence (AI), especially Large Language Models (LLMs) like ChatGPT, becomes more integrated into sectors like healthcare and finance, concerns about privacy have surged.


This blog explores how LLM-powered chatbots handle sensitive information, focusing on the risks of retaining and potentially leaking personally identifiable information (PII) and protected health information (PHI), and evaluates methods to prevent such breaches.


####


Understanding the Basics


First, let's cover some key concepts:


- **Large Language Models (LLMs)** , such as GPT-3, can generate text that's remarkably human-like but face challenges in securely managing sensitive data.


- **Prompting** involves giving structured inputs to LLMs to elicit specific responses, critical for directing how chatbots process information.


- **Privacy Risks** are heightened when LLMs process PHI and PII, with potential for serious privacy breaches.


- **Chatbot Privacy** is a concern due to their capacity to remember and share user data, posing risks in settings that demand confidentiality.


####


Study Focus


The study examined two critical areas:


1. **Input Regurgitation:** Investigating ChatGPT's tendency to store and reveal PHI and PII from interactions.


1. **Prompt-Induced Sanitization:** Assessing if tailored prompts can guide ChatGPT to comply with privacy laws like HIPAA and GDPR.


####


Key Findings


Using synthetic medical records and cover letters, the study found:


- **PII Retention:** ChatGPT often retained and reproduced PII without sanitization prompts. With these prompts, however, the rate significantly decreased.


- **PHI Handling:** A similar pattern was observed with PHI, indicating the effectiveness of specific prompts in enhancing privacy.


- **Sanitization Prompts:** These are crucial for minimizing privacy risks and ensuring legal compliance.


####


Implications and Next Steps


This research highlights the importance of careful LLM deployment in privacy-sensitive areas and the potential of prompt-based techniques to improve privacy safeguards.


Future efforts should refine these techniques and explore new methods to protect sensitive data in LLM outputs.


####


Conclusion


LLMs like ChatGPT bring revolutionary benefits to various sectors but must be employed cautiously in privacy-sensitive areas.


This study demonstrates the effectiveness of prompt-based strategies in reducing privacy risks, paving the way for safer LLM use in such settings.


####


Contributions


This work adds to the understanding of privacy challenges with LLMs in sensitive sectors and highlights the effectiveness of prompt-based privacy measures, laying groundwork for future advancements in privacy protection in AI.


Ensuring LLMs handle sensitive information responsibly is crucial. As AI evolves, so too must our privacy protection strategies, ensuring technological progress does not come at the cost of individual rights.


####


References


The study was informed by HIPAA and GDPR regulations, and research on LLMs, AI privacy risks, and secure data handling, providing a well-rounded perspective on the privacy challenges of using LLMs in sensitive applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
