---
schema_version: "1.0.0"
document_id: "58acf3da01009516e1cf031fb056668113bfb90621e98f426eb2e374ac91b1cb"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/embedding-drift-detection"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:7761eede73398afd0706d891d1a2837e0e4954a433ff1178e77bc0c2f4fd1546"
---

# Shift happens: we compared 5 methods to detect drift in ML embeddings

***TL;DR:*** *Monitoring embedding drift is relevant for the production use of LLM and NLP models. We ran experiments to compare different drift detection methods. We implemented them in an*[open-source library](https://github.com/evidentlyai/evidently) *and recommend model-based drift detection as a good default.*


What you’ll find in the blog:


- **Experiment design** . We created artificial shifts on three datasets and chose five embedding drift detection methods to test.
- **Comparison of drift detectors.** We summarize how they work and react to varying data changes. The goal is to help shape the intuition of the behavior of different methods.
- **Colab notebooks with all the code.** You can repeat the comparisons on your data by introducing artificial shifts with the same approach as we did.
- **Open-source library to detect embedding drift.** We implemented our findings in[Evidently](https://github.com/evidentlyai/evidently) , an open-source Python library to evaluate, test and monitor ML models.‍


## Why monitor embeddings?


### **\[fs-toc-omit\]** Data drift detection in ML


When ML systems are in production, you might need to keep tabs on the[data drift](https://www.evidentlyai.com/ml-in-production/data-drift) . This is a component of[machine learning model monitoring](https://www.evidentlyai.com/blog/batch-inference-monitoring) .


**Getting the ground truth can be a challenge.** An ML model predicts something, but you do not immediately know how well it works. Sometimes you simply wait. Say, you predict food delivery time, and half an hour later, you know the model error. In cases like credit scoring, the wait is much longer. In scenarios like text classification, you might need to label the data to evaluate the model quality. Otherwise, you are flying blind.


**Tracking drift in the model inputs and outputs can be a useful proxy.** The assumption is that if the input data shifts compared to a reference period, the model might perform worse since the environment is no longer familiar. If the model outputs (= what the model predicts) change, this is also a good reason to investigate.


##### \[fs-toc-omit\] **Building an LLM-powered product?**


> Sign up for our free email course on LLM evaluations for AI product teams. A gentle introduction to evaluating LLM-powered apps, no coding knowledge required.[Save your seat ⟶](https://www.evidentlyai.com/llm-evaluations-course)


### **\[fs-toc-omit\]** Embeddings drift


When you work with texts, images, or other unstructured data, it is common to use **embeddings** . In this case, you create a numerical representation of the input data.


For example, you can take pre-trained models like[BERT](https://en.wikipedia.org/wiki/BERT_(language_model)) to convert your raw texts into vectors. This process maps each object in the dataset (say, a text that the model should classify) to a multi-dimensional space and represents it as a set of numbers.


**Monitoring data drift is just as relevant when you work with embeddings.** When an NLP or LLM-based application is in production, you can detect when the data become too different or unusual and react before it affects the model quality.


**How can data drift in texts look?** Here are a few examples:


- **A new popular topic or word** . Say, a model analyzes social media data: a new popular meme or a world event might confuse the model.
- **A new class or change in balance.** You might classify texts into five topics, but then a 6th topic arises, or an existing one becomes more prevalent.
- **Outliers or spam.** Imagine a spike in unrelated content, automatically generated or corrupted texts.
- **Shift in text sentiment.**
- **Texts in a new language.**


Drift detection helps notice when something changes enough to warrant an investigation. You can then retrain or tune the model or send the data for labeling.


But how exactly to detect drift? How to measure the “change”?


In tabular data, drift detection is[somewhat easier](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets) since features are often interpretable. You know what it means if, say, the average value of the "age *"* column changes. There are also ways to[detect drift in the raw text data](https://www.evidentlyai.com/blog/tutorial-detecting-drift-in-text-data) . However, if you use embeddings, you need a way to catch drift in the numerical representation of words instead!


In this blog, we will explore how.


## Experiment design


How we approached the comparison:


- Selected **three text datasets.**
- Used **two pre-trained embedding models** for each dataset (thus getting a total of six embedding datasets to work with).
- Split each dataset into two parts and **introduced an artificial shift** to the second one. We varied the change, gradually increasing it.
- Picked **five embedding drift detection methods** and evaluated how each method's “drift score” reacts to the artificial drift.
- Compared the results with and without **dimensionality reduction** (PCA).


> **A few disclaimers: To keep things simple, we focused on text data.** While the findings might apply to other types of embeddings (e.g., images), this was out of the experiment's scope. **This is not academic research.** Our goal is to build intuition on the behavior of the different methods and help develop heuristics for ML practitioners. **The definition of drift.** We imitated drift by increasing the presence of one of the classes. The learnings might not precisely translate to differently-shaped changes.


The research is fully reproducible if you want to challenge some of our findings!


### **\[fs-toc-omit\]** Datasets


We worked with 3 datasets:


- **Wikipedia comments** . Human evaluators labeled some as containing toxic behavior.
- **News categories** . Short descriptions of news articles are tagged by the topic, such as “food and drink,” “sports,” or “business.”
- **Food reviews** . Contain plain text reviews and user-submitted ratings from “1” to “5”.


> **Sources:**
> - Wikipedia comments.[Source on Kaggle](https://www.kaggle.com/competitions/jigsaw-toxic-comment-classification-challenge/data) .
> - News categories.[Source on Kaggle](https://www.kaggle.com/datasets/rmisra/news-category-dataset) . *Citation: Misra, Rishabh. "News Category Dataset." arXiv preprint arXiv:2209.11429 (2022).*
> - Food reviews.[Source on Kaggle](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews?select=Reviews.csv) . *Citation: J. McAuley and J. Leskovec.*[From amateurs to connoisseurs: modeling the evolution of user expertise through online reviews](http://i.stanford.edu/~julian/pdfs/www13.pdf) *. WWW, 2013.*


We pre-processed datasets to have two classes each time. Wikipedia comments were already split into “toxic” and “not.” For news, we contrasted the “food and drink” category against all others. With food reviews, we picked “neutral” comments (ranked as “3”) against “positive” (rated “4” or “5”).


We used two methods to convert raw texts into embeddings:[BERT](https://en.wikipedia.org/wiki/BERT_(language_model)) and[FastText](https://fasttext.cc/) . These are relatively different pre-trained models that help obtain vector representations of input texts.


Let's take a look at the resulting embedding distributions. We sampled the data and projected them into a two-dimensional space. The two classes in each dataset are color-coded.


When using BERT:


When using FastText:


The results are visibly different.


**The choice of vectorization method matters.** When creating the model, a data scientist selects one of many pre-trained embedding models and fine-tunes it (or not). This affects how well the classes separate in the dataset – meaning, how far the objects of different classes are from each other.


In our case, the objects in the **food reviews** separate worse than others: there is no clear visual grouping.


### **\[fs-toc-omit\]** Artificial shift


We split each dataset into two parts: “reference” and “current.” We treat “reference” as a representative dataset that shows what to expect from the data. In practice, it could be a validation dataset or a curated golden set. “Current” is the production data we test for drift.


**Then, we injected artificial drift into the current dataset** . We gradually added a bit (and then a lot!) of the objects of a specific class.


Let’s illustrate the approach on the Wikipedia comments dataset:


- We take reference data that has 99% ordinary comments and 1% labeled as “toxic.”
- We take the current dataset, which has a *1% plus delta* share of the “toxic” comments. *Delta* is the size of the shift we introduce.
- We gradually increase the *delta* , adding more “toxic” class examples to the current data. We move from 0 to 30%.
- Eventually, we compared the current dataset with 31% “toxic” comments against the reference with only 1% “toxic” comments.


We did the same for other datasets: injected more reviews of “documentaries” and “neutral” food reviews accordingly.


These are not drastic changes: we add examples of a known class, not a totally new one. However, even with the same approach, the results vary. The shift might be less vivid when a category is not easy to identify in the first place (like “neutral food reviews”) and vice versa.


### **\[fs-toc-omit\]** Drift detectors


We picked the following drift detection methods:


1. Euclidean distance (takes values from 0 to infinity).
2. Cosine distance (takes values from 0 to 2).
3. Classifier model (ROC AUC can take values from 0 to 1).
4. Share of drifted embedding components (takes values from 0 to 1).
5. MMD (can take values above zero).


### **\[fs-toc-omit\]** Dimensionality reduction


We tested each method with and without dimensionality reduction. In the latter case, we used the[PCA](https://en.wikipedia.org/wiki/Principal_component_analysis#:~:text=Principal%20component%20analysis%20(PCA)%20is,the%20visualization%20of%20multidimensional%20data.) (Principal Component Analysis) method, reducing the dimensions to 30.


The idea behind PCA is to further “compress” information in each embedding. If you map it to a smaller number of dimensions, you can also speed up the calculations in some cases.


Our goal was to test whether the results of each drift detection method are different with PCA while the initial “drift” remains the same.


## Experiment results


How do the chosen drift detection methods compare? We sum up the results below.


We’ll use the following evaluation criteria:


- **How easy is it to understand the method?** Since embeddings themselves do not have interpretation, selecting a “perfect” method is hard. However, we can judge if the drift detection method is easy to grasp.
- **How easy is it to interpret the drift score?** It is useful when you can build intuition about the resulting values and how to tune the threshold.
- **How stable are the results for different embeddings?** If you work with different pre-trained models, picking a method that works consistently makes sense.
- **How stable are the results with and without PCA?** PCA-agnostic detection methods are convenient since they require less threshold tuning across applications.
- **How fast are the computations?** Some methods might take longer to calculate. We ran a separate experiment to compare the computation speed.


> **Experiment code.** We share the Colab notebooks with the results for each method. Here is the[summary notebook](https://colab.research.google.com/drive/1YTOvKrg9kT8guzDVV2Xcrl5piuTiNaeV?usp=sharing) . There are also 6 for each dataset with different embeddings: food reviews (with[BERT](https://colab.research.google.com/drive/1HmXvlkWzTjEZW1XWDVWTxDpsbckCHfx2?usp=sharing) and[FastText](https://colab.research.google.com/drive/131I6JDdZ60fRjqv6OM5pxTx4n6yrGVTP?usp=sharing) ), Wikipedia comments (with[BERT](https://colab.research.google.com/drive/131I6JDdZ60fRjqv6OM5pxTx4n6yrGVTP?usp=sharing) and[FastText](https://colab.research.google.com/drive/1SgS2OO5XU0tEYyYKl5TUJmH_spc7N1S5?usp=sharing) ), and news categories (with[BERT](https://colab.research.google.com/drive/1aAcT4O4bhYahFL7_3FEBVt-I85KgsArB?usp=sharing) and[FastText](https://colab.research.google.com/drive/1aAcT4O4bhYahFL7_3FEBVt-I85KgsArB?usp=sharing) ).


### Euclidean distance


**How it works.** We average all embeddings in the “current” and “reference” data to get a single representative embedding for each dataset. Then, we measure the distance between the embeddings to evaluate how far they are from each other.


In this experiment, we use[Euclidean distance](https://en.wikipedia.org/wiki/Euclidean_distance) . This is probably the most straightforward distance metric. If you have two vectors, it measures the length of the line that connects them.


> **There are other distance metrics** . For example, Cosine, Cityblock (Manhattan), or Chebyshev distance. While each metric measures “how far” the vectors are, they work differently. It’s best to choose a familiar metric and run a few experiments to set the thresholds. In the[Evidently](https://github.com/evidentlyai/evidently) Python library, we implemented all the listed methods.


**Drift score.** The drift score is the value of the Euclidean distance. It can take any value from 0 to infinity. Identical datasets will have a distance of 0. Intuitively, a smaller distance indicates that the two vectors are similar, while a larger distance shows they are further apart.


> 🔬 **Statistical hypothesis testing.** With large datasets, you can directly use the Euclidean distance to measure drift. With smaller samples (e.g., <1000), you can apply statistical testing. For example, compare the measured distance to the possible distance values for the reference data at a set percentile (we picked 95%[as the default](https://docs.evidentlyai.com/user-guide/customization/embeddings-drift-parameters/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=embedding-drift-detection) ). This helps avoid false positive drift detection. You can do the same for other methods, such as Cosine distance, MMD, or model-based drift detection.


**Experiment results.**


For each method, we plot the drift score against the “size” of the introduced drift. We show four plots: for BERT and FastText embeddings, with and without PCA.


We color-coded our datasets on all plots:


- Green: Wikipedia comments.
- Red: News categories.
- Purple: Food reviews.


Here is how Euclidean distance detects drift in our datasets.


The drift on the food reviews dataset is more subtle, and Euclidean distance appears less sensitive.


**Pros and Cons.**


- **Familiar method.** Euclidean distance has a common sense definition. It is widely used in machine learning.
- **Not a very interpretable threshold. Pros:** This measure communicates "distance," which is easy to perceive intuitively. The values grow smoothly following the size of the drift introduced. **Cons:** It is hard to set a threshold since it is an absolute distance and can go up to infinity. You might need to experiment with your data to tune it. **‍**
- **Fast computation. However, using PCA slows it down.**
- **Consistent behavior with PCA.** The drift detector results are very close both with and without PCA. **‍**
- **Inconsistent behavior for different pre-trained embeddings.** The results for BERT and FastText vary. Since each model has its own way of mapping objects to embeddings, they eventually exist in spaces with *different* dimensions. As a result, the positions of individual objects and the distance between them also differ. You might need to experiment with thresholds for your specific datasets.


> **Impact of sample size.** The experimental notebooks include the code to compare the drift detection results for samples of different sizes. If you want to play around more on your own, check the section for Euclidean distance in any of the notebooks to get the sample code. You can replicate it for other methods (just mind the computation time!)


### Cosine distance


**How the method works.** Once again, we average all embeddings in the “current” and “reference” data to get a single representative embedding for each dataset.


But instead of measuring the “length,” we look at the[Cosine similarity](https://en.wikipedia.org/wiki/Cosine_similarity) between the embeddings. It measures the cosine of the angle between the two vectors in a multi-dimensional space. To get the Cosine **distance** , you should subtract the cosine from 1.


> *Cosine distance = 1 — Cosine similarity*


If two vectors are identical, the Cosine similarity is 1, and the Cosine distance is 0.


This is a popular measure in machine learning. For example, it is used for similarity search in information retrieval or recommendation systems.


**Drift score.** The drift score, in this case, is the Cosine distance value. It can take values from 0 to 2.


> 🔬 For smaller datasets, you can apply statistical hypothesis testing.


**Experiment result.** Here is how drift detection using Cosine distance works on our datasets.


Once again, the drift on the food reviews dataset is harder to detect.


**Pros and Cons.**


- **Familiar method.**
- **Non-interpretable threshold.** While the threshold has a limit between 0 and 2, drift can be detected at very low thresholds (starting from 0.001) and is not very intuitive to tune.
- **Fast computation.**
- **Does not work with PCA.** The results are very inconsistent: likely because the angle between the vectors is not “preserved” during the transformation.
- **Different behavior for BERT and FastText.** The results are different for different vectorization methods.


### Classifier


We also refer to this method as “domain classifier” or “model-based drift detection.”


**How it works.** We train a binary classification model to discriminate between data from reference and current distributions. If the model can confidently identify which embeddings refer to the “current” and “reference,” you can consider the two datasets significantly different.


You can read more about the domain classifier approach in the[paper](https://arxiv.org/pdf/1810.11953.pdf) “Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift.”


**Drift score.** The drift score is the ROC AUC score of the domain classifier computed on a validation dataset. It can take values from 0 to 1. Everything over 0.5 is better than random. 1 is an “absolute drift” when the model can perfectly define where the data belongs.


For example, you can set the threshold to 0.55.


> 🔬 For smaller datasets, you can apply statistical hypothesis testing. In this case, you’d contrast the obtained ROC AUC against the quality of the “best random model.”


**Experiment result.** Here is how model-based drift detection works on our datasets.


**Pros and Cons.**


- **Familiar approach.** It is easy to understand the idea behind the method. Building binary classification models is the bread-and-butter of any machine learning practitioner.
- **Interpretable threshold.** ROC AUC is a common measure of the classifier’s quality. The higher the ROC AUC, the easier for the model to classify the samples as belonging to the old or new distributions.
- **Normal computation speed.** It is slower than Cosine or Euclidean distance but still within reason.
- **Consistent behavior with PCA.** The drift detector results are similar both with and without PCA.
- **Consistent behavior for different pre-trained embeddings.** The drift detector results are similar both for BERT and FastText embeddings.


All in all, this is an excellent default approach: easy to use and interpret, with reasonable compute speed, and with consistent results for different embeddings with and without PCA.


### Share of drifted components


We also refer to this method as "ratio" or "numerical drift detection."


**How it works.** With embeddings, you get a numerical representation of the input objects. Say, you convert each text into 300 numbers. Effectively, you get a structured tabular dataset. Rows are individual texts represented as embeddings, and columns are components of each embedding.


You can treat each component as a numerical “feature” and check for the drift in its distribution between reference and current datasets. If many embedding components drift, you can consider that there is a meaningful change in the data.


**What is the intuition behind the "ratio" method?** You can treat each embedding as a set of coordinates of a vector in a multidimensional space. While you cannot interpret their meaning, you can evaluate the share of coordinates that have changed. If many coordinates drift, it is likely that the dataset now looks different.


We use[Wasserstein (Earth-Mover) distance](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets#wasserstein-distance-earth-mover-distance) with the 0.1 threshold to detect drift in individual components. There is some intuitive explanation for the metric: when you set the threshold to 0.1, you notice changes in the size of the "0.1 standard deviations" of a given value.


> **📊 There are other numerical drift detection methods** . For small datasets, you can use statistical tests like Kolmogorov-Smirnov. For larger datasets, you can use distance and divergence metrics like Jensen-Shannon or Kullback-Leibler divergence. We gave an overview of methods in the blog “[Which test is the best?](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets) ”.


> **💡 Should you correct for multiple hypothesis testing?** We believe it is not necessary. In this scenario, we do not need to be certain about the drift in specific embedding components. Instead, we look for significant shifts in a large dataset. A type I error (false positive drift detection) occurs when trying to detect small changes in insufficient data. In this case, the likelihood of this error is small. Even if we falsely detect some components as drifting, this won’t be critical: what we want to evaluate is the overall share of drifting elements.


**Drift score.** After you test individual columns for drift, you can estimate the overall share of drifted embedding components. For example, you can set a threshold to 20%.


This means that if each object is represented by 300 numbers, and 60 of them drift across all objects, you consider data drift to be detected.


**Experiment result.** Here is how the method works on our datasets.


Interestingly, the method does not detect drift on the food reviews dataset: the obtained scores are very low. It again appears not very sensitive, given that the initial dataset was more “noisy.” You might want to tune the numerical drift detection thresholds (e.g., Wasserstein distance in this case).


**Pros and Cons.**


- **Familiar approach.** You might already use methods like Wasserstein distance (or[others](https://www.evidentlyai.com/blog/data-drift-detection-large-datasets#kolmogorov-smirnov-ks-test) ) to detect drift in tabular data and simply reuse your favorite approach.
- **Fairly interpretable threshold. Pro:** you can develop some expectations about the "share" of drifted components. There is a perceived intuition to "10% of components drift" vs. "100% of components drift". **Con (or pro for some?):** you have a high degree of freedom since you can manipulate both the drift detection method for individual components and the share of drifted components when setting the threshold. **‍**
- **Normal computation speed.** It is slower than Cosine or Euclidean distance but reasonable. With PCA, it becomes just as fast. **‍**
- **Less sensitive when PCA is applied.** If you apply PCA, the method only detects more significant shifts. This can be explained since you "compress" the information, making it more difficult to detect minor changes – however, not all drift detection methods behave this way. When you set the threshold, it is crucial to consider whether you will use dimensionality reduction and tune it accordingly. ****


All in all, this is a useful approach: fairly interpretable and with consistent results for different embeddings. It can be helpful if you are already familiar with numerical drift detection.


### Maximum mean discrepancy


**How it works.** Maximum Mean Discrepancy (MMD) measures the distance between the means of the vectors. This multi-dimensional distance is calculated using a kernel function defined in terms of a Hilbert space. You can read more in the paper “[A Kernel Method for the Two-Sample Problem](https://arxiv.org/abs/0805.2368) .”


The goal is to distinguish between two probability distributions **p** and **q** based on the mean embeddings µ **p** and µ **q** of the distributions in the[reproducing kernel Hilbert space](https://en.wikipedia.org/wiki/Reproducing_kernel_Hilbert_space) **F** . Formally:


This looks a bit confusing, doesn't it?


Here is another way to represent MMD:


Where **K** is a reproducing kernel Hilbert space. To simplify, you can think about it as some measure of closeness. The more similar the objects, the larger this value.


This becomes more intuitive. If the two distributions are the same, MMD should be 0:


If the two distributions are different, MMD will be larger than 0:


*Comparing different distributions with MMD.*


**Drift score.** The drift score, in this case, is the obtained distance measure (MMD). It can take values above zero. MMD is 0 for identical distributions.


> 🔬 **Statistical hypothesis testing.** Some MMD implementations include statistical hypothesis testing. For example, one can compare the obtained MMD values in the current dataset against possible MMD values in reference at a set percentile. This method is useful on smaller datasets (under 1000 objects) but might not be necessary with large datasets. In this case, you can set the MMD threshold directly. This way, you also significantly increase the computation speed, which still remains lower than with other methods.


**Experiment result.** Here is how drift detection using MMD works on our datasets.


MMD is also not very sensitive to drift on the food reviews dataset.


**Pros and Cons.**


- **The approach might be hard to explain.** Not everyone has heard about Hilbert Space!
- **Not interpretable threshold.** It can take values above zero and is quite hard to set. In our example, we see that the values when drift is introduced can be as low as 0.001.
- **Very slow computation.**
- **Slightly more sensitive when PCA is applied.** The threshold should be set keeping this in mind. **‍**
- **Consistent behavior for different pre-trained embeddings.** The drift detector results appear similar both for BERT and FastText embeddings.


### Computation speed


To evaluate the computation speed for each method, we ran a small experiment using the same instance of Google Colab. We compared the computation for smaller (1000 objects) and larger (15000 objects) datasets with and without PCA.


## Summing up


### **\[fs-toc-omit\]** Comparing methods


Let’s sum up the findings!


**Model-based** drift detection is a good default. You can intuitively tune the ROC AUC threshold to react only to the confident drift.


Evaluating the **share of drifted embedding components** comes a close second. You can tweak the thresholds to your data and reuse the methods from tabular drift detection. Just make sure to account for the impact of dimensionality reduction!


To **track the “size” of drift in time** , you can pick metrics like Euclidean distance. However, you might need a few experiments to tune the alert thresholds since the values are absolute.


### **\[fs-toc-omit\]** Pragmatic drift detection


A few more thoughts on applying drift detection in practice.


**Data drift detection is a heuristic.** It helps notice changes of a *certain* magnitude. You need to make assumptions about what you want to detect.


There is no universal way to define data changes that strictly correlate to model quality. Even if the data shifts, some models generalize well. You might also have different error tolerance. Sometimes you want to react to major changes, and sometimes to the smallest shift.


To avoid false alarms, you should tune your drift detection method and thresholds to the particular use case and data. There is no silver bullet here!


**Data drift detection can be separate from interpretation.** You might use embeddings to detect drift (meaning, get an alert that *something* changed). However, after drift is detected, it makes sense to analyze raw data to locate and interpret the specific changes, such as new topics appearing in the dataset.


*Example of drift detection with Evidently using raw data.*


## Evidently open-source


We implemented all the mentioned drift detection methods in[Evidently](https://github.com/evidentlyai/evidently) , an open-source Python library to evaluate, test and monitor ML models.


You need to pass a DataFrame, select which columns contain embeddings, and choose the method (or go with defaults!)


*Source: example notebook on*[embedding drift detection](https://github.com/evidentlyai/evidently/blob/main/examples/how_to_questions/how_to_calculate_embeddings_drift.ipynb) *.*


‍


The visualization is based on[UMAP](https://github.com/lmcinnes/umap) . It projects the embeddings to a two-dimensional space. Instead of plotting individual points, it summarizes them as contours. This is easier to perceive for a large number of data points.


What else is cool about it?


- You can pass a **dataset** that contains both embeddings and structured data (e.g., metadata about your texts) in one go and detect drift for the complete dataset.
- You can use Evidently in **any Python environment** or as part of a production pipeline.
- You can get the output as **HTML, JSON, or Python dictionary** and integrate Evidently with tools like **Airflow, MLflow,** and **Grafana** .
- There are **100+ evaluation metrics** covering drift detection for different data types, data quality checks, and model quality analysis.


You can check the[Getting Started tutorial](https://docs.evidentlyai.com/get-started/tutorial/?utm_source=website&utm_medium=referral&utm_campaign=blog_text&utm_content=embedding-drift-detection) to understand the Evidently capabilities or jump directly to a[code example](https://github.com/elenasamuylova/evidently/blob/main/examples/how_to_questions/how_to_calculate_embeddings_drift.ipynb) on embedding drift detection.


## What about LLM systems?


Embedding drift detection is especially useful when you are working with large datasets or training your own ML models. It helps identify significant shifts in the input data when you assume that the production data should remain similar to what the model was trained on. This is often the case with smaller, task-specific models or when you want to catch changes early across high volumes of data.


With LLM systems, embedding drift can still be relevant – particularly in components like retrieval pipelines or high-volume user input analysis. For example, if you are using static embeddings in a RAG setup, monitoring shifts in the embedding space can signal changes in query types, document content, or language.


But if you are working with smaller datasets or expect frequent changes in topics and use cases, drift detection alone may not tell the whole story.


[LLM evaluations](https://www.evidentlyai.com/llm-guide/llm-evaluation) bring new ways to track quality and behavior. You can use[LLM-as-a-Judge](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) setups where one LLM scores or reviews the outputs of another, enabling scalable, automated evaluations without manual labeling. This helps evaluate things like relevance, tone, completeness, or factual accuracy.


To explore these new approaches, check out the updated examples and tutorials on LLM evaluation methods in the[Evidently documentation](https://docs.evidentlyai.com/) .


##### \[fs-toc-omit\] **Get started with AI observability**


> Try our open-source library with over 25 million downloads, or sign up to Evidently Cloud to run no-code checks and bring all the team to a single workspace to collaborate on AI quality.[Sign up free ⟶](https://www.evidentlyai.com/register)[Or try open source ⟶](https://github.com/evidentlyai/evidently)
