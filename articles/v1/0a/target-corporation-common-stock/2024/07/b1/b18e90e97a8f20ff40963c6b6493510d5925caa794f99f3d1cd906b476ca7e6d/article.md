---
schema_version: "1.0.0"
document_id: "b18e90e97a8f20ff40963c6b6493510d5925caa794f99f3d1cd906b476ca7e6d"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/predictive-modeling-for-availability"
published_at: "2024-07-24T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:9561cfe94bceda82c5a754f83953baa31efaf6489ad0b47ca5442db33a62f97c"
---

# Predictive Modeling for Availability of Inventory

Target guests place millions of online orders weekly, making a strong supply chain essential for efficient order fulfillment and improving the overall shopping experience. In the final leg of the supply chain, Inventory-Not-Found (INF) modeling is integral to achieving these objectives. INF work focuses on improving the accuracy of predicting the availability of products in a store. Target’s data sciences team developed a new method to remarkably decrease our INF rates through our data segmentation approach as we will detail below.


What is INF?


Figure 1: Example of Inventory-Not-Found (INF)


INF is an abbreviation for “Inventory-Not-Found.” To illustrate, let's say a guest made an online purchase of 2 units of detergent on Target.com. Our order allocation system determines which store will fulfill this order, meaning which store will ship the item to the guest's location. In the designated store, a Target employee searches for the item on the shelf to prepare it for packaging. However, during the search, the employee only locates 1 unit of the item. The other unit cannot be found. As a result, this situation is labeled as INF.


INF Overview: Causes, Effects, and Scope


Overview of INF, including scope of impact


INF can occur for various reasons, such as discrepancies in inventory. This means that the amount of inventory available in the store may have changed between the time the order was placed and when it was packaged. Initially, the allocation engine observed enough inventory in the store, but it decreased by the time of packaging, resulting in an INF. Additionally, INF can occur when guests directly purchase items in a Target store, causing the store to run out of the necessary number of units to fulfill the online order. Lastly, INF can be caused by time constraints for order fulfillment. Team members must find and pack the item within a specific time limit, and if they are unable to do so, it is marked as an INF.


The impact of INF is significant. One consequence is the need for order reallocation. When an item is not available at the intended store, the order allocation engine must be run to find another store that can fulfill the order for the guest. This process is costly and time-consuming, so working to reduce INFs can alleviate the burden on the allocation engine. Secondly, when items are not available in the same store, an order split occurs resulting in an excessive number of packages and higher shipping costs for Target. INF can also cause delays in delivery, leading to a negative experience for our guests.


By integrating machine learning and engineering expertise, we have an opportunity to improve our forecasting of INF rates. These rates may rise for different product categories, such as clothing, office supplies, and playthings, regardless of whether there are current promotions for these items. Working to improve the predictability of these INF rates can help Target trim our yearly shipping costs by decreasing overall INF rates.


Data Challenges


Figure 2: Challenges in a classification problem


Our current INF prediction task belongs to the family of “classification problems.” Its objective is to determine the likelihood of an item becoming INF at a store. In an ideal situation, if the likelihood of an item being INF is high, the order allocation engine will not select this location for order fulfillment. Generally, the complexity of a classification problem can significantly increase due to various challenges encountered at the data level, some of which are illustrated in Figure 2. Here are a few examples of the most frequently encountered classification problems:


- Class imbalance


: This occurs when the dataset is heavily skewed towards one class, known as the majority class, with very few samples representing the other class, known as the minority class. In such cases, classification algorithms tend to be biased towards learning the patterns from the majority class.


- Class overlap


: This refers to instances of multiple classes sharing a common region in the data space. This makes classification challenging as there is no clear decision boundary between the two classes.


- Class imbalance + Class overlap


: This combination of challenges makes classification difficult, especially for the minority class. This is because there are fewer minority patterns available for learning, while simultaneously, these patterns bear resemblance to the majority class.


Figure 3: Data level modeling challenges in INF


