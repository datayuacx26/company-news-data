---
schema_version: "1.0.0"
document_id: "9b01af1ffa5465175c4cc3dc45bb360ad455ef3104014552193d4e4298f3e5ce"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/validate-your-llm-answers-with-real"
published_at: "2025-05-25T00:06:24+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:91f670bbdce4fc76c6ddd07c2dcdd05e554ffcf7ac1e5df4916330184806d792"
---

# Validate your LLM answers with real respondents

# Validate your LLM answers with real respondents


### Here's a quick example of methods for generating a web-based version of your LLM survey and analyzing human and LLM responses together at your workspace.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


May 25, 2025


Sometimes it’s helpful to run your LLM-based survey with some real respondents. You can do this in EDSL using built-in methods for auto-generating a web-based version of your LLM survey and comparing human and LLM responses. Code for the quick example below is available in this


[downloadable notebook](https://www.expectedparrot.com/content/RobinHorton/human-results-example-notebook) at Coop, our platform for creating and sharing AI research.


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


## 1. Create & run a survey in EDSL


Start by constructing questions in EDSL, our


[open-source package](https://github.com/expectedparrot/edsl) for running surveys and experiments with AI agents and LLMs. Choose from many


[common question types](https://docs.expectedparrot.com/en/latest/questions.html) based on the form of the response that you want to get back from a model. You can optionally


[design personas for AI agents](https://docs.expectedparrot.com/en/latest/agents.html) to answer the questions, and


[specify which of many popular LLMs](https://docs.expectedparrot.com/en/latest/language_models.html) you want to use to generate the responses. Run the survey by adding the agents and models to the survey and calling the


` run()` method. This generates a formatted dataset of results that you can


[analyze with built-in methods](https://docs.expectedparrot.com/en/latest/results.html) :


When you run your survey at Expected Parrot your results are automatically stored at your


[Coop account](https://docs.expectedparrot.com/en/latest/coop.html) where you can access and share them. You can check a progress report while the survey is running, and see details on costs for each response when it is done:


## 2. Inspect results


[Results](https://docs.expectedparrot.com/en/latest/results.html) include information each component of the survey job: questions, prompts, agents, models, raw responses, costs, etc. You can inspect results at your Coop account, and also use methods for analyzing them at your workspace. Here we select the responses, and the comments that are automatically added to them (learn more about


[modifying user and system prompts](https://docs.expectedparrot.com/en/latest/prompts.html) for your specific research needs):


## 3. Generate a web-based version of the survey


To collect actual human responses to your survey, call the


` humanize()` method on the


` Survey` object to generate a web-based version of it. You’ll get a link that can be shared with anyone you want to answer the survey, and another link for accessing responses at your Coop account (the


` admin_url` ):


You can also use our interactive survey builder tool to construct a different web-based survey, or to edit the one you generated from your EDSL survey (e.g., if you want to add some screener questions for your real respondents).


## 4. View & share the survey URL


Here I go ahead and answer the survey myself, as my response can offer a reliable check on the AI agent’s from above:


No problem!


## 5. Combine & analyze results


LLM and human results are datasets that you can analyze with


[built-in methods for working with results](https://docs.expectedparrot.com/en/latest/results.html) (e.g., to use them as inputs for follow-on questions for LLMs). You can also export them, e.g., as dataframes:


We’re always adding new methods like these — please send us your feature requests! Send us an email ([\[email protected\]](https://blog.expectedparrot.com/cdn-cgi/l/email-protection) ),


[post a message at our Discord](https://discord.com/invite/mxAYkjfy9m) or


[DM us on X](https://x.com/ExpectedParrot) .


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
