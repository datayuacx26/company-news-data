---
schema_version: "1.0.0"
document_id: "f615f9376a1557f65d9256fdc41ea7c3542b9d5cf02a44ef28bfd31d914c2f7c"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/artificial-intelligence/guardrails-data-protection-age-genai"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:7b5a9d83d5d744df41b6a2c99d17d27705264add2cf3ea7328476180359fa87a"
---

# Guardrails for data protection in the age of GenAI

In my[previous blog](https://www.cgi.com/en/blog/artificial-intelligence/embracing-responsible-ai-move-automation-creation) , I shared the key dimensions of a “responsible use of AI framework,” such as transparency, risk management, governance, inclusivity and humans in the loop. In a nutshell, such frameworks must be robust, ethical and trustworthy to harness the unlimited potential of AI. The principles of robustness include reliability and safety, as well as data privacy and security, which are the focus of this blog in which I further explore the safe use of generative AI (GenAI) for business practices.


Accessibility combined with the vast knowledge available in GenAI tools, such as Google Bard, ChatGPT, etc., makes these new AI tools compelling. However, using public GenAI models for business purposes poses risks of exposing proprietary information to a model’s training or data sources through user inputs. It also poses risks of using unreliable and/or unverifiable information from AI outputs, as there is no way to verify the information sources.


Security concerns around public GenAI tools largely result from limited transparency into the sources of information, including limited visibility into how the models are trained, and how content provided through interactions with models is used, stored and retrieved.


> Due to these potential risks, CGI’s responsible use of AI guidelines suggest that organizations should use the same guidance they have in place for public sharing of information as guidance for the use of public GenAI tools and organizational data and assets.


### Multi-model AI requires an overarching, responsible use framework


To mitigate the risks of data exposure, major AI and cloud technology vendors provide responsible use frameworks and the ability to use GenAI tools in secure environments without risking data leakage to the public models. However, in a[multi-model AI ecosystem](https://www.cgi.com/en/blog/artificial-intelligence/future-artificial-intelligence-multi-model-ecosystem) , where AI models are fine-tuned with proprietary data, organizations also need to supplement vendor safeguards with their own. While most GenAI tools run in the cloud, there also can be hybrid situations where some processing happens on premises.


CGI’s responsible use of AI guidelines recommend that, for business use cases involving sensitive data, organizations should restrict GenAI technologies within a secure infrastructure where the model can be applied against organizational data and intellectual property without risk of exposure, and where greater transparency into the sources of data and AI responses can be built into the interactions.


The benefit of building GenAI as a cloud service is that organizations use their existing investments in cloud technologies and refine existing access management to address AI services and output. Within these secure environments, GenAI models can be restricted by use case-driven models and configurable parameters to ensure the models only respond within the boundaries of their purpose. The models also can restrict user access to responses based on user roles and authentication requirements that make the solutions even more powerful for data-driven decision-making at all levels.


> We recommend organizations employ an ethical AI framework that extends the GenAI and cloud vendor responsible use frameworks. We advise industry leaders to leverage existing privacy and security protocols to achieve an even more secure and robust AI infrastructure, enabling greater value and return on investment (ROI) from their use of AI solutions.


### Key ethical principles for a responsible use of data within an AI framework include the following:


- **Inclusivity and relevancy** : Ensure data used for training and outputs is validated by the business and validated for inclusiveness and statistical relevancy to the AI problem statement. Continue to monitor AI outputs for relevancy.
- **Bias protection** : Ensure AI models are addressing algorithmic limitations such as bias and variance.
- **Secure data pipelines** : Ensure any data ingested by AI uses secure data pipelines with sensitive data protections implemented to manage personally identifiable information (PII), IP and other sensitive or proprietary information.
- **De-identification** : Restrict access to any PII through de-identification, masking and analysis of the risk of re-identification as part of the data ingestion processes. These transformations should be implemented prior to access by any AI tools or models.
- **Cloud security** : Ensure that data and information storage and data movement are secured within the cloud tenancy and apply access restrictions appropriate to the user level.
- **API encryption** : Use secure and encrypted application programming interface (API) calls for AI access to any information or data retained on premises and apply de-identification to on-premises data, where appropriate.
- **SME validation** : Ensure interpretation of AI responses and outputs is conducted with business and data subject matter experts to validate accuracy and relevancy to business operations.
- **Transparency** : Provide insight on the source of information in the responses as part of the AI outputs through citations and links to source information or documents.


### Applying responsible use of AI best practices by ensuring “humans in the loop”


One example of ethically developed AI in a secure environment is a new test solution to support radiologist decision-making at Helsinki University Hospital. In collaboration with CGI and Planmeca, a leading manufacturer of high-tech digital marketing devices, the hospital is developing an[AI solution that assists radiologists in interpreting brain CT scans](https://www.cgi.com/en/cgi-helsinki-university-hospital-develop-ai-solution-reviewing-brain-ct-scans-detecting-brain-hemorrhages) and detecting the most common types of non-traumatic brain hemorrhages. By detecting brain bleeds that are challenging to detect with the human eye, this AI solution is using early analysis to help save lives through early diagnosis and treatment.


This implementation leverages responsible use best practices by ensuring a “human in the loop” approach. The radiologist and AI first analyze the images independently, and then the AI outputs provide expert advice to the clinician. After the radiologist has made their diagnosis, they can compare their assessment with the AI results.


Key to this project has been the application of a responsible use of AI framework that ensures privacy and security risks are addressed for the data, the environment, and any data movement from the diagnostic imaging through to analysis. The solution also employed academic rigor and best practices to ensure the model and its outputs were accurate, that the solution was scalable, and that experts were engaged in the design through interpretation of outputs—so the solution could be operationalized in clinical workflows.


At CGI, we follow a principle-based approach to managing risk throughout the design, implementation and operationalization of AI solutions for clients. We ensure scientific rigor and apply a thorough AI ethics risk assessment at every phase of an implementation. We also define intended benefits of the AI model at the outset and monitor the operationalization to assess whether the benefits and return on investment are achieved. Finally, CGI ensures that there is a “human in the loop” with our AI solutions so that AI outputs are a supporting tool rather than a replacement of the human in the decision-making process.


[Connect with me](https://www.cgi.com/en/experts/diane-gutiw) to learn more about how our Responsible Use of AI framework allows organizations to harness GenAI while mitigating risk.
