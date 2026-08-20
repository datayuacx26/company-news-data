---
schema_version: "1.0.0"
document_id: "ee1e5d3760853dd9b5d4f3e634e39d5aa8e70bd042006ce1d41a4a86dad2cbee"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-a-robot-to-walk-with-large-language-models"
published_at: "2023-11-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:3c4ccc83f3540551114b52eaf84fd38f870d8f40d126c8b86a89fd495dbd2265"
---

# Prompt a Robot to Walk with Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2309.09969](https://arxiv.org/abs/2309.09969)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2309.09969](https://arxiv.org/abs/2309.09969)


**By:**[Yen-Jen Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Y) ,[Bike Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20B) ,[Jianyu Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20J) ,[Koushil Sreenath](https://arxiv.org/search/cs?searchtype=author&query=Koushil)


**Abstract:**


> Large language models (LLMs) pre-trained on vast internet-scale data have showcased remarkable capabilities across diverse domains. Recently, there has been escalating interest in deploying LLMs for robotics, aiming to harness the power of foundation models in real-world settings. However, this approach faces significant challenges, particularly in grounding these models in the physical world and in generating dynamic robot motions. To address these issues, we introduce a novel paradigm in which we use few-shot prompts collected from the physical environment, enabling the LLM to autoregressively generate low-level control commands for robots without task-specific fine-tuning. Experiments across various robots and environments validate that our method can effectively prompt a robot to walk. We thus illustrate how LLMs can proficiently function as low-level feedback controllers for dynamic motion control even in high-dimensional robotic systems. The project website and source code can be found at:
>
>
> [this https URL](https://prompt2walk.github.io/)


---


###


Summary Notes


####


Using AI to Control Robots: Exploring New Frontiers


The world of artificial intelligence (AI) is constantly evolving, and large language models (LLMs) like GPT-4 are at the forefront, pushing the boundaries from understanding text to solving complex problems.


One of the most intriguing applications is using LLMs to control robots, a challenge that combines AI's predictive power with the need for physical actions in the real world.


####


The Challenge: AI Meets the Physical World


Robots in real-world settings face unpredictable environments, making it tough to apply AI's text-based prowess to physical tasks.


The question arises: can the vast knowledge and generative abilities of LLMs be translated into actions for robots, especially for intricate tasks like walking?


####


Innovative Robotic Control with LLMs


A pioneering method by researchers Yen-Jen Wang, Bike Zhang, Jianyu Chen, and Koushil Sreenath suggests using LLMs to create control commands for robots.


This approach skips the need for detailed task-specific adjustments, instead relying on a few examples to guide the model in crafting actions from text prompts.


####


How It Works:


- **Starting with LLM Policy** : The model learns about robot control by studying pairs of observations and actions from an existing controller.


- **Crafting Prompts** : The key is in creating detailed prompts that describe the task, expected actions, and technical specifics about the robot.


- **Using a PD Controller** : The robot's movements are guided by a PD controller, interpreting the LLM's output into precise actions.


####


Testing the Approach


The team's experiments spanned different robots and simulation environments, like MuJoCo and Isaac Gym.


Their findings highlight the role of well-designed prompts and the LLM's ability to control robots' movements adaptively, without extra fine-tuning.


####


Key Insights:


- **The Art of Prompt Design** : Effective prompts are essential for generating actionable commands.


- **Observation Matters** : The way a robot's state is presented to the LLM can greatly influence performance, underlining the importance of selecting suitable metrics and normalization techniques.


####


Discussion: The Role of Textual Prompts


Moving from numerical controls to a text-based system marks a significant shift. Textual prompts offer a more intuitive way to communicate with AI, but crafting these prompts and dealing with LLM inference speeds remain challenges, especially for real-time tasks.


####


Conclusion: A New Era for AI in Robotics


Using LLMs to direct robot actions opens up a world of possibilities for more adaptable and intelligent robotic systems.


This method not only shows the potential of text-based commands in robotics but also encourages further research into combining advanced language models with physical tasks.


As this new field unfolds, the integration of LLMs and robotics promises a future where robots can perform complex tasks as easily as chatting, heralding a significant leap forward in AI's capabilities.


####


Further Reading


For those interested in the details of this groundbreaking research, the original paper, "Prompt a Robot to Walk with Large Language Models" by Yen-Jen Wang, Bike Zhang, Jianyu Chen, and Koushil Sreenath, provides in-depth insights into their methods and findings, contributing to the ongoing evolution of AI and robotics.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
