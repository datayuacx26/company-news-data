---
schema_version: "1.0.0"
document_id: "84fd4ebe18dc063115c3510c6ddccbb7e2708054ee6eae53bc80f2ec36baaef5"
company_key: "yc-lightly"
company: "Lightly"
source_id: "yc-lightly-news-import-b9754ffe9935"
canonical_url: "https://lightly.ai/blog/how-redundant-is-your-dataset"
published_at: null
first_seen_at: "2026-07-22T02:25:05.125057+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:cba04961d6e42e018271da3899a21b4fc17b2411635f4dd2a378da040df06e49"
---

# How to filter redundant data

‍


*Many interesting Deep learning applications rely on the use of complex architectures fueled by large datasets. With growing storage capacities and easier data collection processes\[1\], it requires little effort to build large datasets. However, when doing so, a new challenge surfaces: data redundancy. Many of these redundancies are systematically introduced through the data collection process. For instance, in the form of consecutive frames extracted from a video or very similar images collected from the web.* *In this blog post, the results of a benchmark study showing the benefits of filtering redundant data with Lightly* ***One*** *are presented. The data was collected by AI Retailer Systems (AIRS), an innovative start-up developing a checkout-free solution for retailers. In this study, we consider an object detection task: an intelligent vision system recognizes products on a shelf or on a customer’s hand.*


‍


Redundancies can take multiple forms, the simplest one being exact image duplicates. Another form is near-duplicates, i.e images shifted with few pixels across some direction or images having slight light changes. Redundancies have also been observed in very known academic datasets: CIFAR-10, CIFAR-100, and ImageNet \[2,3\]. This does not only lead to biased results of the model’s performance, be it accuracy or mean average precision mAP score, but also lead to high annotation costs.


