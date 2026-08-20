---
schema_version: "1.0.0"
document_id: "8a1a4eff15b54cccbaf9246d05cf0de01eaf3a8c10f36196c88cb4ab5d538622"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2017/03/08/image-search-using-joint-embeddings-part-one"
published_at: "2017-03-08T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:27:28.230343+00:00"
content_hash: "sha256:0d823c4feabc63055c507623ef3106a6db17bae8702570ad750a2616a6d269da"
---

# Image Search Using Joint Embeddings (Part One)

On the Shutterstock search team, our focus is serving the best possible images for a given query. To do this, we use behavioral data from our customers such as downloads to help us rank the quality of an image for a given search. While this approach often works very well because it allows us to harness the collective intelligence of our great users, it does have one very serious drawback: new content from our contributors is more challenging to surface in search results. This is commonly known as the[cold start problem](https://en.wikipedia.org/wiki/Cold_start) .


Not only do the new images not have a chance to get in the top results because they have no historical data associated with them, but popular images also continue to entrench themselves because customers see the images with many downloads first. This impacts both contributors of new content and consumers. Producers of new content cannot get their images in front of users, and customers might not be able to find their perfect image even though it is available in Shutterstock’s collection. How can we solve the cold start problem?


One obvious approach to try would be to not use behavioral data and just rely on contributor-provided data such as keywords and descriptions. While contributor-provided data provides a good signal for retrieval of images, they are not very useful at ranking them because it is hard to measure the quality of a keyword for one image versus another. There is one data source that has more information about the image than all of the metadata combined: the images themselves.


Over the past few years, a great deal of progress has been made in machine understanding of images largely due to progress in deep learning and the explosion of labeled data. Much of the emphasis has been on large-scale image classification as exemplified by the[ILSVRC](http://www.image-net.org/challenges/LSVRC/) . Deep neural nets (DNNs) can now achieve top-5 accuracy out of 1,000 classes of 3.57%; for comparison, humans can get 5.1%. While being able to classify images more accurately than (non-expert) humans is great, it is of little use to us other than auto suggesting keywords to users.


However,[DeCAF: A Deep Convolutional Activation Feature for Generic Visual Recognition](https://arxiv.org/abs/1310.1531) from the Berkeley Vision and Learning Center showed that intermediate representations from a DNN perform well when used as inputs to a variety of machine learning tasks. Furthermore, these representations (image embeddings) are very compact and contain both visual and semantic information. In fact, we use image embeddings in our reverse image search. The reverse image search shows that these representations perform quite well when searching using an image as your query.


To solve the cold start problem, we need a relationship between a query and an image rather than between two images. A 2011 paper by Ronan Collobert et al.,[Natural Language Processing (almost) from Scratch](https://arxiv.org/abs/1103.0398) , offers some help. It demonstrates that we can create dense vector representations of words using neural networks. These representations (word embeddings) can be used, and perform well, for a variety of natural language processing tasks. Both the image and word embedding approaches do something similar: They take a piece of human understandable information and embed it into a dense vector space such that similar pieces of information are close to each other in the vector space.


Combining image and word embeddings seems like a natural fit for the cold start problem. Fortunately, there is no shortage of research involving feature embeddings. A recent paper from[Etsy](https://etsy.com/) ,[Images Don’t Lie: Transferring Deep Visual Semantic Features to Large-Scale Multimodal Learning to Rank](http://arxiv.org/abs/1511.06746) , demonstrates that leveraging image embeddings can yield better search results. Unfortunately, the approach in this paper requires building a separate ranking function for each query, which doesn’t generalize well enough for our domain. A Google paper,[Show and Tell: A Neural Image Caption Generator](https://arxiv.org/abs/1411.4555) , from 2015 describes how they combined image embeddings and word embeddings to generate captions for images. This is almost what we are after. We would like images and queries to live in the same embedding space which would allow us to search by comparing images directly to queries. Ultimately, we decided to go with a simpler and more direct approach that was largely informed by our previous experience and[Unifying Visual-Semantic Embeddings with Multimodal Neural Language Models](https://arxiv.org/abs/1411.2539) from the University of Toronto.


### Approach


Our model has two parts: a neural net image embedder and a neural net query embedder. To train the image embedder, we begin with a domain specific DNN classifier which classifies images as one of several thousand classes. Training pairs for the data set were created by matching our most popular queries with the images that were downloaded for those queries. We can then use this model to embed images as described above.


The second part of our approach uses a neural net language model which is trained on positive and negative pairs of queries and images. The positive pairs are created by matching queries with images that customers selected for that query. This is a superset of the data used to train the image embedder. Rather than thousands of queries as in the previous set, this set contains millions of queries. The negative pairs are created by randomly matching queries and images. We train the language embedder using a loss function that maximizes the difference between each positive and negative pair.


Once the models are trained, they can embed images and queries in the same space, called the multimodal space, making images and text directly comparable. This allows us to perform a search using only the models. Given a query, we encode it into a point in the multimodal space and find the images within our library that are closest to that point.


### Examples and results


#### Cold start results


To test our approach, we ranked all of our images using both our traditional ranker and the joint embedding ranker against our 100,000 most popular searches, considered the top 100 and top 20 results for each query. We calculated the average percentage of images that are newer than one month and newer than three months as a measurement of effectiveness in handling the cold start problem. As can be seen, by the results below, the joint embedding ranker exposes more than twice as many images newer than three months than our traditional ranker. It exposes an even higher multiple of images newer than one month.


Ranker Top 100 newer than 1 months Top 100 newer than 3 months Top 20 newer than 1 month Top 20 newer than 3 months


Traditional 0.72% 4.71% 0.28% 4.02%


Joint 2.68% 9.6% 2.59% 9.35%


*Percent of new images for 100,000 most popular queries for our traditional ranker and joint embedding ranker*


These results consider each of the 100,000 queries equivalently, but the most popular query is searched for approximately 1,000 times more often than 100,000th most popular. To properly represent the images that will be shown to customers, we should take into account the number of executions of each query in the results. Since our traditional ranker prefers images with more behavioral data, and we are more likely to gather behavioral data for more common queries, we expect the difference to be greater for those queries. Indeed, when we normalize the metrics by the number of executions of each query, the difference becomes more pronounced:


Traditional 0.47% 4.33% 0.21% 3.07%


Joint 2.63% 9.36% 2.54% 9.25%


*Percent of new images for 100,000 most popular queries, weighted by popularity of query, for our traditional ranker and joint embedding ranker*


In addition to improving the cold start problem, we found that using joint embeddings addresses a number of problems inherent to our ranking approach”, which we will explore in part two.
