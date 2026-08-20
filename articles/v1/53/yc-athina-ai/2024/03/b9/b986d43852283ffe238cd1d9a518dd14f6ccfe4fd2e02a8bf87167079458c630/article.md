---
schema_version: "1.0.0"
document_id: "b986d43852283ffe238cd1d9a518dd14f6ccfe4fd2e02a8bf87167079458c630"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/inferring-properties-of-graph-neural-networks"
published_at: "2024-03-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:a3d425e47908870486e163f14fdb176adb610ec9200b762fc175807af8e640ce"
---

# Inferring Properties of Graph Neural Networks

Do not index


Original Paper


[https://arxiv.org/abs/2401.03790](https://arxiv.org/abs/2401.03790)


Blog URL


[blog.athina.ai /inferri...networks](https://blog.athina.ai/inferring-properties-of-graph-neural-networks)


**Original Paper:**[https://arxiv.org/abs/2401.03790](https://arxiv.org/abs/2401.03790)


**By:**[Dat Nguyen](https://arxiv.org/search/cs?searchtype=author&query=Nguyen%2C%20D) (1),[Hieu M. Vu](https://arxiv.org/search/cs?searchtype=author&query=Vu%2C%20H%20M) (2),[Cong-Thanh Le](https://arxiv.org/search/cs?searchtype=author&query=Le%2C%20C) (1),[Bach Le](https://arxiv.org/search/cs?searchtype=author&query=Le%2C%20B) (1),[David Lo](https://arxiv.org/search/cs?searchtype=author&query=Lo%2C%20D) (3),[ThanhVu Nguyen](https://arxiv.org/search/cs?searchtype=author&query=Nguyen%2C%20T) (4)[Corina Pasareanu](https://arxiv.org/search/cs?searchtype=author&query=Pasareanu%2C%20C) (5) ((1) University of Melbourne, (2) Independent Researcher, (3) Singapore Management University, (4) George Mason University, (5) Carnegie Mellon University)


**Abstract:**


> We propose GNNInfer, the first automatic property inference technique for GNNs. To tackle the challenge of varying input structures in GNNs, GNNInfer first identifies a set of representative influential structures that contribute significantly towards the prediction of a GNN. Using these structures, GNNInfer converts each pair of an influential structure and the GNN to their equivalent FNN and then leverages existing property inference techniques to effectively capture properties of the GNN that are specific to the influential structures. GNNINfer then generalizes the captured properties to any input graphs that contain the influential structures. Finally, GNNInfer improves the correctness of the inferred properties by building a model (either a decision tree or linear regression) that estimates the deviation of GNN output from the inferred properties given full input graphs. The learned model helps GNNInfer extend the inferred properties with constraints to the input and output of the GNN, obtaining stronger properties that hold on full input graphs.
>
>
> Our experiments show that GNNInfer is effective in inferring likely properties of popular real-world GNNs, and more importantly, these inferred properties help effectively defend against GNNs' backdoor attacks. In particular, out of the 13 ground truth properties, GNNInfer re-discovered 8 correct properties and discovered likely correct properties that approximate the remaining 5 ground truth properties. Using properties inferred by GNNInfer to defend against the state-of-the-art backdoor attack technique on GNNs, namely UGBA, experiments show that GNNInfer's defense success rate is up to 30 times better than existing baselines.


---


###


Summary Notes


####


Simplifying GNN Analysis and Security with GNN-Infer


Graph Neural Networks (GNNs) are transforming our approach to graph-structured data, from analyzing social networks to improving recommendation systems.


However, the complexity of their architecture introduces significant security and understandability challenges. GNN-Infer is a cutting-edge framework designed to enhance how we analyze, debug, and secure GNNs.


This blog post explores GNN-Infer's approach, its application, and its impact on the future of GNN applications.


####


Understanding GNN Complexity


GNNs are unique in their ability to handle varying input sizes and complex structures through a message-passing mechanism.


This flexibility brings analysis and verification challenges, especially when facing adversarial attacks. GNN-Infer offers a solution by providing new ways to understand and improve GNNs.


####


Key Features of GNN-Infer


GNN-Infer addresses GNN complexities by:


- **Identifying Key Graph Structures:** It locates graph structures that significantly impact the GNN's outcomes.


- **Mapping to Feedforward Networks:** These structures and the GNN are mapped onto an equivalent Feedforward Neural Network (FNN), allowing the use of established FNN analysis methods.


- **Generalizing Inferred Properties:** It generalizes key properties from specific structures to broader scenarios.


- **Improving Property Accuracy:** The framework refines these properties by estimating the GNN output deviations across the entire input graph.


####


Implementation and Impact


Tested on both synthetic and real-world datasets, GNN-Infer has proven effective in identifying GNN properties and enhancing security against backdoor attacks.


It accurately infers and generalizes GNN properties, improving resistance against attacks more effectively than current methods.


####


Contributions to GNN Analysis


GNN-Infer introduces:


- **A Novel Conversion Technique:** Transforming GNNs into FNNs, facilitating the use of traditional neural network analysis tools.


- **A Comprehensive Framework:** For inferring and generalizing GNN properties, focusing on critical graph structures.


- **Enhanced Security Measures:** Demonstrating significant potential in securing GNN applications against backdoor attacks.


####


Future Directions


The development of GNN-Infer is ongoing, with plans to increase its efficiency and application range. Its potential in GNN verification and debugging is vast, promising exciting future developments.


####


Conclusion


GNN-Infer represents a significant advancement in GNN analysis and security. It makes GNNs more analyzable, aiding in debugging, verification, and securing against adversarial attacks.


This development is crucial for future research and application in GNN models, heralding a move towards more secure and understandable GNN applications across various domains.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
