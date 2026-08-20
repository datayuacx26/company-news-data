---
schema_version: "1.0.0"
document_id: "7d627b7409525e36c73af0b4e87e8d629afb4470f58629bda933372a2fe51830"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/detecting-faulty-deployments/"
published_at: "2025-06-03T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:1a628347e9400706042fe7abb3622ef353f9aed9f7f469cde3a3854dcaf9fc1e"
---

# Detecting faulty deployments: Our journey from unlabeled data to supervised learning

David Asker


Sebastien Levy


Deployments are the backbone of software development, but they can be scary: Even when code is well tested, bugs can creep in. In today’s fast-paced tech landscape, deployments need to happen more frequently, at a wider scale, and in many different data centers. But these requirements also mean that bugs are introduced more frequently and at a wider scale, often leading to high-severity incidents. According to[Google SRE](https://sre.google/sre-book/introduction/) , deployments account for approximately 70 percent of incidents. Therefore, it is essential to identify and address bad deployments as promptly as possible.


In this post, we’ll share how we developed and improved[Datadog Automatic Faulty Deployment Detection](https://docs.datadoghq.com/watchdog/faulty_deployment_detection/) , a feature that quickly identifies faulty deployments in services that are tracked by[Datadog Application Performance Monitoring (APM)](https://docs.datadoghq.com/tracing/) . We’ll walk you through the various stages of our journey, from starting with a large, unlabeled dataset of service changes to developing a supervised learning model by using weak supervision. We’ll explore common challenges in building real-world data science models, such as dealing with a lack of labels and data imbalance, and explain how we addressed these issues through an iterative unsupervised approach. Although we have tested this methodology for deployments only, we believe that you can apply it to many other use cases that present similar challenges.


## The challenges: Lack of labels, data imbalance, and diversity of profiles


To detect faulty deployments, engineers examine varied sources of data: requests, errors, previous deployments, and other telemetry. No ground truth dataset is available to train a model to detect faulty deployments because different people have different definitions of what *faulty* means in the context of a deployment. The definition depends on your application. Even if we could get some annotated faulty deployments, the results would likely not be widely applicable to different types of applications. This absence of clear labels forced us to look into unsupervised solutions.


As is the case with many other failure detection models, we also had to deal with data imbalance because faulty deployments are relatively rare events. We knew that manually annotating randomly sampled deployments would likely yield very few or no examples of faulty deployments. This imbalance introduced an additional challenge to the modeling process: Even if our false positive rate (the ratio of false positives to all non-faulty deployments) turned out to be quite low, our precision (the ratio of faulty deployments to all predicted faulty deployments) also could be relatively low.


Another big challenge was the diversity of applications that we needed to support. The definition of *faulty* would likely be very different for highly seasonal applications (where errors would naturally occur at periods with more requests) compared with applications that have very low traffic (where it would take more time to detect faulty versions). For applications in which new versions are deployed every couple of hours, it might be difficult to pinpoint which deployment caused an increase in errors, or if the increase came from a root cause other than a deployment.


## Defining a faulty deployment


Before we could detect faulty deployments, we needed to define them. Because the most impactful bad deployments lead to an increase in errors, we focused on identifying deployments characterized by a significant increase in error rate.


To narrow the definition, we considered three main attributes:


-


**Impact** : We cared about only the deployments where the total number of errors was high enough relative to the baseline and where the increase in error rate was significantly higher than in previous versions.


-


**Temporal correlation** : To be confident that the increase in error rate was caused by the deployment, we wanted to validate that the increase aligned with the introduction of the new version.


-


**Persistence** : Error rate is a relatively noisy metric. Small increases can often be attributed to other factors, such as the deployment process itself. We considered a deployment faulty only when an increased error rate was sustained over time.


## Implementing an iterative framework


After we defined the attributes of faulty deployments, we needed a way to automatically measure them. Our initial approach was to create simple statistical rules for each attribute and apply them to the first 60 minutes of data that followed each deployment. This time window was necessary to accumulate enough data to measure the persistence of errors. We then manually annotated our results to verify that our initial predictions were correct, allowing us to measure the precision of our method. However, this approach had two major drawbacks: It required a considerable amount of human annotation, and it did not provide any estimation of recall.


To overcome these challenges, we developed an iterative framework. The objective of the framework was to progressively refine a set of statistical checks that validated different requirements of a faulty deployment. A check could be as simple as a statistical comparison of the error rate before and after a deployment, or a comparison of a new deployment to the previous deployments. A check could also address specific types of applications, such as applications that have periodic hits and errors or applications that have a sparse traffic pattern.


We then assembled the checks into an ensemble model with unanimous voting, where a deployment was considered faulty if all the checks flagged it as such. The framework followed these steps:


1.


We started with a simple set of a few checks, focusing on high recall with potentially lower precision.


2.


We manually labeled a sample of predicted faulty deployments to assess the precision of that initial set of checks. We then analyzed the false positives (the deployments wrongly labeled as faulty) to identify additional requirements for the ensemble model within our three main attributes.


3.


We iteratively added checks to improve precision and re-tuned the threshold of the existing checks to improve recall.


The original unsupervised model used an ensemble of checks to evaluate deployments after 60 minutes. The original unsupervised model used an ensemble of checks to evaluate deployments after 60 minutes.


By using the iterative framework, we kept a high level of precision while iteratively improving recall. To further improve recall and identify more false negatives (faulty deployments that the model did not identify), we also used incident information and version rollbacks that are typically associated with faulty deployments.


## Managing the trade-off between time to detection and recall


The iterative approach helped us build a well-performing unsupervised model. We started with a simple model and then increased the complexity to account for more nuanced use cases such as error and hit periodicity, hit sparsity, and concurrent versions. However, as the complexity grew, we faced an increasing challenge in adjusting the model for one behavior without causing a drop in performance for another behavior. Adapting to new types of faulty deployments seemed almost impossible.


An even more important problem was time to detection. While our approach helped find a good trade-off between precision and recall, we needed to wait 60 minutes before the model could give a confident decision—too late to be useful in many cases. While you could wait that long to be alerted if the error pattern is subtle (for example, a slow rollout or a small increase in error rate), you would want to be alerted quickly when there is an obvious issue with a new version. The trade-off was now between precision, recall, and time to detection.


We decided to solve this problem by defining a sequence of models that ran at different times (for example, 10 minutes, 20 minutes, and 60 minutes) after a deployment. Each model was tuned differently. All the models needed high precision, and we expected recall to increase as time elapsed from the start of a new deployment. This logic followed the intuition that with limited data (close to the rollout), we could detect only a small subset of faulty deployments: the more obvious ones. Then, as we accumulated more observations, we could identify a growing number of the more subtle faulty deployments.


In this iteration of the solution, three models evaluated deployments after 10 minutes, 20 minutes, and 60 minutes, respectively, to improve time to detection. In this iteration of the solution, three models evaluated deployments after 10 minutes, 20 minutes, and 60 minutes, respectively, to improve time to detection.


## Switching to supervised learning


Although sequential modeling seemed reasonable, we quickly realized that it would be tedious to achieve with our unsupervised approach. We would need to manually adjust every check to adapt to the amount of available data at different times from the new deployment. We also would need to tune the parameters to keep high precision with varying levels of recall.


Those factors motivated us to drastically change our approach. Could we train each sequential model to predict the output of the ensemble model in a supervised fashion?


We already had all the information available. Our ensemble model, which ran 60 minutes after every deployment, could provide us with high-quality labels that assessed whether the deployments turned out to be faulty. Additionally, we could use various components of the statistical checks that we computed at 10 minutes and 20 minutes after a deployment as input features for the model.


We used this two-step approach to gather more information to train our supervised model. We also added lower-level features to our feature set to describe specific characteristics of a deployment. We then ran an additional job to compute labels 180 minutes after the deployment to better handle cases where, even after an hour, it was unclear whether a deployment was faulty.


At that point, our objective became the following: Could we predict with this limited early data (10 minutes or 20 minutes after the deployment) whether the ensemble model would consider this deployment faulty 180 minutes after the deployment?


We used our new sets of features and labels with some relevant sampling and a feature selection layer to train two traditional machine learning models. The models ran 10 minutes and 20 minutes, respectively, after each deployment. For the prediction of whether a deployment was faulty, we used a random forest.


This iteration of the solution used unsupervised models 60 minutes and 180 minutes after a deployment to get labels of faulty or not faulty. Those labels and a new set of features were used to train supervised models that ran 10 minutes and 20 minutes after a deployment. This iteration of the solution used unsupervised models 60 minutes and 180 minutes after a deployment to get labels of faulty or not faulty. Those labels and a new set of features were used to train supervised models that ran 10 minutes and 20 minutes after a deployment.


## Improving label quality through weak supervision


This approach of training a model to predict later results at an earlier timestamp allowed us to improve our time to detection and recall. However, the method had an obvious inherent limitation: If the later results were wrong, the predictive model would learn to reproduce those mistakes. To make big improvements in model accuracy, we needed higher-quality labels.


We could have tried to obtain those labels manually by tediously hand-annotating deployment health. However, judging the health of a deployment can be complicated, slow, and difficult enough to require expert knowledge. In addition, while you can estimate precision by annotating predicted faulty deployments, finding missed predictions (false negatives) is as difficult as finding a needle in a haystack. Faulty deployments are rare events, and finding bad deployments by sampling from deployments predicted to be healthy would be extremely slow and laborious. It was clear to us that such a process was not a scalable way to improve the quality of our labels.


Instead, we tackled this problem by using[weak supervision](https://ai.stanford.edu/blog/weak-supervision/) . Weak supervision is a framework for supervised learning in which authoritative labels are not available, but some *weak* labels are available. These weak labels might have limited coverage (they might be available for only a subset of observations) or limited accuracy (they have some probability of being wrong).


We could have aggregated these imperfect labels by using a voting method, but the weak supervision framework allowed us to go a step further and learn how accurate each weak label was. We accomplished this task in two steps. First, we directly modeled the coverage and accuracy of each type of weak label. Then, we used a maximum likelihood method to estimate what the most likely label would be. If we found enough high-quality weak labels, we then inferred a high-accuracy strong label for each observation.


We took the components of our earlier rule-based model and considered each rule a weak label in this framework. We then supplemented those rules with external information from Datadog APM about the deployments, such as whether they were subsequently rolled back to a previous version, whether they were unusually short-lived, whether they contained new error signatures, and so on. By applying weak supervision on this information and our handwritten statistical rules, we obtained higher-quality labels for training. The results were additional improvements in the precision and recall of our models.


In this iteration of the solution, weak labels came from the unsupervised models and external information about each deployment. A weak supervision model then combined those labels to make strong labels that were used to train the models that ran 10 minutes and 20 minutes after a deployment. In this iteration of the solution, weak labels came from the unsupervised models and external information about each deployment. A weak supervision model then combined those labels to make strong labels that were used to train the models that ran 10 minutes and 20 minutes after a deployment.


## Results


Generally, we can use metrics such as precision, recall, and accuracy to evaluate supervised models. However, our case was different. Because the labels were already the output of our weak supervision model, directly using them for traditional classification metrics would propagate errors from the weak supervision.


To avoid that situation, we decided to evaluate our models differently. We separated the evaluation into two parts. First, we evaluated the labels estimated from our weak supervision framework. Separately, we evaluated our sequence of supervised models to determine how quickly they detected deployments that were faulty (in other words, our time to detection).


### Weak supervision labels


To evaluate the labels estimated by the weak supervision framework, we typically would need to manually annotate enough deployments and then compute precision and recall of our labels compared to ground truth from the annotated deployments. Given how time-consuming this process can be, we instead focused on evaluating the weak supervision labels by comparing them to the labels from the previous method (use of the latest unsupervised model, after 180 minutes). We conducted this evaluation by having a set of experts manually annotate a sample of cases where the labels from the two methods differed. (We could assume in most cases that if both models agreed, it was highly likely that they were both correct.)


We’ve summarized our results in table 1.


Metric Percent improvement


Precision on disagreements 11


Recall on disagreements 31


### Sequential supervised models


Next, let’s examine the improvements that we obtained by using incremental supervised models (at 10 minutes after a deployment and 20 minutes after a deployment), with and without weak supervision. To compare all the models together, we used the output of the weak supervision model as labels. Because we tuned all the models to have a similar level of precision, we cared about recall only.


Instead of using recall directly—which, again, would be only an estimation because we didn’t have ground truth labels—we focused on the *percentage of full coverage* . The percentage of full coverage is defined as the proportion of all faulty deployments that were identified at a given time delay, with and without using weak supervision.


Without weak supervision, we got 21.5 percent of the full coverage at 10 minutes and 25.9 percent of the full coverage at 20 minutes. With weak supervision, those numbers improved to 24.7 percent and 37.1 percent, respectively. The original unsupervised model provided 62.9 percent of the full coverage at 60 minutes. Details are available in table 2.


Detection method Percentage of full coverage without weak supervision Percentage of full coverage with weak supervision


Supervised model at 10 minutes 21.5 24.7


Supervised model at 20 minutes 25.9 37.1


Original unsupervised model at 60 minutes 62.9 Did not use weak supervision


Labels obtained after the fact 71.9 100


Supervision and weak supervision produced improvements in time to detection from the original unsupervised model that ran 60 minutes after a deployment. We obtained a 35 percent improvement in time to detection with the sequence of the supervised model that ran 10 minutes after a deployment, the supervised model that ran 20 minutes after a deployment, and the unsupervised model that ran 60 minutes after a deployment. When we added weak supervision, we improved time to detection by 45 percent for that same combination of models. The results are available in table 3.


Sequence of models used Time to detection


Unsupervised model at 60 minutes 1.0x (baseline)


Supervised model at 20 minutes and unsupervised model at 60 minutes 0.72x (28 percent improvement)


Supervised model at 10 minutes, supervised model at 20 minutes, and unsupervised model at 60 minutes 0.65x (35 percent improvement)


Weakly supervised model at 10 minutes, weakly supervised model at 20 minutes, and unsupervised model at 60 minutes 0.55x (45 percent improvement)


## Conclusion


Detecting faulty deployments in APM-tracked services is a complex task. But when we implemented an iterative framework and then shifted toward supervised learning, the task became more achievable to us. These efforts helped us improve our precision, recall, and time to detection. With our new approach, Datadog customers can now receive early alerts about faulty deployments. Additionally,[Bits AI](https://www.datadoghq.com/blog/bits-ai-autonomous-investigations/) , our autonomous investigation agent, can identify faulty deployments for customers during incident responses.


Overall, this project offers insights into how data science and machine learning can contribute to improving the reliability and performance of software deployments. But we believe that the project’s scope should not end there. Many of the techniques that we employed can be reused in different settings:


-


Our iterative approach can be a great way to explore complex classification problems that require in-depth data analysis so that you can estimate unobservable labels.


-


Sequential models can be reused in problems where the gradual availability of data creates trade-offs between precision, recall, and time to detection.


-


Weak supervision coupled with strong domain expertise can help solve many classification problems where labels aren’t readily available.


Interested in working on projects such as this one? Datadog is[hiring](https://careers.datadoghq.com/engineering/?child_department_Engineering%5B0%5D=Data%20Science) .


-
-
-
