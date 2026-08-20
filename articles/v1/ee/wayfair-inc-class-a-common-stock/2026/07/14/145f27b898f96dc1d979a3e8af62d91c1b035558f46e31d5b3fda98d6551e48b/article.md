---
schema_version: "1.0.0"
document_id: "145f27b898f96dc1d979a3e8af62d91c1b035558f46e31d5b3fda98d6551e48b"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/getting-customers-the-right-resolution-the-first-time-with-machine-learning"
published_at: "2026-07-17T13:20:43.920+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:77021679b98c2ecce92c87039e16a981681170aeb38d8755550a6a7abc14bb3e"
---

# Getting Customers the Right Resolution the First Time With Machine Learning

### Introduction: The Right Resolution, The First Time


Imagine you’ve just had a new bookshelf delivered. During assembly, you discover one of the main shelves is cracked, but the rest of the unit is perfectly fine. When an issue like this occurs, the customer service team at Wayfair strives to resolve it on the first try, without the hassle of follow-up calls.


This is harder than it looks. For that cracked shelf, our associate could offer several solutions: a partial refund, shipping just the replacement shelf, or sending an entire new bookshelf. To help our associates give the best resolution, we turned to machine learning.


Our goal is to improve the customer experience by getting the resolution right the first time. For a model to learn this, we needed to define what a "right" resolution looks like. We consider a resolution successful if it solves the customer's problem, eliminating the need for the customer to contact us again. This success rate, which we call First Contact Resolution, is a practical proxy for customer satisfaction and gives our model a clear signal to optimize.


**Our goal: Turn multi-contact resolutions into one-and-done experiences.**


This article explains how Wayfair built a multi-layered machine learning system to improve this resolution process, starting with what makes this a surprisingly hard problem.


### Why This Is a Hard Problem


Choosing the right resolution for a customer's issue is complex. Even a seemingly simple problem, like a damaged part, can have multiple potential solutions. To make a smart recommendation, a system has to weigh three distinct and competing factors for every single choice:


1. **Eligibility** : Is this resolution even possible? This is the first and most basic criteria. For example, is the specific replacement part in stock at a nearby warehouse? Does the supplier for this bookshelf even offer individual parts?
2. **Success Propensity** : If the customer accepts this resolution, will it solve their problem for good? This models the risk of a follow-up contact. For instance, if we send a replacement part, what is the probability it will arrive undamaged, be the correct part, and be easy for this customer to install?
3. **Acceptance Propensity** : If we *offer* this resolution, will the customer prefer it? This models the customer’s preference. For example, will this customer be happy with a replacement part that requires a DIY installation, or do they prefer a full replacement unit?


A major challenge is that our historical data is incomplete. We only record the resolution that was ultimately chosen and whether that customer called back. Consequently, we do not know what the outcome would have been if a different resolution had been chosen.


Furthermore, the system does not have access to all the information a human associate does. The associate is talking to the customer in real-time and can pick up on crucial context, such as the customer’s frustration level or a specific constraint they mention, like “I’m not handy at all.” For this reason, our system is designed to augment the associate’s expertise, not to make an autonomous decision.


We designed a two-layer architecture to tackle these distinct challenges.


### A Two-Layered Solution: Suitability and Ranking


Instead of building one monolithic model to do everything, we broke the design into two separate layers, creating a system that is robust, interpretable and effective.


**Layer 1: The Suitability Layer** The first layer’s only job is proactive failure prevention. It uses both historical data and, when available, visual analysis of customer photos to assess whether an option has a high risk of failing to solve the customer's problem. If the answer is yes, it's flagged as "unsuitable" and removed from consideration.


**Layer 2: The Ranking Layer** The second layer’s job is optimization. It receives suitable resolutions from the first layer and asks a different question: "Of these good options, which one is the *best* choice to solve this customer's problem on the first try?" It then ranks the alternative suitable resolutions accordingly.


This two-step process ensures the guidance given to agents is both reliable and optimized. The Suitability Layer prevents predictably poor choices from ever being considered, and the Ranking Layer then intelligently ranks the remaining good ones.


**The Suitability Layer filters high-risk options, while the Ranking Layer ranks the remaining solutions by success metric.**


Let’s take a closer look at how each of these layers works.


### Layer 1: Is the Resolution Suitable?


The Suitability Layer’s job is proactive, holistic failure prevention. It combines two complementary components: a **Historical Risk** model that analyzes historical data and a **Visual Viability** model that analyzes visual evidence.


The **Historical Risk** model prevents failures that are predictable from past events. To make this tangible, let's consider our bookshelf example again. What if historical data shows that for this specific part from this supplier, there's a high rate of stock issues or shipping damage, often leading to a second, frustrating contact for the customer? This model answers the question: "Even if it's physically possible, what is the outcome risk based on historical data?" It calculates a "success probability" for resolutions by analyzing millions of past interactions, flagging those with a high risk of repeating known issues. This score predicts how likely the resolution is to solve the problem if the customer accepts it, and is directly tied to the First Contact Resolution metric.


