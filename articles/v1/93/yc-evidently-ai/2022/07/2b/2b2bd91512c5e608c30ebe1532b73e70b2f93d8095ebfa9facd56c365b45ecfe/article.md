---
schema_version: "1.0.0"
document_id: "2b2bd91512c5e608c30ebe1532b73e70b2f93d8095ebfa9facd56c365b45ecfe"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/ml-monitoring-metrics"
published_at: "2022-07-18T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:242764d02aea2cf1e27d80cc2578f90be375a289f86d5b4004105d8f55089161"
---

# Monitoring ML systems in production. Which metrics should you track?

When one mentions "ML monitoring," this can mean many things. Are you tracking the service latency? Model accuracy? Data quality? The share of visitors that click on the recommendation block?


ML monitoring can be all of the above or none.


‍ **This blog organizes all metrics into a single framework.** It is high-level, but we hope a comprehensive overview. Read on if you are new to ML monitoring and want a quick grasp of it.


Along with the blog, we'll also link to articles that companies like Doordash, Nubank, Booking.com, and Linkedin wrote about how they approach ML monitoring.


## Why you need ML monitoring


For starters, why even talk about monitoring?


When you deploy an ML system in production, it integrates into the business. It's ROI time! You expect it to deliver value. For example, a recommendation system should improve user experience and increase revenue. ****


