---
schema_version: "1.0.0"
document_id: "cb35aa12e38ea2ae7a0fbe0c70914c87f15707d147344ca98d1aaecc33672d03"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/i-want-to-see-how-openais-o3-compares"
published_at: "2025-04-22T21:37:01+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:18f97d60900e141ecd518877099d44c679ab509b8e62453132e623ba54b2c1df"
---

# I want to see how OpenAI's o3 compares to...

# I want to see how OpenAI's o3 compares to...


### EDSL is an open-source tool that lets you readily compare performance for many language models at once.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


Apr 22, 2025


Several key features of


[EDSL](https://docs.expectedparrot.com/en/latest) make it a convenient tool for comparing the performance of language models:


-


**Unified access** : Connect to multiple language model providers with a single API key and universal methods—no software engineering required


-


**Structured data collection** : Automatically formatted datasets of responses eliminate manual data cleaning work


-


**Analysis tools** : Built-in methods for analyzing, reproducing and exporting results


## How it works


EDSL is an


[open-source Python package](https://github.com/expectedparrot/edsl) designed to simplify conducting experiments with AI agents and language models. A typical workflow consists of the following steps:


-


Create


[questions](https://docs.expectedparrot.com/en/latest/questions.html) (free text, multiple choice, numerical, matrix, etc.)


-


Combine questions in


[surveys](https://docs.expectedparrot.com/en/latest/surveys.html) with custom logic (skip patterns, stop rules, etc.)


-


(


*Optional* ) Create personas for AI


[agents](https://docs.expectedparrot.con/en/latest/agents.html) to answer the survey


-


Select


[language models](https://docs.expectedparrot.com/en/latest/language_models.html) to generate the responses


-


Analyze


[results](https://docs.expectedparrot.com/en/latest/results.html) in specified datasets


## Simplified access to models


EDSL is designed for researchers who want to work with language models without spending lots of time on software development. It eliminates the need for one-off coding required to access individual service providers by letting you connect to them all in the same straightforward way.


In EDSL, you can access models individually or collectively by simply selecting them and specifying desired parameters. For example, here we create a list of OpenAI models in order to use them all simultaneously with a survey when we run it. We can inspect the default parameters for the models that will be used:


You can also create multiple instances of the same model with different parameters. Here we create a new model list where we adjust the temperature of a single model:


## Results as formatted datasets


Running a survey with multiple models at once allows us to readily compare responses in the dataset of results that is generated. Here we run a simple survey (consisting of a linear scale question and a numerical question—see


[examples of all question types](https://docs.expectedparrot.com/en/latest/questions.html) ) and inspect the models’ responses:


These results can be viewed at Coop as well:


Other columns of the


[results](https://docs.expectedparrot.com/en/latest/results.html) include:


-


User and system prompts


-


Question details


-


Agent traits and instructions


-


Model temperature and other settings


-


Raw and formatted responses


-


Log probs


-


Input and output tokens


-


Costs


## Works with many models


EDSL works with many other popular


[language models](https://docs.expectedparrot.com/en/latest/language_models.html) as well. You can check available models, current token prices and daily performance on test questions


[here](https://www.expectedparrot.com/models) :


*Please get in touch to request other service providers that you want to use!*


## Getting started


Our


[documentation page](https://docs.expectedparrot.com/en/latest) includes tutorials and demo notebooks for getting started using EDSL with language models, including examples of methods for


[evaluating model responses](https://docs.expectedparrot.com/en/latest/notebooks/models_scoring_models.html) .


*If you have a use case you don’t see, please see us a message and we’ll create a notebook for you!*


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
