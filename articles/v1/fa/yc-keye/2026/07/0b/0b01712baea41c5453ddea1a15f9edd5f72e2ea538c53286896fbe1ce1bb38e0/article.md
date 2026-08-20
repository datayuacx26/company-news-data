---
schema_version: "1.0.0"
document_id: "0b01712baea41c5453ddea1a15f9edd5f72e2ea538c53286896fbe1ce1bb38e0"
company_key: "yc-keye"
company: "Keye"
source_id: "yc-keye-news-import-36b1eda744e0"
canonical_url: "https://www.keye.co/post/ai-enterprise-primer"
published_at: null
first_seen_at: "2026-07-25T10:42:08.002984+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:9b961541687db70e89b5b6b53780b490bbea9efd3de91c60e24bfd7e5e0ef4b0"
---

# A Primer for Buyers of Enterprise AI

With enterprises in buzzy rush to adopt generative AI, many providers, from anonymous startups to large consulting firms are racing to bring offerings to market. The industry as a whole is investing hundreds of billions of dollars to build and upgrade large language models, and pushing chip manufacturers to trillion dollar valuations. The underlying business case for this investment necessarily imagines an explosion of applications that take enterprises to new heights and unlock consumer use cases we haven’t even discovered. Downstream of this, companies are building or integrating generative AI into applications, agents or specialized models of their own with a specific customer base in mind.


These companies are using models and underlying training data in industry-specific ways to drive value for customers, requiring them to retrain, combine or apply AI models to a singular use case to achieve their aims.


By the time these offerings are brought to market and communicated to the customer, much of the magic has been lost in translation. Because marketing teams speak in terms of user benefits (as they should), buyers are sometimes left in the dark about what underlying technology they are actually purchasing. While benefits and use cases are often paramount to purchasing decisions, being able to discern what sorts of components, models and techniques went into building a certain product is equally important in order to understand whether the product can actually live up to its promised outcomes.


Sometimes a simple, off-the-shelf model is a great starting point for an enterprise team, though more often than not, a tailored offering is a better fit. Compare, for example, Intuit’s Quickbooks to an accounting plug-in for Microsoft Excel. Both could theoretically be used for corporate bookkeeping, and both will deliver gains in terms of usefulness and efficiency beyond pen-and-paper. But while one is suited to large enterprise reporting, combining cloud computing, bank APIs and an intuitive user interface, the other might be better suited to a sole proprietor looking to run basic calculations for quarterly tax payments.


In the same way that enterprises should not view Quickbooks the same way they view a spreadsheet application (nor pay the same for them), companies should not view all AI applications in the same light. This article will break down what we view as three classes of products in the LLM space, the levels of use cases they are able to unlock, and some of the key terms to understand.


The three classes of products are:


1. Model Wrappers
2. Data Systems
3. Custom Model Systems


While this article will provide just a high-level overview of each, future posts in our series will address how these levels break down in other parts of the stack further from the LLM, and examine specific products making rapid AI adoption possible.


**1. Model Wrappers**


In the early days of the current generative AI boom (late 2022-early 2023), much of the excitement was around the basic new applications that could be built simply by leveraging GPT-3 and the other large language models that closely followed it. These were often web-based apps that played with the capabilities of generative AI without modifying the output in any way other than through ***prompt engineering.***


Prompt engineering is often the first or only pseudo-technical term that those familiar with only the surface-level of AI have heard. This is because it is by far the most accessible to lay-people, and essentially just involves using more text to manipulate the output of the LLM. Basic front-end work can add easier interactivity for users, making it simple enough to build simple chat bots and automated applications.


Are model wrappers and prompt engineering actually good for anything? We would argue yes. Firms that want to help employees or customers get faster answers to basic questions can narrow and sanitize (to some extent) the set of possible AI outputs using prompt engineering.


Prompt engineering is basically knowing how to ask the right question to get a useful reply from an AI chatbot or image generator. Educational use cases, situational brainstorming or other broad-based thinking exercises can be enabled here by guiding towards specific use cases and taking the burden of “asking the right question” away from the user.


However, it soon becomes clear there is little “defensible advantage” in the space, nor much incentive for serious developers to invest. OpenAI launched its “GPT Store” and Plug-In Marketplace sooner after launch, though we have yet to hear of a single major application emerging from these ecosystems. For this reason, much of the investment in terms of both time and money, at least among enterprise consumers, has shifted up to Level 2.


**2. Data Systems**


In a model wrapper or basic chatbot, the response of the AI application is constrained by the information that is available to the LLM in its training data. In a higher class of data systems, the array of possible answers is vastly broadened. By “Data Systems”, we essentially mean any application that combines a database with a large language model to produce a different response than would normally be obtained based on the training data alone.


To get to the next level, AI needs “smart context”. RAG, or “retrieval-augmented generation”, quickly emerged as the most popular architecture for providing this context. Many application developers in the space consider context the magic that can actually make AI more useful.


