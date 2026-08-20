---
schema_version: "1.0.0"
document_id: "3311df1324c2d757d52499dc2acad8b4ce9c7ae9cacfdd063972ac9ca2e41788"
company_key: "yc-vibrant-labs"
company: "Vibrant Labs"
source_id: "yc-vibrant-labs-news-import-1f38412d73f2"
canonical_url: "https://vibrantlabs.com/research/ragas/aligning-llm-as-judge-with-human-evaluators"
published_at: "2024-12-11T00:00:00+00:00"
first_seen_at: "2026-07-27T06:02:47.548224+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:abbfdd041e0e18eb8e2c11e14e4b2e625a863f19c9e99290d08b057a6ef2c646"
---

# Aligning LLM as judge with human evaluators

# Introduction


Let’s face it, your AI product evals are most probably broken. In this blog, I will explain you why it’s broken, how you can fix it and the underlying mechanism that makes the solution work.


## The rise and fall of LLM based evaluators


As LLMs like GPT3.5 came out and got wide adoption in early 2023, more AI applications than ever were getting built. At one point or another in this product development journey every AI engineer and stakeholder realised that they need evaluations to further scale their AI product efficiently. This created a vacuum, the reason for this vacuum was the open question how do one evaluate such systems?


The traditional set of evaluation metrics like string match, bleu score, etc were proving to be ineffective against this new found generative and reasoning capability of LLMs. The introduction of the concept that LLMs can be used as evaluators tried to fill this vacuum. Early works like[G-Eval](https://arxiv.org/abs/2303.16634) and[GPTScore](https://arxiv.org/abs/2302.04166) empirically prove that LLMs can be effectively used as evaluators to score outputs. Later in the year, as some of the biases of using LLM to directly score outputs were discovered works like[RAGAS](https://arxiv.org/abs/2309.15217) (our MVP) tried to bypass it by formulating different set of metrics for evaluation.


This went on for a while, until works like[who validates the validator](https://arxiv.org/abs/2404.12272) , L[LMs instead of human judges?](https://arxiv.org/abs/2406.18403) started questioning the validity of all of the above methods. Similar works challenging the effectiveness of using LLMs as evaluators detected a difference in alignment with LLMs and human evaluators as the main cause of failure. To understand this, we need to first understand what is alignment in evaluation.


###### Mis-alignment between LLM and Human evaluator


There is always some form of subjectivity in evaluations which varies depending on the domain, product, etc. The developer who works on it knows and understand this and evaluates the outputs from this perspective, but when the task is automated using an LLM it fails to meet expectations because it simply does not have the context that the developer or human evaluator had when evaluating outputs. This is depicted in the simple example shown above. This is why most evals fail. So, how can you fix it?


## Aligning LLM as evaluators with human evaluators


While working in ragas, we have encountered this problem occurring to users on day today basis. Now the question is how to solve this? This problem from a model research perspective is a classic alignment problem. Now to align the model (means make model reason the way you prefer), there is gradient based (finetuning, DPO, etc) and gradient free (prompt, few shot optimisation) approaches. We went for the latter approach because it’s cheap, requires far less data and works amazingly well. Let’s understand how to align metrics with ragas.


**Step 1:** Select the required LLM based metrics and run evaluations


```text
critic   =  AspectCritic  (  name  = "answer_correctness"  ,  definition  = "Given the user_input, reference and response. Is the response correct compared with the reference"  ,  llm  = llm  )
results   =  evaluate  (  eval_dataset  ,  metrics  = [  critic  ]  )
```


```text
results   =  evaluate  (  eval_dataset  ,  metrics  = [  critic  ]  )
```


**Step 2** : Review your evaluation results in[app.ragas.io](https://app.ragas.io/) and give feedbacks (at least 15-20)


```text
results  . upload  (  )
```


```text
results  . upload  (  )
```


###### Reviewing and annotating evaluation results


**Step 3** : Automatically train and align your evaluators with the collected data


```text
critic  . train  (  path  = "feedback_data.json"  )
```


```text
critic  . train  (  path  = "feedback_data.json"  )
```


Once your evaluators are auto-aligned by ragas, you may use it again on larger evaluation datasets. As you do it you will see that your evaluators are now much more aligned to your judgement. You can continue this workflow of evaluation→review→align and your evaluators are going to become better each time. To start check out this[documentation](https://docs.ragas.io/en/stable/howtos/customizations/metrics/train_your_own_metric/) .


And, that’s how you fix your evals!


## How does it work?


Since ragas is open source, you can actually see how all of these works. Here I’ll try to briefly explain some our research, experiment and results on this. Fundamentally, what happens underneath is prompt optimisation ( when I say prompt, it means instruction and demonstrations). In our case, an LLM based metric may have one or more prompt forming an LLM chain.


Open-source projects like[DSPy](https://dspy.ai/) ,[Textgrad](https://textgrad.com/) ,[zenbase](https://github.com/zenbase-ai/core) , etc has contributed greatly for advancing this field. We also referred several other research publications including[PhaseEvo](https://arxiv.org/pdf/2402.11347) which inspired us to take a genetic algorithm based approach for solving this. Especially since there is no gradient or back propagation involved in optimising LLM chains compared to optimising neural networks.


We use a genetic algorithm based approach for optimising and aligning metrics. Every LLM based metric in ragas contains one or more prompts. The basic idea is to first create few candidates ( a candidate is a version metric containing unique combination of prompts), then use evolutionary methods like mutation to create more varieties, followed by selection ( done by assessing the accuracy of each candidate against collected data) to yield the best set of prompt that work for the given metric and data.


For optimising demonstrations, we have come to the conclusion that there is no one set of demonstrations that fits every input. So, for this we embed the reviewed and accepted samples and retrieve the top k similar ones during inference.


We evaluated the effectiveness on our internal dataset (median response length 100 tokens). I have briefly described our process, you could also do the same if you have an annotated evaluation dataset containing expected scores for at least 50 or more samples for the task you want to evaluate.


-


Split the data into train and test.


-


Select a LLM based metric for your task.


-


Run evaluation with the selected metric on your both train and test splits.


-


For the test set, quantify the performance of the evaluation metric by comparing predicted scores vs expected scores. This will be your baseline performance.


-


For the train set, upload the results to[app.ragas.io](http://app.ragas.io/) to review and annotate the evaluations.


-


Download this annotated data and then train the metric using it.


-


Re-evaluate on the test split using trained metric and quantify the performance again. This is performance on the optimized metric.


-


Compare between baseline score vs optimized metric score to quantify the improvement.


We reviewed and annotated a random set of 20 samples ( 12 accepted, 8 rejected and 4 edited) as train and then evaluated it’s against 50 test samples. We used aspect critic metric (binary) combined with GPT-4o model for this experiment.


###### From the results, we were able to observe a clear improvement on the alignment of the evaluation metric with human evaluator. You can view these results in the table shown above, where the default settings achieves the lowest f1 score.


## Conclusion


If you think this was useful,[try it out](https://docs.ragas.io/en/stable/howtos/customizations/metrics/train_your_own_metric/) yourselves and let us know your feedback. To support us, star us on[Github](https://github.com/explodinggradients/ragas) and follow us on[X](https://x.com/ragas_io) to get updates on the work we are doing on improving evals.


If you found this useful, please cite this write-up as:


```text
@ article  {  ragas2024alignmetric  ,
title     =  {  Aligning   LLM  as    judge    with    human    evaluator  }  ,
author    =  {  Shahul  }  ,
journal   =  {  blog  . ragas  . io  }  ,
year      =  {  2024  }  ,
month     =  {  Dec  }  ,
url       =  {  https  :// blog  . ragas  . io  / aligning  - llm  - as  - judge  - with  - human  - evaluators  }
}
```


```text
@ article  {  ragas2024alignmetric  ,
title     =  {  Aligning   LLM  as    judge    with    human    evaluator  }  ,
author    =  {  Shahul  }  ,
journal   =  {  blog  . ragas  . io  }  ,
year      =  {  2024  }  ,
month     =  {  Dec  }  ,
}
```


#### Citations


1.


[DSPy](https://dspy.ai/)


1.


[TextGrad](https://textgrad.com/)


1.


[PhaseEvo](https://arxiv.org/pdf/2402.11347)


1.


[A comparative study of DSPy Teleprompters](https://arxiv.org/abs/2412.15298)


1.


[Choosing threshold for LLM evaluators](https://arxiv.org/abs/2412.12148)
