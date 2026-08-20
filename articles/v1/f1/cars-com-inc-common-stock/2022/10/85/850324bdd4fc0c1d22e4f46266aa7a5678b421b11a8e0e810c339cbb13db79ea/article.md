---
schema_version: "1.0.0"
document_id: "850324bdd4fc0c1d22e4f46266aa7a5678b421b11a8e0e810c339cbb13db79ea"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/understanding-auto-retailers-and-their-competitors-using-embeddings-and-graph-networks-753caaaef427"
published_at: "2022-10-04T21:21:42+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:30.015715+00:00"
content_hash: "sha256:3d2d8243415f6a5a8e3e0f0f5804aed10c528e6fccc0d5cc637645e72a37a869"
---

# Understanding Auto Retailers and their Competitors using Embeddings and Graph Networks

# **Understanding Auto Retailers and their Competitors using Embeddings and Graph Networks**


[Scott Bishop](https://medium.com/@scottandrewbishop?source=post_page---byline--753caaaef427---------------------------------------)


7 min read


·


Oct 4, 2022


--


Press enter or click to view image in full size


**Introduction**


As a platform serving dealers operating in the automotive industry, CARS aims to provide robust tools that assist dealers in reaching prospective shoppers more efficiently and effectively. In order to do so it is important to understand the dealers we serve, whether that understanding relates to their individual representation or their competition.


However, this task is complex when we serve over 19,000 dealers contributing to nearly 2 million listings accessed by over 25 million online shoppers monthly. To address this problem CARS’s data science team has developed the *Dealer Embedding and Competition Model* . This model produces a universal dealer entity representation in the form of an embedding, and leverages these embeddings to identify other top competitors for dealers.


**Dealer Embeddings**


An[embedding](https://developers.google.com/machine-learning/crash-course/embeddings/video-lecture) can simply be viewed as a list of numbers that capture characteristics of an entity they aim to represent. For dealer entity representation, embeddings can be a list of numbers that captures both the inventory attributes specific to a dealer and the shopper demand attributes specific to a dealer’s location.


Inventory attributes represented by a dealer’s embedding include characteristics like what vehicle brands are listed, price and mileage metrics related to listed vehicles, and how the listed vehicles compare to local market expectations as defined by CARS’s[Price Badge Model](https://tech.cars.com/cars-deal-badges-4dac3ad15bf5) . To ensure the inventory attributes are represented compactly in a dealer embedding, the raw numerical values representing inventory characteristics are encoded using[Principal Component Analysis (PCA)](https://royalsocietypublishing.org/doi/10.1098/rsta.2015.0202) .


Shopper demand attributes represented by a dealer’s embedding include characteristics of visitor traffic shared between dealers. These attributes are represented compactly by constructing a dealer network using the[NetworkX](https://networkx.org/) library, and encoding that graph’s topology using a modified implementation of the[Node2Vec](https://snap.stanford.edu/node2vec/) algorithm, which is open source and developed as part of[Stanford’s Network Analysis Project (SNAP)](http://snap.stanford.edu/index.html) .


The figure below visualizes a sample of the generated dealer network. The nodes represent dealers, a node’s color represents the number of neighbors that node has — brighter nodes have more connections, and the proximity between nodes is based on shared visitor traffic characteristics.


Press enter or click to view image in full size


Sample of the dealer graph generated using the NetworkX library


The figure below visualizes the frequency of neighbor counts across all nodes; the majority of dealers have under 100 neighbors that share visitor traffic characteristics.


Press enter or click to view image in full size


**Node2Vec**


The Node2Vec algorithm views a graph as a corpus: the nodes are words, and all other nodes that can be accessed via edge traversal from a given start node is a document. Node2Vec is then able to represent each node as a set of sentences through a set of random walks. Each random walk in a set begins with a start node, and then probabilistically traverses edges to neighbor nodes; each visited node in the walk is a word in the sentence.


Node2Vec’s step probabilities are based on both node connectivity and the edge weight between nodes. There exist two parameters weighting these step probabilities: one will prioritize the structural equivalence between dealers in an embedding, and the other will prioritize the homophily between dealers in an embedding. Homophily is prioritized as the goal of representing shared visitor traffic between dealers will be based on the proximity between dealers.


Using this representation, Node2Vec utilizes the[Word2Vec](https://jalammar.github.io/illustrated-word2vec/) algorithm under the hood to generate the component of a dealer’s embedding representing a dealer’s shopper demand attributes. A more technical introduction to the Node2Vec algorithm is provided in SNAP’s original[article](https://arxiv.org/abs/1607.00653) ; open-source code is provided in the following[GitHub](https://github.com/aditya-grover/node2vec) repository.


**Dealer Competitors**


The dealer embeddings used to represent dealer entities are constructed using a combination of PCA to encode dealer inventory attributes and Node2Vec to encode shopper demand attributes. These embeddings are able to identify the top competitors for a dealer, whether the competition is based on shared visitor traffic or inventory similarity.


## Get Scott Bishop’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The figure below visualizes the[T-SNE](https://www.analyticsvidhya.com/blog/2017/01/t-sne-implementation-r-python/) representation of the embedding’s competition component reduced to two dimensions. It is apparent that the aforementioned approach to construct embeddings is successful in capturing competition within dealer communities.


Press enter or click to view image in full size


T-SNE representation of the embedding’s competition component reduced to two dimensions


When competition between dealers is based on shared visitor traffic, all competitors of a dealer are determined and ranked by comparing a target dealer’s embedding with the embeddings of all candidate competitors. Candidate competitors are other dealers in a target dealer’s community that are within a set max distance threshold; the threshold optimizing candidate selection. As the rank of a dealer’s competitor approaches 1, that dealer’s collection of visitor traffic shared with that competitor increases.


It would be assumed that competitors are likely to be within geographical proximity to a target dealer, thus it would also be expected that as the rank of a dealer’s competitor approaches 1, that competitor would be closer to that dealer’s geographical location. The figure below illustrates that this assumption is accurate. A dealer’s competitor at rank 1 is approximately 12 miles away on average. As a competitor’s rank increases, so does the distance between a dealer and that competitor; distance is derived using the[haversine formula](https://www.igismap.com/haversine-formula-calculate-geographic-distance-earth/) .


**Model Evaluation**


Evaluating the embeddings in this case is not a straightforward task. First, there is no scalable approach to build the golden data set of dealer competitors. Second, embeddings are generally helpful in downstream tasks related to other machine learning models, which cannot be captured by any single metric. To get a fair sense of how the model is performing the ground truth of dealer competitors is approximated, then used to calculate the recall, precision, f1-score, and jaccard’s distance.


The set of true competitors for each dealer include all other dealers sharing visitor traffic with a target dealer, and the set of predicted competitors are those identified using dealer embeddings output by the model. The table below represents several statistics for each evaluation metric when considering the top fifty competitors.


Press enter or click to view image in full size


On average, over half of the competitors identified by the model share visitor traffic with a target dealer; they are first degree competitors. The competitors identified by the model who do not share visitor traffic with a target dealer are second or third degree competitors; they may not be in competition with a target dealer now, but may potentially become a competitor in the future.


**Model Deployment**


The CARS data science team leverages our open-source technology, called[Skelebot](https://tech.cars.com/skelebot-3903ae47c7b3) , to automate the generation of Docker images encapsulating model environments and job execution;[source code](https://github.com/carsdotcom/skelebot) and[documentation](https://carsdotcom.github.io/skelebot/) is available through[GitHub](https://github.com/carsdotcom/skelebot) . We also use[cars-forge](https://github.com/carsdotcom/cars-forge) to manage costs associated with our ML jobs.


These generated Docker images are deployed to[AWS ECR](https://aws.amazon.com/ecr/) using[Jenkins Pipelines](https://www.jenkins.io/doc/book/pipeline/) , and the results of the productionalized models are saved to[AWS S3](https://aws.amazon.com/pm/serv-s3/?trk=fecf68c9-3874-4ae2-a7ed-72b6d19c8034&sc_channel=ps&s_kwcid=AL%214422%213%21536452728638%21e%21%21g%21%21aws+s3&ef_id=CjwKCAjwpqCZBhAbEiwAa7pXeeZiiWVx-qvxKQw8bF1uBbe9SiJn5iXyqeXWEhqzpGh0l0sSa4nMARoCFVgQAvD_BwE%3AG%3As) **** and queried using[AWS RedShift](https://aws.amazon.com/pm/redshift/?trk=62b3fc85-63c0-49b4-a297-fb73de829c5c&sc_channel=ps&s_kwcid=AL%214422%213%21534559295207%21e%21%21g%21%21redshift&ef_id=CjwKCAjwpqCZBhAbEiwAa7pXeQtFmb9IsMcO79TFLA4jKlHaBJAJEFrdp_X1cJHJ0GNokrvDiqBiwBoCNloQAvD_BwE%3AG%3As) . The results are available in a dashboard built using the[Dash](https://plotly.com/dash/) library; the dashboard includes both anomaly detection and explainability for model results.


Model performance dashboards are vital for us at CARS as they assist in initiating the necessary protocols to ensure our models maintain the best performance possible while at the same time ensure our models are interpretable for both internal and external stakeholders.


**Conclusion**


CARS is dedicated to providing optimal value to dealers, whether that value comes from tools used to drive visitor traffic, or tools used to aid our internal teams to supply actionable insight and assistance to dealers. The *Dealer Embedding and Competition Model* introduces an efficient and effective approach to understand the value delivered to any dealer by comparing a dealer with their competitors identified by the model.


The sales, product, and data teams can now visualize various metrics like conversion and leads for a dealer with respect to its competitors, ultimately helping dealers better strategize. CARS also plans to leverage these embeddings to develop and evaluate other machine learning systems, like search and dealer churn.


We will continue to improve the *Dealer Embedding and Competition Model* by looking for ways to increase model performance. For example, one opportunity would be to leverage our recently released[Dealer Trust Signals](https://tech.cars.com/identifying-ambiguous-and-low-quality-vehicle-listings-with-huggingface-transformers-64560fe4b39b) model trained using[HuggingFace transformers](https://huggingface.co/docs/transformers/index) **.** This can be used to identify sellers which have ambiguous pricing in their listings.


CARS is actively searching for new talent to help grow the data team. If you or anyone you know is interested in working on similar projects at CARS head over to our[careers](https://www.cars.com/careers/#job-listings) page and browse the current opportunities!
