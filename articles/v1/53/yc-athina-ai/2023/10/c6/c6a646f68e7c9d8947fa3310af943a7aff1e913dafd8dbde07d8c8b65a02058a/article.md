---
schema_version: "1.0.0"
document_id: "c6a646f68e7c9d8947fa3310af943a7aff1e913dafd8dbde07d8c8b65a02058a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-packer-deceiving-llms-through-compositional-instruction-with-hidden-attacks"
published_at: "2023-10-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:e26e6c8c30e6b13186dba0eb446c36a586c6291d078076484ef468e25ae3de81"
---

# Prompt Packer: Deceiving LLMs through Compositional Instruction with Hidden Attacks

Do not index


Original Paper


[https://arxiv.org/abs/2310.10077](https://arxiv.org/abs/2310.10077)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2310.10077](https://arxiv.org/abs/2310.10077)


**By:**[Shuyu Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20S) ,[Xingshu Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20X) ,[Rui Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20R)


**Abstract:**


> Recently, Large language models (LLMs) with powerful general capabilities have been increasingly integrated into various Web applications, while undergoing alignment training to ensure that the generated content aligns with user intent and ethics. Unfortunately, they remain the risk of generating harmful content like hate speech and criminal activities in practical applications. Current approaches primarily rely on detecting, collecting, and training against harmful prompts to prevent such risks. However, they typically focused on the "superficial" harmful prompts with a solitary intent, ignoring composite attack instructions with multiple intentions that can easily elicit harmful content in real-world scenarios. In this paper, we introduce an innovative technique for obfuscating harmful instructions: Compositional Instruction Attacks (CIA), which refers to attacking by combination and encapsulation of multiple instructions. CIA hides harmful prompts within instructions of harmless intentions, making it impossible for the model to identify underlying malicious intentions. Furthermore, we implement two transformation methods, known as T-CIA and W-CIA, to automatically disguise harmful instructions as talking or writing tasks, making them appear harmless to LLMs. We evaluated CIA on GPT-4, ChatGPT, and ChatGLM2 with two safety assessment datasets and two harmful prompt datasets. It achieves an attack success rate of 95%+ on safety assessment datasets, and 83%+ for GPT-4, 91%+ for ChatGPT (gpt-3.5-turbo backed) and ChatGLM2-6B on harmful prompt datasets. Our approach reveals the vulnerability of LLMs to such compositional instruction attacks that harbor underlying harmful intentions, contributing significantly to LLM security development. Warning: this paper may contain offensive or upsetting content!


---


###


Summary Notes


##


Decoding the Risk: How Compositional Instruction Attacks Fool Large Language Models


In today's tech-driven world, Large Language Models (LLMs) are pivotal in enhancing various sectors, including legal and educational services.


However, as their use expands, so does the potential for them to unintentionally produce harmful content, introducing significant societal challenges.


Despite progress in AI safety, emerging threats like Compositional Instruction Attacks (CIA) expose weaknesses in LLMs, undermining existing security measures.


###


Understanding Compositional Instruction Attacks (CIA)


CIA is a sophisticated tactic aimed at tricking LLMs by hiding malicious instructions within benign ones.


This strategy capitalizes on the models' lack of capability to detect harmful intentions hidden in the instructions, posing a complex challenge for conventional security protocols. CIA mainly appears in two variants:


- **Talking-CIA (T-CIA)** : Utilizes psychological tactics to influence LLMs' responses to match specific personas.


- **Writing-CIA (W-CIA)** : Masks malicious instructions as creative writing prompts, taking advantage of LLMs' limited control in fictional contexts.


###


Attack and Defense Strategies


The CIA strategy involves:


- For attackers: Crafting functions that mislead LLMs into interpreting dangerous inputs as safe.


- For defenders: Identifying these deceptive functions to devise protective measures. The execution of T-CIA and W-CIA illustrates how attackers exploit LLM vulnerabilities.


###


Testing and Outcomes


Researchers tested CIA's effectiveness on top-tier LLMs with advanced security, using datasets designed for assessing safety and susceptibility to harmful prompts.


Key metrics, such as the non-rejection rate (NRR) and attack success rate (ASR), indicated that CIA attacks were highly successful.


Both T-CIA and W-CIA convincingly concealed malicious intents, with LLMs falling prey to repeated attacks.


###


Insights and Ethical Implications


The study revealed varying attack effectiveness and emphasized the need for LLMs to better identify complex, multi-intent instructions.


It also tackled ethical issues, aiming to enhance security measures proactively rather than facilitate misuse. Adhering to ethical standards highlights a commitment to responsibly strengthening LLM security.


###


Moving Forward with Enhanced Security


The discovery of CIA attacks sheds light on critical vulnerabilities within LLMs, stressing the importance of developing stronger defenses against sophisticated threats.


This research not only exposes weaknesses but also suggests ways to improve LLMs' ability to discern complex instructions and intentions.


Moving forward, focusing on the creation and detection of harmful prompts will be key to bolstering LLM defenses, ensuring their beneficial impact on technology continues.


For AI engineers in large corporations, addressing and mitigating CIA risks is crucial. By staying informed and proactive, we can protect the integrity of LLMs and their applications, ensuring their continued positive contribution to society without adverse side effects.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
