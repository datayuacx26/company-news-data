---
schema_version: "1.0.0"
document_id: "f5e4a1e8c63ee0b878220a26ebc4f4e09e4c48fc7c10bfa3c540ea4eed384064"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/information-retrieval-introduction"
published_at: "2025-02-08T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:80ec0c01910bdf4b021e5a3f7370637565989a45b53e26f339849889b408f62c"
---

# Exploring Information Retrieval from BoW to BM25

## Abstract


*This post explores foundational algorithms of Information Retrieval, specifically describing the evolution from Bag of Words to TF-IDF and BM25. It details how these models function by leveraging term frequency and document length normalization to rank search results effectively. While these methods remain important for modern production search systems due to their efficiency, the analysis also highlights their inherent limitations in handling synonyms and semantic context. The discussion concludes by positioning these traditional techniques as essential baselines that pave the way for understanding newer neural-based retrieval methods.*


---


## Introduction


Every day, businesses and individuals generate and search through vast amounts of text, for example by retrieving internal documents, navigating customer support tickets, or browsing e-commerce catalogs. Finding relevant information efficiently within large datasets is a fundamental challenge, and this is where Information Retrieval (IR) plays a crucial role.


First introduced by Calvin N. Mooers in 1950 \[1\], the term Information Retrieval describes the process of storing, organizing, and retrieving textual data in a way that optimizes relevance and efficiency. Early keyword-based retrieval systems formed the foundation of search, and more sophisticated ranking models, such as TF-IDF and BM25, followed. These approaches remain widely used in modern search engines due to their effectiveness in ranking documents by relevance.


More recently, deep learning has enabled semantic search, which moves beyond exact keyword matching by capturing meaning through vector representations. While these methods introduce new capabilities, they also come with added complexity.


This post explores traditional retrieval methods, focusing on TF-IDF and BM25, the two most widely used algorithms for ranking search results. We will examine how these models work, why they are effective, and where they struggle with challenges such as synonyms, typos, and query intent mismatches. Finally, we will discuss their limitations and set the stage for future discussions on neural-based retrieval methods.


## Categories of Information Retrieval Models


At the core of information retrieval is the idea that words appearing in similar contexts tend to be related. This concept, known as the distributional hypothesis \[2\], suggests that relevant terms are more likely to co-occur within documents, while unrelated terms appear in different contexts. Retrieval models build on this assumption by representing documents in ways that enable efficient identification of relevant content.


Retrieval models can be broadly categorized into three main types.


**Set-theoretic models** treat documents as sets of words or phrases, retrieving matches based on operations like intersection and union. A classic example is the Boolean model, which relies on exact term matching. More flexible variations, such as fuzzy retrieval, allow for approximate matches to account for variations in phrasing


**Algebraic models** represent text using numerical structures, such as vectors or matrices, enabling similarity computations. Examples include the Vector Space Model (VSM) and Latent Semantic Analysis (LSA), which measure how closely a query and a document align in a high-dimensional space.


**Probabilistic models** estimate the likelihood that a document is relevant to a given query. These models, such as BM25 and Language Models for Information Retrieval (LMIR), rank documents using statistical inference and probability distributions.


Each of these approaches captures relationships between words in different ways, for example through direct term matching, statistical co-occurrence, or probabilistic reasoning. Before exploring more sophisticated techniques, we first examine a fundamental retrieval method: keyword matching.


## Selected Retrieval Models


Retrieval models vary in complexity, from simple keyword matching to advanced ranking algorithms. Understanding the fundamentals provides a solid foundation for more sophisticated techniques. We begin with keyword matching, the most basic retrieval method, before introducing the bag-of-words model, which improves flexibility by considering word frequency. Building on these concepts, we then explore TF-IDF and BM25, two ranking models that remain widely used in modern search systems.


### Keyword Matching


The simplest form of information retrieval is keyword matching, where a document is considered relevant if it contains the exact words from the query. This set-theoretic approach relies on direct string matching without any understanding of context, synonyms, or word importance.


For example, given the query "machine learning models", a keyword-based retrieval system would:


- Retrieve documents that contain "machine learning models" as an exact phrase.
- Fail to retrieve documents that use synonyms, such as "AI algorithms" or "deep learning architectures".
- Treat all matched documents equally, without ranking them based on relevance.


While keyword matching is computationally efficient, its rigid nature limits retrieval effectiveness. It cannot handle variations in phrasing, fails to account for word importance, and does not differentiate between highly relevant and marginally relevant documents. These limitations lead to poor retrieval performance in real-world applications, where queries often include synonyms, paraphrases, and varying word orders. To improve upon this, we turn to the Bag-of-Words model.


