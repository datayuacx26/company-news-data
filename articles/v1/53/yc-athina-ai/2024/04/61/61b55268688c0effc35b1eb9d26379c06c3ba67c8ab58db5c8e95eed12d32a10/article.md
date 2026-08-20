---
schema_version: "1.0.0"
document_id: "61b55268688c0effc35b1eb9d26379c06c3ba67c8ab58db5c8e95eed12d32a10"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/who-validates-the-validators-aligning-llm-assisted-evaluation-of-llm-outputs-with-human-preferences"
published_at: "2024-04-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:b07faaba8564d7562a949b1b4f68b568bc0c9b714756745072a523896c9f9814"
---

# Who Validates the Validators? Aligning LLM-Assisted Evaluation of LLM Outputs with Human Preferences

Do not index


Original Paper


[https://arxiv.org/abs/2404.12272](https://arxiv.org/abs/2404.12272)


Blog URL


[blog.athina.ai /who-val...ferences](https://blog.athina.ai/who-validates-the-validators-aligning-llm-assisted-evaluation-of-llm-outputs-with-human-preferences)


**Original Paper:**[https://arxiv.org/abs/2404.12272](https://arxiv.org/abs/2404.12272)


**By:**[Shreya Shankar](https://arxiv.org/search/cs?searchtype=author&query=Shankar%2C%20S) ,[J.D. Zamfirescu-Pereira](https://arxiv.org/search/cs?searchtype=author&query=Zamfirescu-Pereira%2C%20J) ,[Björn Hartmann](https://arxiv.org/search/cs?searchtype=author&query=Hartmann%2C%20B) ,[Aditya G. Parameswaran](https://arxiv.org/search/cs?searchtype=author&query=Parameswaran%2C%20A%20G) ,[Ian Arawjo](https://arxiv.org/search/cs?searchtype=author&query=Arawjo%2C%20I)


**Abstract:**


> Due to the cumbersome nature of human evaluation and limitations of code-based evaluation, Large Language Models (LLMs) are increasingly being used to assist humans in evaluating LLM outputs. Yet LLM-generated evaluators simply inherit all the problems of the LLMs they evaluate, requiring further human validation. We present a mixed-initiative approach to \`\`validate the validators'' -- aligning LLM-generated evaluation functions (be it prompts or code) with human requirements. Our interface, EvalGen, provides automated assistance to users in generating evaluation criteria and implementing assertions. While generating candidate implementations (Python functions, LLM grader prompts), EvalGen asks humans to grade a subset of LLM outputs; this feedback is used to select implementations that better align with user grades. A qualitative study finds overall support for EvalGen but underscores the subjectivity and iterative process of alignment. In particular, we identify a phenomenon we dub \\emph{criteria drift}: users need criteria to grade outputs, but grading outputs helps users define criteria. What is more, some criteria appears \\emph{dependent} on the specific LLM outputs observed (rather than independent criteria that can be defined \\emph{a priori}), raising serious questions for approaches that assume the independence of evaluation from observation of model outputs. We present our interface and implementation details, a comparison of our algorithm with a baseline approach, and implications for the design of future LLM evaluation assistants.


---


###


Summary Notes


##


Advancing LLM Evaluation with EvalGen: A Human-Centric Approach


In the rapidly growing field of Artificial Intelligence (AI), Large Language Models (LLMs) like GPT have become essential in a variety of applications.


Despite their advancements, evaluating their outputs remains a challenge, often resulting in outputs that don't always meet human expectations.


Enter EvalGen, an innovative tool designed to enhance LLM evaluation by ensuring outputs align more closely with what humans actually want.


###


Understanding Evaluation Challenges in LLMs


LLMs are powerful tools for tasks ranging from writing assistance to coding but they're not perfect.


They can produce incorrect information or fail to follow instructions, diminishing their usefulness.


Traditional evaluation methods struggle to effectively address these issues, highlighting the need for a more effective approach.


###


The Evolution of Prompt Engineering and Auditing


- **Prompt Engineering (PE)** has become key in guiding LLMs to better understand and respond to user prompts.


- **Model Auditing** is critical for spotting biases and inaccuracies in LLM outputs.


Tools like ChainForge and promptfoo have improved our ability to analyze and direct LLM behavior, yet they often miss a way to verify if LLM evaluations meet user standards. This is where EvalGen steps in.


###


How EvalGen Works


EvalGen integrates with LLM interfaces like ChainForge to offer a user-friendly platform for creating and applying evaluation standards. Its features include:


- **User-Generated Evaluation Criteria** : Users define what good output looks like for them.


- **Automated Suggestions** : Offers ways to apply these criteria through code or prompts.


- **User Grading** : Utilizes user-graded outputs to refine the evaluation process.


EvalGen uses a dynamic system that evolves with user input, ensuring that the evaluation criteria remain in line with user preferences.


###


Insights from Evaluation and User Studies


Testing showed EvalGen aligned better with human preferences than previous methods, using fewer assertions.


Feedback from nine industry professionals confirmed its practical value, though it also highlighted the challenge of defining stable evaluation criteria due to the complex nature of human judgment.


####


Key Takeaways from the User Study:


- **Balance Between Automation and Control** : Users liked the automation but wanted to keep control over the evaluation process.


- **Criteria Drift** : The evaluation criteria evolved as users interacted more with LLM outputs, showing the iterative nature of defining quality.


- **Demand for Quick Iteration** : Users expressed a need for a system that allows fast adjustments to criteria and their applications.


###


Discussion and Future Directions


EvalGen's development and user feedback emphasize the importance of flexible and adaptive evaluation criteria in LLM tools, capable of evolving with user needs and the changing outputs of LLMs.


###


Conclusion: Why EvalGen Matters


EvalGen represents a significant step forward in making LLM evaluations more in tune with human preferences.


It provides a robust framework for improving LLM outputs and paves the way for more interactive, user-informed AI tools.


As we continue to unlock the potential of LLMs, tools like EvalGen will be vital in ensuring these models meet the complex and changing standards of human judgment.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
