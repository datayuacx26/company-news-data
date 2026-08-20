---
schema_version: "1.0.0"
document_id: "977b3b36aa669305fd38336f6de246b97627306070e6c70ac0e6c502cc7619fa"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/lessons-from-building-our-copilot-bot-1b5acc419baa"
published_at: "2024-09-24T11:17:26+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:4886cf3f7d06419f8ce398b714c9dab78e27a7c39785867a3484ac9d41eea426"
---

# Lessons from building ClearTax Copilot

# Lessons from building ClearTax Copilot


[Satwik Gokina](https://medium.com/@satwik.gokina?source=post_page---byline--1b5acc419baa---------------------------------------)


13 min read


·


Sep 24, 2024


--


Press enter or click to view image in full size


ClearTax Copilot


## Introduction


The consumer team at Clear recently concluded a highly successful personal tax filing season, achieving significant growth in both customer satisfaction and business metrics compared to the previous year. A major contributor to this success was the deployment of Artificial Intelligence (AI) and Machine Learning (ML) projects, which enhanced various aspects of the product. These initiatives provided valuable insights into the complexities of running Generative AI (GenAI) projects at scale. In a series of three blogs, our engineering team will share these lessons with the broader community.


One of our standout projects was the development of AI Neha, a Copilot bot designed to assist users of our Do-It-Yourself (DIY) tax filing product. Tax filing can be a stressful and time-consuming process, often leading to multiple queries and long waits for customer support responses. AI Neha was created to address these common tax-related questions within seconds, utilizing the extensive knowledge base ClearTax has built over the past decade.


Discover how we developed and refined the Copilot, leading to an impressive 3% increase in full-funnel conversion rates.


## Design principles


Here are the design principles we set for ourselves while building the Copilot system:


### Serve “simple” tax questions.


Simple is defined as questions that can be answered by a document lookup. 80% of questions from our users are simple.


### No read/write use cases


The Copilot cannot read or write to the user’s tax return. We imposed this limitation to thoroughly perfect the product before advancing it to read/ write.


### Responsible AI


We believe people like interacting with AI as long as some conditions are met:


- Let people know that they are talking to AI.
- Inform them that AI makes mistakes.
- Allow users to request and get human support seamlessly.
- Ensure user privacy.


## RAG 101


In this article, we assume you are already familiar with the basic RAG setup. Here is a short refresher on RAG and the terminology we will be using:


- In chat, the user asks a question, termed as a **Query** .
- For every user query, we retrieve the relevant documents from a **Knowledge base** . This is called the **Retrieval** step.
- To evaluate the relevancy of the documents to the query, we use **Cosine Similarity** on **Document Embeddings** .
- We prompt the LLM to answer the user query, referring to the relevant documents in **Context** .
- The model generates a response. This is called either the **Synthesis** or **Generation** step.


Press enter or click to view image in full size


Retrieval Augmented Generation (RAG)


## Knowledge Base Management


The key to any RAG-powered Copilot experience is often the knowledge base. Here were the questions we had when we started the process of building out the knowledge base.


- How does one develop a Knowledge base that is “good” at serving a RAG use case?
- Should we hire a team of subject matter experts to create the Knowledge base?
- How do we ensure the documents are continuously kept up to date?


Let’s begin by looking at the strategy we followed to create the Knowledge base.


Press enter or click to view image in full size


Creating the Knowledge base


## Taxonomy Design


Taxonomy design is the process of structuring your knowledge base into logical, constituent parts (called topics). The Knowledge base should be partitioned into distinct, non-overlapping topics. Every fact that the user may ask can be mapped to a single topic.


For ClearTax, here is the taxonomy we created for individual tax filing:


### Income sources


- Salary
- Mutual funds
- Stocks etc.


### Deductions


- 80C
- 80D
- HRA etc.


### ClearTax platform


- Account management
- Product guide
- Errors and resolutions etc.


### User persona documentation


- NRIs
- Freelancers etc.


Also, we defined a standard format in which documents about these topics will be created.


## Low Quality to High Quality documents


Collect all the internal information sources which can be


- Blogs written for customers
- Internal wikis
- Customer support training guides
- Exemplar support tickets and resolutions


Visit the OpenAI playground (or its equivalent from other AI vendors) and create a new Assistant. Upload all internal knowledge sources to this Assistant.


Iterate through the taxonomy topics and prompt this Assistant to create a high-quality (denser) document by providing the following details:


- Topic of focus.
- Standard format defined in the last step.
- Word limit and other prompt engineering techniques to make the document information-dense.


### Example prompt


Here is an example prompt we used to accomplish this


> Process all the information you have about section 80C of the income tax and create a short, succinct but comprehensive guide.
>
>
> If they are conditionals, (i.e. if conditions) order them such that they go from most likely to least likely.
>
>
> It needs to have the following parts. “Title” , “One line explanation” , “List of rules and exceptions” (Bulleted list), “FAQs”.
>
>
> Use markdown to highlight headers.
>
>
> When listing different options don’t use phrases like “Covers investments like ELSS, PPF, Life Insurance Premiums” instead list down all possible options as sub bulleted points.
>
>
> The name of the section and the content of the section should be in separate lines.
>
>
> For the FAQ part, ensure at-least 6+ questions.
>
>
> Start every question with a “Q:” and every answer with “A:”
>
>
> Ensure question and answer in the FAQ section are in separate lines.


## Expert Review


These documents are then reviewed by subject matter experts. Any errors are corrected, and any missing information is added. This is a more efficient use of their time than having them write documents from scratch.


We now have a knowledge base that is “good enough” to launch.


## Updating the Knowledge Base


Once this system is rolled out to users, we start collecting a set of queries for which the answers are incorrect or hallucinated. Let’s look at solving the incorrect responses.


If the partitioning of the knowledge base was done well, we should be able to trace every answer to a single document (unless the question was a summary request). Identify sources for every wrong or unhelpful answer. Edit/correct them to improve quality.


We also recommend identifying the top 20% of documents that are referenced for 80% of the questions (Pareto principle). These documents should be updated and reviewed more frequently than others.


## Prompt Management


One of the key levers to drive LLM system behavior is the prompt. There is a certain degree of skill required to tune the system effectively.


However, we are not fans of the term “prompt engineering” or “trained prompt engineers.” Firstly, it is not an exact science. No one can write the perfect prompt for a given use case. Secondly, best practices vary from model version to model version and from AI vendor to AI vendor (e.g., OpenAI, Anthropic, Llama).


That said, here are the elements that go into making a effective prompt:


**The standard elements**


- **Role** (e.g., *“You are a customer support agent for company X,” “You are an analyst”* )
- **Output format** (e.g., *“Prefer elaborate replies,” “Be empathetic,” “Refuse to output code”* )
- **Tool usage context:** defines when to use a tool (e.g., *“Always call getDeepLink when the user asks ‘where’ questions”* )
- **Delimiters:** use them liberally **** (e.g., *“Context is enclosed in triple back ticks”* )


**Iteration strategy**


The output of LLM systems is stochastic. For such systems, the effective way to control behavior, is to roll out the prompt in production, observe behavior, then modify the prompt to correct undesired behavior.


We used a **Special Instructions** section. A catch-all section where all other instructions go. This is where most iterations will occur.


e.g., For all tax-related questions, the bot initially preferred solutions using government resources, but there were simpler ways to accomplish them using ClearTax platform. So, we added this instruction, *“Prioritize ClearTax solutions, then government resources”.*


A few iterations of the roll-out, observe, and tune strategy will lead to a prompt that works well for your use case.


**Miscellaneous strategies**


“Do *this* ” instructions are more effective than “Do not do *this* .”


Users often frame questions imprecisely (e.g., *“80C?”, “parents medical where?”* ). Since RAG works on semantic retrieval, the more precise the question, the better the retrieval. Here are a few ways to address this:


- Display suggested questions in the conversation screen, subtly educating users on effective question framing.
- Implement a UI that nudges users away from short queries (e.g., an emoticon that gets progressively happier as words are added).
- Encourage the bot to ask clarifying questions and provide examples of how effective clarifications look.
- When you move to agents (read next section), you can set the description for the RAG tool to only ask descriptive questions. This encourages the agent to expand on user query.


## Chatbot to Agent


In a traditional RAG system, the Retrieval step is triggered on every user query. This is not desirable, especially when user refers to some part of the previous conversation. Here is a scenario that illustrates the problem:


Simple RAG fails at retrieval when conversations have self reference


The problem here is that there is no judgment on when retrieval is being triggered. The decision to query the knowledge base can be left to the LLM.


## Enter Agents


An agent is an entity that can use tools. In our case, the tool is a function that takes a user query as input and returns an answer from the knowledge base. Internally, the function is using RAG to answer, but the agent doesn’t know that.


Press enter or click to view image in full size


Agents with RAG tool can rewrite queries to prevent self reference in retrieval


Agents use the function-calling ability of LLMs. The details of this are slightly complex, but let’s not get into them right now. All we need to know is that if we create an LLM conversation and register a set of tools it can use, the agent will use the tools appropriately to accomplish tasks requested by the user.


## New Tools for Agents


Now that we’ve explained how agents are more effective than chat bots, we can deliver more value to the user by building and integrating more tools.


We will illustrate this by explaining how and why we built a navigator tool for the copilot.


## Case Study: Deep Link Navigation Tool


Press enter or click to view image in full size


## Discovery


After rolling out the copilot, we did daily manual reviews of all chat transcripts. One common type of question identified was search-adjacent. Users were asking where to add details of specific income sources or deductions (e.g., “Where to add parents’ medical insurance?”, “I have an education loan, how to show it”).


In retrospect, this was obvious, given there are over 100 input fields in the ClearTax product, corresponding to the complexity of direct tax. Users understandably needed help locating things.


## Solution 1


The UI of the product was described in text and provided to the copilot via a system prompt. However, this approach had severe limitations:


- The process of describing the UI was time-consuming.
- The prompt needed to be updated with every UI change.
- Users had to decipher the description and follow the instructions to reach the field of interest.


## Solution 2


Provide a deep navigation (deep nav) link that users can click on and directly go to the field. A document was created from the deep nav links and added to the RAG knowledge base. This perfectly solved the user experience if the correct link was provided.


However, the copilot struggled to provide the correct link. The culprit was the retrieval step. The retrieval process uses text similarity to identify relevant documents. However, this is not nuanced enough to distinguish between and identify the correct link between similar looking URLs.


## Solution 3


The deep nav links were directly added to the system prompt. We also reduced the number of deep links to the top 30 that accounted for 80% of user queries. This ensured the copilot always knew the correct deep link for the query.


Similar-looking URLs still confused the copilot. These URLs didn’t have any semantic meaning that it could leverage. This resulted in many inaccurate links shown to the user.


## Final Solution


Tools were the right tool (pun intended) for the job. The key insight was that the copilot didn’t need to remember the links. All it needed to do was identify if the user query was about navigation and, if yes, which of the 30 fields was being asked for.


Then the copilot could simply ask a tool called getDeepLink, which responds with a deep link based on the input. The input to the tool was constrained to be an enum (a string with a limited set of values, 30 in this case). This solution worked very well for us. A total of 100K deep links were generated across 157K conversations.


## Learning


The key insight here is, “Only make the AI do the things it is really good at.” AI is excellent at interpreting what the user wants and choosing the right tool/action to accomplish it. The action itself that the user wants to take, by putting it behind a tool, makes it predictable and ensures zero chance of it going wrong.


In the example above, all AI had to infer was which deep link the user was looking for. Looking up the correct deep link and providing it is trivial for software; there was no reason to expect AI to remember and provide this deep link.


Here are a few other tools we used along with a description and insights on why.


## All Tools used


### Get Deadlines


Addresses user questions like


- *“last date for filing for previous financial year”*
- *“when does filing open for this year”*
- *“Can i file for the last 3 financial years”*


Similar to deep link tool, We first tried forcing the AI to remember all dates for last 5 financial years. This didn’t work. Instead, we designed a tool in which, all AI needed to do was to call it with the right financial year. The tool responded with all the key deadlines relevant to the year.


### Plan Explainer


Address questions like


- *“what are all your pricing plans”*
- *“what is the difference between plan x and plan y”*


Plans are constantly changing. Instead of baking the description into the prompt, the copilot can query get plans and also query with two different plans to get comparison


### Ticket Creator


This creates a ticket on the customer support portal.


This was absolutely vital to address failure modes for the AI. Whenever the user expresses frustration or the conversation goes to long, we switched the chat to an customer support agent.


The objective of this system is not to replace humans, but to provide quick answers for low effort questions so that experts can focus on complex queries.


## Performance Evaluation


Now that we have fully built and deployed an LLM system, the next step is to understand how we are doing. Here is the full suite of strategies we used to evaluate the LLM system.


Press enter or click to view image in full size


Performance Evaluation strategies


### Manual Inspection


A super boring strategy, but also super important. Nothing replaces reading actual user conversations and understanding where the copilot is going wrong. No matter how big the system gets, we always recommend reading a random sample of 100 conversations.


Key insights on manual inspection were published daily to all stakeholders.


### LLM Review


We created a framework defining the parameters of a good conversation:


- *“Was the copilot helpful to the user?”*
- *“Were the answers accurate?”*
- *“Was the copilot empathetic to the users?”*
- *“Was there abuse or any other unsafe interactions?”*


We then converted this framework into a prompt to ask the LLM to score the conversation on these parameters. We typically use a cheaper LLM and/or batch processing to minimize costs.


Every day, all the chats are run through the LLM review, and metrics are published. All the conversations flagged as unsafe or frustrating are reviewed.


### Thumbs-up Thumbs-down


Giving users the ability to provide feedback on each message helps us identify the most the interactions causing the most delight or frustration. Additionally, daily 👍👎 percentages offer a quick temperature check of the system. All down voted messages were manually reviewed.


### A/B Testing


To test all key changes, starting with “copilot vs. no copilot”, “3.5 vs. 4”, and “deep nav tool vs. no tool”, we ran A/B tests. Key metrics like conversion rate and thumbs-up percentage were evaluated through statistical tests.


A/B tests helped us decided that GPT 3.5 was good enough for our use case. We switched it to GPT 4O mini after it was launched (after a quick A/B test).


### Evals


Testing and evaluation are some of the most important parts of building with LLMs. These are even more critical for these systems than for classical software systems. Systems with stochastic behavior need new strategies for testing and evaluation. We discovered/relearned a lot of insights here. To keep this post concise, we’ll discuss this topic in the third blog post of this series.


## Impact


Copilot was one of the mission-critical projects for ClearTax this year.


The Copilot had over 150K unique conversations with our users, with over 1M messages exchanged. Additionally, around 100K deep links were generated, helping users quickly navigate to the field of interest.


The enhanced customer experience resulted in a 3% increase in conversion from landing page to payment in the A/B test.


## Future Work


For the future, we are looking to drastically increase the capabilities of our Copilot. Currently, our Copilot only helps with navigation.


### Conversational Form Fill


We wish to boost user convenience even further by allowing them to provide details via chat. The Copilot will handle filling in these details, which will be reflected in the UI. This way, there will be seamless integration between the product UI and the Copilot.


### Assurance


Users are often curious and anxious about various computations made by the ClearTax platform, most notably the final tax calculation. The product does a great job of explaining how this is done, but we believe that allowing users to understand the computation in a conversational manner will help them get more assurance and reduce anxiety.


### Drop’n’Forget


There are multiple fields that also accept document uploads as input, such as Form 16, Capital Gains documents, etc. If we allow users to upload all relevant documents and let the Copilot identify, extract, and fill in all relevant fields, we take one step closer to be able to provide a Charted Account (CA) like experience in a DIY product.


All user has to do is click on attachments button in the copilot chat and drop all the documents that they believe are relevant. To be able to automatically extract key information from the documents, we plan on leveraging Clear Document AI, a separate in-house AI solution built at Clear that can read and extra information from document.


The system was designed with AI safety in mind. Concerns about data privacy, sovereignty, and safety were all considered and adhered to while building this project.


### **Built by**


Engineering: Sreekanth Reddy, Lovepreet Singh, Shivaprasad K S, Nitin Jain


Product: Arjun Venugopal


Data Science: Satwik Gokina
