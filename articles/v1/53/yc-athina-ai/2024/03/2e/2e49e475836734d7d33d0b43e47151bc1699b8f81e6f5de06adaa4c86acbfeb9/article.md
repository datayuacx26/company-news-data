---
schema_version: "1.0.0"
document_id: "2e49e475836734d7d33d0b43e47151bc1699b8f81e6f5de06adaa4c86acbfeb9"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/detect-llm-hallucinations-in-ci-/-cd-evaluate-your-rag-pipeline-using-github-actions-athina-/-ragas"
published_at: "2024-03-05T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:13.235024+00:00"
content_hash: "sha256:384bd905b7d9dbaba1e1748242c11b210fe228b682528fb01bf0d25867b1bfce"
---

# Detect LLM Hallucinations in CI / CD: Evaluate your RAG pipeline using GitHub Actions + Athina / Ragas

Do not index


Original Paper


Blog URL


If you've ever worked on coding projects, you know how important it is to make sure your code is solid before showing it to the world.


That's where CI/CD pipelines come into play. They're like your coding safety net, catching bugs and problems automatically.


So why not have the same process for your LLM pipeline?


The best teams will implement an evaluation system as part of their CI / CD system for their RAG pipelines.


This makes a lot of sense - LLMs are unpredictable at best, and tiny changes in your prompt or retrieval system can throw your whole application out of whack.


Athina can help you detect mistakes and hallucinations in your RAG pipeline your code's quality with a really simple integration. We're going to walk you through how to set this up using GitHub Actions.


---


**You can use Athina evals in your CI/CD pipeline to catch regressions before they get to production.**


Here is a guide for setting athina-evals in your CI/CD pipeline.


All code described here is also present in our[GitHub repository](https://github.com/athina-ai/athina-evals-ci/) .


###


GitHub Actions


We're going to use GitHub Actions to create our CI/CD pipelines. GitHub Actions allow us to define workflows that are triggered by events (pull request, push, etc.) and execute a series of actions.


Our GitHub Actions are defined under our repository's` .github/workflows` directory.


We have defined a workflow for the evals too. The workflow file is named` athina_ci.yml` .


The workflow is triggered on every push to the` main` branch.


```text
name: CI with Athina Evals


on:
push:
branches:
- main


jobs:
evaluate:
runs-on: ubuntu-latest


steps:
- uses: actions/checkout@v3


- name: Set up Python
uses: actions/setup-python@v4
with:
python-version: '3.9'


- name: Install Dependencies
run: |
python -m pip install --upgrade pip
pip install -r requirements.txt  # Install project dependencies
pip install athina  # Install Athina Evals


- name: Run Athina Evaluation and Validation Script
run: python -m evaluations.run_athina_evals
env:
OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
ATHINA_API_KEY: ${{ secrets.ATHINA_API_KEY }}
```


####


Athina Evals Script


The` run_athina_evals.py` script is the entry point for our Athina Evals. It is a simple script that uses the Athina Evals SDK to evaluate and validate the Rag Application.


For example we are testing if the response from the Rag Application answers the query using the` DoesResponseAnswerQuery` evaluation from athina.


```text
eval_model = "gpt-3.5-turbo"
df = DoesResponseAnswerQuery(model=eval_model).run_batch(data=dataset).to_df()


# Validation: Check if all rows in the dataframe passed the evaluation
all_passed = df['passed'].all()


if not all_passed:
failed_responses = df[~df['passed']]
print(f"Failed Responses: {failed_responses}")
raise ValueError("Not all responses passed the evaluation.")
else:
print("All responses passed the evaluation.")
```


You can also load a golden dataset and run the evaluation on it.


```text
with open('evaluations/golden_dataset.jsonl', 'r') as file:
raw_data = file.read().split('\n')
data = []
for item in raw_data:
item = json.loads(item)
item['context'], item['response'] = app.generate_response(item['query'])
data.append(item)
```


You can also run a suite of evaluations on the dataset.


```text
eval_model = "gpt-3.5-turbo"
eval_suite = [
DoesResponseAnswerQuery(model=eval_model),
Faithfulness(model=eval_model),
ContextContainsEnoughInformation(model=eval_model),
]


# Run the evaluation suite
batch_eval_result = EvalRunner.run_suite(
evals=eval_suite,
data=dataset,
max_parallel_evals=2
)


# Validate the batch_eval_results as you want.
```


####


Secrets


We are using GitHub Secrets to store our API keys.


We have two secrets,` OPENAI_API_KEY` and` ATHINA_API_KEY` .


You can add these secrets to your repository by navigating to` Settings` >` Secrets` >` New repository secret` .


####


Further reading


We have more examples and details in our[GitHub repository](https://github.com/athina-ai/athina-evals-ci/)


---


Alright, we've covered how to add Athina to your CI/CD pipeline with GitHub Actions - with this simple modification, you can ensure your AI is top-notch before it goes live.


If you're interested in continuous monitoring and evaluation of your AI in production, we can help.


Watch this[demo video](https://bit.ly/athina-demo-feb-2024) of Athina's platform, and feel free to[schedule a call with us](https://cal.com/shiv-athina/30min) if you're interested in setting up safety nets for your LLM.
