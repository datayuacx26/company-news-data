---
schema_version: "1.0.0"
document_id: "e6b0046d400b51280bbc87a752ef9967c2c442a5681f6f3b7f14c1777af67b9d"
company_key: "taboola-com-ltd-ordinary-shares"
company: "Taboola.com Ltd."
source_id: "taboola-com-ltd-ordinary-shares-news-import-1349b39c90c0"
canonical_url: "https://www.taboola.com/engineering/predicting-ad-intent/"
published_at: "2025-02-25T08:00:37+00:00"
first_seen_at: "2026-07-22T15:33:14.584366+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:df7479d9667cd2b95fddf637f309746e0559d3e2999fe7f5690fd98e799a5e48"
---

# From Zero Labels to 85% Accuracy: Our Journey in Conversion Intent Prediction

## Introduction


Understanding the commercial intent behind online content is essential for optimizing ad targeting and maximizing revenue. At Taboola, we strive to deliver more relevant advertisements by aligning ads with the intent of the content. This strategic alignment can help improve user engagement, may increase conversion rates, and aims to satisfy both publishers and advertisers.


By identifying whether a web page encourages visitors to take action—a concept we refer to as Conversion Intent (CI)—we can tailor our ad placements more effectively. For example, an article titled **“The Top 10 Running Shoes for 2024”** signals strong commercial intent, indicating that readers might be in a buying mindset. Placing relevant ads in such content increases the likelihood of a sale, potentially benefiting the reader, advertiser, and publisher.


Conversely, a headline like “They Took The Same Picture For 40 Years (You Will Surely Get Emotional)” may not exhibit explicit commercial intent. Recognizing this allows us to adjust our advertising approach to maintain a seamless and engaging user experience without disrupting the reader’s journey.


Conversion Intent (CI) may be key to ensuring our recommendation systems deliver relevant advertisements to publishers. Additionally, CI can serve as a useful filtering parameter within our Contextual Targeting product. For example, an advertiser promoting sports shoes could target placements where the category is “sports/basketball” and CI is marked as true. This approach ensures their ads are shown to audiences with a higher likelihood of conversion.


The goal for this project was to build a model that predicts CI with 80–90% accuracy while minimizing effort and resources. Due to time constraints and the high cost of human annotations, we ruled out manually labeled data as an option.


## Development Process


I approached the task during a month without utilizing human labels. The development process comprised three main steps:


1. **Investigate Previous Work**
2. **Try Existing Models**
3. **Improve Based on Domain Knowledge**


Method 1 – Previous in-house model Method 2 – GPT-3.5 Method 3 – BERT model based on domain knowledge Ensemble results Proposed BERT model Sample number


Publisher accuracy 0.66 0.74 0.75 0.84 0.82 200


Sponsor accuracy 0.77 0.82 0.77 0.87 0.89 199


Average accuracy 0.71 0.78 0.76 **0.86** 0.85


### 1. Investigate Previous Work


Leveraging existing work is crucial, especially under tight schedules. Unfortunately, there was a scarcity of open-source data or models suitable for our specific context of publisher and advertisement content. However, we had an in-house model designed to predict whether a page sells products. Although its primary goal was slightly different—detecting commercial or product links—it provided a valuable starting point.


This in-house model was trained using noisy pseudo-labels obtained through web crawling and rule-based filtering. On our newly collected evaluation set, it achieved approximately **70% accuracy** , serving as a baseline for further improvements.


### 2. Try Existing Models


Large Language Models (LLMs) have become indispensable tools for data scientists due to their ability to handle various language tasks with minimal effort. While they may not always offer the best performance out of the box, they provide strong baselines for rapid development.


At the time of this project, **GPT-3.5** was the most advanced model available. After applying prompt tuning, the model achieved **78% accuracy** . Although the improvement was modest, it confirmed the potential of LLMs in handling CI prediction without extensive training data.


### 3. Improve Based on Domain Knowledge


Understanding the nuances of our domain provides additional avenues for improvement. Taboola operates on both the supply side (publisher pages) and the demand side (advertisement landing pages). While it’s intuitive to assume that advertisement pages have higher CI than publisher pages, empirical verification was necessary.


Data analysis revealed that certain types of advertisements—such as **Direct Ads** , **Calls to Action** , and **Branded Content** —had a higher likelihood of exhibiting CI. In contrast, most publisher items did not encourage direct actions. Leveraging this insight, we trained another model with **BERT-base** as the backbone and achieved **76% accuracy** .


## Ensembling Models for Enhanced Performance


With three baseline models—the in-house product detection model, the GPT-3.5-based model, and the BERT-based model informed by domain knowledge—we explored combining their strengths. By ensembling their binary predictions through a simple voting mechanism, we achieved a significant performance boost.


The ensemble prediction reached an impressive **86% accuracy** . This substantial improvement is likely due to the heterogeneity of the models, as they were trained using diverse data sources and methodologies.


To ensure scalability and handle high traffic, we retrained a **BERT-base** model using the ensemble’s pseudo-labels. This final model maintained high performance with **85% accuracy** , making it suitable for deployment in a production environment.


## Conclusion


The development of a Conversion Intent (CI) prediction model without human annotations presented both challenges and opportunities. Our endeavor to understand and predict commercial intent behind online content was important for improving ad targeting and potentially increasing revenue. By integrating insights from an in-house model, an LLM, and a domain-informed BERT model, we successfully leveraged heterogeneous models to collectively enhance performance, surpassing the capabilities of individual models. This process underscores that with the right methodologies and resources, it’s possible to develop high-performing models on tight schedules and limited budgets.