During our exploration of Target’s data, we discovered that we are dealing with two challenges: class imbalance and class overlap. Most of the samples, over 90%, belong to the non-INF class, which is consistent with our historical data. Additionally, we found that the distributions of all the features overlap significantly. This means that the range of feature values for the non-INF class is the same as the INF class, as shown in Figure 3. This imbalance and overlap pose difficulties for classification.


In our use cases, it is crucial for us to accurately identify both non-INF and INF cases. There are two reasons for this.


First, if a non-INF case is mistakenly classified as INF, our allocation engine may overlook a perfectly suitable store for order fulfillment. This could result in choosing a store that is far away from the guest location, leading to increased shipping costs.


Second, if INF cases are mistakenly identified as non-INF, our allocation engine will need to be re-run, causing order split, excess packages, and overall higher operational costs.


Figure 4: Data drift


Our next observation was the presence of noise and drift in the data as seen above. The proportion of data from each department is shown across the different months of 2022. Many departments like departments 24 and 25 have very few purchases throughout the year. These act as noise in the data, whose contribution to pattern learning is minimal and is overshadowed by popularly purchased items. Also, the proportion of data from different departments varies during events such as holidays or the varied seasons in the year like Back-To-School, Thanksgiving, Holiday, and New Years. Many new departments get introduced during specific months. These indicate the presence of data drift.


Given these challenges, we developed a strategy to address both class imbalance and class overlap through data segmentation while refreshing the models for events to address drift.


New Approach for INF Prediction


The concept behind data segmentation involves dividing the data into smaller subsets where patterns are more consistent, with the aim of reducing class imbalance and overlap.


To achieve this, we used our understanding of the business to create three distinct segments, each with a reduced class imbalance. For example, let’s say if an item has not been sold at a location for a long time, then its replenishment does not happen. In such a case if an order gets assigned to this store, it is difficult to locate this item and can cause INF. Additionally, we have employed various feature engineering techniques, such as addressing missing values and outliers, to minimize the overall degree of overlap in the data. This can all be observed in Figure 5.


Figure 5: Handling data level challenges in INF using segmentation


The process flow for the INF prediction model is as shown below in Figure 6.


Figure 6: Process flow in INF prediction


Process Steps


:


- Data segmentation


: This is a crucial step to achieve a good modeling output. We start by segmenting the data into three distinct parts using business rules. We ensure that the segments have repeating and stable properties for the validity of the models on unseen data.


- Feature handling


: Each of these segments undergoes feature engineering, where we focus on analyzing historical INF data, inventory data, as well as store and item attributes. Missing values are handled. New features are derived from existing ones, and outliers are treated through the


