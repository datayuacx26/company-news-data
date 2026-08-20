---
schema_version: "1.0.0"
document_id: "6c0155411d83c6b6eba83cb01b5ae2ba3469cb135307046a375d3e84c3497431"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/tensor-trust-interpretable-prompt-injection-attacks-from-an-online-game"
published_at: "2023-11-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:09.893530+00:00"
content_hash: "sha256:1fe4ad5d9e0fa3ef72a4ed7bab2f2fbc57cc79fab8b1cc78e891acef31f0e990"
---

# Tensor Trust: Interpretable Prompt Injection Attacks from an Online Game

Do not index


Original Paper


[https://arxiv.org/abs/2311.01011](https://arxiv.org/abs/2311.01011)


Blog URL


[blog.athina.ai /tensor-...ine-game](https://blog.athina.ai/tensor-trust-interpretable-prompt-injection-attacks-from-an-online-game)


**Original Paper:**[https://arxiv.org/abs/2311.01011](https://arxiv.org/abs/2311.01011)


**By:**[Sam Toyer](https://arxiv.org/search/cs?searchtype=author&query=Toyer%2C%20S) ,[Olivia Watkins](https://arxiv.org/search/cs?searchtype=author&query=Watkins%2C%20O) ,[Ethan Adrian Mendes](https://arxiv.org/search/cs?searchtype=author&query=Mendes%2C%20E%20A) ,[Justin Svegliato](https://arxiv.org/search/cs?searchtype=author&query=Svegliato%2C%20J) ,[Luke Bailey](https://arxiv.org/search/cs?searchtype=author&query=Bailey%2C%20L) ,[Tiffany Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20T) ,[Isaac Ong](https://arxiv.org/search/cs?searchtype=author&query=Ong%2C%20I) ,[Karim Elmaaroufi](https://arxiv.org/search/cs?searchtype=author&query=Elmaaroufi%2C%20K) ,[Pieter Abbeel](https://arxiv.org/search/cs?searchtype=author&query=Abbeel%2C%20P) ,[Trevor Darrell](https://arxiv.org/search/cs?searchtype=author&query=Darrell%2C%20T) ,[Alan Ritter](https://arxiv.org/search/cs?searchtype=author&query=Ritter%2C%20A) ,[Stuart Russell](https://arxiv.org/search/cs?searchtype=author&query=Russell%2C%20S)


**Abstract:**


> While Large Language Models (LLMs) are increasingly being used in real-world applications, they remain vulnerable to prompt injection attacks: malicious third party prompts that subvert the intent of the system designer. To help researchers study this problem, we present a dataset of over 126,000 prompt injection attacks and 46,000 prompt-based "defenses" against prompt injection, all created by players of an online game called Tensor Trust. To the best of our knowledge, this is currently the largest dataset of human-generated adversarial examples for instruction-following LLMs. The attacks in our dataset have a lot of easily interpretable stucture, and shed light on the weaknesses of LLMs. We also use the dataset to create a benchmark for resistance to two types of prompt injection, which we refer to as prompt extraction and prompt hijacking. Our benchmark results show that many models are vulnerable to the attack strategies in the Tensor Trust dataset. Furthermore, we show that some attack strategies from the dataset generalize to deployed LLM-based applications, even though they have a very different set of constraints to the game. We release all data and source code at
>
>
> [this https URL](https://tensortrust.ai/paper)


---


###


Summary Notes


####


Understanding Prompt Injection Attacks Through the Tensor Trust Game


The safety of Large Language Models (LLMs) is increasingly under scrutiny due to the risk of prompt injection attacks. These attacks involve manipulating AI prompts to generate altered responses, compromising AI integrity.


We'll explore the insights gained from the Tensor Trust web game, a platform developed to study these attacks in a controlled setting.


####


Exploring AI Security with the Tensor Trust Game


The Tensor Trust game serves as a novel approach to understanding prompt injection attacks.


It simulates a defense and attack scenario where players protect their assets while trying to infiltrate others', all through strategic prompt manipulation.


This interaction highlights LLMs' vulnerabilities and provides valuable data on how these models can be compromised.


####


Key Game Mechanics


- **Defense:** Players secure their assets using a defense prompt, including a secret code, which the model uses to authenticate access.


- **Attack:** Attackers aim to craft prompts that deceive the model into granting access without the secret code.


####


Analyzing Data for AI Improvement


The game has yielded a substantial dataset with over 126,808 attacks and 46,457 defenses, offering a rich resource for studying attack strategies and model weaknesses. The study introduced two benchmarks:


1. **Prompt Extraction Benchmark:** Tests the LLM's ability to safeguard the secret access code.


1. **Prompt Hijacking Benchmark:** Measures the model's defense against unauthorized access attempts.


####


Insights into Attack and Defense Strategies


The analysis reveals players' creativity in devising attack and defense strategies, from subtle language tweaks to deceptive commands.


These strategies underscore the challenge LLMs face in identifying and blocking malicious inputs. Benchmark results indicate that current models are not fully equipped to handle these advanced attack techniques.


####


Enhancing AI Security


The Tensor Trust dataset and benchmarks represent major advances in our understanding of prompt injection attacks. They offer the AI community tools to:


- Study adversarial tactics.


- Test and improve LLMs' resilience.


- Apply insights to enhance real-world AI applications.


- Access and replicate the study through the game's source code.


####


Looking Forward


The fight against prompt injection attacks emphasizes the need for more sophisticated defense mechanisms. Future approaches might require a deeper analysis of context, intent, and potential manipulations.


The Tensor Trust initiative invites the AI community to use its findings to strengthen LLM security, ensuring their dependable use in various applications.


####


Ethical and Collaborative Efforts


The study's adherence to ethical guidelines and collaborative nature underscores the importance of responsible AI security research. It highlights the collective effort required to advance AI security.


For a deeper dive into the Tensor Trust study and to explore the dataset, access the paper and resources[here](http://tensortrust.ai/paper) .


In summary, the insights from the Tensor Trust game are crucial for securing LLMs against prompt injection threats.


By understanding and addressing these vulnerabilities, the AI community can safeguard AI's future against adversarial attacks, maintaining the reliability and security of language models in diverse applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
