---
schema_version: "1.0.0"
document_id: "63dcbd1f637954cac49e800c8bb8dc21487bad92a9b79a69eb6da6a381f0730c"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/how-to-launch-a-survey-with-llms"
published_at: "2025-07-22T20:55:04+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:1307739a33abdfe2647c573ca0a6c8d5c4b75a97c2591c0e256e5927231659c8"
---

# How to launch a survey with LLMs and humans

# How to launch a survey with LLMs and humans


### Our tools let you design and run the same survey with AI agents and human respondents, and seamlessly compare results.


[Expected Parrot](https://substack.com/@expectedparrot)


and[Robin Horton](https://substack.com/@yerkes)


Jul 22, 2025


There may be a number of reasons to incorporate human feedback into your AI experiment. When using AI agents and LLMs to simulate human responses—whether for surveys,


data labeling , or user research—you often need real human validation. This might involve cognitive testing of your questions and instructions, or verifying that AI responses align with realistic human behavior.


We've built features that streamline this human-AI comparison process in both our open-source


[EDSL](https://github.com/expectedparrot/edsl) package and our


[no-code builder](https://www.expectedparrot.com/getting-started/build) . The workflow consists of: creating a survey, running it with AI agents, then sending it to human participants. Since AI and human results use identical formatting, you can analyze both datasets with the


[same methods](https://docs.expectedparrot.com/en/latest/results.html) .


Below we demonstrate this process with a simple survey about some unforgettable Jane Austen quotes, gathering responses from both AI agents and human participants.


*Note* : Every step shown in code can also be accomplished through our no-code builder—reach out if you need help getting started!


# Create a survey


We start by creating a


[survey](https://docs.expectedparrot.com/en/latest/surveys.html) . EDSL comes with


[many common question types](https://docs.expectedparrot.com/en/latest/questions.html) we can choose from based on the form of the response that we want to get back from models (and humans):


Note the use of


**{{ scenario.placeholders }}** in the questions.


[Scenarios](https://docs.expectedparrot.com/en/latest/scenarios.html) allow you to create versions of your questions efficiently with different texts, PDFs, CSVs, images, videos, tables, lists, dicts, etc. Here we post some images to Coop and inspect them:


When we create scenarios we can add any extra fields for metadata that we want to include in the survey results, without having to add it to the questions directly—e.g., we did not include placeholders in the questions for “novel” and “speaker” but we can make them columns of the results that are generated:


# Run with AI


Next we can


[design AI agents](https://docs.expectedparrot.com/en/latest/agents.html) to answer the questions and


[select models](https://docs.expectedparrot.com/en/latest/language_models.html) to generate those responses. EDSL works with many popular models; you can check details on current available models


[here](https://www.expectedparrot.com/models) and see which ones are being used the most at our


[cache stats](https://www.expectedparrot.com/cache-stats) page. Here we create some simple personas and select a couple vision models to use with the survey. Note that agent traits and personas can be *much* more detailed than this—


*we’re working on lots of features for helping you generate them, please get in touch to test* —and you can specify the


[model parameters](https://docs.expectedparrot.com/en/latest/language_models.html) (here we use defaults). We add the scenarios, agents and models to the survey when we run it:


This generates a dataset of results that we can immediately begin analyzing (see all results in the


[notebook](https://www.expectedparrot.com/content/RobinHorton/pride-and-prejudice-politeness-notebook) and at


[Coop](https://www.expectedparrot.com/content/RobinHorton/pride-prejudice-quotes-survey) ).


# Run with humans


To run the same survey with humans, we start by calling the


[humanize](https://docs.expectedparrot.com/en/latest/humanize.html) method to generate a web version of the survey and a


**Project** for collecting responses:


We can inspect and share the respondent URL:


We can create a


[study](https://docs.expectedparrot.com/en/latest/prolific.html) to launch the same survey at Prolific:


Responses are automatically added to your Project page and can be pulled into your notebook for analysis:


# References


Details on methods, tutorials and example code for a variety of use cases are available at our


**[documentation page](https://docs.expectedparrot.com/en/latest)** .


A complete notebook of example code shown above is available


**[here](https://www.expectedparrot.com/content/RobinHorton/pride-and-prejudice-politeness-notebook)** .


A slide deck version of the code is available


**[here](https://docs.google.com/presentation/d/1oenBc49ElXmXvtCnGiEE_qZXBYyaoTlzIg17cHUPCh0/edit?usp=sharing)** :


*Thanks for reading! Subscribe for free to receive new posts and support our work.*


A guest post by


[Robin Horton](https://substack.com/@yerkes?utm_campaign=guest_post_bio&utm_medium=web)


Co-Founder @ Expected Parrot


[Subscribe to Robin](https://yerkes.substack.com/subscribe?)
