---
schema_version: "1.0.0"
document_id: "07628988e55560a92490eda7fb7d6266e8a17343acefd578b4cda715e15013c3"
company_key: "yc-tavus"
company: "Tavus"
source_id: "yc-tavus-news-import-04156f4a70a3"
canonical_url: "https://www.tavus.io/blog/understanding-multi-turn-llms-through-search"
published_at: "2025-11-12T00:00:00+00:00"
first_seen_at: "2026-07-24T03:24:31.995752+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:7422eb4d1f7c07cd6656091dda842b3049841db73e437a343036672e14979cf8"
---

# Understanding intuition behind multi-turn LLMs through the prism of search

*This is an intro to a deeper dive published on*[Karthik's Medium](https://medium.com/@a.karthikragunath/understanding-intuition-behind-multi-turn-llms-through-the-prism-of-search-c4a0eb8ad44a) *.*


#### Large language models (LLMs) are at their most impressive not when they merely recite facts, but when they **think out loud.**


Thinking out loud entails breaking a complex question into smaller steps, consulting outside knowledge if needed, and then weaving everything into a coherent answer.


This post explores that skill, known as **multi-turn reasoning** , through the concrete example of an LLM that performs web searches to find information. We will outline how such an LLM can be trained via reinforcement learning to become an active problem-solver rather than a passive text generator.


Before diving into how multi-turn reasoning works, it’s useful to understand previous approaches to augmenting LLMs with external knowledge (like search) and why a full reinforcement learning approach is such a leap forward.


## Prior approaches


**1. Retrieval-Augmented Generation (RAG):** The model first retrieves documents (for example, using embedding similarity to find relevant text) based on the user query, then incorporates those documents into its prompt or context before answering. This injects external knowledge into the LLM’s response. However, retrieval in RAG is usually a one-shot affair – the model grabs some info once and then stops. If the needed information is scattered across multiple sources or if the initial retrieval is slightly off, the answer may be inaccurate or incomplete.


**2. Treating search as a tool:** Another approach is to let the LLM call a search engine or other tools as part of its reasoning process. In practice, one can prompt an LLM with instructions on how to use a search API (for example, *“If you need more information, you can ask the search engine”* ). This can be done through clever prompting or by training the model with examples of tool use. While this *tool-use via prompting* can work, it often struggles to generalize. The model might not have seen similar tool-using examples during its pre-training, so it may fail on novel tasks or require careful prompt engineering for each new scenario.


**3. Fine-tuning the LLM on custom data:** A more direct method is to fine-tune or train the LLM on datasets that demonstrate the desired behavior (e.g. sequences where the model is taught step-by-step reasoning or searching). Fine-tuning can adapt the model better than prompting alone. However, this approach is **difficult to scale** . It demands large amounts of high-quality, task-specific training data (annotated step-by-step trajectories), and training these huge models for every new use case or every time new data arrives is extremely costly. For companies dealing with rapidly changing or real-time data, constantly re-training a giant LLM is impractical.


These approaches each have strengths, but also clear limitations. This sets the stage for a more dynamic solution: **training the LLM itself to decide when and how to search, in a multi-turn interactive manner, using reinforcement learning.**


## The idea: multi-turn LLM reasoning with search (via RL)


Instead of relying on static retrieval or single-turn prompts, we make the **LLM an active retriever and reasoner** . The idea is to train the LLM (using reinforcement learning) to **decide when to search, what to search for, and how to use the results** over multiple turns, in order to produce a correct final answer. In other words, the LLM becomes an agent that can autonomously call the search engine as needed, multiple times if the question requires it, and integrate those findings into its reasoning process.


This approach turns question-answering into an interactive dialogue between the LLM and a search tool. The LLM might start by querying something, read the results, then ask another follow-up query, and so on, until it has gathered enough information to answer the user. By using reinforcement learning (RL) to train this capability, the model can **learn from experience** which search strategies lead to correct answers. We don’t have to hard-code when to search or rely on human-written examples for every possible query — instead, the model figures out the strategy through trial and error, guided by a reward signal (which we’ll describe shortly).


Why go through the trouble of RL training? Because it has been observed that this kind of **search-augmented, multi-turn strategy can significantly improve accuracy** on challenging questions. In some cases, research has reported *20–40% boosts in accuracy* over standard one-shot retrieval approaches. Intuitively, it allows the model to fetch exactly the information it needs, when it needs it, and to correct itself if an initial attempt didn’t find the answer. It’s a leap forward in capability: rather than being a one-pass answer generator, the LLM becomes a **problem-solving agent** that actively gathers information.


In the rest of this post, we’ll outline how such a system is set up. We’ll cover the core components of the agent, how the training process works (at a high level), and walk through a simplified example of the LLM agent in action using web search.


## Architecture overview: policy, value, and reward models


Training an LLM to use search in a multi-turn conversation involves three main components working together in an *actor-critic* reinforcement learning framework:


- **Policy model (Actor):** This is the LLM itself (for example, a GPT-style or LLaMA model) which generates the next *action* at each step of the conversation. An action can be either a piece of the answer (natural language text as part of the reasoning) or a special instruction to perform a search. In practice, we might denote a search action with a special token or format (e.g., the model outputs` <search> ... </search>` around a query string to indicate it wants to issue that query to a search API). The policy model produces a probability distribution over the next token in the sequence at each step. It is the **brain of our agent** , deciding what to do or say next. We will be fine-tuning this model through RL so that it learns useful search habits.
- **Value model (Critic):** This model estimates how good the current state (or the conversation so far) is in terms of expected final outcome. Typically, the value model has the same architecture as the policy LLM but with an extra output head that predicts a single scalar value (a score). You can imagine the value model looking at everything that’s happened so far and answering, *“Given the conversation up to now, how likely is it that we will end up with a correct answer?”* The value serves as a **guide** or baseline for training, helping the learning algorithm discern which actions turned out better or worse than expected. It’s called the critic because it critiques the actor’s moves by predicting future reward.
- **Reward model:** This is an external evaluator that provides feedback at the end of each dialogue (episode). In our case, we design a simple reward model: for example, **+1 if the final answer is correct, 0 if it is incorrect** . The reward model is like a judge who knows the correct answer or can measure success (it could be a programmatic check against a known answer, or potentially even a learned model that scores the answer’s quality). Importantly, the reward model **only gives feedback at the end of the interaction** , not at every step. It doesn’t tell the LLM whether each search query was good or bad, it only says “your final answer was right” or “wrong” (or perhaps gives a score on a scale for quality).


These three components form the backbone of the training algorithm. The policy (actor) chooses what to do at each step, the value (critic) predicts the eventual reward from each state to help assess the actions, and the reward model delivers the actual result at the end of the process. Using these, we can shape the LLM’s behavior through reinforcement learning: we will update the policy to favor actions that lead to correct answers (using the critic’s guidance to do so efficiently) and update the value model to better predict which states are promising.


*Why have a critic at all?* One intuitive explanation is to think in terms of practicing a skill. Training the policy is like practicing your shots in basketball — you adjust your technique to score more baskets. The value model is like developing an instinct for what a “good shot” looks like — it helps you figure out which attempts are promising or not. Without a critic, the policy would only know if the final result was success or failure, but not *how that compared to expectation* . The critic’s estimate acts as a baseline or reference, so the policy doesn’t get overly excited by lucky successes or too discouraged by unlucky failures. In short, the critic stabilizes learning by providing context for the rewards.


## Training via reinforcement learning (PPO with search)


How do we actually **train** the policy LLM to use the search engine effectively? We set it up as a reinforcement learning task using an algorithm called Proximal Policy Optimization ( **PPO** ), which is a popular choice for fine-tuning language models with RL. PPO is known for its stability – it avoids making overly large updates to the model in a single step, which is important when dealing with large language models.


At a high level, the training works as follows:


1. **Rollout (interaction step):** We have the LLM (policy) attempt to answer a question, with the ability to call the search tool as needed. This means the LLM will generate a sequence of actions: possibly a search query, then see the results, then maybe another query, and eventually an answer. This sequence of interactions with the environment (the search engine providing results) is called a **trajectory** or rollout. It’s essentially the transcript of the conversation including the LLM’s queries and the retrieved information it got in return.
2. **Assign reward for the outcome:** When the LLM produces its final answer, we check it against the ground truth (or use another criteria) and assign a reward. In our simple setup, the reward is **+1 for a correct answer, 0 for an incorrect answer** . This reward is assigned *only at the end of the trajectory* . The intermediate steps (the content of the queries, etc.) don’t get an explicit reward from the environment. In other words, the LLM only finds out after the fact whether its answer was right or wrong. *(In practice, we may also include a small penalty at each step to discourage unnecessary or out-of-control behavior. One common technique is adding a* ***KL-divergence penalty*** *to keep the policy’s behavior from straying too far from the original pre-trained model’s style. This ensures the LLM doesn’t start generating bizarre queries or incoherent text just to get a reward.)*
3. **Calculate advantages for each action:** Using the **value model (critic)** , we estimate how much each action contributed to the final result. The idea of **advantage** is a core concept in actor-critic RL: it measures how much better (or worse) the outcome was, compared to what the critic expected at that moment. If the final reward was higher than anticipated, then the actions leading up to it get a positive advantage (meaning those decisions were better than the baseline expected). If the outcome was worse than expected (e.g., the model was confident but got a reward of 0), then the actions get negative advantages. These advantage signals tell us which actions should be encouraged or discouraged during learning.
4. **Policy update (actor learning):** Now we update the policy LLM’s weights using the advantages as guidance. PPO uses a form of **policy gradient** update: essentially, it nudges the policy to increase the probability of actions that had positive advantage and decrease the probability of actions with negative advantage. A key feature of PPO is that it **clips** these updates to avoid making any single change too large. For example, if the model initially gave a certain token a 5% probability, and the learning signal says it should be higher, PPO might increase it to, say, 6% (but not all the way to 15% in one jump). This controlled update helps keep the language model stable (hence “proximal” – staying close to the previous policy).
5. **Value update (critic learning):** We also update the value model so that its predictions of expected reward align better with what actually happened. Using the observed outcome, we adjust the critic to reduce the error between its predicted value and the actual return. This typically involves a regression loss (mean squared error) between the critic’s prediction and the *actual* return (the final reward, adjusted for any intermediate discounts if used). PPO also clips the value updates to avoid wild swings. The end result is that next time, the critic will hopefully predict a reward closer to what we observed, making the advantage calculations more accurate.
6. **Repeat with many examples:** This process is repeated across many questions and scenarios. Over numerous training iterations, the policy LLM gradually learns a strategy that yields higher rewards (i.e. more correct answers with efficient use of search), and the value model becomes better at anticipating the outcomes. The interplay of actor and critic ensures that the LLM refines its ability to decide **when to search, what to search, and how to incorporate the information** into a final answer.


Through this reinforcement learning loop, the LLM acquires the intuition for multi-turn reasoning. It learns, for instance, that if it’s unsure about a fact, calling the search engine is valuable, or that certain queries lead to better information. It also learns when to stop searching and finalize an answer. All of this is learned from the simple signal of whether the answer was correct or not, without needing explicit step-by-step labels from humans.


## Example: multi-turn search in action


To make this concrete, let’s walk through a simplified example of a trained LLM agent solving a factual question by querying a search engine. Consider the question:


**User:** “What is the population of the largest city in Texas?”


A capable multi-turn LLM agent will reason as follows: First, it might need to find out which city is the largest in Texas (since the question is indirectly asking for the population of that city). Then, once it knows the city (Houston, in this case), it should find the latest population of Houston. Finally, it can give the answer. Here’s how an interaction might look, with special tags to indicate the search actions and information retrieved:


` Question: What is the population of the largest city in Texas?`


` <search>largest city in Texas</search>`


` <information>Houston is the largest city in Texas...</information>`


` <search>Houston population 2024</search>`


` <information>As of 2024, Houston's population is 2,304,580...</information>`


` <answer>The largest city in Texas is Houston, with 2,304,580 people.</answer>`


Let’s break down what happened in this dialogue:


- The **user** asks a question: *“What is the population of the largest city in Texas?”*
- The **LLM (policy)** decides the first thing it should do is search for “largest city in Texas.” It outputs the action` <search>largest city in Texas</search>` . This signals the system to perform a web search for that phrase.
- The **environment (search engine)** returns a result, which is provided to the LLM in an` <information>` tag. The content might say something like “Houston is the largest city in Texas…”, confirming that Houston is the largest city.
- The LLM reads that information (which is now part of its context for the next step) and decides to search again, this time for “Houston population 2024” by outputting` <search>Houston population 2024</search>` .
- The search engine returns an` <information>` snippet: “As of 2024, Houston’s population is 2,304,580…”.
- With this data in hand, the **LLM** now has what it needs. It produces a final answer:` <answer>The largest city in Texas is Houston, with 2,304,580 people.</answer>` .


In this example, the agent successfully broke the problem into two searches and then answered the question. The special` <search>` and` <information>` tags are just one way to implement the interaction; they mark when the LLM is asking the tool for help and when the tool’s response is given. The final` <answer>` tag indicates the agent is now giving the user the result.


This kind of trajectory interleaves the model’s reasoning with tool use. During training, such an interaction would be one episode on which we can compute a reward. In this case, since the final answer was correct, the agent would receive a reward of +1 at the end. The intermediate steps (the search queries) didn't themselves receive reward signals (apart from implicit penalties or the lack of positive reward). The learning algorithm would use this outcome to reinforce the actions that led to success (in this case, choosing to search for the largest city, then searching for the population of Houston, then giving the answer). Over many similar trials, the LLM learns that this multi-step approach is effective for these kinds of questions.


**Outcome-based reward:** It’s worth highlighting that the training reward in our setup is **solely based on the final outcome** . The agent isn’t directly rewarded for doing a search or using multiple steps; it’s only rewarded if the final answer is correct. This is a deliberate design choice. It simplifies the feedback: we don’t need to label the correct sequence of actions for the agent, we just tell it whether it got the answer right. The downside is the agent has to figure out *why* the answer was right or wrong, but that’s where the value model’s guidance and the many training iterations help. In essence, the agent learns that certain behaviors (like those searches) tend to precede getting the answer correct, so it increases those behaviors in the future.


## Wrapping up


Multi-turn reasoning with an LLM and a search tool marries the language understanding of modern models with the vast knowledge on the web. By training the LLM with reinforcement learning, we enable it to **iteratively retrieve information and refine its answers** , leading to far more accurate and robust performance on complex queries. This approach goes beyond one-off retrieval or pre-programmed tool use; it creates a dynamic agent that learns how to research answers on its own.


The example we walked through is just a taste of what’s covered in the full post. In the original article, you’ll find a deeper dive into the mathematics of the training process (how we calculate the advantages, how the PPO update works in detail, with pseudocode and even a token-level reward example). If you’re interested in the nitty-gritty of how each token’s probability is adjusted and how the training converges, be sure to **read the full post on Medium for all the details** .


***About the author:*** *Karthik Ragunath Ananda Kumar is one of our AI researchers at Tavus. This abridged article is based on*[his Medium blog post](https://medium.com/@a.karthikragunath/understanding-intuition-behind-multi-turn-llms-through-the-prism-of-search-c4a0eb8ad44a) *. For the complete technical exploration, check out the full post linked above.*


## Ready to converse?


Get started with a free Tavus account and begin exploring the endless possibilities of CVI.


[Get started](https://platform.tavus.io/auth/sign-up?utm_content=inline-cta)
