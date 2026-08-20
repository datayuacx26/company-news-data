---
schema_version: "1.0.0"
document_id: "9c0b2a5ee04d3dfa3acb74ba81e7f439ae9acbd29cedc5d1abdae57e04866ce1"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/hill-climbing-llm-summarization/label-data-and-improve-llm-apps"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:f4275b4d496b915518ad0d7b6fb2af0d34e3388a603a7c81f82394c6c93dd407"
---

# Hill climbing generative AI problems

[Joschka Braun](https://joschkabraun.com/) on Aug 23, 2024


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


For many generative AI applications it is expensive to create ground truth answers for a set of inputs (e.g. summarization tasks). This makes experimentation slow as you can’t even run a LLM eval assessing if the output matches the ground truth. In this guide, you will learn about how to quickly experiment with your LLM app as you still figure out your data.


In such scenarios, you want to split your experimentation process into two hill climbing phases with different goals. The term[hill climbing](https://en.wikipedia.org/wiki/Hill_climbing) is inspired by the numerical optimization algorithm of the same name which starts with an initial solution and iteratively improves upon it. Concretely:


1. Hill climb your data : Iterate on your application to understand your data & **find ground truth values/targets** .
2. Hill climb your app : Iterate on your application to **find a compound system fitting all targets** .


While your ultimate goal is to increase the “accuracy” of your LLM app (a lagging indicator), you will get there by maximizing learnings, i.e., running as many experiments as possible (a leading indicator). Read more about[focusing on leading metrics](https://jxnl.co/writing/2024/08/19/rag-flywheel/#2-focus-on-leading-metrics) by[Jason Liu](https://jxnl.co/) .


## ​


Phase 1: Hill climb your data


Your goal in this phase is to find the best ground truth values/target for your data. You do that by iterating on your LLM app and judge if the new the outputs are better, i.e. you **continuously label your data** .


So, taking the example of summarization. To have *some* ground truth values, you can use a simple version of your LLM app on your unlabeled dataset to generate initial summaries. After manually reviewing your outputs, you will find some failure modes of the summaries (e.g. they don’t mention numbers). Then, you tweak your LLM system to incorporate this feedback and generate a new round of summaries.


Now you are getting into hill-climbing mode. As you compare the newly generated summary with the ground truth summary (the previous one) for every sample, update the ground truth summary if necessary. During that pairwise comparison, you will get insights into the failure modes of your LLM app. You will then update your LLM app to address these failure modes, generate new summaries, and continue hill-climbing your data. You can stop this phase once you don’t improve your summaries anymore. Summarizing in a diagram:


Hill climb your data


How do you keep track of the best version of your LLM app? While this process does not entail a direct comparison between different iterations of the LLM app, you can still get a sense of it. You can use the pairwise comparisons between the new and ground truth summaries to score any item in your experiments with +1, 0 or -1, depending on if the new summary is better, comparable or worse than the ground truth one. With that information you can approximately assess which experiment is closest to the ground truth summaries.


This process is akin to how the training data for Llama2 were created. Instead of writing responses for supervised finetuning data ($3.5 per unit), pairwise-comparisons ($25 per unit) were used. Watch Thomas Scialom, one of the authors, talk about it[here](https://www.youtube.com/watch?v=CzR3OrOkM9w) .


## ​


Phase 2: Hill climb your app


In this phase, you focus on creating a[compound AI system](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/) which fits all targets/ground truth values at the same time. For that you need to be able to measure how closely your outputs are to the ground truth values. While you can assess their closeness by manually comparing outputs with targets, LLM-based evals come in handy to speed up your iteration cycle.


You will need to iterate on your LLM evals to ensure they are aligned with human judgement. As you manually review your experiment results, measure the alignment with your LLM eval. Then tweak the eval to mimic human annotations. Once, there is good alignment (as measured by Cohen’s kappa for categorical annotations or Spearman correlation for continuous judgement), you can rely more on the LLM evals and less on manual review. This will unlock a faster feedback loop. Those effects will be even more pronounced when domain experts such as lawyers or doctors manually review responses. Before any major release, you should still have a human-in-the-loop process to verify quality and to assess the correctness of your LLM evals.


Note, you may find better ground truth values during manual review in this phase. Hence, dataset versioning becomes important to understand if any drift in evaluation scores is due to moving targets.


## ​


Continuous improvement


Once you have data with good ground truth values/targets and an application which is close to those targets, you are ready to launch the app with your beta users. During that process, you will encounter failure cases which you haven’t seen before. You will want to use those samples to improve your application.


For the new samples, you go throughPhase 1 followed byPhase 2 . Whereas for the previous samples in your dataset, you continue with Phase 2 as you tweak your application to fit the new data.


## ​


How does Parea help?


You can use Parea to[run experiments](https://docs.parea.ai/welcome/getting-started-evaluation) , track ground truth values in[datasets](https://docs.parea.ai/platform/test_hub/overview) , review &[comment on logs](https://docs.parea.ai/blog/hill-climbing-llm-summarization/manual-review/logs-view#commenting-on-logs) , and compare experiment results with ground truth values in[a queue](https://docs.parea.ai/manual-review/queue#viewing-the-queue) duringPhase 1 . ForPhase 2 , Parea helps by[tracking the alignment](https://docs.parea.ai/evaluation/overview#investigating-relationship-between-statistics) of your LLM evals with manual review and[bootstraping LLM evals from manual review data](https://docs.parea.ai/blog/self-improving-domain-specific-alligned-llm-evals-with-human-judgement) .


## ​


Conclusion


When ground truth values are expensive to create (e.g. for summarization tasks), you can **use pairwise comparisons of your LLM outputs to iteratively label** your data as you experiment with your LLM app. Then, you want to **build a compound system fitting all ground truth values** . In that later process, **aligned LLM-based evals are crucial** to speed up your iteration cycle.
