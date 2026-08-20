---
schema_version: "1.0.0"
document_id: "9bbfbb9c21e656306d60ecdb3e0e5a89aac92e44e2521ab893634ca92356c080"
company_key: "s-p-global-inc-common-stock"
company: "S&P Global Inc."
source_id: "s-p-global-inc-common-stock-rss-ff630ac34bbe"
canonical_url: "https://engineering.global.com/unlocking-the-power-of-ai-with-effective-prompt-engineering-194f6d380601"
published_at: "2024-05-22T14:45:25+00:00"
first_seen_at: "2026-07-20T04:36:47.908335+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:e31d4b671ffd90c3afa26ecb1cacefdc09e8a0980a78cae40623d2229543fb1a"
---

# Unlocking the Power of AI with Effective Prompt Engineering

Press enter or click to view image in full size


Prompt Engineering


# **Unlocking the Power of AI with Effective Prompt Engineering**


[Global Engineering](https://global-engineering.medium.com/?source=post_page---byline--194f6d380601---------------------------------------)


8 min read


·


May 22, 2024


--


*By Marlon Gaspar*


### **Understanding Prompts in Generative AI**


A prompt acts as the primary interface between the user and generative AI models. This interaction is predominantly textual; you instruct the model via text, and the model executes the task based on your directions. Essentially, the prompt is your way of communicating what you want the AI to achieve.


For large language models (LLMs) such as GPT-4, prompts can range widely. They might be as straightforward as a query like “What is 1 + 1?” or as complex as a problem statement embedded with various data types, including raw data from a CSV file.


### **Understanding Prompt Engineering**


Now that we’ve explored what a prompt is, let’s delve into the field of prompt engineering.


I like this succinct definition from McKinsey & Company “Prompt engineering is the practice of designing inputs for generative AI tools that will produce optimal outputs.” As an analogy think of prompt engineering as a master chef crafting a gourmet recipe. Just as the chef carefully selects and combines ingredients to create a perfect dish, a prompt engineer carefully chooses words and structures them to elicit the desired output from an AI.


Continuing the chef analogy, prompt engineering blends domain knowledge with an understanding of different AI models. Essentially, the same way a chef must have an idea of what they want to cook before actually cooking and that the same ingredients yield different results under various methods, a prompt engineer must have a vision of their goals before prompting and recognise that different AI models respond uniquely to the same prompts.


### **The Business Case for Prompt Engineering**


While the benefits of prompt engineering in enhancing AI interactions with Large Language Models (LLMs) such as ChatGPT and Claude are clear to those familiar with the field, they might not be as obvious to everyone. To clarify, here are four compelling reasons why businesses should consider incorporating prompt engineering:


1. **Improved Accuracy and Relevance:** Prompt engineering fine-tunes the interaction between users and AI, leading to more accurate and relevant responses.
2. **Increased Efficiency:** By optimising prompts, businesses can reduce the time and resources spent on revising outputs. Efficient prompts lead to better first-time responses, speeding up workflows and reducing the need for follow-up queries.
3. **Innovative Use Cases and Applications:** Well-crafted prompts can unlock new capabilities and applications for AI, from personalised marketing campaigns to advanced data analysis.
4. **Competitive Advantage:** Businesses that master prompt crafting can leverage AI more effectively than their competitors, leading to enhanced operational capabilities.


### **Principles of Prompt Engineering**


With a solid understanding of what prompt engineering is and its benefits, we now turn our attention to the core principles that should guide every prompt creation process for Large Language Models (LLMs). These principles are essential for crafting effective and efficient prompts that maximise the AI’s capabilities.


Press enter or click to view image in full size


With the key principles of prompt engineering in mind, consider the example below. Analyse the prompt, identify which principles it violates, and think about how you might modify it to improve both the prompt and the resulting output. This exercise is designed to test your ability to integrate these principles into your approach to prompting.


*Exercise Example:*


Press enter or click to view image in full size


Press enter or click to view image in full size


*Task:*


1. Identify Violations: Examine the prompt and list which of the six principles it breaches.
2. Propose Improvements: Suggest how to rephrase the prompt to make it more effective based on the principles outlined earlier.


*Bonus Points:* You identify issues beyond the principles I gave you.


Discussion: Reflect on how your proposed changes could alter the AI’s response, potentially making it more precise and relevant to the user’s needs.


### **Foundational Prompt Engineering Techniques**


Now stepping into techniques, below is an overview of more common techniques which you can deploy in your prompts while also considering the principles I gave you earlier.


**Few shot prompting:**


Image source:[https://newsletter.theaiedge.io/p/prompt-engineering-and-llmops-building](https://newsletter.theaiedge.io/p/prompt-engineering-and-llmops-building)


Description: **** Few-shot prompting is a technique where demonstrations or examples are included in the prompt.


*Example:*


Image source:[https://the-prompt-engineer.beehiiv.com/p/3-fewshot-prompting](https://the-prompt-engineer.beehiiv.com/p/3-fewshot-prompting)


**Chain of thought prompting:**


Description: Chain of thought (CoT) is a technique that structures an input prompt in a way that mimics human reasoning.


*Example:*


Press enter or click to view image in full size


Image source:[https://www.promptingguide.ai/](https://www.promptingguide.ai/)


**Positive Prompting**


A common and effective strategy in prompt engineering is to emphasize what the model should do rather than what it should avoid. This approach encourages specificity and directs attention toward the details that are crucial for eliciting good responses from the model.


*Example:*


> Instead of saying, “Don’t give me too much technical jargon,” rephrase the prompt to say, “Use simple language and avoid technical jargon.” This tells the model exactly how to tailor the response to meet your needs.


**Leading Words**


Instructing a model to “think step by step” before generating a response can lead to more accurate and reliable results. This technique, known as using “leading words,” strategically guides the model toward a more structured problem-solving approach.


**Ensuring clarity in your prompts**


Just as quotation marks, bullet points, and line breaks make text easier for us to parse, these formatting elements also enhance the readability for LLMs.


### **Advanced Prompt Engineering Techniques**


Below are some examples of more advanced techniques which you could consider using where the foundational techniques mentioned previously are not quite up to par or suitable.


**Self-Consistency:**


Press enter or click to view image in full size


Description: Diverse reasoning paths are sampled in few-shot CoT, selecting the most consistent answer to enhance performance in tasks like arithmetic and common sense reasoning.


## Get Global Engineering’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


*Example:*


Press enter or click to view image in full size


Image source:[https://learnprompting.org/docs/intermediate/self_consistency](https://learnprompting.org/docs/intermediate/self_consistency)


**Prompt chaining:**


Description: Prompt chaining uses sequential prompts to guide language models through complex, multi-step tasks.


*Example:*


Press enter or click to view image in full size


Image source:[https://cohere.com/blog/chaining-prompts](https://cohere.com/blog/chaining-prompts)


**Tree of thought:**


Press enter or click to view image in full size


Image source:[https://www.promptingguide.ai/techniques/tot](https://www.promptingguide.ai/techniques/tot)


Description: Tree of thought prompting” is a method that structures thinking like a tree. It begins with a central idea, the main trunk, and expands into related subtopics, the branches.


*Example:*


Press enter or click to view image in full size


Image source:[https://blog.searce.com/tree-of-thought-prompting-unleashing-the-potential-of-ai-brainstorming-9a77a7d640b7](https://blog.searce.com/tree-of-thought-prompting-unleashing-the-potential-of-ai-brainstorming-9a77a7d640b7)


**Tailoring Solutions for Specific Needs**


Prompt engineering also encompasses more specialised methods that can be particularly beneficial for Businesses. In this section, we explore two such methods: Retrieval-Augmented Generation (RAG) and the use of agents.


**Retrieval-Augmented Generation (RAG):**


Retrieval-augmented generation combines the capabilities of a standard language model with the power of information retrieval systems. This technique enhances the model’s responses by allowing it to access and incorporate external data dynamically.


Press enter or click to view image in full size


Image source:[https://deci.ai/blog/fine-tuning-peft-prompt-engineering-and-rag-which-one-is-right-for-you/](https://deci.ai/blog/fine-tuning-peft-prompt-engineering-and-rag-which-one-is-right-for-you/)


**Potential application of RAG within Business operations**


- **Internal chatbot** : Enhances accuracy and timeliness of responses in internal chatbots, helping employees access up-to-date company policies.


**The Use of Agents:**


Agents in prompt engineering refer to specialised prompts that instruct the AI to act as an intermediary or facilitator in achieving complex tasks. These agents can manage multi-step operations or handle tasks that require a series of interactions.


Image source:[https://www.promptingguide.ai/research/llm-agents](https://www.promptingguide.ai/research/llm-agents)


**Potential applications of Agents within business operations:**


- **Post-campaign analysis:** Automates the creation of detailed PowerPoint slides for marketing campaign analysis.


### **Call to action**


Now that we’ve explored prompt engineering, it’s clear that there are significant opportunities for both individual employees and businesses as a whole to harness these capabilities. Here are some practical steps we can take to integrate and benefit from prompt engineering in businesses:


**Actions for Individual employees:**


**1. Enhance Your Skill Set**


- Engage in workshops and training sessions provided by your employer
- Consider completing a free course on prompt engineering:


[https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)


[https://www.youtube.com/watch?v=kCc8FmEb1nY](https://www.youtube.com/watch?v=kCc8FmEb1nY)


[https://www.youtube.com/watch?v=dOxUroR57xs](https://www.youtube.com/watch?v=dOxUroR57xs)


- Use daily tasks as opportunities to practice and refine your prompt crafting skills.
- Refer to the resource section included and have a look through some of the links for an even greater depth in Prompt engineering.
- Remain Informed on the latest AI initiatives within your company and at large.


**2. Innovate and Share**


- Test out different prompts in your work area and observe the outcomes. Document these experiments and share your insights with colleagues.
- If you are currently using AI tools in your work area and find them useful share them with others either directly in conversation or via posts on relevant channels.


**Actions for Businesses:**


**1. Invest in Resources**


- Acquire or subscribe to AI platforms and tools that allow non-experts to experiment with and deploy AI models.
- Build a comprehensive repository of best practices, successful prompts, and troubleshooting tips accessible to all employees.
- Offer training to understand AI technologies and prompt engineering basics. Online courses, workshops, webinars or even blogs like this one can be valuable.


**2. Foster a Culture of Continuous Improvement**


- Recognise and reward individuals and teams who contribute significantly to advancing our prompt engineering capabilities.


### **Resources for Further Reading**


To deepen your understanding of prompt engineering, I’ve compiled a list of resources that have been instrumental in shaping the insights shared in this blog. These resources are categorised to help you navigate according to your interests, needs and level of current understanding:


1) Well-rounded source for overviews and detailed information:[https://www.promptingguide.ai/readings](https://www.promptingguide.ai/readings)


2) More focused on their own models but still a good general source:[https://community.openai.com/c/prompting/8/l/top](https://community.openai.com/c/prompting/8/l/top)


2a)[https://platform.openai.com/docs/guides/prompt-engineering/strategy-write-clear-instructions](https://platform.openai.com/docs/guides/prompt-engineering/strategy-write-clear-instructions)


3) Very advanced content:[https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/)


4) Additional resource to consider:[https://github.com/brexhq/prompt-engineering](https://github.com/brexhq/prompt-engineering)


5) Comprehensive list of 26 principles for prompt crafting:[https://arxiv.org/pdf/2312.16171v1.pdf](https://arxiv.org/pdf/2312.16171v1.pdf)


6) Encompasses beginner to advanced content:[https://promptengineering.org/](https://promptengineering.org/)
