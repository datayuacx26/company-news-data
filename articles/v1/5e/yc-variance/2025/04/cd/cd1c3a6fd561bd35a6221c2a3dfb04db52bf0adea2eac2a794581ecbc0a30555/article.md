---
schema_version: "1.0.0"
document_id: "cd1c3a6fd561bd35a6221c2a3dfb04db52bf0adea2eac2a794581ecbc0a30555"
company_key: "yc-variance"
company: "Variance"
source_id: "yc-variance-news-import-ff45b1fc661c"
canonical_url: "https://www.variance.com/blog/training-decision-trees-on-sae-outputs"
published_at: "2025-04-21T00:00:00+00:00"
first_seen_at: "2026-07-22T18:28:25.562528+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:ba55cded9bf6b40247afc6953a71d35e7367a48ea7650dccbfd877fe38008fa8"
---

# Lessons from Using Sparse Autoencoders to Interpret AI Flags for Violent Threats

> **Note:** This is early work! We have not yet benchmarked this against standard evaluation metrics and the current results are a cursory look based on a small dataset and have not been tested at scale or against adversarial inputs. Some of the latent features remain difficult to interpret consistently. We're sharing this to illustrate a direction we're exploring, and will follow up with more comprehensive validation as the work matures.


## Context


Variance processes petabytes of data every month. Buried in that stream are edge cases, content that seems benign at first glance but meets escalation criteria under policy. While lower recall is acceptable for certain policies, it is not for others, especially as it pertains to physical safety.


Consider a policy where specificity signals risk: if someone is specific or detailed in their threat, it may mean that they have seriously considered taking action. Often, imminent threats must be treated in ways distinct from general violent content, and it can be helpful to have a classifier that's able to make this nuanced detection.


Although traditional classifiers can solve this task, they often lack interpretability in their results, outputting only a single score for a decision. Similarly, language models can be prompted to output explanations for decisions, but tracing the reasoning behind an actual decision can be challenging. This is acceptable in certain task automation use cases, but not in investigation and escalation workflows where a flag could reach law enforcement.


Given the high-precision workflows Variance is entrusted with, the team spun up an exploratory project to build a classifier that's both human-interpretable and one that enforces clear reasoning within a language model by combining human-interpretable features generated from sparse-autoencoders with decision trees. This effort was made to give to our customers access to a new type of policy-aligned enforcements that are


1. Explainable
2. Auditable
3. Deterministic


which is mission-critical in high-risk high-precision use cases.


Here, we demonstrate how SAE-generated features trained on Llama 3.3 70B can help solve a nuanced moderation use case: building a classifier that flags content containing threats with a high degree of **specificity** or **imminence** .


## Sparse Autoencoders


Sparse Autoencoders (SAEs) are interpretable models designed to help humans interpret an underlying model's internal workings. Although some model behavior can be understood by directly looking at the underlying neurons – that is, the flow of weights moving in and out of an individual connection in a neural network[\[1\]](https://openai.com/index/language-models-can-explain-neurons-in-language-models/) , SAEs have proven much more helpful in model interpretation by transforming these raw model outputs into a latent space with much more capacity that is more easily interpretable[\[2\]](https://www.lesswrong.com/posts/CJPqwXoFtgkKPRay8/an-intuitive-explanation-of-sparse-autoencoders-for) . The interesting thing about SAEs is that the fact that they are interpretable at all may suggest that LLMs build useful representations of language that can be utilized or exploited, such as steering the model toward specific outputs[\[3\]](https://www.anthropic.com/news/golden-gate-claude) , or even entirely removing certain behavior such as safety refusals[\[4\]](https://www.lesswrong.com/posts/jGuXSZgv6qfdhMCuJ/refusal-in-llms-is-mediated-by-a-single-direction) .