### Bag of Words (BoW)


The Bag-of-Words model enhances retrieval by representing documents as collections of words, ignoring their order while preserving their frequency \[2\], \[3\]. This abstraction allows retrieval systems to consider documents with partial matches, making search results more flexible than exact keyword matching.


For example, consider the following two documents:


1. "Machine learning is powerful." → {machine:1,learning:1,is:1,powerful:1}\\{machine: 1, learning: 1, is: 1, powerful: 1\\}


{


ma


c


hin


e


:


1


,


l


e


a


r


nin


g


:


1


,


i


s


:


1


,


p


o


w


er


f


u


l


:


1


}


2. "Deep learning models are advancing." → {deep:1,learning:1,models:1,are:1,advancing:1}\\{deep: 1, learning: 1, models: 1, are: 1, advancing: 1\\}


{


d


ee


p


:


1


,


l


e


a


r


nin


g


:


1


,


m


o


d


e


l


s


:


1


,


a


re


:


1


,


a


d


v


an


c


in


g


:


1


}


If a user searches for "learning models," keyword matching would fail unless a document contained the exact phrase. However, with BoW, both documents could be retrieved since they contain at least one matching word.


This set-theoretic approach introduces two key improvements over strict keyword matching. First, it allows ranking of documents based on word occurrence, meaning that a document containing a query term multiple times is considered more relevant than one where it appears only once. Second, it enables partial matches, meaning a document does not need to contain the exact phrase to be considered relevant.


However, BoW has a major drawback: it treats all words equally, failing to distinguish between important terms and common stopwords like "is," "are," and "the." This can lead to noisy results where documents are retrieved based on unimportant words rather than truly relevant content.


BoW can also be used as an algebraic model, where documents are represented by feature vectors. This enables machine learning models such as SVM or logistic regression to perform classifications, an application that is used for example in spam detection systems \[4\].


To address the weaknesses of BoW, weighting mechanisms like Term Frequency-Inverse Document Frequency (TF-IDF) and BM25 refine the retrieval process by prioritizing informative terms. We explore these next.


### Term Frequency–Inverse Document Frequency (TF-IDF)


As the Bag-of-Words model treats all terms as equally important, it struggles to differentiate between common and meaningful words. TF-IDF (Term Frequency-Inverse Document Frequency) improves on this by assigning weights to terms, increasing the importance of words that are significant to a document while down-weighting commonly occurring words that appear across many documents \[5\]. This makes it one of the earliest and most widely used techniques for ranking documents by relevance.


This concept establishes the foundation for statistical text retrieval. TF-IDF remains widely used today in search engines, document classification, and information retrieval systems due to its simplicity and effectiveness.


TF-IDF assigns a numerical score to each term in a document, capturing both how frequently the term appears and how important it is in distinguishing relevant documents. It is defined as the product of two components: Term Frequency (TF) and Inverse Document Frequency (IDF).


**1. Term Frequency (TF)**


Term frequency measures how often a term appears in a given document. A term that occurs frequently is considered more relevant to that document. The raw count of a term is normalized by the total number of words in the document to prevent longer documents from being unfairly favored:


TF(t,d)=ft,d∑w∈dfw,d\\text{TF}(t, d) = \\frac{f_{t,d}}{\\sum_{w \\in d} f_{w,d}}


TF


(


t


,


d


)


=


∑


w


∈


d


​


f


w


,


d


​


f


t


,


d


​


​


where ft,df_{t,d}


f


t


,


d


​


is the count of term tt


t


in document dd


d


, and the denominator sums over all word occurrences in dd


d


.


While term frequency helps identify relevant terms, it alone is insufficient. Common words like "the" or "is" might appear frequently but provide little value in distinguishing documents. This is where Inverse Document Frequency (IDF) comes in.


**2. Inverse Document Frequency (IDF)**


Inverse Document Frequency reduces the weight of terms that appear too frequently across documents, as they are less useful for retrieval. The IDF of a term is computed as:


IDF(t)=log⁡(1+N1+∣{d∈D:t∈d}∣)\\text{IDF}(t) = \\log \\left( \\frac{1 + N}{1 + |\\{ d \\in D : t \\in d \\}|} \\right)


IDF


(


t


)


=


lo g


(


1


+


∣


{


d


∈


D


:


t


∈


d


}


∣


1


+


N


​


)


where NN


N


is the total number of documents in the collection, ∣{d∈D:t∈d}∣|\\{ d \\in D : t \\in d \\}|


∣


{


d


∈


D


:


t


∈


d


}


∣


is the number of documents containing the term. The +1+1


+


1