The **Visual Viability** model uses our Vision Language Model (VLM) based system named[Saffron](https://www.aboutwayfair.com/careers/tech-blog/vision-language-models-in-the-wild-damage-diagnostics-from-customer-photos) that analyzes customer photos to assess physical damage. For example, it helps answer the question, "Is this drawer physically fixable?" by identifying a replaceable drawer handle versus a shattered drawer front that would require a whole new component.


These two models work together to provide a comprehensive suitability check, catching different kinds of potential problems with very little overlap. A resolution is only removed from consideration if this holistic assessment flags it as unsuitable and a clearly better alternative exists. This ensures the layer is conservative and doesn't limit associate options unnecessarily.


Now that we have a list of suitable resolutions, how do we pick the best one for the associate to offer first? That’s the job of our second layer: the Ranking Layer.


### Layer 2: Which Suitable Resolution Is Best?


Historically, resolution options were ordered using a fixed set of business rules. The Ranking Layer replaces that static logic with a dynamic model that learns from data and context to find the best recommendation for each unique situation.


The ranking component computes a score for each resolution representing the **Expected Value** of making that offer. This score is calculated by summing the value of every possible outcome, each weighted by its probability. The score for offering a resolution *r* is a sum over all *k* possible resolutions a customer might actually accept:


Where *P(accepted=k|offered=r)* is the *Acceptance Propensity* , *Value(k)* is the value of each accepted resolution given its success or failure probability:


Where *P_success(k)* is the success probability of resolution *k* if accepted. *Cost_current(k)* and *Cost_followup(k)* capture the business cost of giving out resolution *k* , plus any follow-up cost incurred should *k* fail. *a* is a coefficient to tune the success/cost tradeoff on a holdout set.


For instance, when the model evaluates the value for offering a "replacement part" it weighs the different paths the outcome might take:


- **Path 1: The part solves the problem.** The model knows the probability and business cost of this direct success.
- **Path 2: The part does not work and we send a full replacement unit next.** The model calculates the probability of this happening, and then for *this* path, it estimates the combined cost of giving out the failed replacement part plus cost of any followup resolutions.


The final "Expected Value" of the initial "replacement part" offer is the sum of the values of both potential success and failure branches, each weighted by its success and customer acceptance propensities. This is how the model learns that a seemingly simple offer is a poor choice if it has a high chance of leading to a frustrating customer experience down the line.


Modeling the customer's choice, or *Acceptance Propensity* , is crucial. A resolution that might seem perfect on paper is less useful if a customer is unlikely to agree to it. For example, a partial refund often has a very high success rate *if accepted* , but it is not a useful recommendation if the customer is set on receiving a physical item and is likely to reject the discount offer. By modeling the probability of acceptance, the system learns to recommend resolutions that are not only likely to succeed but also likely to be chosen.


The propensity and cost models that power this calculation are implemented using[CatBoost](https://catboost.ai/) , a gradient boosting library that is highly effective for the kind of tabular data common in this domain. However, the raw, uncalibrated scores from these models are not directly comparable. For example, a 0.8 success score for a 'replacement part' may not mean the same thing as a 0.8 for an 'in-home repair.' To solve this, we use a process called **calibration** , where we adjust the scores using[Platt scaling](https://en.wikipedia.org/wiki/Platt_scaling) on a holdout calibration dataset. This ensures we are making a fair, apples-to-apples comparison when ranking different types of resolutions.


So, with this two-layered system of suitability and ranking in place, how well does it actually perform? Let’s look at the evaluation.


### Impact and Lessons Learned


To evaluate the system, we trained it on over one million recent customer interactions, using a rigorous, time-based split to validate and test the model on different time periods. In extensive offline simulations, the two-layer system showed a projected relative improvement of nearly **2%** in our First Contact Resolution metric compared to the previous system without ML. This represents a substantial outcome across our customer base of hundreds of millions.


Beyond the metrics, this project provided several key lessons for building machine learning systems that act as a co-pilot for customer service associates.


1. **Eligibility Isn't Suitability.** We learned that a resolution being "allowed" by business rules doesn't mean it will resolve a customer's issue. Building a dedicated layer to assess suitability from multiple angles was critical. Our Historical Risk model prevents predictable failures based on past data, while our Visual Viability model uses photographic evidence to catch issues simple eligibility checks would miss.
2. **Separate Failure Prevention from Ranking.** The Suitability Layer cleanly handles the suitability question ("Will this likely fail?"), which allows the Ranking Layer to focus entirely on the optimization question ("Which of these good options is best?"). This separation makes the system more modular, interpretable and robust.
3. **Model the Real World with Expected Value.** Moving from a simple success prediction to an Expected Value model that considers all possibilities, including customer acceptance, was key. A recommendation is ineffective if the customer rejects it. By accounting for these variables, we provide more practical and effective guidance for our agents.


**What’s Next?**


Based on its strong offline results, the model is a candidate for live A/B testing with our customer service associates to measure its real-world impact on customer satisfaction and business metrics. Looking forward, the team is exploring future applications for this technology, such as powering recommendations in our self-service channels or improving logistics by optimizing resolution paths with our suppliers.


### Join the Team


If solving complex, high-impact problems like this sounds interesting to you, consider a[career](https://www.wayfair.com/careers/jobs?teamCategoryIds=6&teamIds=1) at Wayfair.
