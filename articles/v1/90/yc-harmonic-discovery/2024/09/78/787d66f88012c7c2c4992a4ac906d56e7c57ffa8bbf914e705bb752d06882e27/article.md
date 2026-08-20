---
schema_version: "1.0.0"
document_id: "787d66f88012c7c2c4992a4ac906d56e7c57ffa8bbf914e705bb752d06882e27"
company_key: "yc-harmonic-discovery"
company: "Harmonic Discovery"
source_id: "yc-harmonic-discovery-rss-ada272b06e31"
canonical_url: "https://news.harmonicdiscovery.com/unlocking-the-full-potential-of-data-in-ai-driven-drug-discovery-a9923818cdb9"
published_at: "2024-09-11T13:18:21+00:00"
first_seen_at: "2026-07-25T07:36:23.550190+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:5e732e4a1441828ac040d66042e91913062ae0776011a62c0d718a973dd1b8f5"
---

# Why hasn’t AI in Drug Discovery had its ChatGPT moment?

# **Why hasn’t AI in Drug Discovery had its ChatGPT moment?**


[Rayees Rahman](https://medium.com/@rayees_76660?source=post_page---byline--a9923818cdb9---------------------------------------)


4 min read


·


Aug 22, 2024


--


Written by Anna Cichońska & Rayees Rahman


Artificial intelligence has been regarded as the next big thing in drug discovery, promising to revolutionize the way we identify new therapies and bring them to market faster. Yet, despite the hype, AI in pharma has not quite delivered on its full potential. Why? Our recent paper, published in[Nature Communications](https://www.nature.com/articles/s41467-024-52055-5) , sheds light on the challenges and opportunities in harnessing AI in early-stage drug discovery, particularly when it comes to data utilization, model benchmarking and prediction interpretation.


## Get Rayees Rahman’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Here are the Four key takeaways:


## Finding drugs isn’t just about getting a yes or no answer — it’s about understanding why


Training a model to generate predictions is easy, but we’ve shown that real progress comes from exploring the model’s underlying uncertainty about its predictions. We’ve demonstrated that by modeling uncertainty as a function of the diversity of the data, we can significantly boost hit rates across **well-studied** and **new protein targets** . *Our point is that it’s not just about getting AI to make guesses, but to explain its reasoning.*


Press enter or click to view image in full size


Understanding the relationship between model confidence, training data and experimental hit rates


## **We’re wasting data like crazy**


Over 40% of training data we have gets tossed out because we’re not integrating it properly. Much of this includes results from inexpensive assays as well as valuable negative data. We’ve built a method to integrate data across multiple bioactivity data sources and show that our approach significantly improves performance across several machine learning models. This approach allows us to train models on data that’s *an order of magnitude more cost-efficient to generate.*


Press enter or click to view image in full size


## **Simpler is often better**


We found that fancy AI models are often overfitted and perform poorly, while simpler models do way better. Kernel ridge regression and random forests blew complex models out of the water and *set the real benchmark for understanding model performance.*


Press enter or click to view image in full size


Performance of selected models in a challenging compound scaffold split


## **Testing predictions in the lab > *in silico* performance**


Most papers in small molecule machine learning neglect real-world testing. In this work, we tested model predictions across 347 **compound-protein pairs** with a stringent activity threshold of 1μM ** — *relevant for real drug discovery* . We got a 40% hit rate and, just as importantly, we correctly identified 78% of the negative cases (a metric rarely reported).


Press enter or click to view image in full size


Experimental validation of model prediction across 13 proteins and 139 tested molecules.