in the denominator prevents division by zero when a term is missing from all but one document


**Final TF-IDF Score**


The final TF-IDF score is the product of TF and IDF:


TF-IDF(t,d)=TF(t,d)⋅IDF(t)\\text{TF-IDF}(t, d) = \\text{TF}(t, d) \\cdot \\text{IDF}(t)


TF-IDF


(


t


,


d


)


=


TF


(


t


,


d


)


⋅


IDF


(


t


)


This score increases when a term appears frequently in a document but is rare across the entire collection, making it a strong indicator of relevance.


Despite its advantages, TF-IDF has several limitations that affect its performance in retrieval tasks. One major drawback is that it ignores word order, treating phrases like "deep learning" and "learning deep" as equivalent, even though their meanings might differ in context. Additionally, TF-IDF assumes term independence, meaning that words are considered separately rather than as part of meaningful phrases. This limitation makes it difficult to capture multi-word concepts such as "New York" or "artificial intelligence" as a single unit.


Another issue is that TF-IDF tends to favor shorter documents, since longer documents naturally contain more terms, leading to inflated relevance scores. As a result, a document with many occurrences of a term may rank higher than a shorter but more precise document. Because TF-IDF does not account for document length explicitly, it can sometimes lead to biased rankings.


Despite these weaknesses, TF-IDF remains a widely used technique due to its simplicity and efficiency. However, modern retrieval models such as BM25 improve upon TF-IDF by introducing document length normalization and saturation, making them more effective for ranking documents based on relevance.


### Okapi BM25


Okapi BM25, or simply BM25 which is short for Best Matching 25, is a ranking function that refines TF-IDF by addressing its key limitations. Developed by Stephen Robertson, Karen Spärck Jones et. al. in the 1990s by building on their work on a probabilistic framework going back to the 1970s \[6\], BM25 remains one of the most effective and widely used ranking functions in modern information retrieval systems. Like TF-IDF, it assigns a relevance score to documents based on term frequency and inverse document frequency, but it incorporates additional mechanisms to improve ranking accuracy.


One significant improvement over TF-IDF is the saturation of term frequency. In traditional TF-IDF, a term appearing more frequently in a document continuously increases its relevance score. However, in reality, after a certain point, additional occurrences of the same term do not necessarily make a document more relevant. BM25 addresses this by applying a saturation function to term frequency:


TF′(t,d)=f(t,d)⋅(k1+d)f(t,d)+k1⋅(1−b+b⋅∣d∣avgdl)TF^{\\prime}(t, d) = \\frac{f(t, d) \\cdot (k_{1} + d)}{f(t, d) + k_1 \\cdot (1 - b + b \\cdot \\frac{|d|}{\\text{avgdl}})}


T


F


′


(


t


,


d


)


=


f


(


t


,


d


)


+


k


1


​


⋅


(


1


−


b


+


b


⋅


avgdl


∣


d


∣


​


)


f


(


t


,


d


)


⋅


(


k


1


​


+


d


)


​


where f(t,d)f(t, d)


f


(


t


,


d


)


represents the frequency of term tt


t


in document dd


d


, ∣d∣|d|


∣


d


∣


is the length of the document, and avgdl\\text{avgdl}


avgdl


is the average document length in the collection. The parameters k1k_{1}


k


1


​


and bb


b


control the influence of term frequency and document length normalization, respectively. Typical values are k1≈1.2k_{1} \\approx 1.2


k


1


​


≈


1.2


and b≈0.75b \\approx 0.75


b


≈


0.75


, but they can be fine-tuned for different datasets. This formulation ensures that term frequency contributions diminish as occurrences increase, preventing overly long documents from dominating rankings solely due to high word repetition.


Another key refinement is document length normalization. In TF-IDF, longer documents naturally accumulate more terms, which can lead to an unfair ranking advantage. BM25 adjusts for this by scaling term frequency based on document length, controlled by the bb


b


parameter. If b=1b = 1


b


=


1


, full normalization occurs, meaning longer documents are penalized more heavily, whereas b=0b = 0


b


=


0


removes normalization entirely. The default value of b=0.75b = 0.75


b


=


0.75


provides a balance, ensuring that longer documents are not unfairly ranked higher while still considering their relevance.


The final BM25 score for a document dd


d


given a query qq


q


is computed as:


BM25(d,q)=∑t∈qIDF(t)⋅TF′(t,d)BM25(d, q) = \\sum_{t \\in q} IDF(t) \\cdot TF^{\\prime}(t, d)


BM


25


(


d


,


q


)


=


t


∈


q


∑


​


I


D


F


(


t


)


⋅


T


F


′


