---
schema_version: "1.0.0"
document_id: "e86dbce7e50ec3b6625e33cf8204f957600d8d5979712f164d6f2d80a59bed37"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-models-are-few-shot-generators-proposing-hybrid-prompt-algorithm-to-generate-webshell-escape-samples"
published_at: "2024-02-12T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:ff315c78a7f1cb7c4facea796590e2192f0ffb0b938c6ecaacd12350adcce265"
---

# Large Language Models are Few-shot Generators: Proposing Hybrid Prompt Algorithm To Generate Webshell Escape Samples

Do not index


Original Paper


[https://arxiv.org/abs/2402.07408](https://arxiv.org/abs/2402.07408)


Blog URL


[blog.athina.ai /large-l...-samples](https://blog.athina.ai/large-language-models-are-few-shot-generators-proposing-hybrid-prompt-algorithm-to-generate-webshell-escape-samples)


**Original Paper:**[https://arxiv.org/abs/2402.07408](https://arxiv.org/abs/2402.07408)


**By:**[Mingrui Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20M) ,[Lansheng Han](https://arxiv.org/search/cs?searchtype=author&query=Han%2C%20L) ,[Chunjie Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20C)


**Abstract:**


> The frequent occurrence of cyber-attacks has made webshell attacks and defense gradually become a research hotspot in the field of network security. However, the lack of publicly available benchmark datasets and the over-reliance on manually defined rules for webshell escape sample generation have slowed down the progress of research related to webshell escape sample generation strategies and artificial intelligence-based webshell detection algorithms. To address the drawbacks of weak webshell sample escape capabilities, the lack of webshell datasets with complex malicious features, and to promote the development of webshell detection technology, we propose the Hybrid Prompt algorithm for webshell escape sample generation with the help of large language models. As a prompt algorithm specifically developed for webshell sample generation, the Hybrid Prompt algorithm not only combines various prompt ideas including Chain of Thought, Tree of Thought, but also incorporates various components such as webshell hierarchical module and few-shot example to facilitate the LLM in learning and reasoning webshell escape strategies. Experimental results show that the Hybrid Prompt algorithm can work with multiple LLMs with excellent code reasoning ability to generate high-quality webshell samples with high Escape Rate (88.61% with GPT-4 model on VIRUSTOTAL detection engine) and Survival Rate (54.98% with GPT-4 model).


---


###


Summary Notes


####


Blog Post: Enhancing Cybersecurity with the Hybrid Prompt Algorithm and Large Language Models


In the face of evolving cyber threats, particularly webshell attacks that exploit web server flaws, the cybersecurity domain is in constant need of innovative solutions.


These webshell attacks allow attackers unauthorized control by injecting malicious scripts. As cybersecurity tools become more advanced, attackers adapt, leading to an ongoing arms race.


One cutting-edge approach to stay ahead is using Large Language Models (LLMs) and a new strategy called the Hybrid Prompt algorithm to generate advanced webshell escape tactics.


####


Understanding the Webshell Challenge


Webshell attacks leverage vulnerabilities in web applications to execute unauthorized commands and maintain access, posing a significant threat due to their evolving nature.


Traditional detection methods often struggle to keep up, lacking comprehensive coverage and suffering from overfitting.


####


The Potential of Large Language Models


LLMs have emerged as a promising tool in cybersecurity, thanks to their:


- **Complex Problem-Solving Skills:** Their advanced reasoning makes them suitable for creating sophisticated webshell escapes.


- **Adaptability:** They can adjust to new threats and detection methods by learning from examples.


####


The Hybrid Prompt Algorithm: A New Approach


The Hybrid Prompt algorithm combines several prompting strategies to improve LLMs' ability to devise effective evasion techniques. It integrates the Tree of Thoughts (ToT), few-shot examples, and the Chain of Thoughts (CoT) to enhance reasoning and generate tailored webshell escapes.


####


Key Components:


- **Data Preparation:** Cleansing and organizing webshell script data for effective LLM training.


- **Prompting Strategies:** Utilizing a mix of techniques to optimize escape sample generation.


- **Thought Process Refinement:** Structuring the LLM's reasoning to develop and improve escape strategies.


- **Selection Mechanism:** Employing methods to choose the most promising escape samples.


####


Proven Success: Advancing Evasion Techniques


Testing against popular detection engines like VIRUSTOTAL has shown that the Hybrid Prompt algorithm significantly outperforms traditional methods. It establishes new benchmarks for Escape Rate (ER) and Survival Rate (SR), marking a substantial advancement in evasion strategy development.


####


Future Directions: Broadening Applications


This research points to the vast potential of LLMs in creating adaptable cybersecurity strategies. Future efforts will aim to extend the algorithm's application across various programming languages and further refine its components for enhanced real-world effectiveness.


####


Conclusion: Pioneering Cybersecurity Innovation


The Hybrid Prompt algorithm signifies a new phase in cybersecurity, utilizing LLMs' advanced capabilities to generate effective webshell escape samples. As the digital threat landscape evolves, so must our strategies and technologies.


This work not only demonstrates the potential of LLMs in crafting advanced evasion techniques but also paves the way for further innovations in cybersecurity.


The Hybrid Prompt algorithm exemplifies how combining AI with inventive approaches can tackle some of today's most challenging cybersecurity issues.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
