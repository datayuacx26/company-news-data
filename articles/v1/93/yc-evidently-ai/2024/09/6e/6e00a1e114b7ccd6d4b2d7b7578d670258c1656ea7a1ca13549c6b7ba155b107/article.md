---
schema_version: "1.0.0"
document_id: "6e00a1e114b7ccd6d4b2d7b7578d670258c1656ea7a1ca13549c6b7ba155b107"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/llm-as-a-judge-tutorial"
published_at: "2024-09-09T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:ed72a85c488dd9b98680681c1ee33b97ec6516161019d8164fea30f617d82815"
---

# Wrong but useful: an LLM-as-a-judge tutorial

How do you evaluate the quality of an LLM-powered system, like a chatbot or AI agent?


Traditional machine learning metrics don't apply to the generative LLM outputs. When an LLM summarizes a text or makes a conversation, it's tough to quantify if the result is "good" or "bad." Humans can judge these things, but it's not scalable to manually score every response.


One way to address this evaluation problem is to use an LLM to evaluate the outputs of your AI system, a practice nicknamed "[LLM-as-a-judge](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) ."


This tutorial shows how to create, tune, and use such LLM judges. We'll make a toy dataset and assess correctness and verbosity. You can apply the same workflow for other criteria.


We will use the[open-source Evidently Python library](https://github.com/evidentlyai/evidently) to run evaluations.


> **Code example** : to follow along, run this[example](https://github.com/evidentlyai/community-examples/blob/main/tutorials/LLM_as_a_judge_tutorial_updated.ipynb) in Jupyter notebook or Colab. There is also a step-by-step[docs guide](https://docs.evidentlyai.com/tutorials-and-examples/cookbook_llm_judge) .


The goal of the tutorial is to demonstrate how LLM judges work — and show why you also need to evaluate the judge itself!


*If you are looking for an introduction to the topic, check this*[LLM-as-a-judge guide](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) *.*


##### **\[fs-toc-omit\]Building an LLM-powered product?**


> Sign up for our free video course on LLM evaluations with 10 hands-on code tutorials. From designing custom LLM judges to RAG evaluations and adversarial testing. If you’re working hands-on with LLMs and have basic Python skills, this course is for you.
>
>
> [Save your seat ⟶](https://www.evidentlyai.com/llm-evaluation-course-practice)


## 📊 What are LLM evals?


[LLM evals](https://www.evidentlyai.com/llm-guide/llm-evaluation) , or evaluations, help measure your LLM’s performance. Is it accurate? Is it consistent? Does it behave like you expect?


Evals are essential during **development** (comparing models or prompts to choose the best one) and in **production** (monitoring quality live). And whenever you make a change, like tweaking a prompt, you’ll need to run **regression tests** to make sure the response quality hasn’t dropped in areas that were working fine before.


Depending on the scenario, your evals can take different forms:


- **Pairwise comparison:** Is response A better than B?
- **Reference-based:** Is response A correct compared to an approved answer?
- **Open-ended scoring:** Is the response detailed, polite, cheerful, etc.?


The first two are typically used for offline evaluations, where you have a “golden” example or can compare responses side by side. In production, evaluations are usually open-ended.


The methods vary as well. If you’ve got the time and resources, it’s hard to beat human evaluation. For constrained tasks like “user intent detection,” traditional ML metrics still work. But for generative tasks, it gets complicated. Yes, you can check things like semantic similarity or scan for specific words – but it’s rarely enough.


That’s why the **LLM judge method** is gaining traction — and for good reason. It works where traditional metrics don’t quite fit, both for offline and online evals.


## ⚖️ What is LLM as a judge?


LLM-as-a-Judge is an approach where you use an LLM to evaluate or "judge" the quality of outputs from AI-powered applications.


Say, if you have a chatbot, an external LLM can be asked to review its responses, assigning a label or score similar to what a human evaluator might do.


Essentially, the LLM acts like a classifier, assessing outputs based on specific criteria or guidelines. For example:


- **Correctness:** Is the response truthful to the source?
- **Relevance:** Does the content fit the topic?
- **Coherence:** Is the response logical and well-structured?
- **Safety:** Does the response avoid harmful or inappropriate language?


At first, it might seem odd to use an LLM to evaluate its “own” outputs. If the LLM is the one generating the answers, why would it be any better at judging them?


The key difference is that classifying content is simpler than generating it. When generating responses, an LLM considers many variables, integrates complex context, and follows detailed prompts. It’s a multi-step challenge. Judging responses, like assessing tone or format, is a more straightforward task. If formulated well, LLMs can handle it quite reliably.


How exactly does it work?


## ▶️ How to create an LLM judge?


It would be great to say that creating an LLM judge is as simple as writing a prompt or picking a metric, but there's a bit more to that.


**It starts with criteria and an evaluation scenario** . LLM judges are not like traditional metrics like[precision](https://www.evidentlyai.com/classification-metrics/accuracy-precision-recall) ,[NDGC](https://www.evidentlyai.com/ranking-metrics/ndcg-metric) , or[Hit Rate](https://www.evidentlyai.com/ranking-metrics/evaluating-recommender-systems) , which are deterministic and give the same output for the same input. LLM judges work more like human evaluators who label data.


You need to define the clear grading criteria for your use case, just like you'd give instructions to a person! For LLM, you do it in a prompt.


Starting with simpler, binary tasks like grading inputs as "correct/incorrect" or "relevant/irrelevant" is often a good idea. Breaking things down this way helps keep the results consistent and easier to verify — not just for the LLM, but for anyone else checking the output.


Because the next step in creating the judge is to…


**Create an evaluation dataset.** An LLM judge is a mini-machine learning project. It requires its own evals!


So, you must first prepare a few example inputs and grade them the way you want LLM to do it later. These labels will act as your ground truth to help you assess how well the LLM is judging things. And as you manually label the data, it forces you to really think through what you want the LLM to catch, which helps refine your criteria even more.


You can pull examples from your own experiments or production data or create synthetic test cases. This dataset doesn’t have to be huge, but it should include some challenging examples — like tricky edge cases where your criteria might need a little tweaking.


**Craft and refine the evaluation prompt.** Once you know what you want to catch, you need an evaluation prompt. Clarity is key. If the prompt is too vague, the results may be inaccurate.


For example, if you want the LLM to classify content as "toxic" or "not toxic," you should describe specific behaviors to detect or add examples.


While there are templates for LLM judges in different libraries (including[ours](https://github.com/evidentlyai/evidently) ), they may not align with your definitions. You must customize — or at least review — the evaluation prompts you use. After all, the real strength of LLM judges is that you can tailor them!


Once you craft your prompt, apply it to your evaluation dataset and compare results to your labels. If it’s not good enough, iterate to make it more aligned.


This LLM judge doesn’t need to be perfect — just "good enough" for your needs. Humans aren’t perfect either! The great thing about LLM judges is their speed and flexibility.


Let’s see it in practice.


## 💻 Code tutorial


In this tutorial, we create a simple Q&A dataset and use an LLM to evaluate responses for correctness and verbosity.


To follow along, you will need:


- **Evidently** open-source Python library.
- **OpenAI API key** to use LLM as a judge.


We will work with two types of evaluations:


- **Reference-based** . Compare new responses to approved reference answers.
- **Open-ended** . Evaluate the outputs for verbosity.


For both cases, we will use **binary judges** : score each response as "correct/incorrect" or "verbose/concise" with an explanation of the decision.


Here’s the steps we take:


- Create a toy dataset
- Manually label the responses for correctness
- Write an evaluation prompt for the LLM judge
- Apply the judge and evaluate its performance


Our focus will be on creating and tuning the LLM judges. Once you create an evaluator, you can integrate it into workflows like[regression testing](https://www.evidentlyai.com/blog/llm-regression-testing-tutorial) .


We recommend running this tutorial in Jupyter Notebook or Google Colab to visualize the results directly in the cell.


### Preparation


To start, install Evidently and run the necessary imports:


```text
!pip install evidently


```


> **Complete code:** follow along with an[example notebook](https://github.com/evidentlyai/community-examples/blob/main/tutorials/LLM_as_a_judge_tutorial_updated.ipynb) and[docs guide](https://docs.evidentlyai.com/tutorials-and-examples/cookbook_llm_judge) .


Next, we need a dataset to work with. We'll create a toy example using customer support questions. Each question will have two responses: one is the "target response" (imagine these as approved answers), and the other is a "new response" (this could be from a different model or prompt).


We manually labeled the new responses as either correct or incorrect, adding comments to explain each decision. This labeled data will serve as the baseline for the LLM judge.


There are both "good" and "bad" examples. Here is the distribution of classes:


### Check correctness


Now, let’s ask LLM to do the same! We will create a custom correctness evaluator using the[Evidently](https://github.com/evidentlyai/evidently) library. Evidently provides evaluation templates and helps visualize and test the results.


We will use a binary classification template for an LLM judge. It classifies responses into two labels, asks for reasoning, and formats the results. We just need to fill in the grading criteria.


Here is how we create the judge:


```text
correctness = BinaryClassificationPromptTemplate(
criteria = """An ANSWER is correct when it is the same as the REFERENCE in all facts and details, even if worded differently.
The ANSWER is incorrect if it contradicts the REFERENCE, adds additional claims, omits or changes details.
REFERENCE:
=====
{target_response}
=====""",
target_category="incorrect",
non_target_category="correct",
uncertainty="unknown",
include_reasoning=True,
pre_messages=[("system", "You are an expert evaluator. You will be given an ANSWER and REFERENCE")],
)


```


The prompt is quite strict: we prefer to mark a correct answer as incorrect than to mistakenly approve an incorrect one. It’s up to you!


Once the judge is configured, run it on the toy dataset:


```text
eval_dataset.add_descriptors(descriptors=[
LLMEval("new_response",
template=correctness,
provider = "openai",
model = "gpt-4o-mini",
alias="Correctness",
additional_columns={"target_response": "target_response"}),
])


```


When you apply this to a “response” column, Evidently will process the inputs row by row and send them the LLM for evaluation. You can inspect where the LLM got things right or wrong by checking the raw outputs:


```text
eval_dataset.as_dataframe()


```


This will show a dataframe with added scores and explanations.


Note that your results will look different: LLMs are not deterministic.


You can also get a summary report to see the distribution of scores.:


Let's also quantify how well the evaluator performs! Treating this as a classification task, we can measure things like:


- **Precision** : How many of the responses labeled incorrect are actually incorrect?
- **Recall** : How many of the incorrect responses were identified by the LLM?


Recall is particularly relevant since our goal is to catch all discrepancies.


> Want to understand these metrics better? Read about[precision and recall](https://www.evidentlyai.com/classification-metrics/accuracy-precision-recall) .


To evaluate the LLM judge quality, we identify our manual labels as **ground truth** and LLM-provided labels as **predictions** and generate a classification report with Evidently.


Here is what we get:


If you look at the[Confusion Matrix](https://www.evidentlyai.com/classification-metrics/confusion-matrix) , you will see one type of error each.


Overall, the results are quite good! You can also zoom in to see specific errors and try refining the prompt based on where the LLM struggled. With the manual labels already in place, this iteration becomes much easier.


You can also try to make results worse: when we experimented with a naive grading prompt ("ANSWER is correct when it is essentially the same as REFERENCE"), this led to only 60% accuracy and 37.5% recall. Specificity helps!


For your use case, you might adjust the focus of the prompt: for instance, emphasizing tone or the main idea instead of looking at every detail.


### Check verbosity


Next, let’s build a verbosity evaluator. This one checks whether responses are concise and to the point. This doesn’t require a reference answer: the LLM evaluates each response based on its own merit. This is great for online evaluations.


Here is how we define the check:


```text
verbosity = BinaryClassificationPromptTemplate(
criteria = """Conciseness refers to the quality of being brief and to the point, while still providing all necessary information.
A concise response should:
- Provide the necessary information without unnecessary details or repetition.
- Be brief yet comprehensive enough to address the query.
- Use simple and direct language to convey the message effectively.""",
target_category="concise",
non_target_category="verbose",
uncertainty="unknown",
include_reasoning=True,
pre_messages=[("system", "You are an expert text evaluator. You will be given a text of the response to a user question.")],
)


```


Here is what we get once we apply it to the same column with "` new_response` ":


You can take a look at individual scores with explanations:


Don’t agree with the results? No problem! Use these labels as a starting point, correct where needed, and you’ll get a golden dataset - just like the one we started with when evaluating correctness. From there, you can iterate on your verbosity judge.


*Want to understand other evaluation methods? Check out the*[LLM metrics guide](https://www.evidentlyai.com/llm-guide/llm-evaluation-metrics) *.*


## What’s next?


The LLM judge itself is just one part of your evaluation framework. Once set up, you can integrate it into workflows, like testing LLM outputs after you’ve changed a prompt or ongoing quality monitoring.


At Evidently AI, we’re building an AI observability platform that simplifies this entire process. With **Evidently Cloud** , you can automate and scale these evaluations without writing code, or use it to track the results of evals you run locally. It’s a collaborative platform that makes it easy to monitor and assess the quality of AI systems.


Ready to give it a try?[Sign up for free](https://www.evidentlyai.com/register) to see how Evidently can help you build and refine LLM evaluators for your use cases!