In its most basic form, the architecture for a RAG-based application is what its name implies: retrieving some outside data in order to augment the response of the LLM. Technically speaking, the user input enters the LLM, triggers retrieval of “[vectors](https://www.elastic.co/what-is/vector-embedding) ” based on embeddings from training documents, and generates an informed, context-aware response by again triggering the LLM.


Source: Stackademic


RAG is a game-changer in terms of generative AI capabilities, because it opens up the informational capabilities beyond the underlying model. As models improve, we can essentially feed any data into an ever-improving brain to get better responses. This means that applications can be developed that have a real data or context-based moat, and moves businesses away from being dependent on any one model.


Many use cases in the finance industry, from equity research to due diligence are enabled by this, because it is also relatively accessible to most enterprises. Take the latter case of a private equity deal workflow. Documents enter the system via a data room, which can then be converted into numeric values and referenced dynamically to answer questions about them, evaluate the data for red flags or deal risks, and highlight areas that require human review. In our analysis, we found these sorts of systems can cut the time required for manual analysis tasks by over 50%. RAG is particularly useful in scenarios where decision-making requires access to vast stores of data, such as legal document review or market research.


RAG is not the only “data system”-based AI application, but it is one of the few new architectures that have emerged. Enterprises may also consider building applications that stitch together LLM-based products with algorithmic programs and existing databases, to more intelligently distinguish between tasks that actually require gen AI and those that might be put at risk by it.


One limitation, in any case, is that the data in a basic LLM-data system does not actually modify the “thinking” of the model, meaning outputs can sometimes feel overly forced into referencing the data, and do not incorporate it naturally into the reasoning. For even more complex tasks, a custom model architecture is generally required.


**3. Custom Model Systems**


The third and highest class of AI-based products generally available today involves underlying systems that leverage a mix of LLMs, proprietary data and underlying modifications to the core architecture of either.


Readers more familiar with the AI space may have heard terms such as:


1. Transfer learning & Fine-tuning
2. Federated learning
3. or Reinforcement learning.


These techniques change the way in which large language models operate by updating the training data or changing the way it is used, which makes the reasoning ability of the applications stronger compared to referencing the data externally via RAG. More on what these terms mean:


**Transfer Learning and Fine-Tuning**


- **Definition and Utility** : Transfer learning involves taking a model developed for one task and re-purposing it for a second, related task. Fine-tuning further adapts this model to specific nuances by training it on a smaller, task-specific dataset.
- **Application** : This methodology is widely used to customize general-purpose AI models to specific industries or functions without the need for extensive data from scratch, reducing development time and cost. An example is adapting a general sentiment analysis model to understand industry-specific jargon in financial markets.


**Federated Learning**


- **Definition and Utility** : Federated learning allows for machine learning models to be trained across multiple decentralized devices or servers holding local data samples, without exchanging them. This method is important for privacy-preserving data analysis.
- **Application** : It's particularly relevant in healthcare and banking, where data privacy is paramount. For instance, federated learning enables banks to collaborate on fraud detection models without sharing sensitive customer data.


**Reinforcement Learning (RL)**


- **Definition and Utility** : RL is an area of machine learning concerned with how software agents ought to take actions in an environment to maximize some notion of cumulative reward. This methodology is suited for applications involving decision-making under uncertainty
- **Application** : RL is used in algorithmic trading where trading bots learn to make buying/selling decisions to maximize profits based on market conditions dynamically.


**Structuring and Warehousing Data**


- **Utility** : Proper data structuring and warehousing are foundational for effectively deploying AI. Structured data storage in data warehouses or lakes facilitates efficient data retrieval and processing, which is crucial for training and deploying AI models
- **Application** : Modern data warehousing solutions like Snowflake and Google BigQuery allow organizations to store and analyze petabytes of structured and unstructured data in near real-time, supporting data-intensive applications like predictive analytics and customer behavior modeling.


Lastly, another common moniker for these more complex systems you may have heard is “AI agents”. While often depicted as near-autonomous personas that will do all of the work for you, most experts agree this might not be possible until we reach something closer to level 3 of AGI. For now, these agents are basically complex systems of models, algorithms and other programs that can execute tasks requiring multiple steps.


Agents do not definitionally require a custom model to operate, but the reality is that the use cases in which agents can add value are mostly those where an off-the-shelf model does not quite get you to a solution. These systems use an LLM compiler to get the best of various models (both off-the-shelf and retrained) to go beyond more basic products.


They also use algorithms to orchestrate real-world actions that would be too complex for a text-based LLM to do on its own. Lastly, these more complex systems may consider the costs and benefits of various LLMs to optimize for more cost-effective performance and deployment at scale, making them promising for large enterprise or government use cases.


We hope this basic primer illustrates some of the complexities of shopping for AI systems, especially with so many firms offering to build custom solutions. The cost of shipping a basic version of OpenAI’s GPT-4 versus embedding multiple models natively into a custom product can vary vastly, but everything along the spectrum will enable vastly different use cases. As we take this series forward, we will elaborate further on the costs, benefits and use cases of each.
