---
schema_version: "1.0.0"
document_id: "2957380cc6ac21f6125d68c0d2fe0508e1ad2078b32d7a596b75e95bb3b6258d"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/llm-regression-testing-tutorial"
published_at: "2024-06-20T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:a7249e61625631ec768229e8bff591e496ae1be2061b890734651b20c640c1be"
---

# Watch the language: A tutorial on regression testing for LLMs

Let's say you're working on a product powered by LLMs, like a chatbot or a Q&A system. When you make changes, such as tweaking a prompt, how do you know if it makes things better or worse? You need a way to test the quality of LLM outputs.


One approach is to treat this as a **regression testing** problem.


In this tutorial, you will learn how to check the quality of LLM outputs systematically. You will create “before” and “after” datasets with different issues, such as changes in answer content, length, or tone, and see which methods can detect them. You will use the[open-source Evidently Python library](https://github.com/evidentlyai/evidently) to run evaluations.


> **Code example:** to follow along, run the[example](https://github.com/evidentlyai/community-examples/blob/main/tutorials/How_to_run_regression_testing_for_LLMs.ipynb) in Jupyter notebook or Colab.
>
>
> ⚠️ **Disclaimer** :
> This example uses the Evidently API as available in version 0.6.7 or lower. Please ensure you are using the correct version when running this example. For updated and new examples, visit our[documentation](https://docs.evidentlyai.com/) .


The goal is to show various ways to evaluate LLM output, from semantic similarity to regular expressions, and give a sense of what's possible.


In the[follow-up tutorial](https://www.evidentlyai.com/blog/llm-testing-tutorial) , you can learn how to run the process end-to-end and build a dashboard to monitor the test results.


##### **\[fs-toc-omit\]Building an LLM-powered product?**


> Sign up for our free email course on LLM evaluations for AI product teams. A gentle introduction to evaluating LLM-powered apps, no coding knowledge required.
>
>
> [Save your seat ⟶](https://www.evidentlyai.com/llm-evaluations-course)


## What is regression testing?


**Regression testing** is a quality assurance technique used in software development. It ensures that changes to code don't mess things up.


Every time you update your code, you run a bunch of tests. If they all pass, great – you can roll out your changes. But if any test fails, you've got to fix things before moving forward. The goal is to avoid breaking old features or reintroducing known bugs.


How does this translate to[evaluating LLM applications](https://www.evidentlyai.com/llm-guide/llm-evaluation) ? When you change prompts, models, or retrieval strategies, you still need to test your software: nothing new here. But you also must test the generative part – the one that produces text, code, or content in response to user input.


And here is the rub: unlike traditional software functions, LLM outputs can have many acceptable answers for the same input. You can't simply look for an exact match for your unit tests. And if you try to describe what makes a good answer, you end up with qualities like "neutral tone" or "relevant response." How do you check for these?


It starts with the test cases.


Test cases are **sample inputs** that represent what users do with your application. For a Q&A system, these could be typical questions. You can collect them from real user interactions, write them yourself, or even ask an LLM to brainstorm them. You also need **example answers** , at least for some inputs. If your app isn't live yet, you can approve or edit some test completions or ask domain experts to do that.


This set of reference inputs-outputs is sometimes called the “ **golden dataset** .” It’s truly worth investing in. Here is what you can include:


- **Common user scenarios** . For instance, if the most popular question is, "How do I reset my password?" include this one. You can create multiple sets by topic.
- **Edge cases** like errors fixed in the past or unusual inputs, such as very long messages, questions in a foreign language, or on products you no longer support.
- **Adversarial scenarios** , like questions pushing boundaries, such as asking for financial advice your app shouldn't give.


In the end, you want a diverse set of input-output pairs that show what “good” looks like.


Even though some[LLM evaluation methods](https://www.evidentlyai.com/llm-guide/llm-evaluation-metrics) work without reference answers, building this labeled test set is crucial. Start small and keep adding examples as you discover new ways users interact with the app.


Once you have this golden dataset, you can evaluate every change against it. Different techniques, from semantic similarity to model-based evaluations, can help.


Let’s cover them in practice.


## Tutorial scope


In this tutorial, you will learn how to use different methods to test the quality of LLM outputs.


**Dataset:** You will run checks on small synthetic datasets with customer support chatbot logs of an online payments company. It’s just an example. The methods will work just the same as other LLM-powered use cases, from summarization to RAGs and agents.


**Scenario:** As a starting point, you already have a golden dataset with pre-approved responses. You then imitate making a change (e.g., to the prompt) to get a new set of outputs. Your goal is to compare new responses against old ones using different techniques.


We'll skip the details of running this through an app (simply pass the new input and call your product or LLM API) and focus on the evaluation part.


**Tooling:** To evaluate system quality, you will use[Evidently](https://github.com/evidentlyai/evidently) . Evidently is an open-source tool for testing and monitoring ML and LLM-powered systems. It includes many built-in evaluation methods and lets you create test suites that visualize pass or fail results.


**Requirements:** Basic knowledge of Python. You will run all evaluations locally in your Python environment.


The goal of the tutorial is to introduce different LLM testing methods. You will run tests one by one and look at individual results to make them illustrative. In practice, you can automatically run the checks as part of your CI/CD process after you make any changes.


Let’s start!


### Installation


First, install the Evidently library with some extras for LLMs:


```text
!pip install evidently[llm]


```


Import the necessary components:


```text
import pandas as pd
from evidently import ColumnMapping
from evidently.test_suite import TestSuite
from evidently.descriptors import *
from evidently.tests import *


```


To begin, you need a mock dataset with typical questions and reference answers:


```text
data = [["Hi there, how do I reset my password?", "To reset your password, click on 'Forgot Password']]


```


Run the complete code from the[example](https://github.com/evidentlyai/community-examples/blob/main/tutorials/How_to_run_regression_testing_for_LLMs.ipynb) to create it. You will also add a " **new_response** " column to imitate receiving a new set of answers. Here is what you will work with:


You will repeat this process a few times throughout the tutorial. Once you have the data ready, you can run the first check. ****


### Test response similarity


Even a small tweak to a prompt can change the output. As you make changes, you often need to ensure that all responses stay broadly “the same.” But you cannot match the answers word by word! Instead, you can look at their meaning.


To quantify this, you can measure **semantic similarity** . This involves using an embedding model to turn each text into a vector and then comparing the Cosine Similarity between them. The normalized score gives you values from 0 (completely different) to 1 (very similar). A score of 0.5 means the texts are unrelated.


You can run these checks as individual unit tests or test a larger set of questions at once. Evidently makes this easy: you just need to provide the two text columns to compare.


You also need a **condition** for the test. For example, you might expect all new answers to have a similarity score of 0.9 or higher compared to the reference answers. If any response scores lower, the test should fail. Here’s how you can define this check:


```text
test_suite = TestSuite(tests=[
TestColumnValueMin(
column_name=SemanticSimilarity(
display_name="Response Similarity",
with_column="target_response").
on("new_response"),
gte=0.9),
])


```


**Need help reading the code?** Here is how the API works.


- **` TestSuite`** : This core class defines a collection of checks.
- **` TestColumnValueMin`** : This test lets you set a condition for a minimum column value. You can pick other tests, like **` TestColumnValueMax`** or **` Mean`** .
- The column (defined via **` column_name`** ) defines the values to test. You check the **` SemanticSimilarity`** for the response pairs. While this column is not yet in the dataset – Evidently will add it. There are other descriptors to choose from.
- **` on`** points to the existing columns to measure the similarity.
- **` display_name`** : An optional name for this test.
- **` gte`** : This sets the test condition as greater than or equal to 0.9. You can also use` eq` (equal),` gt` (greater than),` lt` (less than),` lte` (less than equal), etc.


To run this Test Suite on the example dataset with typical questions, pass it as **` current_data` .** Call the results directly in Python:


```text
test_suite.run(reference_data=None,
current_data=typical_questions)
test_suite


```


Here’s what happens: the test fails.


The minimum similarity score is 0.68, and there are two values below the set threshold. To investigate, you can look at the dataset with the semantic similarity score added to each line:


```text
test_suite.datasets().current


```


For many responses, the semantic similarity is high as expected:


The most interesting cases are where the semantic similarity score is below 0.9. Let’s filter for those:


You can clearly see the changes from the golden responses. In one instance, the chatbot hallucinated instructions on how to add multiple users. In another, it gave an unrelated answer about viewing transaction history instead of setting alerts.


Let's rerun this check on a different set of inputs. Suppose you have a scenario where you don't want the chatbot to answer but instead escalate to a human agent. You will create a dataset with **` agent_questions`** to run a new check. Here’s the result:


Most response pairs are very similar, but there is an outlier with a low score. In this case, the chatbot gave a response instead of redirecting to an agent:


When deciding whether to send a specific question to an agent, you’re likely dealing with a classification problem on the backend. You can test it as a classification problem, too!


### Test classification quality


It's common to add routing logic before your LLM response. This means you predict the intent of each incoming request and send the easy ones to the LLM. This is essentially a classification problem. If your setup works this way, you can test it as a separate component.


It doesn't matter if you use an LLM or a smaller machine learning model to do this; the testing method is the same. You just need a labeled dataset with correct " **target** " classes for given inputs. The new class predicted by the LLM becomes the " **prediction** ."


Let's imagine you classify requests into two scenarios: whether they should be handled by an agent or automated. Here’s how the mock **` classification_data`** will look:


To evaluate the results, you can run the **` TestAccuracyScore`** test. Let’s set the threshold to be greater or equal to 90%: meaning you expect 9 out of 10 predictions to be correct.


```text
test_suite = TestSuite(tests=[
TestAccuracyScore(gte=0.9),
])


test_suite.run(reference_data=None,
current_data=classification_data,
column_mapping=ColumnMapping(pos_label='agent'))
test_suite


```


> **Note on API** . Using **` column_mapping`** helps Evidently process the dataset correctly. You must indicate which columns contain the target and prediction – or name them accordingly. For binary classification, you must also set the positive label (unless it's called "1"). In this case, the positive class is "agent," since you predict which requests to route to a human.


Here are the results. Everything looks good:


But accuracy isn't always the best metric, especially if the classes in your dataset aren't balanced or the costs of errors are different.[Precision and recall](https://www.evidentlyai.com/classification-metrics/accuracy-precision-recall) might be more useful. For instance, you may expect all fraud-related queries to go to human agents. You don't want an LLM to handle these when it should escalate instead: this is the type of error to avoid.


If that is the case, **recall** could be a good metric. It helps check that you correctly identified all the instances of the (more risky) positive class.


If you run the **` TestRecallScore`** with the same 0.9 condition, the test fails:


You can interpret the results by looking at the[confusion matrix](https://www.evidentlyai.com/classification-metrics/confusion-matrix) . This shows two false negatives—when the system incorrectly predicted that a question should be handled automatically rather than routed to an agent. There was only one opposite error.


These predictive problems, like classifying queries, are often part of larger LLM-powered products, so it's good to check if you can test them as such.


Still, most applications are open-ended: what else can you test for them?


### Test text length


Here is a simple example: checking how long the chatbot responses are. You can count symbols, words or sentences.


Length is often a limitation. You might need responses to fit neatly within a chat window or adhere to a style preference where the bot should stay concise and to the point.


Imagine you have a strict rule that all responses should be from 50 to 180 symbols. You can test this using the **` TextLength`** descriptor. The good news is that you don't even need the golden answer. Run the condition against the new response column:


```text
test_suite = TestSuite(tests=[
TestValueRange(
column_name=TextLength(
display_name="Response Length").
on("new_response"),
left=50, right=180),
])


```


Great! All our responses are within the limits:


You might also set only one boundary. Maybe you want all responses to be 150 symbols or shorter. You can test this by setting a maximum value:


```text
test_suite = TestSuite(tests=[
TestColumnValueMax(
column_name=TextLength(
display_name="Response Length").
on("new_response"),
lte=150),
])


```


In this case, the test fails since there is one outlier response that is too long.


### Test competitor mentions


Let's consider another scenario: ensuring your chatbot doesn’t talk about competitors. Suppose your company is "OurFinCo," and your competitors are "AnotherFinCo" and "YetAnotherFinCo." While it’s unlikely that the bot will bring them up on its own, it might happen if users ask about them.


To test this, you can create a dataset with questions that probe about competitors.


You could test against reference responses using semantic similarity, but there's a simpler, faster way that doesn’t need a golden example: use a **regular expression** with a list of competitor brands to see if they come up.


Regular expressions might seem tricky, but there are ready-made descriptors you can use. The **` Contains`** descriptor lets you define a list of items and see if they are present in the text. Each text gets a True/False result for this pattern.


You can run a test using **` TestCategoryCount`** and set the condition to be equal to 0, meaning you don't expect any competitor mentions:


```text
test_suite = TestSuite(tests=[
TestCategoryCount(
column_name=Contains(
items=["AnotherFinCo", "YetAnotherFinCo"],
display_name="Competitor Mentions").
on("new_response"),
category=True,
eq=0),
])


```


Let’s run this check against the new dataset with questions about competitors. The test failed:


There are two instances where the chatbot added unexpected commentary on why our products are better:


You can use regular expressions in lots of ways: for example, to test that the chatbot always greets the users, includes required disclaimers, or provides links in responses.


But what about more complex conditions?


### Test toxicity


One popular method involves treating the[LLM as a judge](https://www.evidentlyai.com/blog/llm-as-a-judge-tutorial) . Here, you feed the chatbot's responses back into the LLM (yes, again!) with a specific prompt designed to classify the responses into predefined categories. It's a pretty neat trick and deserves a separate blog, given all the nuances. It has downsides, too: speed, cost, and reliance on external LLM.


The good news is that you don't always need fancy LLMs for this. There are plenty of narrow classification models out there, and many of them are freely available under open-source licenses. Let’s try some of them out.


You can run a built-in Evidently descriptor that tests for the toxicity of the generated text. We don't really expect the chatbot to return hate speech, but it is always good to check.


You can now unite all previous examples into a single dataset with **` misc_questions`** :


```text
misc_questions = pd.concat([typical_questions, agent_questions, competitor_questions], ignore_index=True)


```


On the backend, the **toxicity** test uses a model from HuggingFace. The check downloads the model and scores the data in your environment.


This model returns a predicted score from 0 to 1. Since you don’t expect any toxicity, you can set the threshold to 0.05. If any response has a larger predicted score, the test will fail:


```text
test_suite = TestSuite(tests=[
TestColumnValueMax(
column_name=HuggingFaceToxicityModel(
display_name="Toxicity").
on("new_response"),
lte=0.05),
])


```


Nothing to worry about: the predicted scores are all about 0.


### Test sentiment


Let's try a more interesting check – text sentiment. You might often care about it. The obvious thing to look out for is negative sentiment in responses. Or, perhaps you want to avoid overly optimistic comments, preferring the chatbot to stay neutral.


The built-in **` Sentiment`** descriptor uses a model from the NLTK library. It returns a score from -1 for very negative to 1 for very positive. Let’s test if the score stays above 0:


```text
test_suite = TestSuite(tests=[
TestColumnValueMin(
column_name=Sentiment(
display_name="Sentiment").
on("new_response"),
gte=0),
])


```


As it turns out, there are responses with a negative sentiment:


Let's take a closer look at them:


It seems that denying to respond or saying that a feature does not exist is seen as somewhat negative. Makes sense!


Depending on your scenario, you can frame this test differently. You might indeed want to review every response flagged as negative – you never know. Another approach is to set some acceptable fail rate – you only want to look if there are too many.


Let's set this fail rate at 10%. If over 10% of responses have negative sentiment, it's worth looking. Otherwise, you are good to go.


To define it, use the **` TestShareOfOutRangeValues`** , set **` left`** and **` right`** boundaries for the expected range, and use **` lte`** as a condition:


```text
test_suite = TestSuite(tests=[
TestShareOfOutRangeValues(
column_name=Sentiment(
display_name="Sentiment").
on("new_response"),
left = 0, right = 1,
lte = 0.1),
])


```


In this case, the test will pass:


### Test neutrality


Let's try something different—testing the emotional tone of responses. There's a[model on HuggingFace](https://huggingface.co/SamLowe/roberta-base-go_emotions) that classifies text into 27 predefined emotions. It could be fun to try in production, especially for scoring user requests!


But for the chatbot responses, you don't expect much emotion. Let's focus on the "neutral" one. This model is a multi-class classifier returning a score between 0 and 1 for each label. Typically, a score of 0.5 or higher means the text belongs to that category. To run the test to detect non-neutral responses, you need to reverse it.


Let the test fail if there are any examples with predicted probability below 0.5:


```text
test_suite = TestSuite(tests=[
TestColumnValueMin(
column_name=HuggingFaceModel(
model="SamLowe/roberta-base-go_emotions",
params={"label": "neutral"},
display_name="Neutrality",
).on("new_response"),
gte=0.5
),
])


```


Notice that you’re now directly referencing a model available on HuggingFace by its name. This approach lets you use many other ready-made models.


Here is the result of the check. While most of the responses are neutral, some are not.


Let’s take a look at those:


Denying the answer and expressing a preference for the company's products are labeled non-neutral. This test helped surface both errors from competitors' checks without explicitly defining them. That's an interesting outcome!


This check is, of course, illustrative. You might find other models more beneficial for your scenarios.


Importantly, you can also combine tests into a single Test Suite. For example, you could bundle emotion and sentiment checks together, setting a 10% acceptable pass rate for each:


### Auto test conditions


Setting specific pass or fail conditions can sometimes be tricky. How do you know which exact condition to set, especially for an open-ended test?


One way to handle this is by learning conditions from your chosen reference data. For checks like text length, you can use your golden dataset.


You need to split your data into two datasets: **current** and **reference** . Each must have the same set of columns and column names.


Then, pass these datasets to your Test Suite. If you don’t set a condition, Evidently will come up with a default: usually within +/-10% of the reference. No need to set anything explicitly.


For example, to check that your text length is within +/-10% of the reference, run this:


```text
test_suite = TestSuite(tests=[
TestValueRange(column_name=TextLength().on("response")),
])


test_suite.run(reference_data=ref, current_data=cur)
test_suite


```


This test fails on a combined dataset. You can see the learned range and spot 3 outliers:


This trick works even if your reference dataset has responses to entirely different questions! If they are representative in length, feel free to pass them as an example.


Another interesting method is[distribution drift detection](https://www.evidentlyai.com/blog/evidently-data-quality-monitoring-and-drift-detection-for-text-data) . It helps if you're looking for shifts in distribution rather than individual outliers. For instance, you might want to notice if all texts become longer or if there are now two large groups: "very long" and "very short" instead of all being “average.” Individual statistics might not always catch such shifts.


Here's how you can run drift detection. With a small number of examples, the default method is a sensitive[K-S](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets) test. This statistical test will fail if the P-value is below 0.05:


```text
test_suite = TestSuite(tests=[
TestColumnDrift(column_name=TextLength().on("response")),
])


test_suite.run(reference_data=ref, current_data=cur)
test_suite


```


In this case, the test passes. The distributions are overall similar, and there are not enough examples to prove otherwise.


## Takeaways


In this tutorial, we explored quite a lot of ways to test the quality of LLM-generated outputs using regular expressions, semantic similarity tests, and model-based evaluation. You can experiment with adapting these for your data.


Here are some learnings to keep in mind:


- **Build a test case bank** . You need examples to test against: start collecting them sooner rather than later. Consider both typical scenarios and corner cases.
- **Start simple.** Begin with general tests like semantic similarity to check for differences in responses. As you notice patterns, add more custom validations.
- **Define a fail rate** . You don’t always need all the checks to pass. You can run tests to keep "failures" under a certain threshold.
- **Automate the process** . While looking at individual test results can help you understand thresholds, the core value is automation. Add regression testing to your CI/CD pipeline, run test suites after making changes, and set the system to notify you if tests fail.


As you run checks over and over, you may also want to visualize the test results over time to see progress. Check out[part 2 of the tutorial](https://www.evidentlyai.com/blog/llm-testing-tutorial) to learn how to create a dashboard to track the results over time.


##### \[fs-toc-omit\] **Get started with AI observability**


> Try our open-source library with over 25 million downloads, or sign up to Evidently Cloud to run no-code checks and bring all the team to a single workspace to collaborate on AI quality.[Sign up free ⟶](https://www.evidentlyai.com/register)[Or try open source ⟶](https://github.com/evidentlyai/evidently)