[Interquartile Range](https://procogia.com/interquartile-range-method-for-reliable-data-analysis/#:~:text=The%20Interquartile%20Range%20(IQR),-Before%20diving%20into&text=The%20IQR%20is%20a%20statistical,non-parametricity%20of%20the%20data.) (IQR) method which has been crucial in addressing skewness and thereby reducing overlap.


- Feature finalization


: Post feature handling, we move on to selecting features that demonstrate noticeable differences between classes. To aid us in this process, we incorporate open source libraries to finalize our set of features.


- Model parameter tuning


: Following obtaining the final set of features, we fine-tune the hyperparameters. Detailed research on the impact of hyperparameters has helped tuning to improve the model output significantly.


- Modeling


: Next, the modeling approach makes use of two types of data:


- Recency, to capture the recent INF patterns and


- Seasonality, to capture the INF patterns from last year during the same time.


This helps the model to be aware of both latest and event specific data. We employ


[tree-based boosting classification algorithms](https://towardsdatascience.com/introduction-to-boosted-trees-2692b6653b53#:~:text=In%20boosting%2C%20new%20trees%20are,is%20not%20ideal%20to%20perform.) for modeling. Lastly, we validate our models through


[out of time validation](https://trsekhar123.medium.com/validation-techniques-ml-a984fa98cbd6) .


Due to data drift during promotions and events, the models are retrained with new sets of recent and seasonal patterns. This has helped in maintaining the model's performance.


Today, the INF model predicts INF probability by incorporating changes in inventory in real-time, for in-stock items at a store, that approximately scales up to ~250M predictions per day.


Model Results


Moving on to the results section, we compare the newly developed model, created using a data segmentation approach, to the baseline model that did not address challenges at the data level. This comparison allows us to measure and quantify the improvements achieved.


Improvement in Recall


Figure 7: Improvement in recall using data segmentation


This chart displays the distribution of INF occurrences based on INF probabilities generated by different models.


For instance, if there are 100 INF cases, we are interested in determining how many of these cases occurred when the model predicted an INF probability of less than 10% or 20%, and so on.


In the case of the baseline model, 25 out of the 100 actual INF cases occurred when the INF probabilities were less than 10%. Similarly, 56 actual INF cases occurred when the baseline model predicted a very low probability of INF (<40%).


This indicates that the baseline model tends to assign low INF probabilities to most cases. Consequently, what should be classified as an INF case is instead predicted as a non-INF case, resulting in an increase in False Negative cases. As the number of False Negatives increases, the recall performance deteriorates. In contrast, the new model produces significantly fewer False Negatives, thereby enhancing the overall recall. The new model is more confident in classifying cases with low INF probabilities as non-INF cases.


Improvement in Precision


Figure 8: Improvement in precision using data segmentation


The chart provided showcases a comparison between the INF rate at various INF probabilities produced by each model.


Let's consider a scenario where 100 cases are forecasted with a 90% INF probability. Among these 100 cases, the baseline model correctly identifies only 42 as INF, whereas the new model identifies 68. Consequently, the new model yields a higher number of True Positives, which enhances the overall precision.


Key Wins


Figure 9: Key wins of INF modeling


The new method has proven to be successful in enhancing precision and recall, resulting in significant accomplishments. These include a remarkable decrease of 4% in the INF rate and a decrease of approximately 3.7% in the average surplus packages.


Conclusion and Next Steps


The INF modeling work established data segmentation as one way to handle complex classification data. For our future work, we will continue to focus on expanding our use case to all item groups and item selling channels, such as Ship-From-Store (SFS) and Order-Pick-Up (OPU). We will also work on building a process to handle data drift automatically to avoid manual intervention. We are also leveraging simulations to understand the impact of INF and how to use it to improve shipping costs as we continue to work on maturing our modeling work.


References


1. [https://hackernoon.com/automatic-feature-selection-in-python-an-essential-guide-uv3e37mk](https://hackernoon.com/automatic-feature-selection-in-python-an-essential-guide-uv3e37mk)


2. [https://medium.com/@rsesha2001/how-to-use-autoviz-to-do-feature-selection-for-your-machine-learning-b92ac5341a93](https://medium.com/@rsesha2001/how-to-use-autoviz-to-do-feature-selection-for-your-machine-learning-b92ac5341a93)


3. [https://www.kaggle.com/code/gauravduttakiit/heart-disease-featurewiz-autoviz](https://www.kaggle.com/code/gauravduttakiit/heart-disease-featurewiz-autoviz)


4. [https://optuna.readthedocs.io/en/stable/](https://optuna.readthedocs.io/en/stable/)


5. [https://catboost.ai/](https://catboost.ai/)


6. [https://lightgbm.readthedocs.io/en/stable/](https://lightgbm.readthedocs.io/en/stable/)


7. [https://xgboost.readthedocs.io/en/stable/](https://xgboost.readthedocs.io/en/stable/)


## RELATED POSTS


### Solving for Product Availability with AI


By Brad Thompson and Meredith Jordan, October 24, 2023


Read about how Target uses AI to improve product availability in stores.


### Bundled Product Recommendations


By Amit Pande, Samaneh Karimi, and David Relyea, April 23, 2024


Learn more about how Target uses bundled product recommendations to enhance shopping experiences for our guests
