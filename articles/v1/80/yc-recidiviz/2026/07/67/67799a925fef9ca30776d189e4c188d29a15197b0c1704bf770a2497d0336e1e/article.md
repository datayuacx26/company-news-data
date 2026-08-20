---
schema_version: "1.0.0"
document_id: "67799a925fef9ca30776d189e4c188d29a15197b0c1704bf770a2497d0336e1e"
company_key: "yc-recidiviz"
company: "Recidiviz"
source_id: "yc-recidiviz-news-import-9fcc86c2623e"
canonical_url: "https://www.recidiviz.org/updates/spotting-good-uses-of-ai-tools"
published_at: null
first_seen_at: "2026-07-22T11:03:47.789737+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:deee718766ed4f0b96cccbdcb8fa421d7dababb6c7dd3668a2b3be1fd9eedf4d"
---

# Spotting Good Uses of AI Tools

***This article was published on the Technically Optimistic Substack as part of Navigating AI, a collection of articles by technology experts exploring key AI topics and providing practical guidance for tech leaders. Each article unpacks AI and offers actionable insights to help CTOs and senior leaders learn, champion, and apply AI at their own pace in their respective organizations. Read the full series***[here](https://technicallyoptimistic.substack.com/t/navigating-ai) ***.***


As co-founder and Chief Technology Officer of Recidiviz, I’ve been fielding occasional questions about how artificial intelligence (AI) could support our work since we were founded in 2019. That’s partly because Recidiviz employs one of the largest tech teams within the criminal justice space. Our 70+-person team works with Departments of Corrections in 19 states to safely and permanently reduce the size of their incarcerated population. And because our core work involves the cleaning, connecting, and leveraging of massive amounts of case-level data, the question of AI has been a logical one to consider. But over the past year, an exponential increase in questions about AI — particularly from potential funders — has made it clear that we are well beyond debating the issue in hypothetical terms.


Recent advances in generative AI (GenAI) have made the conversation very real and rather urgent. AI is spreading faster and further than many of us anticipated, and the push to incorporate it is threatening to outpace our understanding of its implications. This is particularly troubling in the criminal justice field, where early applications of predictive analytics were often shown to reproduce the very same biases they sought to resolve. An algorithm built with biased data will reproduce that bias in its results. But so, too, might the latest AI approaches.


It’s critical that technical and organizational leaders approach AI thoughtfully, even as industries around the world race to enter the fray. (And as funders and partners increasingly ask organizations to incorporate AI.) As you begin your explorations, consider the criteria by which you can evaluate potential use cases in your own field. Doing so can help avoid rushing headlong into production with a model that isn’t ready, which can harm your organization, your product, your users, and your mission.


The following are some simple questions you and your team can ask as you get started. These were originally devised with the criminal justice field in mind, but there are a number of criteria that are relevant for anyone doing public interest or social impact work. Consider the following:


### **1. What is the threshold for accuracy?**


In other words, must you be completely accurate, or is a good approximation enough?


Let this be the very first question you ask yourself. Because if accuracy is paramount — and the AI needs to be interpretable, more on that below — then you **should not** use large language models (LLMs). LLMs are approximations that are well known for their unpredictability. If we are the discerning adults, then LLMs are the eager-to-please children. They look for correlations between data points in order to answer your question — even if they don’t have a satisfying answer.


These false or misleading results are referred to as hallucination, and their consequences can be very real. So it’s very important to consider the “cost” of being wrong, for the people who it could directly impact but also for your organization.


There have been recent examples of these hallucinations in practice. In the legal space, lawyers have used LLMs to produce legal briefs, only to have those briefs cite cases that never existed. The LLM’s desire to produce a citation created a hallucination that compromised the validity of the brief, not to mention the credibility of the lawyers. If that had been, say, a civil rights litigator arguing on behalf of an individual, a person’s liberty would also be at stake.


So if your accuracy (or inaccuracy, as it were) can have a real-world impact, you should consider something that is simpler and more deterministic (e.g., a rule-based search). In fact, a good rule of thumb when considering AI usage is to ask yourself, “Could I do something simpler instead?” And if you do decide to use AI, you should additionally ask, “Is this the simplest AI approach?”


And lastly — and no matter what you choose to do — if accuracy is important, you should strive to have a human in the loop. Whatever the tool or product or workflow is, a human being remains to be (at least for now) the most accurate way to review your output.


### **2. How important is interpretability (or at least explainability)?**


Meaning, can you understand how the decision was reached?


In some cases, interpretability might be a legally or contractually required part of the work. For example, the Equal Credit Opportunity Act requires that creditors provide a clear explanation for why an application for credit was denied. “The model says so” would be insufficient. Beyond legal obligations, when decisions made will affect the quality of life or trajectory of an individual, interpretability remains an imperative. For example, AI could make decisions that affect a person’s liberty in the criminal justice system; the level of one’s access to health care; or the opportunity to obtain financial products like loans and mortgages. For such life-altering decisions, if a model is going to be used, it is important that it be interpretable. This is, in part, because it promotes institutional trust: Imagine a scenario where a judge denies your bail because of a black box model and shrugs and says, “Well, my computer told me to.” But it’s also important for continued research and development: If the engineers, scientists, and analysts responsible for a model struggle to explain why it made a high-stakes decision, they’re going to struggle to improve future decision-making.


If you work at an organization where your mission is oriented towards people’s wellbeing or a public benefit, then it’s especially important to be able to explain the decisions that are made in pursuit of those benefits. AI — and particularly LLMs — can’t always do that.


### **3. Is it a problem if the future looks like the past?**


AI models may appear as an exciting future technology, but these models are fueled by data from the past. And their output is only as good as the data that was used to train them — meaning that if you use poor or problematic data, you will replicate those results (“garbage in, garbage out”).


Ask yourself the following questions:


- Is the data that I have representative of my use case in the future?
- What are its limitations?
- Is it incomplete or inconsistent?
- Does it contain examples of bias?


With any AI model — even those as sophisticated as frontier LLMs — you are using data from the past to produce a prediction or inference about the future. If you want to make predictions that are qualitatively different from the past, then you must train a model with data that represents that change. Ultimately, AI can’t magically extract information that isn’t there. For example, say you suspect that human-labeled data is sometimes incorrect; we cannot expect a model to “just know” when it’s incorrect and how to fix it unless you have some other source of information to reference (e.g., a small set of higher-quality labels, or some expert-guided rules to identify outliers). What you seek to predict must therefore be reflected in the training data and in a large enough quantity for the model to see it.


Let’s take the example of a human resources department at a government agency or corporation that’s facing retention issues and wants to use AI to understand why people are leaving. The training data might include exit interviews, which LLMs can derive a structure from, but the results will almost certainly be skewed. That’s because exit interviews are not necessarily mandatory, and the people who were most unhappy with the position are also the most likely to have skipped them. And so the training data is informed only by the people who left on relatively good terms — and generates only a partial view.


Or we can take a hypothetical example from the justice system. Say you are focused on disparities in sentencing and you believe that some defendants are receiving relatively punitive sentences as compared to other defendants from the same set of judges. Using AI to inform sentencing decisions so as to rectify these disparities could backfire: If you train the model on the past decisions from that set of judges, the result would be a model that merely predicts what those judges are most likely to do, not one that makes qualitatively different recommendations.


### 4. Is your organization equipped to use AI, let alone to build and deploy it?


The return on investment with AI is driven largely by the choice of model and the means of deploying it. It’s the latest instance of the classic Build vs. Buy question: How much would it cost in compute, storage, and networking to deploy your own model and access that from your applications? How much would it cost instead to pay for API calls to a model provider given the expected traffic and usage patterns for your use case?


There are also serious questions surrounding organizational capacity. Namely, do you have the data and expertise necessary to run an AI model? In order to run AI that is actually productive, your organization needs the domain expertise to interpret what the model is doing — both what the input actually represents and what the output actually means — not to mention the technical skills to set it up in the first place.


Even more fundamental is the data — which needs to be cleaned, digitized, and centralized — and the testing, establishing the proper infrastructure and processes for validating and testing model output under a comprehensive set of circumstances. While hosted providers can do much of this work for you, it may not be legal to engage with a third-party provider for the purpose of your use case. Government policy in your jurisdiction, or contract language for a potential customer, might place stipulations and restrictions around training, deployment, and usage, particularly when sensitive data may need to be transferred to a third party.


Either way, there is a level of expertise that must be obtained — and not every organization has that expertise today. For many, it likely makes more sense to have humans do the task itself rather than acquire the human expertise necessary to complete it with a model.


This is just a starting point for considering AI, but the answers to these four questions should help filter for more promising applications. AI offers us a world of exciting possibilities, but applying it to less promising applications could limit that potential or even cause harm. The current, urgent push to incorporate AI should be channeled into a push to understand its implications for one’s organization and mission. Approaching this work intentionally and inquisitively now can help develop best practices that will pay dividends for all of us in the long run.


‍
