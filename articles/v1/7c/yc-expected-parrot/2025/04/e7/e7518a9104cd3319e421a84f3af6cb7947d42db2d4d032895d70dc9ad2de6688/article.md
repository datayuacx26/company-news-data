---
schema_version: "1.0.0"
document_id: "e7518a9104cd3319e421a84f3af6cb7947d42db2d4d032895d70dc9ad2de6688"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/what-are-my-options-for-running-edsl"
published_at: "2025-04-09T13:31:35+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:838d74cfb09ea5460a8b1d8c4ed7abb94e6899bac3e1a07a1606281f395cc813"
---

# What are my options for running EDSL surveys?

# What are my options for running EDSL surveys?


### Here's a quick recap of features for specifying where and how to run your LLM-based surveys in EDSL.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


Apr 09, 2025


This posts highlights features for choosing


*where* to run your EDSL surveys—on your own computer or at the Expected Parrot server—and


*how* to run them—using your own API keys for language models and/or an Expected Parrot key that provides access to all available models.


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


## Choosing


*where* to run your surveys


EDSL is an


[open-source Python package](https://github.com/expectedparrot/edsl) that lets you conduct surveys with language models either “locally” on your own computer or “remotely” at the Expected Parrot server.


### Getting started


The first steps are the same for either option:


1.


**Install the EDSL package.** See


[instructions](https://www.expectedparrot.com/getting-started) .


2.


**Create a survey.** See our


[starter tutorial and other examples](https://docs.expectedparrot.com/en/latest/starter_tutorial.html) . You can also use our


[GPT for generating EDSL code](https://chatgpt.com/g/g-67d17648ea5481919004bfc0bb1c2a8e-expectedparrot) .


Then decide whether you want to run your survey locally or remotely.


Please


[see our docs page](https://docs.expectedparrot.com/) for tips on


[getting started](https://docs.expectedparrot.com/en/latest/getting_started.html) and demo notebooks with example code for many use cases.


### Activate remote services


To active remote services, start by


[logging into your account](https://www.expectedparrot.com/login) and navigating to your


[Settings](https://www.expectedparrot.com/home/settings) page. Remote inference is activated by default when you create your account:


While the toggle is on, you can run surveys remotely simply by calling the


` run()` method on a survey at your workspace:


A table of information about the status of the survey job will appear while it is running, with a link to a progress report. When the job is completed, a link to view the results at Coop will appear as well:


You can also access and analyze the results directly in your workspace.


To run surveys locally instead you can either turn the toggle off or pass a parameter


` disable_remote_inference=True` to the


` run()` method:


### What happens when you use remote or local inference


When you run a survey


**locally** , your prompts are sent to language models from your computer and responses are delivered back to your computer.


When you run a survey


**remotely** , your prompts are sent from and responses are delivered back to the Expected Parrot server. Details on the survey job and results are automatically added to your Coop account, where you can choose whether to share them and how you want to work with them. Prompts and responses are also added to the


[universal remote cache](https://www.expectedparrot.com/cache-stats) . This means that if you re-run any prompts in the survey, or share the survey with anyone else, you will have the option to retrieve the stored responses at no cost. You can also specify that you want to generate fresh responses instead.


The


[universal remote cache](https://www.expectedparrot.com/cache-stats) lets you retrieve stored responses for free. Learn more about


[how it works](https://docs.expectedparrot.com/en/latest/remote_caching.html) .


## Choosing


*how* to run your surveys


EDSL


[works with many popular language models](https://docs.expectedparrot.com/en/latest/language_models.html) you can choose from to generate responses to your surveys, including models from Anthropic, Azure, Bedrock, Deep Infra, DeepSeek, Google, Groq, Mistral, Ollama, OpenAI, Perplexity, Together and Xai. You can check available models, token prices and daily performance on test questions


[here](https://www.expectedparrot.com/models) :


When you run a survey remotely, you can connect to language models using your own API keys from service providers or using a key from Expected Parrot that provides access to all available models.


*Note:* You must provide your own keys when running surveys locally.


### How it works


Your Expected Parrot key is accessible at your


[Keys](https://www.expectedparrot.com/home/keys) page, where you can reset it at any time:


If you have other keys that you want to use when running surveys


**remotely** , you can select the


**“Add key”** button to add them. You can deactivate and delete them at any time:


You can also choose whether to grant access to your keys to other users (without sharing keys directly) and set usage limits.


If you are running surveys


**locally** , you must store your keys on your computer. We recommend doing this using a


` .env` file. See


[instructions](https://www.expectedparrot.com/getting-started/edsl-api-keys) on managing your keys.


### Your Expected Parrot key also provides access to Coop


Your Expected Parrot key also lets you connect to


[Coop](https://www.expectedparrot.com/content/explore) : an integrated platform for creating and sharing AI-based research:


Your account lets you post, download and share content, including surveys, agents, results and notebooks. When you run surveys


**remotely** , results and survey job details are automatically available at your Coop account, where you can choose whether to publish and share them privately with team members or publicly. Results that are generated remotely will also show a verified ✔️ checkmark for


[reproducibility](https://docs.expectedparrot.com/en/latest/remote_caching.html) .


You can also post any other content from your workspace to Coop, whether created locally or remotely. Learn more about


[methods for working with Coop](https://docs.expectedparrot.com/en/latest/coop.html) .


*Thanks for reading! Subscribe for free to receive new posts and support our work* .


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