For this blog post, we use[Goodfire AI's public SAEs](https://www.goodfire.ai/blog/sae-open-source-announcement/) trained on Llama 3.3 70B to help identify helpful features and train a classifier to help filter high-priority violations for a content moderation use case.


Note that SAEs can be trained on specific datasets to expose better feature representations for a target use case, for instance, to get more detailed use cases for bioweapons data[\[5\]](https://transformer-circuits.pub/2024/features-as-classifiers/index.html) . We find that even though this SAE was not pre-trained with this content moderation task in mind, this SAE has some helpful features (along with auto-interpreted explanations[\[6\]](https://openai.com/index/extracting-concepts-from-gpt-4/) ):


- **Feature 64302** : A feature that fires on "First-person declarations of violent intent"
- **Feature 19585** : A feature that fires on "References to scheduled near-future events."
- **Feature 26498** : A feature that triggers on "References to meaningful or significant physical locations"


We can use these features to build trees. The sparsity of SAEs is helpful here: many similar features only fire on very specific language patterns, which is desired for the classifier we will train below.


## Decision Trees


To build our imminent threat policy, we first curate a dataset of content that's focused on violations and borderline violations. We are interested in sampling around this grey area (for instance, a generic threat against a political figure) to help us identify helpful features for our classifier.


```text
A threat is considered a serious threat if it satisfies one of the following:
1.   The threat is specific and detailed       2.   The threat is directed towards a specific person or location       3.   The threat describes a specific time or date
```


To collect activations from the SAE, we run each text sample through Llama 3.3 70B, using Goodfire's SAE to aggregate using the max-pool feature activations across the whole context. This returns the set of features with the highest activations across the entire context.


```text
A threat is considered a serious threat if it satisfies one of the following:
Determine if the below content is a serious threat.
<  Text  >    {text}    </  Text  >
First think in brackets called   <  Thinking  >  </  Thinking  >    Then output your answer in   <  Answer  >  </  Answer  >   with an explanation in   <  Explanation  >  </  Explanation  >  .
```


Next, we sample relevant features from the SAE by comparing feature activations between pairs of borderline violative with violative examples. On top of this, we apply a semantic search to filter the top features related to our three criteria based on their interpreted label descriptions.


Here are some sample features ranked high in this process based on our threats dataset. Note that many of them relate to threats, timing, and particular events.


```text
Threats of future consequences or retribution   Military or intelligence targeting and attacks on specific objectives   References to scheduled near-future events   Future deadline and target date specifications   First-person declarations of violent intent   Forceful movement or destruction through obstacles   Military forces initiating hostile actions   First-person hunter perspective during pursuit and anticipation   Narratives about totalitarian plans to reshape society   Content involving ethical questions about violence and vigilantism that requires careful moral consideration   News reporting of mass shootings and gun violence   Discussion of the January 6th Capitol attack and related political violence   Technical destinations and targets (including string literals, service endpoints, and algorithmic goals)   Content discussing weapons or violence that may require moderation   Content related to neo-Nazis and far-right extremism requiring careful handling   Descriptions of gunfire and shooting incidents   Narrative descriptions of conflict escalation and violence breaking out   Derogatory insults and offensive language targeting individuals   Breaking news headlines announcing major developments or changes   Systematic elimination of enemies in action sequences
```


We can train a decision tree to get an interpretable classifier with these candidate features. Here is an example of an interpretable classifier and some sample inputs that reflect the classifier's final decision.


## Performance


Below, we benchmark performance of the above decision tree classifier against logistic regression as directly trained against the feature inputs.


We also compare the performance of prompting Llama 3.3 70B directly. Note that the zero-shot prompt tends to over-flag content relative to the decision tree and logistic regression classifiers.


Here is a sample false positive from the zero-shot prompt, where it overflags the input "White House" as a serious violent threat:


### Prompt


```text
A threat is considered a serious threat if it satisfies one of the following:   – The threat is specific and detailed   – The threat is directed towards a specific person or location   – The threat describes a specific time or date   Determine if the below content is a serious threat.
<  Text  >    White House    </  Text  >
```


### Output


```text
<  Thinking  >    The given content is "White house". To determine if this satisfies any of the clauses, the content mentions a location. This could potentially satisfy clause 2, which states the threat is directed towards a specific person or location.    </  Thinking  >
<  Answer  >    Yes    </  Answer  >
<  Explanation  >    The text is considered a serious threat because it is directed towards a specific person or location. Despite the brevity and apparent attempts to downplay the intent, the request for information could potentially facilitate potential harm or harassment towards a specific direction towards a particular person, which meets the criteria.    </  Explanation  >
```


It is interesting to note that for the above sample, the language model flags that it is violative, and the threats classifier based on the decision tree would not have flagged the above content. For especially sensitive applications, these decision trees can double-check that a model's outputs are correctly grounded and justified and not based on spurious inputs.


At Variance, we're building agentic systems for investigations that can surface the right evidence, reason through edge cases, and justify their conclusions. The old approach relied on static heuristics and black-box models. This time, we're designing for transparency, adaptability, and oversight from the start.
