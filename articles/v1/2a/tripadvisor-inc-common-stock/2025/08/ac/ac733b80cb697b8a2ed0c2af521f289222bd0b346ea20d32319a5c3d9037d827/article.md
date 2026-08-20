---
schema_version: "1.0.0"
document_id: "ac733b80cb697b8a2ed0c2af521f289222bd0b346ea20d32319a5c3d9037d827"
company_key: "tripadvisor-inc-common-stock"
company: "TripAdvisor Inc."
source_id: "tripadvisor-inc-common-stock-rss-6295d6870799"
canonical_url: "https://medium.com/tripadvisor/evolving-tripadvisor-search-building-a-semantic-search-engine-for-travel-recommendations-830f464318b7"
published_at: "2025-08-18T15:32:59+00:00"
first_seen_at: "2026-07-20T23:18:15.449539+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:c5e00ee6dd5d304f2841aec4c4a74caf5449a97a25edb572cf8839b47c9acdb4"
---

# Evolving Tripadvisor Search: building a semantic search engine for travel recommendations

# Evolving Tripadvisor Search: building a semantic search engine for travel recommendations


[Tripadvisor](https://medium.com/@tripadvisor-tech?source=post_page---byline--830f464318b7---------------------------------------)


9 min read


·


Aug 18, 2025


--


Press enter or click to view image in full size


*How Tripadvisor is evolving beyond keywords to deliver smarter, faster, and more personalized travel recommendations.*


*By ASK Sathvik, Jaskirat Singh Benipal, Jay DeStories, Luis Baía, Sabreena Rajan, Pedro Balage, Tripadvisor*


## The Challenge: Beyond Keywords in Travel Recommendations


At Tripadvisor, travelers seek personalized recommendations that help them experience the world in the ways they enjoy most. With 11M+ businesses and 1B+ reviews and contributions, users are often overwhelmed by the sheer volume of content available. Delivering the most relevant information quickly and intelligently is essential to creating a seamless experience.


Historically, our search and recommendation systems have been based on keyword matching. While this approach enabled fast retrieval, it relied heavily on word matching and keywords identification, limiting its ability to understand nuanced user intent and falling short when it comes to delivering contextually rich and dynamic recommendations. This meant that queries such as “Find me a peaceful place for a romantic dinner where we can see the sunset” could not be fully processed or supported.


To address this, we are transitioning towards a semantic matching approach. This paradigm shift focuses on understanding the meaning and intent behind user queries and matching them with the underlying semantics of our vast content, including listings data and, crucially, the sentiments, experiences, and descriptive details found within millions of user reviews and forum posts (currently only English content). All of this is done within just a few milliseconds, enabling us to surface real-time recommendations based on traveler experiences, delivering a smarter, faster, and more personalized discovery process.


### Powering Next-Generation Features


Our semantic search service is already enhancing several key Tripadvisor AI experiences:


**AI Trip Builder:** This tool generates travel itineraries based on user inputs like destination, travel dates, companions, and interests.


- By leveraging semantic search, we ensure that the recommendations for each itinerary are highly relevant to the user’s specific preferences.
- For instance, when a user indicates they’re interested in outdoor activities, semantic search interprets their preferences holistically, surfacing recommendations that align with those interests even if the exact search terms aren’t mentioned.


**AI Travel Assistant** : Our AI Travel Assistant enables users to ask open-ended questions through a conversational chat interface, providing easy access to tailored travel recommendations.


- When a user asks a specific question — like “Where can I find the best sunset views in Santorini?” or “What are some romantic hotels in Paris?” — their request is parsed and processed by our semantic search engine. Instead of relying on rigid keyword matching, the engine interprets the user’s intent and retrieves the most relevant results by comparing the query against rich, multi-faceted representations of each listing, including the title, description, reviews, and other metadata
- This ensures that users receive recommendations that align closely with what they’re truly looking for, not just based on keyword matches.


AI Travel Assistant interface


In both of these features, our semantic retrieval engine powers what is known as a Retrieval-Augmented Generation (RAG) strategy, where the recommendations are later refined by Large Language Models (LLMs) for better contextualization in the user response.


One important characteristic is that we don’t serve Point of Interest (POI) recommendations (such as attractions, restaurants, and hotels) directly from the LLMs. Although we recognize that these models are highly capable of generating good recommendations, we believe that by leveraging Tripadvisor content, our traveler reviews and forum posts, we can provide even better and more contextualized suggestions.


## Designing a Scalable Semantic Search Architecture


To support more nuanced and context-aware recommendations, we transitioned from traditional lexical search to a semantic retrieval system grounded in vector representations of content and queries. This shift required rethinking how we represent both the traveler’s intent and our listings — not as strings of words, but as dense, meaning-rich vectors. At a high level, our approach centers on three principles:


- **Unified semantic representation** : We embed both user queries and Tripadvisor content (e.g., POIs, reviews) into a shared vector space, allowing us to compute relevance based on meaning, not just words.
- **Multisource signals** : We incorporate diverse data types — listing metadata, user reviews, and behavioral context — into the semantic representation to maximize coverage and precision.
- **Efficient real-time retrieval** : We built a low-latency vector search pipeline capable of delivering relevant results within milliseconds, balancing relevance with speed for production use.


This framework sets the foundation for the embedding strategies, vector infrastructure, and hybrid retrieval techniques we describe in the sections that follow.


## Embeddings for Semantic Understanding


At the heart of semantic search lies the ability to represent text numerically in a way that captures meaning. For this purpose, we use machine learning models to generate embeddings for our content.


Press enter or click to view image in full size


An embedding model converts Tripadvisor points of interest (POIs) and reviews into numerical representations that capture their semantic meaning. These representations enable similarity-based search and personalized recommendations. Press enter or click to view image in full size


Review sentences are embedded as vectors in a 3D space, where semantic similarity is reflected by spatial proximity. Sentences with similar meanings cluster together, illustrating how embeddings capture contextual relationships between different types of experiences.


**Embedding Model:** We utilize embedding models like gte-base, which demonstrated good results for semantic matching. In addition, to optimize this model for the unique language and concepts within the travel domain (e.g., specific amenities, types of experiences, review sentiment), we fine-tuned these models on Tripadvisor’s extensive corpus of reviews, POI descriptions, and query logs to maximize relevance. These models transform text into high-dimensional vectors (e.g., 768 dimensions for gte-base) where semantically similar concepts are located closer together in the vector space.


**Embedding Strategy:** We generate separate embeddings for different content types:


- **POI Embeddings:** Derived from structured data like titles, descriptions, categories, and key attributes.
- **Review Embeddings:** Generated from the text of user reviews, capturing the nuances of traveler experiences and sentiments. This separation allows for flexible retrieval strategies. When a user query (e.g., “best honeymoon destinations”) is converted into its own embedding, we can compare it against both POIs and review embeddings.


**Semantic Matching:** The system calculates similarity scores (e.g., cosine similarity) between the query embedding and the content embeddings. This allows us to find relevant POIs even if the exact keywords aren’t present. A review mentioning “perfect for our anniversary trip” contributes to the POI relevance for “romantic getaways,” capturing the underlying semantic connection.


## Retrieval Engine


The core of any semantic search service lies in its retrieval engine, a component responsible for delivering fast, relevant, and contextually accurate results. While several strong solutions are available on the market, such as ElasticSearch, Milvus, Weaviate, and Pinecone, we chose to partner with **Qdrant** to power the semantic search capabilities of Tripadvisor. This decision was driven not only by Qdrant’s high-performance vector search capabilities, but also by its advanced support for **Geospatial Vector Search** , a crucial requirement for a travel-oriented platform like ours, where proximity and location relevance are essential to the user experience.


Qdrant serves as a vector database that supports cutting-edge retrieval techniques, enabling us to represent both POI data (e.g., hotels, restaurants, experiences) and user reviews as dense embeddings. These embeddings are generated to reflect both semantic meaning (capturing the intent and context of the text) and lexical similarity, ensuring that important keywords and phrases are not lost in the transformation. This dual-layered embedding strategy enhances recall and precision, particularly when handling complex and nuanced travel queries.


To deliver results that are both accurate and aligned with user intent, we implemented a **two-step retrieval algorithm** . This multi-phase approach allows us to consider a broader set of factors that influence what makes a recommendation relevant, useful, and expert-driven.


In the first step, we retrieve a set of relevant POIs by combining lexical and semantic similarity. This ensures that we capture results that not only match the terms the user has typed, but also those that are conceptually or contextually related. This step is crucial to capture content from travelers who may describe the same intent in different ways.


In the second step, we go deeper by retrieving all reviews associated with the POIs identified in the first phase. The retrieved POIs and their respective user-generated reviews are then re-scored based on semantic similarity to the user’s query. This allows us to prioritize POIs whose reviews best reflect the user’s intent, sentiment, or expectations.


Components diagram showing the two-step retrieval algorithm to semantic search Tripadvisor listings using both POI data and user reviews


A key differentiator in our approach is the emphasis we place on **user-generated content (UGC)** . As a platform built around the contributions of millions of travelers, Tripadvisor views UGC not just as supplementary information, but as a primary signal of quality and relevance. Reviews offer rich, firsthand perspectives that help future users make informed decisions. That’s why the second step on re-ranking based on the semantic similarity of reviews is arguably the most critical. It ensures that our recommendations aren’t just algorithmically sound, but are grounded in the experiences and voices of fellow travelers.


After retrieving the most similar results, a final step is considered to balance the relevance of the recommendations with **popularity** (recognition) and **quality** (user referencing). For popularity, we measure the number of reviews and as well as engagement with the POI, while for quality, we use our famous bubble score (a 5-grade average bubble rating on how other travelers recommend a specific POI).


## Evaluation and Quality Criteria


We evaluate the quality of our semantic search engine through several key methods to ensure it provides relevant and accurate results. By combining internal testing, relevance scoring, AI-based assessments, and user feedback, we gather comprehensive insights to refine search performance across all our use cases.


**Quality Assurance (QA) Testing** : We conduct QA across each of our AI experiences to ensure our semantic search engine performs consistently and accurately.


- Using a curated set of sample queries for each experience, we test search responses in different contexts, identifying any gaps or inconsistencies and making adjustments to optimize results.


**LLM as a Judge Relevance Assessmen** t: We use LLM-as-Judge evaluation, leveraging LLM complex models to analyze and rate the relevance of each result based on the user query information alongside location information and reviews matched.


- The LLM’s nuanced language understanding aids in identifying subtle relevance factors that might not be captured by traditional metrics, allowing a more thorough evaluation of search quality.


**Guardrail Metrics** : We know that popularity and quality are important factors. For this reason, we keep these metrics closely monitored, as well as identifying the presence of expected POIs for specific queries.


**End-User Feedback** : User feedback is a key metric in evaluating search quality.


- Explicit Feedback: We collect “thumbs up” and “thumbs down” responses directly from end users on search results, providing insight into real-world relevance.
- Implicit Feedback: We have tracking in place to capture interactions such as clicks, saves, and bookings, with the recommendations to help determine the quality of the results.


## Challenges and Ongoing Improvements


Building and refining a semantic search system presents ongoing challenges:


- **Precision vs. Recall:** Balancing finding all relevant items (recall) without including too many irrelevant ones (precision). E.g., searching for “sushi” might initially surface related concepts like “ramen”; tuning is needed to improve precision.
- **Data Sparsity:** Handling queries where the relevant information isn’t explicitly in reviews (e.g., “hotels with free Wi-Fi” required improving listing data integration).
- **Cold Start:** Effectively recommending new listings with few reviews.
- **Multilingual Support:** Ensuring consistent performance across the many languages Tripadvisor supports.
- **Cost:** Efficiently managing the computational resources required for embedding generation, indexing, and querying at scale.
- **Uptime and Latency:** Keep our Service Level Agreement (SLA) for uptime and latency response time as we promote new features. Our current latency response is around 200ms.


We are continuously iterating, exploring new modeling techniques, refining our retrieval and ranking strategies, and improving data integration to overcome these challenges.


## Conclusion


Tripadvisor’s move to semantic search represents a fundamental enhancement of our ability to **connect travelers with experiences that truly match their needs and preferences** . By leveraging deep learning models for semantic understanding, partnering with advanced vector database technology like Qdrant, implementing sophisticated hybrid retrieval and re-ranking strategies, and employing rigorous evaluation techniques, we are building a more intelligent and intuitive discovery engine. This ongoing evolution allows us to better harness the collective wisdom within millions of reviews, ultimately helping travelers plan and book their perfect trip.
