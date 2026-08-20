---
schema_version: "1.0.0"
document_id: "66bb5220d56e497c7e73d6e313724b435739e946f8145abb05b12e52b4cdf138"
company_key: "yc-guide-labs"
company: "Guide Labs"
source_id: "yc-guide-labs-news-import-41fe5d9b8b28"
canonical_url: "https://www.guidelabs.ai/post/introducing-guide-labs-engineering-interpretable-and-auditable-ai-systems/"
published_at: "2024-11-17T00:00:00+00:00"
first_seen_at: "2026-07-25T07:19:32.933709+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:5a16e0954c24da26ff82c21232f7e315a238bb27b749f24074440411dbbf60af"
---

# Introducing Guide Labs: Engineering Interpretable and Auditable AI Systems

Guide Labs is building a new class of interpretable AI systems that humans and domain experts can reliably understand, trust, and debug. As individuals and businesses around the world quickly work to integrate AI into their existing workflows, and governments seek to regulate frontier AI systems, the demand for models that can be reliably debugged, steered, and understood is ever increasing. To help bring interpretable and reliable models to market, we have successfully closed our seed funding of $9.3 million led by Initialized Capital. We are excited to have participation from Tectonic Ventures, Lombard Street ventures, Pioneer Fund, Y Combinator, E14 Fund, and several prominent angels.


## Current AI systems are not reliable, not interpretable, and are difficult to audit


The prevailing paradigm that most AI companies use is to train models as monoliths — typically using the transformer architecture — trained solely for narrow performance measures like next word prediction. However, this results in models that are difficult to work with, debug, and reliably explain. Even more alarming, current systems produce explanations and justifications that are completely unrelated to the actual processes the system used to arrive at its output.


-


**Current AI systems produce explanations that are unrelated to their decision making process:** Models can do everything from medical diagnoses, to candidate resume analysis, to determining qualifications for a home loan, and yet these models are inherently biased. In fact, when a model’s training process is unchecked, the default behavior is that the model’s explanations and justifications are entirely unrelated to the model’s output. This status quo is untenable because it renders current levers for arriving at insights about AI models ineffective. We need AI models that provide reliable justifications that are faithful — and truthful — to the way the AI models arrive at their output.


-


**You cannot reliably debug a system that you don’t understand:** When you call your favorite generative model API for a task, and the response is incorrect, or copied verbatim from its training data, what do you do? You can change the prompt, but it’s unclear which part to change, and even then, it doesn’t guarantee the right output. If changing the prompt doesn’t work, it’s difficult to understand whether you need to fine-tune , add more in context examples, or switch API models altogether. We need AI systems that can provide actionable insights that allow us to address these issues.


Gemini Struggles with recitation of its training data.


[https://github.com/google/generative-ai-docs/issues/257](https://github.com/google/generative-ai-docs/issues/257)


- **Difficult to control and align current AI systems:** Even when you’ve identified the cause of a problematic behavior from your model, it often requires a lot of trial and error to change the model behavior so that it no longer makes the mistake that you’ve identified. Fine-tuning and prompting models is too unreliable. What we need is fine-grained control.


While these large-scale models are still in their infancy, it is already clear that training for narrow performance measures like next word prediction without consideration for interpretability leaves too much room for error when it comes to mass public adoption. Repeatedly, in[computer vision](https://arxiv.org/abs/1905.02175) ,[natural language](https://arxiv.org/abs/2103.06922) ,[medical images](https://www.nature.com/articles/s41746-019-0105-1) ,[image generative models](https://www.bloomberg.com/graphics/2023-generative-ai-bias/) , and[especially LLMs](https://www.bloomberg.com/graphics/2024-openai-gpt-hiring-racial-discrimination/?leadSource=uverify%20wall) , it is the norm that optimizing for narrow performance measures does not yield reliable models.


## A new path: AI systems that are engineered to be interpretable


**At Guide Labs we believe you cannot reliably debug, align, and trust a model you don’t understand.** These critical properties cannot just be left unaddressed until after a model has been trained; they should guide the entire model development pipeline. Instead, we are rethinking the entire pipeline–model architecture, datasets, and training procedure—to engineer models that are interpretable, safe, trustworthy, and easier to debug and fix.


We want to enable reliable interaction, understanding, and controllability of models and AI systems. More specifically, we want:


- A medical doctor to understand why a medical LLM is making a particular diagnosis.
- A loan officer to verify whether an LLM is unfairly relying on legally protected attributes like gender, race, etc. for loan decisions.
- A biologist to be able to interactively understand why a protein language model has generated a particular sequence, and be able to interactively control the biophysical properties of the model.


To fulfill these requirements and many more, we need models that: produce reliable and trustworthy outputs; provide insights about which human-understandable factors are important; and indicate which part of the prompt, context, and training dataset are responsible for the output.


Collectively, our team has more than 20 years of experience focused on the interpretability and reliability of AI systems. We have published more than two dozen papers at top machine learning venues. Critically, we have shown that machine learning models trained solely for narrow performance measures, without regard for interpretability, result in models whose explanations are[mostly unrelated to the model’s decision-making process](https://arxiv.org/abs/1810.03292) , and are not[aligned with humans for consequential decisions](https://arxiv.org/abs/2410.15471) . Even worse, explanations of unchecked models can[actively mislead](https://arxiv.org/abs/2011.05429) . More recently, we’ve shown that[self-explanations, like chain-of-thought, of LLMs are unreliable](https://arxiv.org/abs/2401.07927) .


These results directly inform our approach to[engineer AI models that are interpretable, reliable, and trustworthy](https://arxiv.org/abs/2405.05386) . Toward this end, we have demonstrated the effectiveness of rethinking a model’s training process for[language models](https://arxiv.org/abs/2310.07819) and[protein property prediction](https://ai4d3.github.io/papers/79.pdf) . We developed one of[the first generative models](https://openreview.net/forum?id=L9U5MJJleF) at the billion-parameter scale that is constrained to reliably explain its outputs in terms of human-understandable factors.


Our past experience has shown that it is crucial to integrate interpretability, safety, and reliability constraints as part of the model development pipeline, and that these constraints can be satisfied without compromising downstream performance. With the new AI systems we are building, we can more easily identify the causes of erroneous outputs, detect when models latch onto spurious signals, and correct the models effectively. We aim to create a world where domain experts shift from merely ‘prompting’ AI to engaging in meaningful and truthful dialogue with AI systems.


## A First Step: Interpretable LLM at the Billion Parameter Scale


To demonstrate that our approach is feasible, and that constraining models do not sacrifice performance, we have developed an interpretable LLM that:


- produces human-understandable factors for any output it generates;
- produces reliable context citations; and
- specifies which training input data have the most effect on the model’s generated output.


We have shown that it is possible to train large-scale generative models that are engineered to be interpretable without sacrificing performance. We are excited to continue to scale these models to match current alternatives, expand the range of interpretability features we provide, and partner with select organizations to test the model. Reach out to us at:[\[email protected\]](https://www.guidelabs.ai/cdn-cgi/l/email-protection#7f161119103f180a161b1a131e1d0c511e16) if you would like to learn more.


## Join Us


We have assembled a team of interpretability researchers and engineers with an excellent track record in the field. We are hiring machine learning engineers, full-stack developers, and researchers to join us. If you are interested in joining our team, reach out to[\[email protected\]](https://www.guidelabs.ai/cdn-cgi/l/email-protection#e586849780809796a582908c818089848796cb848c) .


[Next blog Atlas: Orienting the Pre-Training data of an LLM →](https://www.guidelabs.ai/post/atlas-concept-annotated-pretraining-release/)
