---
schema_version: "1.0.0"
document_id: "6bd39d25bda7d412930f6a8365b4be13f0606e24dfbfed65557d2beca53438bb"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/prompt-engineering-for-transformer-based-chemical-similarity-search-identifies-structurally-distinct-functional-analogues"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:8b63f89f19e4bf6d697fe6d370e2b0805900d6eaced9ce77389df1ac732bc778"
---

# Prompt Engineering for Transformer-based Chemical Similarity Search Identifies Structurally Distinct Functional Analogues

Do not index


Original Paper


[https://arxiv.org/abs/2305.16330](https://arxiv.org/abs/2305.16330)


Blog URL


[blog.athina.ai /prompt-...nalogues](https://blog.athina.ai/prompt-engineering-for-transformer-based-chemical-similarity-search-identifies-structurally-distinct-functional-analogues)


**Original Paper:**[https://arxiv.org/abs/2305.16330](https://arxiv.org/abs/2305.16330)


**By:**[Clayton W. Kosonocky](https://arxiv.org/search/physics?searchtype=author&query=Kosonocky%2C%20C%20W) ,[Aaron L. Feller](https://arxiv.org/search/physics?searchtype=author&query=Feller%2C%20A%20L) ,[Claus O. Wilke](https://arxiv.org/search/physics?searchtype=author&query=Wilke%2C%20C%20O) ,[Andrew D. Ellington](https://arxiv.org/search/physics?searchtype=author&query=Ellington%2C%20A%20D)


**Abstract:**


> Chemical similarity searches are widely used in-silico methods for identifying new drug-like molecules. These methods have historically relied on structure-based comparisons to compute molecular similarity. Here, we use a chemical language model to create a vector-based chemical search. We extend implementations by creating a prompt engineering strategy that utilizes two different chemical string representation algorithms: one for the query and the other for the database. We explore this method by reviewing the search results from five drug-like query molecules (penicillin G, nirmatrelvir, zidovudine, lysergic acid diethylamide, and fentanyl) and three dye-like query molecules (acid blue 25, avobenzone, and 2-diphenylaminocarbazole). We find that this novel method identifies molecules that are functionally similar to the query, indicated by the associated patent literature, and that many of these molecules are structurally distinct from the query, making them unlikely to be found with traditional chemical similarity search methods. This method may aid in the discovery of novel structural classes of molecules that achieve target functionality.


---


###


Summary Notes


####


Blog Post: Revolutionizing Chemical Discovery with AI and Language Models


The field of chemical discovery, especially in pharmacology and material science, is undergoing a significant transformation, thanks to artificial intelligence (AI) and transformer-based language models.


These technologies are reshaping how new molecules are discovered and developed.


####


The Emergence of Chemical Language Models


Chemical Language Models (CLMs), powered by transformer technology, are making strides in identifying molecules with specific properties and functionalities.


They go beyond traditional methods by finding molecules that are structurally diverse but functionally similar. This breakthrough has vast implications for drug discovery, material development, and more.


####


Key Innovations in Chemical Search


- **Chemical Semantic Search (CheSS):** This approach uses the Simplified Molecular-Input Line-Entry System (SMILES) to digitize molecules. These are then converted into feature vectors by a chemical language model, and similarities are determined using cosine similarity measures. CheSS enables the rapid identification of molecules with desired traits from vast databases.


- **ChemBERTa's Role:** ChemBERTa, a model trained on millions of SMILES strings, excels at creating detailed embeddings of molecules. This allows for the precise identification of functionally similar yet structurally distinct molecules.


- **Importance of Canonicalization:** The process of converting molecules into a unique SMILES string is crucial. The choice of canonicalization algorithm can greatly influence the search results, impacting the discovery process.


####


Discovering New Functional Analogues


A significant achievement of this technology is the identification of functional analogues with little structural similarity to the original query.


This is particularly beneficial for drug discovery, offering new avenues for developing therapeutic agents and advancing in various scientific fields where molecular functionality is key.


####


Broader Implications for Science and Medicine


The use of transformer-based chemical similarity search has profound implications, potentially accelerating innovation in drug development and material science.


By finding functional analogues efficiently, it also paves the way for drug repurposing, potentially speeding up the introduction of new treatments.


####


Conclusion


AI and transformer-based language models are heralding a new era in molecular discovery. Tools like ChemBERTa and Chemical Semantic Search empower researchers to navigate the complex molecular landscape more effectively.


As this technology evolves, its impact on scientific innovation, particularly in drug discovery and material science, is expected to be significant.


####


Acknowledgements and Further Reading


The Texas Advanced Computing Center at The University of Texas at Austin and various organizations have played a pivotal role in this research.


For those interested, project codes and results are accessible on GitHub, offering a rich resource for further exploration.


####


Looking Ahead


This paradigm shift not only equips researchers with advanced tools for tackling diseases and material challenges but also highlights the transformative potential of integrating AI with science.


The journey into AI-driven molecular discovery is just beginning, with endless possibilities on the horizon.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
