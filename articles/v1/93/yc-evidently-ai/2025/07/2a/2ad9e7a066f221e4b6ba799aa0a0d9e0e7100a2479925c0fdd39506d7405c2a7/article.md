---
schema_version: "1.0.0"
document_id: "2ad9e7a066f221e4b6ba799aa0a0d9e0e7100a2479925c0fdd39506d7405c2a7"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/llm-judge-prompt-optimization"
published_at: "2025-07-11T00:00:00+00:00"
first_seen_at: "2026-07-24T09:14:03.070856+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:8bb704554c2721aea89d00ab89d0ef863fd1053919344b43719b45f30f5cd0c4"
---

# Evidently 0.7.10: open-source prompt optimizer for LLM judges

Evaluating LLM outputs is hard – especially when the criteria are subjective. One popular way to tackle this is[LLM-as-a-judge](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) where you prompt an LLM to evaluate the text outputs. It acts as an automated reviewer – scoring responses as “good” or “bad”, or classifying them according to your custom rubric.


But there’s a catch: to use this approach, you need to write a good evaluation prompt. And that’s often where most of the effort goes.


We built a prompt optimizer to make this easier.


It’s implemented as a new feature of the open-source[Evidently Python library.](https://github.com/evidentlyai/evidently)


You pass in a labeled dataset – ideally with expert feedback – and it generates a judge prompt that captures your criteria. It’s like supervised learning, but for prompts.


In this post, we’ll explain how it works, where it helps, and walk through two examples.


> Like the idea? Give us a star on[GitHub](https://github.com/evidentlyai/evidently) to support the project.


## The problem


LLM judges are powerful tools for AI product development. They let you automate[LLM evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) , adding output quality checks to your production and testing workflows.


If you’ve seen our earlier posts (like this introduction to[LLM judges](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) or hands-on[video tutorial](https://www.youtube.com/watch?v=kP_aaFnXLmY&list=PL9omX6impEuNTr0KGLChHwhvN-q3ZF12d&index=6) ), you already know how they work: you prompt an LLM to act as a judge, and it will evaluate text outputs for things like clarity, tone, safety, or any other custom-defined criterion.


But for a judge to work, you need to encode your criteria in the evaluation prompt. How exactly do you create one?


*The prompt engineering workflow for an LLM judge.*


A good starting point is to collect expert-labeled examples – where humans review outputs just as you want the LLM judge to do later. This helps you solidify and express your evaluation criteria. You can then use those examples to come up with a prompt that lets an LLM replicate these decisions consistently.


But prompt engineering isn’t trivial. It requires analyzing the inputs and labels and figuring out how to turn them into clear, structured instructions that the model can follow.


We built something to help with this.


## Introducing: LLM judge prompt optimizer


We created a tool that automates LLM judge prompt generation based on expert-labeled datasets. Here's how it works:


**1. Label the examples.** You start with a dataset of example LLM outputs – generated summaries, replies, reviews, etc. – and label them yourself or with the help of domain experts. You can use binary (e.g., good/bad) or multi-class labels. Basically: act as the judge first.


**2. (Optional but powerful) Add expert comments** . Include short notes explaining why a label was assigned. For example: “too wordy,” or “misses this key detail.” These help the optimizer extract underlying patterns and build a stronger prompt.


You can run the optimizer with just labels, or labels plus comments.


**3. Feed the dataset to the optimizer.** The optimizer runs multiple prompt variants. It uses the examples (and comments) to generate an updated prompt – asking LLM to act as a prompt writer. Each iteration is scored based on how well the LLM judge output aligns with the original human labels. It uses accuracy as the primary metric.


‍ **4. Review the result.** The final prompt is returned to you so that you can use it in evaluations.


The idea is to “reverse-engineer” the labels and comments into a natural language prompt that captures your labeling logic. It can generalize feedback into a rubric, or optionally add few-shot examples inside the criteria.


It’s almost like training a machine learning model where you start with labeled data, and then try to learn the patterns behind it. But instead of training a model, you're creating a prompt that explains the rules in plain language.


And just like when training a model, your results improve with better input data. The optimizer works best when you include contrastive examples and signal-rich annotations that explain why an output is good or bad.


It’s fully open-source, and released as part of our[Evidently Python library.](https://github.com/evidentlyai/evidently)


## Example 1: Binary judge on code review quality


> See the[full code example](https://github.com/evidentlyai/evidently/blob/main/examples/cookbook/prompt_optimization_code_review_example.ipynb) here.


Let’s walk through the same example we used in our LLM judge[video tutorial](https://www.youtube.com/watch?v=kP_aaFnXLmY&list=PL9omX6impEuNTr0KGLChHwhvN-q3ZF12d&index=6) . In this example, the goal is to evaluate the quality of code review comments generated by an LLM.


The setup is simple:


- You have an LLM that leaves review comments on pull requests.
- You want to ensure the comments are helpful and constructive, so you want to create an LLM evaluator that can judge it.
- To start, you’ve collected a dataset of generated comments, and human experts have labeled them as either “good” or “bad”.
- Reviewers also left short comments explaining why the judgment was made.


Here is how the dataset looks:


To create an LLM judge, you can start with a simple baseline criteria like:


` "A review is good when it is constructive and actionable. Classify as good or bad."`


In this case, we use an Evidently[LLM judge template](https://docs.evidentlyai.com/metrics/customize_llm_judge) , so you only need to formulate the criteria. The shared parts of the prompt, like asking for classification, returning structured output, or providing reasoning, are already handled by the template.


Then you can run the optimizer to refine the prompt: it will use the labeled examples and reviewer comments (e.g., “too vague”, “tone too harsh”) to create a better version. It runs several attempts, checks alignment with the human labels, and iterates.


In the original tutorial, we manually iterated on the prompt and eventually got to the 98% accuracy. In our case, automatic prompt optimization took mere seconds and raised accuracy from 64% to 96% (tested with GPT-4o mini).


Here is the resulting prompt we got:


` A review is classified as GOOD when it provides actionable, constructive feedback that clearly guides the recipient toward improvement, without being overly critical, vague, or focused on minor details without substantive suggestions. A review is classified as BAD when it is non-actionable, overly critical, lacks clarity or specificity, or when it offers praise without actionable suggestions. The review should avoid ambiguity and provide concrete steps for improvement when suggesting changes.`


## Example 2: Multi-class topic classifier


> See the[full code example](https://github.com/evidentlyai/evidently/blob/main/examples/cookbook/prompt_optimization_bookings_example.ipynb) here.


Not all judges have to assess “quality.” Sometimes, you may want to categorize outputs by another meaningful criterion – such as topic classification for production analytics.


Let’s say you’re building a support tool and want to understand what a user is asking about when they send their first message. You want to classify each message into a topic: “Booking”, “Technical problem”, etc. so that you can track those in time.


You can collect a small labeled dataset:


- Each row contains a user message.
- A human labels it with a topic category.
- No comments or explanations are provided – just the labels.


Here is how the dataset looks:


You might start with a basic LLM evaluation prompt that lists the intended classes and expects the LLM to catch on their meaning simply based on the class name:


` "Booking": "bookings",
"Technical": "technical questions",
"Policy": "questions about policies",
"Payment": "payment questions",
"Escalation": "escalation requests"`


Then you run the optimizer. It refines and expands the prompt based on the label patterns in your dataset and gets us to 96% accuracy – higher than a naive baseline.


Effectively, the LLM expanded on the meaning of the categories, using the actual examples to learn from.


` Classify the following types of queries based on their primary intent, considering phrases and context that indicate what the user is seeking help with.`


` 1. **Payment** - Inquiries about issues related to payments, fees, or refunds.`


` 2. **Booking** - Questions about bookings, confirmations, or modifications related to reservations.`


` 3. **Escalation** - Requests for urgent assistance due to unresolved issues or frustrations.`


` 4. **Technical** - Queries regarding technical issues with applications, websites, or systems.`


` 5. **Policy** - Inquiries concerning rules, regulations, or policies governing transactions or services.`


` Use this classification framework to analyze the queries and select the most appropriate category based on the context provided.`


Voila – you now have a lightweight, zero-shot topic classifier. You can run it in production, monitor the topic distribution, and improve routing logic for your users.


## What’s next


Our goal is to make it easier to evaluate and monitor LLM outputs using your own standards – and automate as much of the setup as possible.


With the current implementation, there are two main input modes for prompt optimization:


- Inputs and expert labels
- Inputs, labels + expert comments


You can use either setup for binary or multi-class evaluation tasks.


This makes it easier to collaborate with domain experts. You don’t need them to write prompts – just to label examples and leave comments. The optimizer takes care of turning that feedback into a working judge.


It's an early release, so this is just the beginning. Here’s what we’re working on next:


- Smarter prompt generation strategies, including few-shot selection and sampling.
- Synthetic input generation, so you can start evaluating even if you don’t yet have real data.


Does this approach resonate with you?


If you’re working on LLM evaluation or testing and want to try the optimizer – come[check it out](https://github.com/evidentlyai/evidently) and join the conversation on[Discord](https://discord.gg/xZjKRaNp8b) . We’d love to hear if it works for you!


And this approach doesn’t just apply to LLM judges – you can use the same method to optimize your main product prompts, too. Stay tuned for a new example soon.
