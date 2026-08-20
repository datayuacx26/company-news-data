---
schema_version: "1.0.0"
document_id: "09e3e39c493ae7d389db189298ffcad04063395a213812378664e3457cf65672"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/not-what-you-ve-signed-up-for-compromising-real-world-llm-integrated-applications-with-indirect-prompt-injection"
published_at: "2023-02-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:af8be1efa37cc97ef3c7100749f48fe0c0f254279e2786df978220b55466cc7b"
---

# Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection

Do not index


Original Paper


[https://arxiv.org/abs/2302.12173](https://arxiv.org/abs/2302.12173)


Blog URL


[blog.athina.ai /not-wha...njection](https://blog.athina.ai/not-what-you-ve-signed-up-for-compromising-real-world-llm-integrated-applications-with-indirect-prompt-injection)


**Original Paper:**[https://arxiv.org/abs/2302.12173](https://arxiv.org/abs/2302.12173)


**By:**[Kai Greshake](https://arxiv.org/search/cs?searchtype=author&query=Greshake%2C%20K) ,[Sahar Abdelnabi](https://arxiv.org/search/cs?searchtype=author&query=Abdelnabi%2C%20S) ,[Shailesh Mishra](https://arxiv.org/search/cs?searchtype=author&query=Mishra%2C%20S) ,[Christoph Endres](https://arxiv.org/search/cs?searchtype=author&query=Endres%2C%20C) ,[Thorsten Holz](https://arxiv.org/search/cs?searchtype=author&query=Holz%2C%20T) ,[Mario Fritz](https://arxiv.org/search/cs?searchtype=author&query=Fritz%2C%20M)


**Abstract:**


> Large Language Models (LLMs) are increasingly being integrated into various applications. The functionalities of recent LLMs can be flexibly modulated via natural language prompts. This renders them susceptible to targeted adversarial prompting, e.g., Prompt Injection (PI) attacks enable attackers to override original instructions and employed controls. So far, it was assumed that the user is directly prompting the LLM. But, what if it is not the user prompting? We argue that LLM-Integrated Applications blur the line between data and instructions. We reveal new attack vectors, using Indirect Prompt Injection, that enable adversaries to remotely (without a direct interface) exploit LLM-integrated applications by strategically injecting prompts into data likely to be retrieved. We derive a comprehensive taxonomy from a computer security perspective to systematically investigate impacts and vulnerabilities, including data theft, worming, information ecosystem contamination, and other novel security risks. We demonstrate our attacks' practical viability against both real-world systems, such as Bing's GPT-4 powered Chat and code-completion engines, and synthetic applications built on GPT-4. We show how processing retrieved prompts can act as arbitrary code execution, manipulate the application's functionality, and control how and if other APIs are called. Despite the increasing integration and reliance on LLMs, effective mitigations of these emerging threats are currently lacking. By raising awareness of these vulnerabilities and providing key insights into their implications, we aim to promote the safe and responsible deployment of these powerful models and the development of robust defenses that protect users and systems from potential attacks.


---


###


Summary Notes


##


How to Protect AI Applications from Indirect Prompt Injection Attacks


Large Language Models (LLMs) like GPT-4 have revolutionized digital services, enabling advanced features in chatbots and coding tools.


But as these AI models get woven into more applications, they bring new cybersecurity risks, especially from indirect prompt injections. This article explores these risks and offers actionable advice for AI Engineers in big companies to secure their systems.


###


Introduction: The Risk of Indirect Prompt Injections


LLMs excel at processing natural language prompts to enhance app functionalities. However, this feature can be a double-edged sword, exposing apps to indirect prompt injections.


Unlike direct attacks that tamper with user inputs, these indirect attacks manipulate the data LLMs use, leading to unauthorized actions or leaks without directly meddling with the user interface.


###


Understanding the Threat Landscape


When LLMs fetch data through external APIs, the door opens for attackers to insert harmful data into the LLM's process.


This indirect method is harder to trace, hiding the attacker within the data supply chain. While most research has focused on direct attacks, it's crucial to understand these indirect methods to fully appreciate the risks involved.


####


Attack Surface and Threat Model


Indirect Prompt Injection (IPI) attacks are complex, where attackers target the data LLMs rely on, potentially causing:


- Unauthorized data access


- Misinformation spread


- Ongoing attacks beyond a single session


- Compromised LLM functions


Examples like Bing's chatbot highlight the real and present danger of these attacks.


####


Taxonomy of Threats


We categorize the threats from IPIs into:


- **Data Theft** : Stealing sensitive info


- **Fraud** : Deceptive schemes for gain


- **Intrusion** : Breaking into systems


- **Malware Distribution** : Spreading harmful software


- **Manipulated Content** : Twisting information


- **Service Disruption** : Blocking normal operations


These categories outline the range of damage IPI attacks can cause.


###


Practical Demonstrations and Implications


Our tests confirm that both hypothetical and real systems can fall prey to IPI attacks, stressing the need for new security measures for LLM-integrated apps. The findings indicate significant implications for the future use of LLMs in vital systems, emphasizing the necessity for a comprehensive security approach against these threats.


###


Mitigation Strategies


To combat indirect prompt injections, we recommend:


- **Rigorous Data Validation** : Strengthen data checks to catch and block harmful inputs.


- **Secure API Integrations** : Make sure APIs connected to LLMs can spot and stop suspicious data.


- **Continuous Monitoring** : Use tools to observe LLM behavior in real-time for quick attack detection and response.


- **Incident Response Plan** : Have a detailed plan ready for dealing with prompt injection attacks, ensuring quick action.


###


Conclusion


Indirect prompt injections pose a new cybersecurity challenge for AI-powered apps. Understanding and defending against these attacks is essential as we move forward into an AI-centric world. This study highlights the urgent need for better security practices and opens doors for more research into safe LLM deployments.


###


Closing Remarks


The threat of indirect prompt injections is serious and demands immediate action from the cybersecurity and AI fields.


As LLMs become integral to critical apps, pinpointing and neutralizing these risks is key to protecting our digital ecosystem. Let's use this insight as a call to arms for more research, cooperation, and innovation in creating secure AI technologies.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
