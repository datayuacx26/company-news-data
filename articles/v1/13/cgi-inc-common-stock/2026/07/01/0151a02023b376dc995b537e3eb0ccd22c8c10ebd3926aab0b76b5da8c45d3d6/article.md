---
schema_version: "1.0.0"
document_id: "0151a02023b376dc995b537e3eb0ccd22c8c10ebd3926aab0b76b5da8c45d3d6"
company_key: "cgi-inc-common-stock"
company: "CGI Inc."
source_id: "cgi-inc-common-stock-rss-66ef697d2497"
canonical_url: "https://www.cgi.com/en/blog/health/explainable-AI-healthcare"
published_at: null
first_seen_at: "2026-07-20T23:21:24.029549+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:96082f6df2356172d252f054d7e0ab807a15ad61682fbdcc98cf46640e9ceaf2"
---

# What is explainable AI (XAI), and why is it important in healthcare?

Over the last decade, the use of machine learning (ML) has grown rapidly across all industries, including healthcare. We experience it in our daily lives through voice recognition, natural language processing and chatbots, which often are used in telehealth to triage patients, direct them to services and answer pressing questions.


One reason behind ML’s popularity is its ability to learn complex patterns and relationships from data without hard-coded rules, increasing accuracy and performance across many applications.


Even though these models are achieving greater precision, it can be difficult to fully understand how their predictions are made. The ability to explain these predictions is a precursor to delivering excellent business value. Often, data scientists avoid state-of-the-art methods due to their inherent complexity and lack of explainability, which leads to poor performance. A key goal of explainable AI (XAI) is to address this problem.


## What is XAI?


XAI is essential research that enables users and stakeholders to interpret and understand how an ML model is making its prediction. Implementing XAI can bring transparency, trust, safety and bias detection to such models by answering:


-


Why does the model predict that result?


-


What are the reasons for this prediction?


-


What are the most vital contributors to the prediction?


-


How does the model work?


Gaining a better understanding of how the model should act when predicting the population or unseen data, makes it easier to recognize whether bias has been introduced.


## How can XAI benefit healthcare?


Applications of XAI for healthcare are limitless and include:


-


Understanding how a specific chronic illness or treatment impacts the length of a patient stay during an ER visit.


-


Identifying how blood pressure and age impact the likelihood a patient will suffer heart failure.


-


Highlighting what the model saw in an image that led to a certain classification (e.g., detecting a brain tumor in MRI).


-


Marking keywords in an email used by a text classification model to determine which department should receive a follow-up email.


## How do XAI algorithms work?


While there are many XAI algorithms that explain ML models, following are three examples:


-


**Shapley additive explanations (SHAP)** – SHAP quantifies each feature's contribution to the prediction made by the model. For example, if a soccer fan continuously watched their favorite team’s games by carefully studying when players were substituted and how this impacted performances, they would soon deduce the performance of each player. Similarly, SHAP carefully analyzes how predictions change as it explores all possible predictions. It soon learns how all the features impact the model and assigns them a SHAP value. One advantage of SHAP is that it provides explanations for the model for an individual prediction, as well as for the entire feature over all possible values. More importantly, these local and global explanations will provide a united explanation.


-


**Local interpretable model-agnostic explanations (LIME)** – While SHAP can become computationally heavy and time-consuming, LIME addresses this issue by creating a sample of data points around the data point being predicted. Weighting this sample by the proximity to the instance, LIME builds a linear regression model. It uses the coefficients from the model to determine the impact of the features on the prediction. It is crucial to note that since this explanation is built only on a sample of data by the instance being explained, LIME is not globally faithful. Think of LIME as building sparse linear models around individual instances/predictions based on data points in its vicinity.


-


**Integrated gradients** – Integrated gradients describe a model's prediction by explaining the difference in predicting an instance from a baseline or masked instance in terms of input features to the model. In other words, integrated gradients start with a completely empty baseline (all the features are gone, or zero) and slowly turn on the features one at a time, keeping track of the change in prediction. This isolates when the changes were the greatest, along with the direction of the change, and identifies the key features in a model and their impact on the prediction. This method is well suited for deep learning models and offers much faster computations than SHAP values. However, the model must be differentiable (gradients must be present).


## Which algorithm works best?


While all the algorithms have the same goal of explaining a model, they do so in slightly different ways. For example, if resources are limited or the prediction needs to be very quick, LIME could be the better alternative since it focuses on analyzing data points, rather than the whole data set. Conversely, if a global explanation that is unified with the local prediction is required, then SHAP would suit the application. In short, the best algorithm can be determined only after truly understanding the data, model and situation.


## Leveraging XAI on your AI journey


Building XAI on top of ML models allows organizations to try new ML algorithms, better understand how predictions are made and what a model will do when seeing new data, strengthen trust in the models, and detect bias. XAI also offers an opportunity for data scientists to work alongside subject matter experts to review what is impacting a model and detect potential flaws.


Regardless of where you are in your AI journey, XAI can help you automate, forecast and accelerate savings in your healthcare organization.


Learn more about how CGI is empowering healthcare organizations to optimize operations and improve patient outcomes through the responsible use of[AI](https://www.cgi.com/en/ai-in-healthcare)


Connect with me to discuss the promise of XAI in healthcare.
