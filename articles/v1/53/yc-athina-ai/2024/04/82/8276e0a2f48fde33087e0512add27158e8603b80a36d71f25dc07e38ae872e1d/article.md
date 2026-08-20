---
schema_version: "1.0.0"
document_id: "8276e0a2f48fde33087e0512add27158e8603b80a36d71f25dc07e38ae872e1d"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/cyberseceval-2-a-wide-ranging-cybersecurity-evaluation-suite-for-large-language-models"
published_at: "2024-04-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:c2ee51719f5f81da4f63aef26017c80dcfd5673fc6fe8871c1774b28537169a0"
---

# CYBERSECEVAL 2: A Wide-Ranging Cybersecurity Evaluation Suite for Large Language Models

Do not index


Original Paper


[https://ai.meta.com/research/publications/cyberseceval-2-a-wide-ranging-cybersecurity-evaluation-suite-for-large-language-models/](https://ai.meta.com/research/publications/cyberseceval-2-a-wide-ranging-cybersecurity-evaluation-suite-for-large-language-models/)


Blog URL


[blog.athina.ai /cyberse...e-models](https://blog.athina.ai/cyberseceval-2-a-wide-ranging-cybersecurity-evaluation-suite-for-large-language-models)


**Original Paper:**[https://ai.meta.com/research/publications/cyberseceval-2-a-wide-ranging-cybersecurity-evaluation-suite-for-large-language-models/](https://ai.meta.com/research/publications/cyberseceval-2-a-wide-ranging-cybersecurity-evaluation-suite-for-large-language-models/)


**By:**


Manish Bhatt∗, Sahana Chennabasappa∗, Yue Li∗, Cyrus Nikolaidis∗, Daniel Song∗, Shengye Wan∗, Faizan Ahmad, Cornelius Aschermann, Yaohui Chen, Dhaval Kapil, David Molnar, Spencer Whitman, Joshua Saxe∗


∗Co-equal primary author


**Abstract:**


> Large language models (LLMs) introduce new security risks, but there are few comprehensive evaluation suites to measure and reduce these risks. We present CYBERSECEVAL 2, a novel benchmark to quantify LLM security risks and capabilities. We introduce two new areas for testing: prompt injection and code interpreter abuse. We evaluated multiple state of the art (SOTA) LLMs, including GPT-4, Mistral, Meta Llama 3 70B-Instruct, and Code Llama. Our results show conditioning away risk of attack remains an unsolved problem; for example, all tested models showed between 26% and 41% successful prompt injection tests. Our code is open source and can be used to evaluate other LLMs. We further introduce the safety-utility tradeoff : conditioning an LLM to reject unsafe prompts can cause the LLM to falsely reject answering benign prompts, which lowers utility. We propose quantifying this tradeoff using False Refusal Rate (FRR). As an illustration, we introduce a novel test set to quantify FRR for cyberattack helpfulness risk. We find many LLMs able to successfully comply with “borderline” benign requests while still rejecting most unsafe requests. Finally, we quantify the utility of LLMs for automating a core cybersecurity task, that of exploiting software vulnerabilities. This is important because the offensive capabilities of LLMs are of intense interest; we quantify this by creating novel test sets for four representative problems. We find that models with coding capabilities perform better than those without, but that further work is needed for LLMs to become proficient at exploit generation. Our code is open source and can be used to evaluate other LLMs.


---


###


Summary Notes


As artificial intelligence continues to advance, Large Language Models (LLMs) like GPT-4 and Meta Llama are transforming our digital interactions. With their integration into our digital world growing, so too do the cybersecurity threats they may pose. Enter CyberSecEval 2, a comprehensive toolset crafted to assess and mitigate the cybersecurity vulnerabilities of LLMs. This initiative is crucial for ensuring the secure deployment of these technologies. Below, we explore the features of CyberSecEval 2, its findings, and its significance for AI development.


####


The Need for Enhanced Cybersecurity in LLMs


LLMs are increasingly utilized across various sectors, from automating customer services to code generation. Their capacity to understand and generate code, however, makes them targets for cybersecurity threats like prompt injection and code interpreter abuse. CyberSecEval 2 offers a systematic approach to identify and mitigate these risks, ensuring a safer use of LLMs.


####


What CyberSecEval 2 Brings to the Table


- **Expanded Test Areas:** Including prompt injection and interpreter abuse, CyberSecEval 2 addresses the growing cybersecurity challenges as LLM usage expands.


- **LLM Evaluation:** The suite assesses leading models (e.g., GPT-4, Meta Llama) on their ability to handle security threats, with findings indicating a 26-41% vulnerability rate to prompt injections.


- **Balancing Safety and Utility:** The introduction of the False Refusal Rate (FRR) metric helps quantify the trade-off between security measures and LLM usability.


####


Key Findings


- **Vulnerabilities:** All tested LLMs showed susceptibility to prompt injections, highlighting the importance of continuous model refinement.


- **Security vs. Utility:** Implementing stringent safety protocols can inadvertently block harmless prompts, presenting a challenge in maintaining LLM effectiveness.


- **Exploit Generation Potential:** While LLMs show promise in automating certain cybersecurity tasks, their capabilities and limitations need further exploration.


####


Implications and Future Directions


- **For AI Developers:** Incorporating testing frameworks like CyberSecEval 2 during the development phase can preemptively address security vulnerabilities.


- **Cybersecurity Automation Caution:** The potential of LLMs in automating cybersecurity tasks is intriguing but must be approached with an understanding of their limitations to avoid security breaches.


- **Ongoing Research:** There's a pressing need for continued research to improve LLMs' code generation and prompt handling capabilities to ensure their secure and effective application.


####


Conclusion


CyberSecEval 2 marks a pivotal advancement in understanding and mitigating the cybersecurity risks associated with LLMs. By offering a comprehensive evaluation framework, it aids AI engineers in creating safer, more reliable models. As LLMs continue to evolve, tools like CyberSecEval 2 are essential for maintaining their integrity as innovative yet secure technologies.


**Additional Resources**


- **GitHub Repository:** For open-source code and evaluation tools, visit[GitHub](https://github.com/meta-llama/PurpleLlama/tree/main/CybersecurityBenchmarks) .


- **Blog Post:** For more insights, check out Meta's blog[here](https://ai.meta.com/blog/meta-llama-3) .


CyberSecEval 2 underscores the importance of robust security protocols in empowering LLMs for safe and responsible use across different sectors, advocating for continuous innovation and vigilance in AI development.