**But these ML systems can fail.** Some of the failures are obvious and trivial, like the service going down. Others are silent and particular to machine learning, such as[data and concept drift](https://evidentlyai.com/blog/machine-learning-monitoring-data-and-concept-drift) . You might also face critical second-order effects. For example, a credit scoring system can show bias towards certain customer groups.


To control these risks, you must monitor the production ML system.


> **Here is how DoorDash describes the motivation for building the ML monitoring:**
>
>
> "In the past, we've seen instances where our models became out-of-date and began making incorrect predictions. These problems impacted the business and customer experience negatively and forced the engineering team to spend a lot of effort investigating and fixing them. Finding this kind of model drift took a long time because we did not have a way to monitor for it".
>
>
> ‍ *Source: "*[Maintaining Machine Learning Model Accuracy Through Monitoring](https://doordash.engineering/2021/05/20/monitor-machine-learning-model-drift/) *," DoorDash Engineering Blog.*


We[once made](https://evidentlyai.com/blog/machine-learning-monitoring-what-it-is-and-how-it-differs) this iceberg image at the header of the blog. It was pretty well-received. Indeed, it does make a point. Monitoring ML models in production means more than tracking software performance. There are a bunch of other things!


But this iceberg is a binary classifier. We compare software-related aspects to the unseen "everything else" that makes ML monitoring different.


Let's now try to organize the rest. What should you look at?


##### \[fs-toc-omit\] **Get started with AI observability**


> Try our open-source library with over 25 million downloads, or sign up to Evidently Cloud to run no-code checks and bring all the team to a single workspace to collaborate on AI quality.[Sign up free ⟶](https://www.evidentlyai.com/register)[Or try open source ⟶](https://github.com/evidentlyai/evidently)


## Meet the monitoring pyramid!


Here is a way to structure all components of the ML system monitoring:


Let's quickly grasp what is there at each layer, bottom-up.


‍ **First, you still have the software backend.** Yep, you cannot ignore this: let's place it at the base of the pyramid.


To generate the predictions, you need to invoke the ML model somehow. A simpler example is **batch inference** . You can run it daily, hourly, or on-demand and use a workflow manager to orchestrate the process. It would access the data source, run the model and write the predictions to a database. The **online inference** is a bit more complex. You might wrap the model as a service and expose REST API to serve predictions at request. There are more moving pieces to track.


At the ground level, you still need to monitor how this software component works. Did the prediction job execute successfully? Did the service respond? Is it working fast enough?


‍ **Second, you have the data.** The production ML models take new data as input, and this data changes over time. There are also many[issues with data quality and integrity](https://evidentlyai.com/blog/machine-learning-monitoring-what-can-go-wrong-with-your-data) that might occur at the source or during transformation.


This data represents the model's reality, and you must monitor this crucial component. Is the data OK? Can you use it to generate predictions? Can you retrain the model using this data?


‍ **Third, you have the hero: the ML model itself.** Finally!


No model is perfect, and[no model lasts forever](https://evidentlyai.com/blog/machine-learning-monitoring-data-and-concept-drift) . Still, some of them are useful and relevant for the given task. Once the model is in production, you must ensure its quality remains satisfactory.


This model-focused component of ML monitoring is the most specific one. Is the model still fit for the task? Are the predictions accurate? Can you trust them?


‍ **Lastly, there is the business or product KPI.** No one uses ML to get "90% accuracy" in something. There is a business need behind it, such as converting users into buyers, making them click on something, getting better forecasts, decreasing delivery costs, etc. You need a dollar value assigned to the model, a measurable product metric, or the best proxy you can get.


That is the ultimate goal of why you have the ML system in place and the tip of the monitoring pyramid. Does the model bring value to the business? Are the product metrics affected by the model OK?


‍ **An ML system has these four components** : the software piece, the flowing data, the machine learning model, and the business reason for its existence.


Since an ML system is all of the above, ML monitoring has to be too.


> **Here is how Booking.com looks at the quality of the ML system as a whole: ‍** "Each model is part of a whole machine learning system where data is being preprocessed, predictions are being made, and finally, these predictions are used to influence the day-to-day operations of our business. Obviously, we want these models to be of good quality, but not only the models: the entire system needs to be of good quality to ensure long-lasting business impact".
>
>
> ‍ *Source: "*[A Quality Model for Machine Learning Systems](https://booking.ai/a-quality-model-for-machine-learning-systems-892118be9e19) *," Booking.com Data Science Blog.*


In this context, we mostly assume that you've trained your own machine learning model on your own data – like a demand forecasting or fraud detection system.


But the same ideas also apply if you are using pre-built models, such as large language models (LLMs) accessed via an API. For example, you might wrap an LLM call inside a document processing pipeline. Either way, the key challenges remain the same: monitoring input data quality, tracking drift, and making sure the model supports your business goals.


## What about the metrics?


This pyramid structure describes the hierarchy of ML monitoring. But it does not answer the "how." Which metrics should you calculate? Do you need a lot of them? ****


**Let's now review each level of the pyramid in more detail** . We will consider:


- The goals of each aspect of monitoring


- How specific it is to ML


- Examples of metrics and what impacts the choice


- What's hard about it


We'll not go into details of the logging and monitoring architecture but will primarily focus on the contents.


Let's dive in!


### 1. ML system health monitoring


Operational metrics help evaluate the software system's health. After all, it does not matter how great your ML model is if the system is down. ****


**What can go wrong with the software component in ML?** Pretty much anything that can go wrong with any other production system. Bugs in the code, human errors, infrastructure issues, you name it.


**What is the goal of ML software monitoring?** First, to know that the system is up and running and immediately intervene if it fails. Second, understand more fine-grained performance characteristics to ensure you comply with service level objectives. If necessary, you can make changes, like spinning up new instances as the service usage grows. **‍**


**How specific is this software monitoring to ML?** Business as usual. There is no difference between ML and non-ML software in this context. You can borrow the monitoring practices from the traditional software monitoring stack.


‍ **Who is usually on call?** A backend engineer or a site reliability engineer.


‍ **What metrics can you monitor? What impacts the choice?** ‍


The monitoring setup depends on the model deployment architecture, including:


- How you serve a model: batch jobs, real-time service, or streaming workflows.


- Whether you embed a model in an existing service or deploy a standalone one.


- How demanding your serving requirements are, e.g., you need low latency.


- Your ML deployment environment, e.g., in the cloud or on edge.


These factors affect not only the metrics choice but the overall monitoring system design.


‍ **In the simplest form, an ML system can look like a set of infrequent batch jobs.** In this case, you can remain in the data engineering realm and treat it, well, like any other data job. You can monitor execution time and job completion and set up a notification if they fail.


‍ **If you wrap the model as an API to serve predictions on request, you'd need more.** You will need to instrument your service to collect event-based metrics. You can then track various operational metrics of software and infrastructure health in real-time, including:


- **Service usage** metrics, such as the total number of model calls, RPS (requests per second), and error rates. The goal is to track model usage and be aware of failed inferences. For example, when the service fails to display the recommendation block and has to use a fallback. **‍**
- **System performance** metrics, such as uptime and latency. For example, you can look at p90 or p99 latency to know how long it usually takes to handle a prediction request. This is important for real-time use cases. Take a model that detects payment fraud: a fast model inference is critical for a good user experience. You don't want to wait minutes for your transaction to be approved. **‍**
- **Resource utilization** metrics, such as memory and GPU/CPU utilization.


[SRE terminology](https://sre.google/sre-book/service-level-objectives/) often refers to these operational metrics as SLIs (service level indicators). The idea is to carefully pick a few measures that quantify different aspects of service performance.


> **Here is how Linkedin monitors latency as part ML Health Assurance system:**
>
>
> "Model inference latency is an important metric for the application owners because this tells the overall time the model took in serving a particular scoring request. We typically monitor the mean, 50th, 75th, 90th, and 99th percentile latency. These quantiles for latency can be used in multiple ways, such as helping isolate the offending piece of a model within the entire lifecycle of a request."
>
>
> ‍ *Source: "*[Model Health Assurance platform at Linkedin](https://engineering.linkedin.com/blog/2021/model-health-assurance-at-linkedin) *," Linkedin Blog.*


### 2. ML data quality monitoring


Let's jump to the data level.


Say the ML service is up and running, and all jobs are completed smoothly. But what about the data that is flowing through? Data quality is the usual culprit of ML model failures. And, the next big thing to monitor!


What can go wrong with data for an ML model? A lot! Here is an incomplete list of[potential data quality issues](https://evidentlyai.com/blog/machine-learning-monitoring-what-can-go-wrong-with-your-data) . Some examples are:


- **Data schema change.** For example, an unannounced update in the 3-rd party data format.
- **A stale data source** . Imagine a broken physical sensor, an incorrectly implemented in-app event tracking, a source table that was not refreshed on time, etc.
- **A broken model upstream.** One model's broken output can be another model's broken input.


- **Unexpected inputs.** If you deal with a user-facing app, errors often happen, starting from simple typos when entering the information.


- **Broken pipelines and bugs** in the feature transformation code.


**What is the goal of data quality monitoring in ML?** To know that you can trust the data to generate the predictions and react if not. There is no use in the model if the data is broken or absent. You'd want to stop and use a fallback until you restore the data quality. In less extreme cases, you can proceed with the predictions but use the monitoring signal to investigate and resolve the issue.


**How specific is this data quality monitoring to ML?** Somewhat! Of course, you also need to monitor the data for other analytical use cases. You can re-use some of the existing approaches and tools. But this traditional data monitoring is often performed at a macro level, for example, when you monitor all data assets and flows in the warehouse.


**In contrast, ML data monitoring is granular.** You need to ensure that particular model inputs comply with expectations. You can still rely on existing upstream data quality monitoring in some cases. For example, if the model re-uses shared data tables already under guard. But often, you'll need to introduce additional checks to control for feature transformation steps, quality of real-time model input, or because the model uses an external data source.


In this sense, ML data quality monitoring is closer to data testing and validation that might exist in other data pipelines. It is often performed as checks on data ingestion before you serve the model.


Who is usually on call? A data engineer (if the issue is with infrastructure), or a data analyst or data scientist (if the issue is with the data "contents").


**What metrics can you monitor? What impacts the choice?**
The exact data monitoring setup again depends on:


- **The model deployment architecture** , be it batch, live service, or steaming workflows. It will affect the complexity of data quality monitoring. Detecting issues in the stream of data on the fly is different from checking a table that is updated once per day.


- **The specifics of the data and the real-world process behind it** . For example, in manufacturing, you might have stringent expectations of the possible value ranges for each feature. With user-generated inputs, you might rather keep tabs on the overall data shape and sanity checks to detect major issues.


- **Use case importance.** If the cost of failure is high, you might design elaborate data quality checks. You might also add online data quality validation that[returns a pass/fail](https://evidentlyai.com/blog/evidently-test-based-ml-monitoring) result before you act on predictions. In other cases, you might be okay with being reactive. You can throw some metrics on a dashboard (such as average values of specific features or share of missing data) to track changes over time.


*Example of the*[Evidently](https://github.com/evidentlyai/evidently) *data quality tests output.*


We can roughly split the types of data metrics and checks into several groups.


- **Missing data.** You can check for the lost data in particular features and the overall share of the missing data in the model's inputs.
- ‍ **Data schema validation.** You can verify if the input schema matches the defined expectations. The goal is to detect erroneous inputs and track issues like the appearance of new columns or categories. **‍**
- **Constraints on the individual feature types.** You can assert the specific feature type if, for example, you expect it only to be numerical. This can catch a share of input bugs, such as the feature arriving in the wrong format. **‍**
- **Constraints on the individual feature ranges.** One can often formulate expectations about the "normal" feature values. These can vary from sanity checks ("age" is less than 100) to domain-specific ones ("normal sensor operating conditions are between 10 and 12"). The violation of constraint can be a symptom of a data quality issue. **‍**
- **Feature statistics.** You can also track a particular feature's mean values, min-max ranges, standard deviation, or percentile distribution. In contrast to hard constraints, this can help expose less obvious failures. For example, the feature might stay within the expected range but start behaving abnormally. The real-world explanation might be that a physical sensor stopped working and the values are "frozen" at the latest measurement. If you deal with[text data](https://www.evidentlyai.com/blog/evidently-data-quality-monitoring-and-drift-detection-for-text-data) , you can track text length, the share of out-of-vocabulary words, etc. **‍**
- **Anomalous impacts.** You can also set up your monitors to detect "unusual" data, using anomaly and outlier detection approaches. They will search for data points different from others and might help catch corrupted inputs. You can focus on detecting individual outliers or track their overall rate.


*Example of the*[Evidently](https://github.com/evidentlyai/evidently) *data quality dashboard with descriptive statistics for a single feature.*


> **How Google designed a data validation system:** In the paper "[Data Validation for Machine Learning](https://research.google/pubs/pub47967/) ," the Google team presents how they designed a data validation system to evaluate and test the data fed into machine learning pipelines. They suggest a data-centric approach to ML, treating the data as an important production asset, together with the algorithm and infrastructure.


**What is tricky with ML data quality monitoring?**


- **Usually, the execution!** We all know that data quality is vital, but setting up monitoring can be quite time-consuming. To set up data quality constraints, you might need to codify expert domain knowledge that is external to the ML team, at scale. Ideally, you'd want to learn basic expectations from your training datasets automatically.


- **Multiple touchpoints.** You might also end up having a large number of checks, especially if you monitor both raw input data and feature values after post-processing. The key is to design the data quality monitoring framework to detect critical issues without being overwhelmed.
- **Data lineage.** After detecting a data quality issue, you'd need to trace back what caused it. This might be quite messy if you have many pipeline steps and transformations. You need the ability to connect the faulty data inputs back to the raw source or a particular component in the pipeline, which, for example, was not updated in time. Data quality monitoring is tightly connected to lineage and tracing; setting this up might require additional work.


### 3. ML model quality monitoring


Even if the software system works fine and the data quality is as expected, does this mean you are covered? Nope! Welcome to the land of ML model issues.


What can go wrong with ML models in production?[They drift](https://evidentlyai.com/blog/machine-learning-monitoring-data-and-concept-drift) !


Models can break abruptly in case of sudden change or start gradually performing worse. We can broadly split the causes into two:


- [Data drift](https://www.evidentlyai.com/ml-in-production/data-drift) : the model is applied to unseen inputs, such as users of new demographics.


- [Concept drift](https://www.evidentlyai.com/ml-in-production/concept-drift) **:** the real-world relationships change, such as evolving user behavior.


Here are some of the things that might cause model drift:


- **Shifts in the environment.** For example, an increase in the inflation rate (or the start of a pandemic!)


- **Deliberate business change.** You might launch an app in a new location or for a new user segment.


- **Adversarial adaptation.** This is usual for spam and fraud detection cases, as bad actors try to adapt to the model behavior.


- **Model feedback loop.** The model might itself influence reality. For example, the recommendation system affects what users see and click on.


- **A mismatch between the model design and usage.** For example, you can build a lead scoring model to predict conversion probabilities, but instead, users start using the model for scenario analysis. They try to feed different input combinations to learn which factors impact the model decisions. This is a different use case that requires a different analytical tool.


When the model drifts, you'd usually see an increase in the model error or the number of incorrect predictions. In the case of radical drifts, the model can become inadequate overnight.


**What is the goal of ML model quality monitoring?** To give you peace of mind that you trust the model and continue using it, and to alert you if something is wrong. A good monitoring setup should provide enough context to troubleshoot the model decay efficiently. You need to evaluate the root cause and[address the drift](https://evidentlyai.com/blog/ml-monitoring-data-drift-how-to-handle) , for example,[trigger the retraining](https://evidentlyai.com/blog/retrain-or-not-retrain) , rebuild the model or use a fallback strategy.


‍ **How specific is this to ML monitoring?** Entirely! This piece is pretty unique to ML systems. You can adapt some of the model monitoring practices from other industries, such as validation and governance of credit scoring models in finance. Otherwise, it is ML monitoring as you know it.


‍ **Who is usually on call?** A data scientist or a machine learning engineer. Whoever built the model and knows "what the feature X is about," or whom to ask about it.


‍ **What metrics can you monitor? What impacts the choice?**


**‍** The ML monitoring setup can vary. Here are some of the things that affect it:


- **The model and data types.** You always need some "accuracy" metric to evaluate the overall model quality. You can generally usually use the same metrics as in model training. But the specific list of metrics will vary depending on whether you have a regression, classification, or ranking model. Monitoring changes in tabular data is different from[tracking text data drift](https://www.evidentlyai.com/blog/tutorial-detecting-drift-in-text-data) and so on.
- **Ground truth delay** . You can evaluate the model quality directly if you get labeled data or feedback soon after the prediction. For example, when predicting the pizza delivery time, you will soon learn how long it took. If you predict sales for the next quarter, you'll need to wait to know how good these predictions were. When there is a long lag between prediction and feedback, you cannot calculate the quality itself. You might need to monitor proxy metrics such as data and prediction drift.
- **Model risks and importance.** The more important the model, the more granular your monitoring might be, and the more specific metrics (like fairness) and monitoring approaches (like detection of individual outliers) you might need to implement. If you have a lot of low-risk models, you can monitor only standard metrics relevant to the model type.


*Example of the*[Evidently](https://github.com/evidentlyai/evidently) *performance dashboard from*[CS 329S tutorial](https://evidentlyai.com/blog/tutorial-evidently-ml-monitoring-cs329s) *.*


There are probably hundreds of different metrics you can calculate! Let's try to group them for a quick outlook.


**Model quality metrics.** This group of metrics evaluates the true quality of the model predictions. You can calculate them once you have the ground truth or feedback (e.g., data on clicks, purchases, delivery time, etc.) Here are some examples:


- Regression: MAE (mean absolute error), RMSE (root mean squared error).
- Classification:[accuracy](https://www.evidentlyai.com/classification-metrics/accuracy-precision-recall) ,[precision](https://www.evidentlyai.com/classification-metrics/accuracy-precision-recall) , F1-score.
- Ranking: top-k accuracy, MAP (mean average precision).


**Model quality by segment.** Aggregate metrics are essential but are often not enough. You might have 90% overall accuracy but only 60% in some important subpopulations like new users. To detect such discrepancies, you can track the model quality for the known segments in data (for example, accuracy for different geographical locations) or proactively search for underperforming segments.


**Prediction drift.** This is the first type of proxy quality monitoring. If you don't know how good your model is, you can at least keep tabs on how different its predictions are. Imagine that a spam detection model suddenly starts assigning the "spam" label in every second prediction. You can raise alarms even before you get the true labels. To evaluate prediction drift, you can[use different drift detection approaches](https://evidentlyai.com/blog/data-drift-detection-large-datasets) :


- Track the descriptive statistics of the model output (e.g., mean predicted values, standard deviation).
- Apply statistical tests (e.g., Kolmogorov-Smirnov test, Chi-squared test) to compare the most recent model outputs with the older ones.
- Use probability distance metrics to compare distributions (e.g., Wasserstein distance).


**Input data drift.** In addition to the prediction drift, you can monitor the shifts in the input data and[interpret them together](https://evidentlyai.com/blog/data-and-prediction-drift) . The goal is to detect situations when the model operates in an unfamiliar environment, as seen from the data. The detection approach is similar to the prediction drift. You can monitor descriptive stats for the individual features (such as frequencies of categories), run statistical tests or use distance metrics to detect distribution shifts. You can also track specific patterns, such as changes in linear correlations between features and predictions.


*Example of the*[Evidently](https://github.com/evidentlyai/evidently) *data drift dashboard from*[CS 329S tutorial](https://evidentlyai.com/blog/tutorial-evidently-ml-monitoring-cs329s) *.*


**Outliers.** You can detect individual cases that appear unusual and where the model might not work as expected. This is[different from data drift](https://evidentlyai.com/blog/ml-monitoring-drift-detection-vs-outlier-detection) , where the goal is to detect the overall distribution shift. You can, of course, still use the rate of outliers as a metric to plot and alert on. But the goal of outlier detection is usually to identify individual anomalous inputs and act on them, for example, flag them for expert review. You can use different statistical methods, such as isolation forests or distance metrics, to detect them.


**Fairness.** This is a specific dimension of the model quality, dictated by the use case importance and risks. If ML decisions have serious implications as they often do in finance, healthcare, and education use cases, you might need to ensure that the model performs equally well for different demographic groups. There are different metrics to evaluate model bias, such as demographic parity or equalized odds. It is particularly important to track these metrics if you have automated model retraining, and its behavior can deviate with time.


You can notice some of the metrics, such as feature stats and outliers, appear in both data quality and model quality contexts. ML and data monitoring often come hand in hand as you look at the data anyway. However, the ML-focused part of the monitoring looks at the data to evaluate model relevance. In contrast, the data quality-focused part of the monitoring looks for corruption and errors in the data itself.


‍ **What is tricky with ML monitoring?** ‍


There are hardly any blueprints!


- **Model quality is context-specific.** There is no "standard accuracy level" or "obvious data drift." Model quality expectations depend on the use case. For example, you might care a lot or very little about the drift in individual features. Some models have seasonality; you should consider it when setting comparison windows. In some cases, you'd want to aggregate your data over minutes; in others, over days. The bottom line is that you need to understand the model and data to pick the right monitoring metrics and heuristics.


- **Monitoring without ground truth.** This half-blind monitoring is very particular to ML. You might also have partial feedback (for example, when experts manually check only some predictions) or ground truth labels arriving at different intervals. Defining a suitable proxy for model quality without getting an overwhelming number of alarms is hard.


- **Computing metrics at scale.** Calculating accuracy over a small set of sitting data might be trivial. However, it becomes more complex with metrics calculation at scale. For example, how do you calculate statistical metrics in a distributed manner and do it fast? It is not so easy to build a scalable, standardized ML monitoring infrastructure, especially for real-time applications.


> **How to implement ML monitoring:**
>
>
> We work to implement some of the best practices in ML monitoring in Evidently,[an open-source ML monitoring toolset](https://github.com/evidentlyai/evidently) we created. If that is something you are looking to solve, jump on[our Discord community](https://discord.com/invite/PyAJuUD5mB) to chat and share or[test out the tool](https://docs.evidentlyai.com/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=ml-monitoring-metrics) on GitHub!


### 4. Business metrics and KPI


The business value, at last!


To judge the performance of the ML model, you ultimately need to tie it to the business KPI. Is the ML model doing the job it is built for?


**What is the goal of business metrics monitoring?** To estimate the business value of the ML system and adjust if things go off track. There is always a risk of a mismatch between the ML model quality and the business value. This can happen due to changing reality, or if the model is not used in the way it was designed (or is not used at all!)


‍ **How specific is this to ML monitoring?** This part is strictly business-specific. The metrics and the way you measure them are all over the board. It can range from tracking engagement metrics in a web app to evaluating savings of raw materials in an industrial plant. It all boils down to the business use case. **‍**


**Who is usually on call?** A product manager, a business owner, or an operational team, together with the data scientists, to bridge the gap.


‍ **What can you monitor?** ‍


- **Direct business KPIs.** You might keep an eye on the immediate business and product metrics. For example, the click-through rate on a page or the volume of raw materials used in a production process (if that is what the ML system helps optimize!). If you are lucky, you might be able to track the business impact of the ML solution just by looking at these metrics. For example, if the ML system exists as a standalone application, you can directly attribute some revenue or savings to it. **‍**
- **A/B test performance.** The target business metrics are often affected by multiple factors. A good old A/B test can help measure the ML system's isolated impact. For example, you can split your traffic to compare the performance of the ML-based recommendation block against the baseline. Does the ML system bring more sales than simply recommending the top 10 most popular items? You might even run this continuously if you have enough traffic, keeping a small portion away from the ML. This can be justified if you are dealing with a new and high-risk area and are concerned with long-term model impact (such as the model affecting the environment). **‍**
- **Indirect metrics.** Sometimes it's hard to tie the ML performance to the topline business metrics such as revenue or conversions. You can still evaluate secondary metrics that reflect the ML model quality in this case. If you have an ML-enabled feature, you can track how often it is used. If you recommend content to the user, you can track engagement and how long they stay on the page after clicking on the recommendation block. If ML helps sort the support tickets, you can track the number of complaints and time to resolution, and so on. You can also introduce some interpretable checks that would help the business owners understand what the model is doing, for example, how many loans are approved.


> **Some advice from the Nubank ML team on monitoring the policy layer: ‍** "Monitor the decisions made using the model. For example: how many people got loans approved by the risk model on each day? How many people had their accounts blocked by the fraud model on each day? It's often useful to monitor both absolute and relative values here".
>
>
> ‍ *Source: "*[ML Model Monitoring – 9 Tips From the Trenches](https://building.nubank.com.br/ml-model-monitoring-9-tips-from-the-trenches/) *," Nubank Blog.*


What is tricky with monitoring business KPIs?


- **You can't always measure them.** Conducting an A/B test can be too expensive, complex, or unfeasible. Often, you cannot isolate the impact of ML systems as multiple factors affect the target KPI. In cases like credit scoring, it can take months or years to evaluate the business impact of making a loan decision. You should always try to find some proxy, or at least a sanity check that business stakeholders can interpret. But you can't always get the value in dollars and cents fast.


- **Business metrics give the least context in the case of model decay.** If the metric is down, you must go through every pyramid layer to sort it out. Is it the model? Is it the data? Is it the software? Business metrics are the most critical indicators of value (they sit at the top of the pyramid for a reason). Still, you need more observability to get timely alerts and debug the system efficiently.


Bottom line: track them if you can, but track other things, too.


## Do you need all of that?


We know, that was a lot!


The goal of this overview was to introduce all the different aspects of the production ML system. Once the ML application is deployed, it is no longer just a model but a complex system made of data, code, infrastructure, and the surrounding environment. You need to monitor it as a whole.


This does not mean that you should look at dozens of metrics and plots all the time.


First of all, there might be **several dashboards or views** used by different people on the team.


You can perform operational monitoring in the existing backend monitoring tool. You can visualize product metrics in the BI dashboard your business stakeholders already use. Depending on the setup, you can add ML monitoring metrics[to the same Grafana](https://evidentlyai.com/blog/evidently-and-grafana-ml-monitoring-live-dashboards) dashboard, check them through a[pipeline test](https://evidentlyai.com/blog/evidently-test-based-ml-monitoring) orchestrated by Airflow, or spin up a standalone dashboard used by the ML team. Different aspects of ML monitoring have different internal users (and problem-solvers). It's fine to have multiple dashboards.


> **Here is the case from Monzo: ‍** Understanding the live performance of a model is a critical part of the model development process. For this stage, we lean on our *reuse over rebuild* principle and have adopted tools that are used across the company. We wanted our monitoring tools to be available to everyone, including people outside of machine learning.
>
>
> ‍ *Source: "*[Monzo's machine learning stack](https://monzo.com/blog/2022/04/26/monzos-machine-learning-stack) *," Monzo Blog.*


Second, you should distinguish **between monitoring and debugging** . You might proactively monitor and introduce alerts only on a handful of metrics most indicative of the service performance. Your goal is to be informed about a potential problem. The rest of the metrics and plots would be helpful during debugging as they provide the necessary context. But you won't actively set alerts or define specific thresholds for them.


For example, if you get your model feedback fast, you might skip alerting on the feature drift. You can evaluate the model quality itself, after all. But if you notice a performance drop, you would need to identify the reasons and decide[how to handle drift](https://evidentlyai.com/blog/ml-monitoring-data-drift-how-to-handle) . It might make sense to pre-build the distribution visualizations for the important features or have an easy way to spin it on demand. In other cases, you might prefer to monitor for data drift,[even if you get the labels](https://evidentlyai.com/blog/ml-monitoring-do-i-need-data-drift) . It depends!


To sum up, the goal of monitoring is to give confidence that the system is running well and alert if not. In the event of failure, you'd need the necessary context to diagnose and solve the problem, and that's where the extra metrics come in handy, but you don't need to look at them all the time.


## Let's recap


ML monitoring means monitoring an ML **system** . To observe and evaluate its performance, you usually need a bunch of metrics that describe the system state. There are several facets of the system to look at.


We can group them into:


- **Software system health.** You can monitor it in the same way as other backend services. It is not specific to data science and machine learning but still requires an appropriate setup.


- **Data quality and integrity.** One should look at the data flowing through a particular ML prediction pipeline. The goal is to check it's not corrupt or broken and that you can still trust this data to become your model inputs. You can often introduce pipeline tests to validate the input data before generating the prediction.


- **ML model quality.** This is the most ML-specific component of monitoring. You want to keep an eye on model relevance to ensure it stays fit for the task. If you have delayed feedback, you might resort to monitoring data and prediction drift as a proxy for model quality.


- **Business KPI.** The business or product metric is the ultimate measure of the model ROI and value but is often the hardest to measure.


**You probably don't need to look in detail at every pyramid layer.** They might have different internal consumers, between backend engineers, data engineering, ML team, and business stakeholders.


The exact monitoring strategy will also depend on whether the model is batch or real-time, how quickly you get the ground truth labels, how critical the model is, and the associated risks. You'd probably use some metrics for the actual monitoring (set an alert on them) while making others available for reporting and debugging purposes (for example, pre-compute and store them somewhere).


Here is a summary with some examples of metrics:


## What's next?


We'll continue our deep dive into the ML monitoring theory and practice. In the following blogs, we'll cover the following:


- [A pragmatic ML monitoring setup](https://www.evidentlyai.com/blog/pragmatic-ml-monitoring-metrics) **:** how to prioritize metrics when you set up monitoring for your first model.


- **ML monitoring architecture:** how to design an ML monitoring system for batch and real-time models.
