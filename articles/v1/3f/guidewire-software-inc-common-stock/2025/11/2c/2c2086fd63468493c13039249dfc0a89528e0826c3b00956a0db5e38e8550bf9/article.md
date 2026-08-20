---
schema_version: "1.0.0"
document_id: "2c2086fd63468493c13039249dfc0a89528e0826c3b00956a0db5e38e8550bf9"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/technology/a-step-by-step-guide-to-building-glm-gam-risk-models"
published_at: "2025-11-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:25:08.967255+00:00"
content_hash: "sha256:666210fc5ce14ff7375a8708b3a695916296e59f841cc0d5b4dbb5e014c47b3c"
---

# A Step-by-Step Guide to Building GLM/GAM Risk Models

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Technology](https://www.guidewire.com/resources/blog/technology)


- A Step-by-Step Guide to Building GLM/GAM Risk Models


## Key Takeaways


- This guide provides a sequential, instructional framework for building robust Generalised Linear Models (GLMs) and Generalised Additive Models (GAMs) for insurance risk modeling.
- The process emphasizes a structured approach, moving from data preparation and feature selection to advanced transformations and finalization.
- Adherence to this process mitigates common risks such as data leakage and overfitting, resulting in more accurate and auditable models.


## Introduction


Generalised Linear Models (GLMs) and Generalised Additive Models (GAMs) are standard tools for predicting actuarial cost in insurance. While potentially less accurate than some machine learning models, their primary advantage is interpretability. They are auditable, less prone to overfitting, and their outputs are readily transformed into rating factors. The pressure to build these models better and faster is significant.


To outperform the competition, insurers must focus on refining their models on existing data. Superior predictive models directly result in increased profitability and market share. The following sections outline a repeatable, step-by-step process for building high-performing GLM/GAMs.


## Step 0: The Pre-Modeling Checklist


Before beginning model construction, complete the following data preparation and setup tasks. Failure to do so will invalidate your results.


- **Data Cleaning** : Verify that the dataset is clean, with no inconsistencies or errors in the raw data.
- **Target & Weight Definition** : Explicitly define and document the target variable (e.g., claim frequency) and the weight variable (e.g., exposure).
- **Distribution Selection** : Choose the family of probability distribution for the model (e.g., Poisson, Gamma) based on the nature of the target variable.
- **Validation Strategy** : Define and execute the train-validation data split. This split must be preserved for all subsequent model comparisons.


## Step 1: Foundational Feature Selection


The objective is to select an initial set of independent, high-impact explanatory variables. An incorrect or biased initial set will fundamentally weaken the model.


Two primary methods exist: **Forward Selection** (iteratively adding features) and **Backward Elimination** (iteratively removing features). Forward Selection is more practical for datasets with a large number of columns.


Your initial variables should be general descriptors of the policyholder or insured subject, avoiding features with excessively high cardinality. Use **One-Way charts** to identify variables where the target's value shows a clear, non-random trend against the variable's values. A **correlation matrix** can also identify features correlated with the target, but you should avoid including two features that are highly correlated with each other.


**Pro Tip: How to Spot Data Leakage**
Data leakage occurs when the model is trained on information that will not be available at the time of prediction. For example, using the final "number of claims" as a feature in a claim frequency model is a form of leakage, as this value is derived from the target itself. A clear symptom is a feature with an unusually high correlation to the target. Always verify the business meaning and origin of every feature before inclusion. A model built with leaked data may show exceptional performance in testing but will fail in a live production environment.


## Step 2: Feature Engineering and Transformation


Once initial features are selected, they must be engineered to maximize their predictive power. The correct technique depends on the data type.


### For Categorical Features (Category Mapping)


If a categorical feature has too many distinct values (high cardinality), it will cause overfitting. The solution is to group categories.


- **Method 1 (Lookup Table)** : Use a pre-defined mapping to group specific values into broader segments (e.g., mapping car makes and models to segments like 'Luxury' or 'Economy').
- **Method 2 (Target-Based Grouping)** : Use a One-Way chart to manually group categories that exhibit a similar average level of the target variable. Validate this grouping on the validation set to confirm its relevance.
- **Best Practice** : The category with the highest exposure should be dropped and serve as the base level, which is captured in the model's intercept.


### For Numeric Features


A raw numeric feature is treated as a linear function. Often, this is too simplistic.


- **Method 1 (Binning)** : Transform the numeric variable into a categorical one by grouping values into ranges (bins). This allows the model to capture non-linear effects.
- **Method 2 (Functional Transformation)** : If a One-Way chart shows a clear mathematical relationship (e.g., logarithmic), apply that function directly to the variable. This is common for features like population density.
- **Method 3 (Splines)** : If the relationship is complex and polynomial, use a spline transformation. This fits piecewise polynomial functions to different segments of the variable, which is highly effective for features like driver's age.


### Handling Tails with Low Exposure


Numeric variables often have very few observations at their extreme ends (tails). This can create spurious trends. For example, a few high-risk individuals may have had zero claims in the observation period by chance, causing the model to incorrectly apply a discount.


- **Solution** : Manually cap or floor the variable's effect using max() and min() functions. This prevents extreme, low-exposure values from creating an enormous and unfair premium calculation.


## Step 3: Modeling Advanced Relationships


### Interactions


An interaction occurs when the effect of one variable depends on the level of another. Have you considered how driver age and vehicle type might interact?


- **How to Identify** : Use a segmented One-Way chart. Place one feature on the X-axis and segment the data by another feature. If the target's curves for each segment have different shapes (e.g., one is increasing while the other is flat), a significant interaction exists and should be modeled.


### Geographical Modeling


Geographic information is often highly predictive.


- **Method 1 (Granular Grouping)** : Group ZIP codes into larger, statistically stable regions. The level of granularity should depend on the volume of data available.
- **Method 2 (Statistical Smoothing)** : Use a variable like population density instead of discrete regions to create smoother transitions.
- **Method 3 (2D Splines)** : This is the most granular method, applying polynomial transformations to latitude and longitude. Use this with caution on smaller portfolios to avoid overfitting.


## Step 4: Model Finalization with Regularization


Regularization is a technique used to prevent overfitting, especially for categories with low exposure. It applies credibility theory by adding a penalty that pulls the prediction for small-exposure categories closer to the grand average. This allows you to include these categories in the model without fully trusting the limited data observed for them.


## Conclusion


Building a high-performing GLM or GAM is a systematic process of selection, transformation, and validation. By following a structured, step-by-step approach - from initial data setup and feature selection to nuanced engineering and regularization - you can create models that are not only highly predictive but also auditable and robust. While automation can assist in this process, your expert judgment remains critical to interpreting the data and ensuring the final model is sound, reinforcing your role as a craftsman balancing the art and science of modeling.


---


### Glossary


- **Cardinality** : The number of unique values in a categorical feature. High cardinality (e.g., hundreds of car models) can lead to overfitting.
- **Data Leakage** : A modeling error where predictive information is used during training that will not be available when the model is used for live predictions.
- **Overfitting** : A modeling error where the model learns the training data too well, including its random noise, and fails to generalize to new, unseen data.
- **Regularization** : A technique that adds a penalty for model complexity to prevent overfitting, often by shrinking the coefficients of less important features.
- **Spline** : A flexible, piecewise polynomial function used to model complex, non-linear relationships in numeric variables.


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