> **💡Pro tip:** To better understand why embedding distance reveals duplicates, our[An Introduction to Contrastive Learning for Computer Vision](https://www.lightly.ai/blog/contrastive-learning) article outlines how contrastive learning structures feature space.


##### AI Retailer System dataset


Short video sample extracted from AIRS video.


The dataset provided by AIRS consists of images extracted from short videos capturing a customer grabbing different products. Two different cameras recorded videos of the shelf, each from a different angle, and 12 different kinds of products, i.e. 12 classes, were present.


The dataset was manually annotated using the open-source annotation tool Vatic. Its annotation rate, a rate quantifying how many frames per time unit were labeled, was 2.3 ± 0.8 frames per minute. Given that there are 51 objects on average in each image, this is equivalent to 0.51 seconds for each bounding box.


Sample image from Camera 1 with annotations (Note: the box color does not represent the article class)


Sample image from Camera 2 with annotations (Note: the box color does not represent the article class)


The annotated dataset has 7909 images. The training dataset has 2899 images, 80% of these images are from camera 2 and 20% from camera 1. For the test dataset, it has 5010 images and all of them from camera 1.


Visualization of the train-test setting for AIRS dataset


This specific design of the train and test datasets was decided upon according to the following rationale: First, an imbalanced dataset with a high fraction of images coming from one camera is built. Second, the object detection task is made hard for the model. With this train-test setting, we can calculate the fraction of images from Camera 1 in the filtered data, and thereby observe if any re-balancing is introduced by the different filtering methods used. In the following section, the methods used in this case study are presented.


> **💡Pro tip:** If you want to go beyond deduplication and decide what to label next, our[The Practitioner Guide to Active Learning in Machine Learning](https://www.lightly.ai/blog/active-learning-in-machine-learning) shows practical selection strategies.


##### Active learning and Sampling methods


To probe the effects of filtering the dataset, we borrowed ideas from the field of **active learning** .


Active learning loop used in this case study


Active learning aims at finding a subset of the training data that achieves the highest possible performance. In this study, we used the pool-based active learning loop that works as follows: A small fraction of the training dataset, called the labeled pool, is the starting point. The model is then trained on this labeled pool. Thereafter, new data points that should be labeled are selected using the model along with a filtering method. The newly selected samples can then be added to the labeled pool and finally the model can be trained from scratch on the updated labeled pool. After each cycle, the model’s performance on the test dataset for each filtering method used is reported. In our case, 5% of the training data was used as the initial labeled pool, the model was trained for 50 epochs, and 20% of the training data was added in each active learning loop.


The object detection model used in this benchmark study is YOLO V3 (You Only Look Once) \[4\], along with the implementation provided by the Ultralytics Github repository. The code was slightly modified in order to introduce the active learning loop.


As for the filtering methods, four different filtering methods provided by Lightly **One** were resorted to:


- “ **RSS** ”: Refers to random sub-sampling, used as a baseline.
- “ **WTL_unc** ”: This method refers to Lightly **One** 's uncertainty based sub-sampling. It selects difficult images that the model is highly uncertain about. The uncertainty is assessed using the model’s predictions.
- “ **WTL_CS** ”: This Lightly **One** method uses image representations to select images that are both diverse and difficult. It combines uncertainty-based sub-sampling with diversity selection. The image representations are obtained using state-of-the-art self-supervised learning methods using the PIP package Boris-ml. The advantage of self-supervised learning methods is that they don’t require annotations to generate image representations.
- “ **WTL_pt** ”: Relies on pre-trained models to learn image representations. The filtering is performed by removing the most similar images. Similarity in this case is given by the L2 distance between image representations.


Both Lightly **One** methods “ **WTL_unc** ” and “ **WTL_CS** ” use active learning, since they use the deep learning model to decide which data points to filter. In contrast, the “ **WTL_pt** ” method does not require neither labels nor a deep learning model to filter the dataset. For curious readers, this article presents a comprehensive overview of different sampling strategies used in active learning.


‍


##### Benchmark study results


The results of the experiments are presented below.


Averaged mAP score for different fractions of the training dataset using 4 seeds


We can see that the mAP score is low at small fractions of the training dataset. In addition, the mAP score saturates when using only 25% of the training data and reaches a value of 0.8. Above the saturation point, the mAP score increases very slowly until it reaches its highest value of 0.84. The saturation at low fractions of the training dataset indicates that there are many redundancies in the dataset.


Moreover, we can notice that for small fractions, i.e 5%, the “ **WTL_CS** ” filtering method is significantly better than the random baseline. As for high fractions, i.e 85%, the “WTL_pt” is able to **achieve the same performance achieved when using the full training dataset** . The “ **WTL_unc** ” method is on par or worse with the random sub-sampling method “ **RSS** ”.


Given that the saturation is reached within a small fraction of the training dataset, a “Zoom-in” experiment was performed where we evaluated the model’s performance using fractions of the training dataset between 5% and 25%. In this experiment, we dropped the “ **WTL_unc** ” due to its poor performance.


Zoom-in experiment: Averaged mAP score for different fractions of the training dataset using 4 seeds


In the results above, it is observed that the sampled subsets using “ **WTL_CS** ” and “ **WTL_pt** ” methods consistently outperform random sub-sampling. In addition, using only 20% of the training dataset, the “ **WTL_CS** ” sampling method is able to achieve a mAP score of 0.80. **We achieve 90% of the highest mAP score using only 20% of the training dataset** .


‍


##### Why do “WTL_CS” and “WTL_pt” perform better than random sub-sampling “RSS”?


To answer this question, simple comparison was made between the images selected with the “ **RSS** ” method and the images selected with “ **WTL_CS** ” and “ **WTL_pt** ”. For this purpose, we computed the fraction of images from camera 1 in the selected samples for different fractions of the training dataset and for different filtering methods. This comparison is done in both the normal and the zoom-in experiments. Note that in the training dataset, the original fraction of images from Camera 1 is around 20%.


‍


The fraction of Camera 1 images in the sampled images as a function of fraction of the training dataset


Zoom-in experiment: Fraction of Camera 1 images in the sampled images as a function of the fraction of the training dataset


We can observe that the sampling methods “ **WTL_CS** ” and “ **WTL_p** t” selected more samples from Camera 1 and therefore, they re-balanced the sub-sampled training dataset. This explains the gain in performance obtained using different samplings other than random sub-sampling. Since both “ **WTL_CS** ” and “ **WTL_p** t” methods select non-redundant data, they choose more images from camera 1, and therefore the sub-sampled dataset is more diverse.


‍


##### Summary and outlook


In this case study, we have shown the importance of filtering the redundancies within a dataset. We have found the following results:


- The AIRS dataset contains lots of redundant images.
- Achievement of the highest mAP score using only 85% of the training dataset.
- Achievement of 90% of the highest mAP using only 20% of the training dataset.
- Filtering re-balanced the AIRS dataset.


This benchmark study showed the importance of filtering redundant data. With **LightlyOne's filtering methods** , it was possible to achieve **annotation costs reductions between 15% and 80%** . We found many redundancies in the AIRS dataset despite the data being collected in a controlled environment. Indeed, there is at least one customer in each video and the customer always grabbed different products. Thus, wee expect the redundancies to be even more pronounced in a general uncontrolled case where different customers grab products at a supermarket.


‍


‍


Anas, Machine Learning Engineer,
Lightly.ai


‍


‍


\[1\]: Tom Coughlin, R. Hoyt, and J. Handy (2017): “Digital Storage and Memory Technology (Part 1)” IEEE report: https://www.ieee.org/content/dam/ieee-org/ieee/web/org/about/corporate/ieee-industry-advisory-board/digital-storage-memory-technology.pdf


\[2\]: Björn Barz, Joachim Denzler (2020): “Do we train on test data? Purging CIFAR of near-duplicates” *ArXiv* abs/1902.00423


\[3\]: Vighnesh Birodkar, H. Mobahi et al. (2019): “Semantic Redundancies in Image-Classification Datasets: The 10% You Don’t Need” *ArXiv* abs/1901.11409


\[4\]: Joseph Redmon, Ali Farhadi (2018): “YOLOv3: An Incremental Improvement” *ArXiv* abs/1804.02767v1


‍
