---
schema_version: "1.0.0"
document_id: "c2676a57c16f289535ce084f0a21a3678682541afc07e9008acacb92893ad49b"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/llm-judges-jury"
published_at: "2025-06-25T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:cbcd8285937133da6ce511d7d4abce3d348e966dbdf9a82e02ec0ef781fe08a8"
---

# Is this email okay? We asked a jury of LLM judges.

Instead of relying on one model to judge LLM outputs, we tried a simple experiment: take AI-generated emails, and have three different models – GPT, Claude, and Gemini – evaluate their tone. Was it appropriate for a U.S. corporate tech setting?


In this blog tutorial, we walk through the idea of LLM juries, the open-source implementation, and what we learned from LLM disagreements.


> **Looking for the code?**[Jump here](https://github.com/evidentlyai/community-examples/blob/main/tutorials/LLM_as_a_jury_Example.ipynb) .


## LLM judges and juries


How do you know if your LLM product is doing a “good” job?


One way is to ask another LLM to tell you. That’s the idea behind **LLM-as-a-judge** : you give an LLM a rubric and an output, and it returns a judgment. Is this correct? Helpful? Safe?


*If you're new to the concept, we wrote*[a guide to LLM judges](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) *.*


This is how many[LLM eval setups](https://www.evidentlyai.com/llm-guide/llm-evaluation) work today in experiments, red-teaming and live monitoring. It's fast, scalable, and surprisingly decent for many use cases.


Of course, success depends on:


- What you’re evaluating
- How well you express your criteria
- The model and prompt you use


And critically, you’ll want to tune your LLM judge against human labels.


*Here’s a*[video walkthrough](https://www.youtube.com/watch?v=kP_aaFnXLmY&list=PL9omX6impEuNTr0KGLChHwhvN-q3ZF12d&index=6) *on how to align LLM judges with human labels.*


But LLMs don’t always agree. (Neither do humans.) And for some tasks, even defining what "good" means is hard.


So here’s another idea: don’t ask one model – ask several. This is the **LLM-as-a-jury **** approach, where you ask multiple LLMs to evaluate the same output. Each will tap into its own training data and give an independent verdict.


This concept is formalized in[Replacing Judges with Juries](https://arxiv.org/abs/2404.18796) *(* Verga et al., 2024).


The authors propose a Panel of LLM Evaluators (PoLL) – a jury of smaller, diverse models from different families. Their result shows that jury-based evaluations align better with human judgment, can reduce model bias and be cheaper than using a single large judge like GPT-4.


*LLM-as-a-jury. You can also aggregate the outputs differently.*


While not every task needs a jury, it’s a powerful tool.


Using multiple models can add stability to your evaluations. Sometimes a single model will flip its decision, especially on borderline cases. Having a panel smooths that out. You can take a majority vote, average the scores or treat even a single “negative” label as a warning.


Second, the disagreement itself can be useful. When models don’t agree, that tells you something. Maybe your criteria are too vague, or there is an edge case you didn’t consider. That’s exactly the kind of output worth flagging. You can use it to improve your prompt, tighten your rubric, or rethink the generation itself.


## Email evaluation example


To make all this more concrete, let’s run a small experiment.


Imagine you’re building an AI assistant that generates emails from user instructions. The user can type a prompt like “decline the invite” and the assistant turns it into a complete email.


If you are building a tool like this, you’d need to evaluate how good your emails are.


We built a minimal dataset with 15 examples which contains user input and generated emails:


Next, we needed an evaluation criterion. We can keep it simple: is the email tone appropriate for a U.S. tech workplace email?


This is exactly what we’ll ask the LLM judges to decide.


We’ll show each model the generated email and ask to return:


- A binary label:` APPROPRIATE` or` INAPPROPRIATE`
- A brief explanation of the decision


Then we’ll aggregate the results to see whether all the judges we assigned (GPT, Claude, and Gemini) agree.


We picked this task because it’s the kind of real-world subjective thing you often want a second opinion on. In real workplace settings, tone varies by culture, company, even personal style. People may disagree. Models will too.


By comparing their judgments, we can surface hidden assumptions – and get closer to a working definition of what “appropriate” really means.


## LLM jury implementation


We will use the[Evidently open-source library](https://github.com/evidentlyai/evidently) to run and compare judgments across multiple models. It’s an open-source toolkit designed for LLM evaluations, with built-in templates, reports and a clean interface for plugging multiple LLMs as evaluators.


Here is how it works.


In Evidently, evaluations are defined through **descriptors** – reusable logic units that define how to score an output. We will create an LLMEval descriptor using the binary classification template. This wraps our core judgment task: Is this email appropriate?


Here is the exact prompt template we used:


```text
us_corp_email_appropriateness = BinaryClassificationPromptTemplate(
pre_messages=[
("system", """You are an expert in U.S. corporate and workplace communication in tech companies.
You will be shown a snippet of an email generated by the assistant.
Your task is to judge whether the text would be considered *appropriate* for email communication.
""")
],
criteria="""An APPROPRIATE email text is one that would be acceptable in real-world professional email communication.
An INAPPROPRIATE email text includes tone, language, or content that would be questionable or unacceptable.


Focus only on whether the tone, style, and content are suitable. Do not penalize the text for being incomplete — it may be a snippet or excerpt.
""",
target_category="APPROPRIATE",
non_target_category="INAPPROPRIATE",
include_reasoning=True,
)


```


If you run the evaluation with one judge (in this case, GPT-4o-mini), you will get a result like this: a judgment with a short explanation.


To run the complete evaluation suite, we’ll need to create several descriptors, one for each LLM:


- GPT from OpenAI (GPT-4o-mini)
- Claude from Anthropic (Claude 3.5 Haiku)
- Gemini from Google(Gemini 2.0 Flash Lite)


They will all reuse the same evaluation prompt and review the same set of 15 test emails.


Additionally, we’ll also add a test condition to each judgment: we expect the model to return an "APPROPRIATE" label. Then, we can add a TestSummary for each row with:


- A final success flag: it will say "True" if all 3 models approve the email.
- The success count and rate: how many judges say "yes".


Having a final score makes it easy to automate what happens next. You can decide to fail a test if even one judge flags the output, or set another threshold. Then, you can define expectations at the dataset level – like requiring 90% of test cases to pass for an experiment to be considered successful. It gives you a simple way to track regressions or enforce quality gates without needing to check each result manually.


For our more analytical task, this will make it easy to spot which emails were unanimously approved or rejected – and to sort the results by level of agreement.


> Check the[complete code example](https://github.com/evidentlyai/community-examples/blob/main/tutorials/LLM_as_a_jury_Example.ipynb) for the implementation details.


We also added an extra rule for convenience: if models don’t all agree, we explicitly flag the row as a “disagreement”.


And in our case, we got 5 examples where models couldn’t agree!


Let’s take a closer look.


Here’s a fun one. The email says: “The server decided to die again.” Claude was fine with the sarcastic tone, seeing it as typical tech-team banter. GPT and Gemini called it unprofessional – too flippant for incident communication.


In another case, the generated email included an emoji. GPT penalized the informality, calling it too casual. Claude and Gemini saw no problem – they considered it appropriate for internal updates.


Another email mentioned that the first draft was very rough. OpenAI judged this self-deprecating tone (“not sure it’s any good...”) as lacking professionalism. Claude and Gemini interpreted it as humble and collaborative.


The phrase “I’ll have to assume this isn’t a priority” triggered disagreement again.


GPT and Claude saw it as passive-aggressive. Gemini considered it a reasonable way to nudge someone after being ignored. One model’s “firm” is another model's “rude!”


Of course, all of this is just a toy example – but one can see how it can be useful in a real setting.


Should your emails include emoji? How casual is too casual? It’s up to you. Seeing where models disagree, you can take those edge cases and use them to improve both your core email generation and your evaluation prompt.


And don’t forget: the other 10 examples in our set were clear-cut – all three models agreed. So you can have some confidence and focus your attention on fixing what matters.


## Do you always need an LLM jury?


Not necessarily.


In many cases, a single strong evaluation model with good prompt engineering works well. It’s hard enough to tune one judge! So investing in prompt iterations and rubric design is often the default choice.


But there are cases where multiple models genuinely help.


**One is when you're still figuring out what “good” even means.** If you're thinking about evaluating vague traits like clarity or helpfulness, asking several models to weigh in can surface subtle differences. This won’t automate your evals, but you can use model disagreement to better define your criteria in the first place.


**Another case is when the task is subjective.** Like “Is this rejection email empathetic enough?” or “Could this meme be seen as offensive?” Even humans would often disagree. That’s where multiple perspectives matter. You can even intentionally assign your judges different personas – or try different models.


**Sometimes it’s just about having a second opinion** . In high-stakes use cases – customer communication, medical summaries, anything safety-sensitive – a single disagreement can be enough to justify a closer look or blocking an output. Having multiple judges makes your evals more robust.


**And finally, there’s coverage** . Different models bring different training data, assumptions, and blind spots, so combining their decisions can give you a more complete picture. For example, you might use multiple LLMs to evaluate the style of generated code. Or, if you are working with less capable smaller models, you can combine them to tap into their collective wisdom.


## What’s next?


You can build on this evaluation ensemble idea. It's not just about multiple LLMs!


For example, one interesting direction is to break down evaluation criteria, even when using the same model. Take something like “correctness” – instead of treating it as a single label, you can split it into sub-judgments: contradiction, omission, unverifiable addition, style drift. Then, combine those into a more nuanced score.


You can also try prompting the same model in different ways – for example, using a neutral evaluator prompt vs. a devil’s advocate one. Or framing it once as “is this good?” and once as “what’s wrong with this?”. Comparing those responses can reveal how much the model's judgment shifts based on framing alone.


In both cases, you’re not just looking for a simple yes/no answer – but iterating on making your evaluations more stable, explainable, and aligned.


## LLM evaluations with Evidently


If this was useful, check out[Evidently](https://github.com/evidentlyai/evidently) – the open-source Python library we used to build this workflow. It includes prompt templates, test logic, and reporting tools to help you evaluate LLM outputs systematically.


We're also building Evidently Cloud – a platform for running evals continuously, managing test suites, and collaborating on prompt and output analysis.


> ⭐ Star us on[GitHub](https://github.com/evidentlyai/evidently) 📬 Or[sign up](https://www.evidentlyai.com/register) to start with Evidently Cloud now.
