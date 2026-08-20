---
schema_version: "1.0.0"
document_id: "f358a674c9fc558099493cbcecb37387d8e92b30377f76cf4c0f6b3b1ce9000d"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/do-i-have-time-for-this"
published_at: "2025-06-18T12:57:52+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:80c479011a5308826eeead6ade5226c9ab572ca122f84cfe8fc14b1ea11e5eb0"
---

# Do I have time for this?

# Do I have time for this?


### Our tools look technical and emphasize tinkering over quick answers. Here's why I still think you should try them.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


Jun 18, 2025


I recently presented our tools to a self-described non-technical team:


-


**[EDSL](https://github.com/expectedparrot/edsl)** , an open-source Python library for simulating surveys and experiments with AI agents and LLMs (


[read our docs](https://docs.expectedparrot.com/) ); and


-


**[Coop](https://www.expectedparrot.com/)** , an integrated platform for conducting hybrid AI/human research interactively (


[login/signup](https://www.expectedparrot.com/) ).


The team had heard about our tools and thought they could be helpful for gathering information about potential customers for startup ideas. But the presentation went sideways when I brought up a slide of EDSL code (and accompanying snapshot of our interactive survey builder tool at Coop) to show how to begin a research experiment-as-a-survey for LLMs:


The same first step at Coop (you can click “Code” to get the EDSL code, for technical and non-technical teams to collaborate):


The team was insistent that early-stage founders, particularly non-technical ones, don’t have time to learn a new tool for time-sensitive market research. In fairness, you can obviously get a ton of fast research done with user-friendly AI chat interfaces. But if you want to do any pressure testing of the answers that you’re getting from LLMs—


*What happens when I emphasize this part of the customer persona instead? What happens when I modify the description of the product, or rephrase the question, or try a different model, or provide more information? etc.* —it quickly becomes unwieldy to collect data and analyze feedback and ideas via unstructured text.


It also makes it hard to compare your AI results to any information that you’re able to collect from your actual target audience. In order to compare your LLM and human answers you’ll want a dataset—even a simple spreadsheet—with organized information about the personas that you included in your LLM prompts, the information that you have about your human respondents, and the answers that each of them provided to your questions. Your dataset should also show all the variations of the prompts that you sent to LLMs, which is only possible if you…


# Tinker with your prompts!


It’s not necessarily obvious what information you have—or do not have—about your target audience is relevant to the questions that you want to ask, or how its presentation in LLM prompts is impacting the responses. If you want to increase your confidence in your AI results you should tinker with your questions and AI personas and include humans in-the-loop when possible. EDSL is designed to make all of these things easy in a number of ways:


-


**Explore question types** —


*EDSL has many common [question types](https://docs.expectedparrot.com/en/latest/questions.html) (free text, multiple choice, etc.) to explore the impact of the presentation of a question on responses.*


-


**Test variants of your prompts** —


*EDSL has many methods for [auto-importing data](https://docs.expectedparrot.com/en/latest/scenarios.html) to efficiently run multiple versions of your questions, instructions and agent personas all at once.*


-


**Compare models** —


*EDSL works with many popular [service providers](https://www.expectedparrot.com/models) , and your account comes with a [key for accessing all of them](https://www.expectedparrot.com/getting-started) at once to easily compare performance on your tasks.*


-


**Validate with humans** —


*EDSL provides methods for [launching your surveys with humans](https://docs.expectedparrot.com/en/latest/humanize.html) to compare results and improve your workflows. Choose whether to send surveys to your own respondents or use our [Prolific integration](https://docs.expectedparrot.com/en/latest/prolific.html) .*


-


**Analytical tools** —


*Results—LLM and human—are returned as formatted datasets containing details about questions, agents, models, responses and costs that you can immediately begin analyzing with [build-in methods](https://docs.expectedparrot.com/en/latest/results.html) , or export* .


# EDSL is easy to learn.


*No,* you do not need to be any kind of coding expert or Python wiz to master EDSL. Our


[docs have demo notebooks](https://docs.expectedparrot.com/) for a variety of


[use cases](https://www.expectedparrot.com/use-cases) that you can download and modify to get started. You can also use


[Coop](https://www.expectedparrot.com/login) to create a project interactively and then get the EDSL code for it (click the “Code” button in the snapshot above), or use our


[GPT for generating EDSL code](https://chatgpt.com/g/g-67d17648ea5481919004bfc0bb1c2a8e-expectedparrot) . The concept—


*design an experiment as a survey answered by AI personas using LLMs to generate the answers—* and the base components are straightforward:


-


Construct


**questions** (free text, multiple choice, linear scale, matrix, etc.)


-


Combine them in a


**survey** and add any desired logic (e.g., skip/stop rules)


-


Optionally design personas for AI


**agents** to answer the questions


-


Send the survey and agent personas to a


**language model** to generate the responses


-


Get the


**results** back in a formatted dataset to analyze with built-in methods


Here’s a notebook with a quick example. I start by constructing a survey and AI persona and select a model to generate the responses:


Next I launch a web version of the survey answer it (at the


[respondent URL](https://www.expectedparrot.com/respond/11f5316f-03c0-4f21-9d61-20e9d6f9bb52) ):


I can pull the results back into my workspace to compare with the LLM results:


… and also inspect them at my Coop account:


# Market research & other use cases


If I can convince you to give it a try, here are some ways to use EDSL to conduct market research:


-


**Simulate customer personas** : Design


` Agent` objects with a variety of traits and preferences to represent different customer segments.


-


**Test messaging and branding** : Use question types such as


` QuestionFreeText` or


` QuestionMultipleChoice` to get feedback on product descriptions or branding materials.


-


**Compare product features** : Create


` Scenarios` for different feature sets and use


` QuestionRank` to have the agents rank preferences or interest.


-


**Evaluate price sensitivity** : Present different pricing scenarios and ask agents to choose or rate affordability using


` QuestionLinearScale` .


-


**Analyze perceived value** : Use open-ended questions to elicit reasons behind preferences or concerns with


` QuestionFreeText` .


-


**Explore distribution channels** : Pose


` Scenarios` about online vs. in-person availability and ask which they prefer and why.


-


**Gauge emotional response** : Include qualitative questions with


` QuestionFreeText` to measure reactions to marketing copy, visual content, or product pitches.


-


**Test variations across contexts** : Vary


` Scenario` and


` Agent` attributes to explore how preferences change by traits or product conditions.


-


**Benchmark against competitors** : Create


` Surveys` comparing your product with existing ones and assess perceived strengths and weaknesses.


*If you have any questions about using our tools for your research, please get in touch! **[\[email protected\]](https://blog.expectedparrot.com/cdn-cgi/l/email-protection)***


Thanks for reading! Subscribe for free to receive new posts and support our work.


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
