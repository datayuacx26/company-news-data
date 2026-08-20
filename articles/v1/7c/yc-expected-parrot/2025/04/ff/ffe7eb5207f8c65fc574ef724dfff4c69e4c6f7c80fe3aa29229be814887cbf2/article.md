---
schema_version: "1.0.0"
document_id: "ffe7eb5207f8c65fc574ef724dfff4c69e4c6f7c80fe3aa29229be814887cbf2"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/new-features-for-monitoring-your"
published_at: "2025-04-28T18:36:19+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:9d88eb636c00e1001dd71e2d8b7e52919fd1c7128827994b3159241b72e282d6"
---

# New features for monitoring your AI research costs

# New features for monitoring your AI research costs


### Our tools are designed to make it easy to understand and monitor your LLM usage. Learn more about our latest features for precisely tracking your costs.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


Apr 28, 2025


Running experiments with language models can be fast and cheap. But you still want to know what all you’re spending on API calls, which models are cost-efficient for your research goals, how much it will cost to replicate or extend your work, etc. You also want to avoid making duplicative or unnecessary API calls, which can add up when you’re iterating on an experiment or working across teams.


When you run a research project with


[Expected Parrot](https://www.expectedparrot.com/) it’s easy to track the details of your LLM costs and usage, using


[built-in methods](https://docs.expectedparrot.com/en/latest/costs.html) at your workspace and at your account at


[Coop](https://www.expectedparrot.com/login) , a platform for creating and sharing LLM-based research. You can also


[replicate and share your results for free](https://docs.expectedparrot.com/en/latest/remote_caching.html) . Below we highlight some new and key features for easily and precisely tracking your costs.


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


## Before you run an experiment…


When you’re developing an LLM research project you want to know (i) the token rates for the models that you plan to use and (ii) estimated costs for your prompts and the responses that you will receive back from the models. You also want to avoid making duplicative API calls to LLMs (resending identical prompts when you do not expect the response to change) to eliminate unnecessary $$ charges.


## Checking model prices & capabilities


It can be time-consuming to gather token rates from various model service providers’ webpages, and to determine which models are capable and appropriate for your research goals and requirements (e.g., do you need a vision model for some of your prompts?).


This is why we created a


[model pricing and performance](https://www.expectedparrot.com/models) page where you can quickly check current token rates for models of all available service providers, and their recent performance on test questions:


The page is updated daily with details on the test questions that were used with each model.


*We are adding test questions that use videos! See [example code for using videos with your questions](https://www.expectedparrot.com/content/RobinHorton/video-scenarios-notebook) .*


## Estimating costs


Once you’ve drafted your user and system


[prompts](https://docs.expectedparrot.com/en/latest/prompts.html) (i.e., created your


[questions](https://docs.expectedparrot.com/en/latest/questions.html) and any


[agents](https://docs.expectedparrot.com/en/latest/agents.html) ), you can use


[built-in methods](https://docs.expectedparrot.com/en/latest/credits.html#estimating-job-costs) for estimating the costs of sending them to models (input tokens) and the costs of the responses that you will get back from the models (output tokens).


For example, here we create a simple question and then use a method for estimating input and output token costs of sending it to two different models. We can see the details also include the total credits that will be placed on hold while the survey job is running and then released when the actual cost is determined:


After a survey is run, we can verify the actual token costs for each model and question in the


[dataset of results](https://docs.expectedparrot.com/en/latest/results.html) that is generated:


You can also view token and cost details at your Coop account. Your


**[Jobs](https://www.expectedparrot.com/home/remote-inference)** page shows the final cost in credits of API calls to each model used with the survey:


Your (new!)


**[Transactions](https://www.expectedparrot.com/home/transactions)** page shows additional details about credits placed on hold based on job cost estimates together with the final cost details:


## Avoiding unnecessary API calls


A key feature of EDSL is


[automated caching](https://docs.expectedparrot.com/en/latest/remote_caching.html) of prompts sent to models and responses received from them. This ability to retrieve responses to questions that have previously been run at no cost means that you can replicate and share any research created in EDSL for free.


For example, if we rerun the example question from above we can confirm that no additional costs are incurred:


This can be useful when iterating on a research project and when publishing your research results for others to review and replicate.


###


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
