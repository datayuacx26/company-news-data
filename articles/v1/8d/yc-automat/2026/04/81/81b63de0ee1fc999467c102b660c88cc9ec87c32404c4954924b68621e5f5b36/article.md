---
schema_version: "1.0.0"
document_id: "81b63de0ee1fc999467c102b660c88cc9ec87c32404c4954924b68621e5f5b36"
company_key: "yc-automat"
company: "Automat"
source_id: "yc-automat-news-import-800367705320"
canonical_url: "https://runautomat.com/blog/how-to-fine-tune-gpt-4o-for-industry-specific-document-processing-and-robotic-process-automation"
published_at: "2026-04-10T22:50:39.623+00:00"
first_seen_at: "2026-07-24T17:51:31.960712+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:6c069762771bec7e98c943e8b12cc58a2aa51fdf111c83324741d6697ec3bb76"
---

# How to Fine-Tune GPT-4o for Industry-Specific Document Processing and Robotic Process Automation | Automat

Thanks to OpenAI's new vision fine-tuning, we are now able to demonstrate promising quantitative results that increase accuracy for both: intelligent document processing (IDP) and robotic process automation (RPA).


> Read more about the launch and OpenAI's article featuring Automat[here](https://openai.com/index/introducing-vision-to-the-fine-tuning-api/)


Automat builds enterprise software automations using AI. We use agents to autonomously create and manage complex workflows, making them 10x faster to build and maintain than traditional processes. Here’s how we to use OpenAI’s vision fine-tuning to supplement and enhance our software:


1. **Industry-specific document parsers:** Custom intelligent document processing (IDP) models trained with proprietary customer data improve accuracy and reduce development time.
2. **UI-based automation building agents:** New agent-driven workflows for Robotic Process Automation (RPA) on Web and Windows native apps accelerate the build process on legacy systems.


‍


‍


UI element localization from text descriptions allows planning agents to perform actions using screen coordinates only


### Use Cases:


‍


At Automat, we leverage vision models to build bots that streamline our customers' operations. Our bots perform actions by navigating GUI software systems, as well as understanding and extracting information from unstructured documents. Many of our enterprise customers process millions of physical documents each year, and go on to build UI-based automations.


**End-to-end vision document extraction** with fine-tuned vision model and JSON schema


For our case study, we used an anonymized dataset of unstructured documents to benefit one of our customers, a Fortune-100 insurance company. We trained a combination of traditional and transformer-based IDP models to analyze and extract information from medical documents such as prescriptions. Below, we highlight the differences and advantages of end-to-end systems, enhanced by GPT-4 vision fine-tuning.


‍


Once data is extracted from the relevant documents, RPA automations begin to reconcile and input the information into legacy internal systems and third-party websites that lack accessible APIs. Traditional RPA methods have several limitations, detailed in the examples below. We conclude that vision fine-tuning, even on a small dataset of interfaces can significantly accelerate the RPA build process.


‍


### **Intelligent Document Processing (IDP)**


‍


We used a mix of complex digital and handwritten health insurance documents (>15 schema fields with recursion). The evaluation is based on similarity between our ground truth dataset of manually labeled documents. Looking forward, we are certain that GPT vision models can surpass the performance of traditional IDP systems due to their end-to-end design. This allows the model to capture the relationships that exist within industry-specific data.


‍


‍


### Robotic Process Automation (RPA) Software Agents:


‍


Historically, RPA software development has been slow to build and challenging to maintain. The improved accuracy and reliability of models like GPT-4o, with advancements in multi-modality and structured outputs, have enabled new agentic applications. In many cases, customers need to run automations on a virtual desktop, restricted to video streams and mouse/keyboard input. This type of automation involves targeting UI-elements by their screen position, which can be tedious and unreliable. Additionally, using fixed coordinates is not robust as the position can vary due to factors like software updates, changing UI, and monitor resolution.


Example: Bots interacting with a variety of UI elements across one website


Vision models like GPT-4o struggle with tasks that require precise spatial localization. We evaluated the performance of GPT-4o (zero-shot) to estimate the center coordinates of a UI element from a text description. Then, using a dataset of website screenshots, we fine-tuned the base model improving the success hit rate by 272%.


‍


‍


### Looking ahead


We are confident that a high-quality dataset can achieve human-level performance. We are excited about the potential of text-to-coordinate agents and reasoning/planning agents like GPT-o1. This opens the door to end-to-end RPA automation, significantly improving efficiency of our company and paving the way for new opportunities with our existing and future customers.


Authors: Perdo Martinez Lopez, Aaron Bannin, Gautam Bose, Shareef El-Sayed


‍


\[button\][Get in touch](https://hijth7stfvb.typeform.com/to/oxCXyZ4g?typeform-source=www.runautomat.com) \[button\]