(


t


,


d


)


where IDF(t) is computed similarly to TF-IDF but with a slight adjustment to prevent extreme values, ensuring that rare terms remain significant without overemphasizing them.


Despite its strengths, BM25 has some limitations. One challenge is the need for hyperparameter tuning. The parameters k1k_{1}


k


1


​


and bb


b


require careful selection, as their values significantly impact retrieval performance. In addition, BM25, like TF-IDF, assumes term independence, meaning it does not capture relationships between words, phrases, or semantic meaning. Nevertheless, BM25 remains the dominant ranking function in search engines and is often the first step before applying more advanced neural retrieval models. Its efficiency, robustness, and effectiveness make it a cornerstone of modern information retrieval systems.


## Retriever Evaluation


To measure the performance of retrieval models, evaluation metrics are needed to assess both the relevance of retrieved documents and the quality of their ranking. A well-performing retrieval system should return the most relevant documents as high as possible in the results list while minimizing irrelevant ones.


Several widely used metrics help quantify this performance. Mean Reciprocal Rank (MRR) evaluates how high the first relevant document appears in the ranking. If a relevant document is found at position one, the score is 1; if at position two, the score is 0.5, and so on. MRR is particularly useful for scenarios where users expect a single, highly relevant result, such as question-answering systems.


Another important metric is Mean Average Precision (MAP), which measures precision at different levels of recall. Unlike MRR, which focuses on the first relevant document, MAP considers the order and relevance of multiple retrieved documents. It averages precision across different recall points and multiple queries, making it a strong measure for ranking quality in cases where multiple relevant results exist.


For applications where relevance is graded rather than binary, Normalized Discounted Cumulative Gain (NDCG) is a commonly used metric. It accounts for the fact that some documents may be more relevant than others and rewards systems that rank highly relevant documents higher in the results. This makes it particularly useful for web search and recommendation systems, where relevance often exists on a scale rather than a simple relevant/irrelevant distinction.


The choice of metric depends on the specific retrieval task. If the primary goal is to return a single correct answer, MRR is often the best fit. When multiple relevant results exist, MAP provides a more holistic measure. In cases where relevance is graded, NDCG offers a more nuanced evaluation.


For a more detailed discussion on these metrics and their applications, I have written dedicated[post](https://deconvoluteai.com/blog/rag/metrics-retrieval) that explore retriever evaluation in more depth.


## Conclusion


In this post, we have explored traditional information retrieval models, starting with the distributional hypothesis that words with similar meanings tend to appear in similar contexts. Based on this idea, models like Bag of Words (BoW), TF-IDF, and BM25 rank documents by scoring their relevance based on term frequency and document importance. BM25, in particular, remains a dominant model due to its simplicity and ability to handle term frequency saturation and document length variations.


While these models are efficient and widely used, they face limitations when it comes to understanding context, handling synonyms, and grasping the true intent behind queries. This is where semantic search comes in, offering a shift from keyword matching to understanding meaning through neural embeddings and deep learning models.


In a future post, we will dive deeper into semantic search techniques, such as word embeddings and transformer-based models, and explore how they can complement traditional retrieval models like BM25 to enhance search quality. Combining the strengths of both approaches could be the key to improving search systems while balancing speed and accuracy.


## References


\[1\] Mooers, Calvin N. *The Theory of Digital Handling of Non-Numerical Information and Its Implications to Machine Economics.* Proceedings of the Meeting of the Association for Computing Machinery at Rutgers University. 1950.


\[2\] Harris, Zellig S. *Distributional Structure.* Word 10, no. 2–3. 1954[https://doi.org/10.1080/00437956.1954.11659520](https://doi.org/10.1080/00437956.1954.11659520) .


\[3\] Maron, M. E., and J. L. Kuhns. *On Relevance, Probabilistic Indexing and Information Retrieval.* Journal of the ACM 7, no. 3. 1960.[https://doi.org/10.1145/321033.321035](https://doi.org/10.1145/321033.321035) .


\[4\] Kawintiranon, Kornraphop, Lisa Singh, and Ceren Budak. *Traditional and Context-Specific Spam Detection in Low Resource Settings.* Machine Learning 111, no. 7 (July 2022): 2515–36.[https://doi.org/10.1007/s10994-022-06176-x](https://doi.org/10.1007/s10994-022-06176-x) .


\[5\] Jones, Karen Spärck. *A Statistical Interpretation of Term Specificity and Its Application in Retrieval.* 1972.


\[6\] Keen, E Michael. *Okapi at TREC 3.* Centre for Interactive Systems Research Department of Information Science City University Northampton Square London EC1V 0HB UK. 1994.
