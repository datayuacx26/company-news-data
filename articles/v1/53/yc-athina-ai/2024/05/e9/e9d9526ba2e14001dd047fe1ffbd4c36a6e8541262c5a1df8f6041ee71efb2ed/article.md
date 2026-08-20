---
schema_version: "1.0.0"
document_id: "e9d9526ba2e14001dd047fe1ffbd4c36a6e8541262c5a1df8f6041ee71efb2ed"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/assessing-prompt-injection-risks-in-200-custom-gpts"
published_at: "2024-05-25T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:6412a3a8a406c8def6836aa54dd6e2c34a95f6373b9e4b72df4eb9bd59083745"
---

# Assessing Prompt Injection Risks in 200+ Custom GPTs

Do not index


Original Paper


[https://arxiv.org/abs/2311.11538](https://arxiv.org/abs/2311.11538)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2311.11538](https://arxiv.org/abs/2311.11538)


**By:**[Jiahao Yu](https://arxiv.org/search/cs?searchtype=author&query=Yu%2C%20J) ,[Yuhang Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Y) ,[Dong Shu](https://arxiv.org/search/cs?searchtype=author&query=Shu%2C%20D) ,[Mingyu Jin](https://arxiv.org/search/cs?searchtype=author&query=Jin%2C%20M) ,[Sabrina Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20S) ,[Xinyu Xing](https://arxiv.org/search/cs?searchtype=author&query=Xing%2C%20X)


**Abstract:**


> In the rapidly evolving landscape of artificial intelligence, ChatGPT has been widely used in various applications. The new feature - customization of ChatGPT models by users to cater to specific needs has opened new frontiers in AI utility. However, this study reveals a significant security vulnerability inherent in these user-customized GPTs: prompt injection attacks. Through comprehensive testing of over 200 user-designed GPT models via adversarial prompts, we demonstrate that these systems are susceptible to prompt injections. Through prompt injection, an adversary can not only extract the customized system prompts but also access the uploaded files. This paper provides a first-hand analysis of the prompt injection, alongside the evaluation of the possible mitigation of such attacks. Our findings underscore the urgent need for robust security frameworks in the design and deployment of customizable GPT models. The intent of this paper is to raise awareness and prompt action in the AI community, ensuring that the benefits of GPT customization do not come at the cost of compromised security and privacy.


---


###


Summary Notes


##


Evaluating the Security Risks of Custom GPT Models Against Prompt Injection


As the use of ChatGPT and its variants grows in various sectors, their customization through the GPT Store, offering over 200 models, raises significant security concerns. Prompt injection attacks, in particular, pose a threat to the privacy and integrity of sensitive information.


This post explores the risks of prompt injection in these custom models, emphasizing the need for stronger security measures.


###


Security Rispects Identified


- **System Prompt Extraction** : The risk involves unauthorized access to system prompts, threatening intellectual property and privacy.


- **File Leakage** : Through prompt injection, sensitive files can be accessed and stolen, compromising confidentiality.


###


Research Methodology


We examined the vulnerability of 200+ custom GPT models to prompt injection by using adversarial prompts to test for system prompt extraction and file leakage.


###


Understanding Custom GPT Models


Custom GPT models are tailored for specific tasks, offering great benefits but also introducing security vulnerabilities, especially to prompt injection attacks.


###


Investigation Approach


- **Scanning for Vulnerabilities** : We searched the models for weaknesses.


- **Adversarial Prompt Injection** : We used specially crafted prompts to extract information.


- **API Exploitation** : Demonstrated how APIs could be manipulated to extract sensitive data.


###


Experiment Findings


- **Prompt Injection Trials** : Showed a high rate of information extraction from custom GPTs, bypassing even defensive measures.


- **Defense Mechanisms Testing** : Defensive prompts were largely ineffective against sophisticated prompt injection techniques.


###


Ethical Considerations and Mitigation


We conducted this research ethically, with transparency and responsible disclosure. Recommendations include not storing sensitive data within GPTs and improving prompt security.


###


Conclusion


Our study underlines the need for the AI community to enhance security measures for custom GPT models to protect against prompt injection attacks, ensuring the reliability of AI technologies in sensitive environments.


###


Additional Resources


The full report contains detailed methodologies, experiment results, and recommendations for AI engineers to mitigate prompt injection risks, supporting the security of AI innovations.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
