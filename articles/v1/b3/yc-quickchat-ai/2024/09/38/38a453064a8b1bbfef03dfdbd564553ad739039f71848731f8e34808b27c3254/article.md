---
schema_version: "1.0.0"
document_id: "38a453064a8b1bbfef03dfdbd564553ad739039f71848731f8e34808b27c3254"
company_key: "yc-quickchat-ai"
company: "Quickchat AI"
source_id: "yc-quickchat-ai-rss-bd17510cf053"
canonical_url: "https://quickchat.ai/post/security-guide"
published_at: "2024-09-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:54.491295+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:1af4107e7c9a8f4e6caf5e762e8237267724b310b96ae923bc3a3fe6c81806b6"
---

# Our Approach to Data Protection: A Transparent Security Guide

When it comes to AI, security is paramount — and for good reason.


To be effective in conversations, your AI Agent needs access to proprietary company data and documents. At the same time, user conversations must be securely stored to meet privacy regulations while being accessible for generating actionable business insights.


The Quickchat AI Platform is built for data privacy, using trusted technologies like **Google Cloud Platform** . We keep your data and your users’ data safe, providing secure and smooth interactions with your systems.


The more high-quality data you provide, the better the AI performs. That’s why a comprehensive Knowledge Base for Quickchat’s AI Agent leads to more accurate and insightful answers.


It’s therefore in our interest to make you feel secure from the moment you upload your data to every stage of its management — from importing the Knowledge Base and integrating with your internal systems, to handling Personally Identifiable Information (PII) in conversations. Our process is designed to be 100% secure, meeting the standards of even the most demanding enterprise applications.


Let’s dive deeper into what security measures we take at each step.


## Knowledge Base upload and initial processing


### Knowledge Base upload


The first step in the entire data pipeline is uploading your data to the **Knowledge Base (KB)** . It could include structured data (such as databases), semi-structured data (e.g., JSON files), or unstructured data (e.g., PDFs, documents). No matter what type of data you’re working with, we’ve designed custom data connectors to make sure your project’s needs are met without sacrificing security.


**Security measures taken** : SSL/TLS uses both asymmetric and symmetric encryption to protect the confidentiality and integrity of data in transit. Before the data even reaches our servers, it is encrypted using **asymmetric encryption** . Asymmetric encryption establishes a secure session between a client and a server, and symmetric encryption safeguards the exchange of data within the secured session.


Your data can only be decrypted with a private key, ensuring that no unauthorized parties can access it. This ensures that sensitive information is protected during the transmission phase and prevents any potential interception.


## Secure storage using dedicated storage buckets with custom encryption keys


### Knowledge Base storage


Once the Knowledge Base data is parsed, it is securely stored in a dedicated database, which is accessible only from the internal network.


> Each connection to this database is encrypted and restricted to the Virtual Private Cloud (VPC), ensuring **your organization’s data remains segregated from other clients’ data** .


**Security measures taken:** The data remains encrypted while at rest, and access controls are put in place. Only authorized personnel or processes (as defined by **role-based permissions** ) have the ability to access the stored Knowledge Base data.


### Integration with user messages


The Knowledge Base data is always available for querying, and can be used to provide context or answer questions when a user sends a message. Your AI Agent efficiently retrieves relevant information from the KB to enhance the user’s experience.


**Security measures taken** : The encrypted Knowledge Base data is securely accessed, ensuring that no unauthorized access or data exposure occurs during retrieval for processing.


## User message collection


When a user interacts with the AI Agent, the input data is collected. The system immediately prepares the data for further processing.


**Security measures taken** : The message data is encrypted using asymmetric encryption upon submission, protecting it during transit to our servers.


## Personally Identifiable Information (PII) detection and processing


Once collected, the user message is passed through the Personally Identifiable Information (PII) scrubber. The system’s machine learning models, which utilize Google Cloud Platform services, scan the input to identify any PII that may have been included in the query. Whether it’s an email address, phone number, or any other personal detail, our models are designed to catch it before further processing.


**Security measures taken:** Any identified PII is scrubbed (replaced with placeholders like \[REDACTED\]). Only the masked and anonymized data is used in downstream processes. The user’s message content remains encrypted until it is processed by necessary components like the PII scrubber and parser.


