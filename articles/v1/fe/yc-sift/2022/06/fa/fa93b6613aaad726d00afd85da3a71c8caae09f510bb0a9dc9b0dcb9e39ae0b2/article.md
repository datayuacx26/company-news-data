---
schema_version: "1.0.0"
document_id: "fa93b6613aaad726d00afd85da3a71c8caae09f510bb0a9dc9b0dcb9e39ae0b2"
company_key: "yc-sift"
company: "Sift"
source_id: "yc-sift-rss-afa0f6dcd9d7"
canonical_url: "https://engineering.sift.com/continuous-ml-improvements-sift-scale/"
published_at: "2022-06-09T18:33:02+00:00"
first_seen_at: "2026-08-12T04:14:07.509173+00:00"
fetched_at: "2026-08-12T04:14:09.389902+00:00"
content_hash: "sha256:f9d669eafd033d4c4c428e983762be58145856b8613bb6baec7b11468e331620"
---

# Continuous ML Improvements @ Sift Scale

Post category:


[Articles](https://engineering.sift.com/articles/)


# [Continuous ML Improvements @ Sift Scale](https://engineering.sift.com/continuous-ml-improvements-sift-scale/)


Michel Goldstein


• June 9, 2022


The SaaS and fraud space is dynamic and ever evolving. Customers, old and new,


bring new fraud use cases that we need to adjust for. Fraudsters are also coming up with new fraud patterns and vectors of attack. That requires us to continuously update our models, experiment with new data sources, new algorithms, etc. We have to make decisions on which of these changes in our machine learning system will be more effective without disrupting our customers’ workflows. This decision making at scale—what we call the “Sift Scale”—sometimes can be challenging considering our large customer base and variety of verticals with peta-bytes of data.


Our ML-based fraud detection service processes over 40,000 events per second. These events represent mobile devices or web browser activity on our customers’ websites and apps. They also come from a variety of sources, from end-user devices to our customers’ back-end servers. From those events, we evaluate multiple ML models and generate our trust and safety score. We evaluate over 6,000 ML models per second, for a total of over 2,000 score calculations per second, and run more than 500 workflows per second. We have customers from many different verticals, from food delivery to fintech. Our customers integrate with Sift in a variety of ways, synchronous to asynchronous integrations, in


tegrations with 3-5 event types to complex integration with over 20 different event types. This allows us to detect different types of fraud (payment fraud, account takeover, content spam & scams, etc.).


In this environment, it is often challenging to make a change and ensure that, from a technical standpoint, we understand the impact it can have on all those integrations and fraud use cases. This is especially relevant when it comes to making changes in our ML models, as they are the core from which our customers use Sift for their trust and safety needs.


To better illustrate the variation on those needs, let’s walk through a couple of common integration patterns we see with our system for e-commerce use cases:


- Customer A is a regional e-commerce website and they send us a minimum set of events, triggering a fraud check before a transaction to determine whether they are going to try to charge the credit card or reject the order. They have a fairly simple rule of blocking all orders with a score higher than 90, and manual review of all scores between 80 and 90.


- Customer B is a well-established multinational e-commerce website with clear country-separated operations and rules. They send us more of their internal information that they have been collecting specifically for the regions they operate and they have fine-tuned rules per region. For example, in the U.S. they may block at a score that gives them a 0.5% block rate (0.5% of orders will be considered fraudulent and rejected), while in Germany they are more conservative and block at 0.7%. Their objective is to maintain a well-tuned risk model around an expected percentage of orders that are fraudulent, known as fraud rate, per region. Ultimately their goal is to keep their chargebacks low.


- Customer C is a high-growth company that is quickly expanding on different geographies with a small Digital Trust & Safety team, being nimble on their setup of score thresholds based on current promotions, expansion and reacting to emerging fraud trends. They mostly have a single threshold globally, but from time to time they would have exceptions for specific geographies or products being promoted.


Different changes will impact those customers differently, as they automate on our scores at diff


erent subsets of data and have different expectations of the immutables in our system (fraud risk at score, or block rate at score, or manual review traffic at score). So how do we handle changes in such an environment?


**Step #1: Have a reason for the change** . All changes should come with a hypothesis of what this change should bring to our customers and system. All changes should have a business value worth doing, and should have measurable objectives.


**Step #2: Engage with stakeholders** . We engage with customers or a proxy through members of our technical services team. They will evaluate the reason for the change and then help in determining how we should evaluate the impact on how our customers use the system.


**Step #3: Ensure visibility on the areas of impact.** We sometimes need to add specific metrics and monitoring to ensure that the risks identified in step #2 can be validated after deployment.


**Step #4: Have a roll back strategy.** Sometimes some changes to ML systems, such as changes to feature extraction that have downstream stateful effects, can make rolling back more challenging. We will always have to discuss the rollback path in case we decide the customer impact is higher than the value identified in step 1.


**Step #5: Deploy and monitor for impact risk and the objective.** The deployment happens and we will monitor for the risks identified before. We will also monitor to ensure that we hit the objective we were aiming for with the change. If we detect an undesirable result for a customer and still want to meet the business objective of the change, it helps us determine whether we want to work with the customer to mitigate their impact, or roll back the change.


**Step #6: Share learnings.** Our goal is to ensure our customers benefit by having higher accuracy and broader fraud use case coverage. After following a fairly thorough process of rolling out any change, if we observe some unplanned results (good or bad), we incorporate learnings from those cases in our Sift Scale. We also share those learnings with our customers so we can better prepare for our next rollout with the next set of decisions.


This multi-step due diligence provides us the guardrails for deploying new ML models and/or infrastructure changes. We couldn’t be more grateful for our customers for their strong partnership to ensure we are staying one-step ahead of the new fraud trends.


## Author


-


[Michel Goldstein](https://engineering.sift.com/author/mgoldsteinsiftscience-com/)