## Integration of the Knowledge Base and user message data


### Data retrieval from the Knowledge Base


After the user message has been scrubbed and parsed, the system searches the Knowledge Base and other integrated systems for all relevant information. The system intelligently retrieves relevant information from multiple sources, including the Knowledge Base, product databases, shipping statuses, and other integrated systems like customer support logs or order histories. It then combines this information with the scrubbed message to provide the most accurate and comprehensive response.


If you populate your Knowledge Base using integrations with your or external systems, each interaction with our AI is tightly controlled.


> You set the rules, defining exactly what our systems can access and how they interact with your data.


In addition to encrypting your data, we also ensure that your systems can access it securely. Our Platform supports a variety of secure access options, including:


- **Site-to-Site VPN or VPC peering** : If you’re using Google Cloud, we offer secure, private connections to your databases, preventing exposure to the public internet.
- **Access controls** : We enforce strict user roles within our Google Cloud projects, preventing unauthorized access to sensitive information like encryption keys.


**Security measures taken** : Both the Knowledge Base data and the message content remain encrypted and protected during retrieval and integration.


## LLM processing and AI Guidelines


### Data transmission to LLM


The scrubbed and parsed message data, along with relevant Knowledge Base information, is securely sent to the Quickchat AI Engine. Rather than passing data to the LLM without oversight, the Engine follows a precise conversational design tailored to your needs, developed with Quickchat’s AI Deployment Experts. It leverages state-of-the-art Large Language Models (LLMs) to understand user messages and deliver responses that are accurate, natural, and aligned with your specific requirements.


> We never use your data to train AI models, and your users’ PII is never shared with external parties like LLM providers. Your data is yours — now and always.


**Security measures taken** : All data sent to the LLM is end-to-end encrypted, ensuring that no PII or sensitive information is exposed. Only scrubbed, non-sensitive messages are transmitted.


### AI Guidelines and filtering


Our system can filter out restricted topics, keeping conversations focused and aligned with your company’s guidelines.


> This automatic filtering ensures that no sensitive or off-topic information is ever part of your data flow.


**Security measures taken** : Quickchat AI prevents any sensitive or inappropriate content from appearing in the final response by blocking disallowed topics in prompt.


## Data output and response generation


After the LLM generates the response, the system reconstructs the output data. No sensitive information is reintroduced into the final response. We prioritize ensuring that no PII is sent to the LLM in the first place, as this data could potentially be exposed to third-party entities hosting or providing LLMs, such as OpenAI.


> Additionally, safety checks are in place to validate the correctness of the model’s output and prevent hallucinations, ensuring reliable and secure responses.


The final response, which includes both the processed message and any relevant Knowledge Base information, is then generated and prepared for delivery to the user.


**Security measures taken** : The response is transmitted securely to the user, ensuring the data remains protected throughout the entire process.


## Data storage for the end output


### Data storage (conversations and Knowledge Base)


After the response is generated and delivered to the user, both the scrubbed message (without PII) and Knowledge Base data, along with the final response, can be stored in secure storage for future use.


> Any Personally Identifiable Information (PII) is never stored in any form, ensuring complete data privacy.


**Security measures taken** : By default, Google Cloud encrypts all data at rest, but we offer the option for you to manage your **encryption keys** for an added layer of control. You can oversee the encryption process yourself, tailoring it to your security needs.


Strict role-based access control ensures that only authorized users or systems have access to this stored data.


## Summary


At Quickchat AI, we treat each enterprise client individually, adapting to specific requirements while keeping security as our top priority. Our AI Platform is built with data privacy at its core, leveraging trusted technologies like Google Cloud Platform to keep your data and user conversations secure at all stages. From the moment you upload your Knowledge Base, through integrating with internal systems and handling PII, we implement stringent encryption and access controls.


This ensures smooth and secure AI interactions, meeting even the most demanding enterprise standards without compromising data integrity or confidentiality.


If you have any specific questions about our security measures, don’t hesitate and reach out to us atcontact@quickchat.ai .
