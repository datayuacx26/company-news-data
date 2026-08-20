---
schema_version: "1.0.0"
document_id: "0119b2072f6deaf01f0676047cc46e7c97813854495755454154b2b7cc197ca8"
company_key: "tripadvisor-inc-common-stock"
company: "TripAdvisor Inc."
source_id: "tripadvisor-inc-common-stock-rss-6295d6870799"
canonical_url: "https://medium.com/tripadvisor/scaling-our-existing-tagging-system-with-hybrid-graph-semantic-search-2322bdaa5b8d"
published_at: "2025-10-27T21:26:13+00:00"
first_seen_at: "2026-07-20T23:18:15.449539+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:aa927092f0b89fa47c91e231f13acc431a33c8954c11bebd27f1e434390730d9"
---

# Scaling our existing tagging system with Hybrid Graph-Semantic Search

# Scaling our existing tagging system with Hybrid Graph-Semantic Search


[Tripadvisor](https://medium.com/@tripadvisor-tech?source=post_page---byline--2322bdaa5b8d---------------------------------------)


15 min read


·


Oct 27, 2025


--


Press enter or click to view image in full size


*How we moved from a manual tagging system to a state-of-the-art hybrid graph-semantic relevance and retrieval platform with knowledge graph, and the dramatic effect it had on how we catalog, validate, and surface information for travelers.*


*By*[Sabreena Rajan](https://www.linkedin.com/in/sabreena-rajan-bba36b133/) *,*[Varuni Gang](https://www.linkedin.com/in/varuni-gang/) *,*[Meng Pan](https://www.linkedin.com/in/meng-pan-579044119/) *and*[Amir Rad](https://www.linkedin.com/in/amir-afrasiabi-rad/)


We typically start our blog posts with a sentence or two about how many reviews we store, how many points of interest we represent, or how many travelers trust us to provide relevant accurate data about their favorite travel destinations. Needless to say, it’s a lot of data and if you don’t believe us, just check[here](https://medium.com/tripadvisor/evolving-tripadvisor-search-building-a-semantic-search-engine-for-travel-recommendations-830f464318b7) ,[here](https://medium.com/tripadvisor/travel-faqs-made-smarter-ai-driven-answers-from-real-world-discussion-ec173da8b1a7) , or[here](https://medium.com/tripadvisor/cutting-processing-time-by-99-improving-data-synchronization-between-tripadvisor-and-a-marketing-60a8401c743e) . What we don’t often talk about however, are the real challenges we constantly face in making sure that travelers are consistently presented with the right information.


When you visit[Tripadvisor](http://tripadvisor.com/) , we want you to immediately see what matters most to you, whether that’s “kid-friendly spots in Rome” or “late-night jazz in London”. Making that happen is harder than it sounds! Behind the scenes, experiences like these are powered by our tagging system, which assigns labels, concepts, or topics to content in our system in the form of a “tag”. For example, when we tag a POI (or point of interest, such as a restaurant or museum) in Rome as *kid-friendly* , it makes it easy to find for families planning their trip. In short, tagging helps us to both understand our content and to present it back to travelers in meaningful and relevant ways.


But tagging at scale comes with hidden challenges. If tags are outdated or incomplete, travelers can’t find what they’re looking for. A restaurant may have switched from vegetarian to vegan, or a trend like “digital nomad” may emerge before our taxonomy can catch up. Those gaps directly impact the traveler experience.


Today, most of our tags exist only on POIs and historically they haven’t always been updated automatically as places evolve. At our scale, this makes discovery inconsistent for travelers. Furthermore, we’ve got millions of related pieces of content like images, forum posts, and articles, that all exist in the system but are not tagged at all. These challenges prompted us to build a new, scalable approach to tagging.


We are excited **** to share a new feature that is now live in our Android and iOS apps: **interest-based POI shelves** . These personalized *shelves* on the app homepage are used to showcase a collection of restaurants, hotels and attractions that are tailored to each traveler’s interests.


In this blog post, we’ll walk through how we built them, why they matter to our users, and how they represent a major paradigm shift in how we tag at Tripadvisor.


## New feature: Interest-based POI shelves


At Tripadvisor, shelves are organized by type of content: hotels, attractions, restaurants. Until recently, they were mostly static, meaning that they were manually created and didn’t automatically update. We’ve now introduced a new set of shelves organized by interests, such as Italian cuisine in New York, Luxury escapes in Paris, or Art & culture in Milan. These shelves are dynamically created for each traveler by identifying their most relevant interests and by fetching the associated POIs through newly created tags.


Press enter or click to view image in full size


*New feature: Interest-based shelves on our application home pages*


The tags are the important piece of the puzzle here since they make it possible to surface POIs that were previously hidden in interest-based searches. For example, a bar with live music that hadn’t been tagged, or a pasta spot that wasn’t linked to Italian cuisine. Without these tags, travelers searching for those interests would have missed them entirely and we wouldn’t have been able to surface them in any kind of dynamic way.


Our internal testing shows that these interest-based shelves have proven highly popular with members, achieving a CTR (click-through rate) over 10% which is significantly higher than other content on our app home page. We also found that CTR was highest for more specific interests, like theme parks. This insight highlighted the importance of stronger taxonomy design and more accurate tagging to capture detailed interests.


While the feature itself was a hit, it served a deeper purpose. It pushed us to rethink how we could build and scale our tagging system, in order to improve accuracy, expand coverage, and uncover new relationships between tags. By strengthening this foundation, we could make it possible to connect travelers with the right experiences faster and at a scale that was previously unachievable.


## Why tags needed a rethink


Tripadvisor uses tags to map shared attributes across places. Tags that represent amenities like pool or Wi-Fi, styles like romantic, or cuisines like Indian or Thai. They are critical for enabling travelers to filter properties like hotels and attractions, power shelves and maps, improve site search and boost SEO traffic.


To manage these tags at scale, we rely on a Knowledge Graph (KG). Think of a knowledge graph as a map of relationships. Each node is an entity in the real world such as a hotel, a restaurant, a city, or a cuisine type and each edge is a relationship between those entities such as *located_in_Paris* , *offers_Italian_Cuisine* , or *is_part_of_hotel_brand* . When a restaurant has live music, that information is stored as an edge in the graph.


Press enter or click to view image in full size


A visualization of POI data in our knowledge graph


Our graph is incredibly powerful because it allows us to run structured queries at scale. For example, we can ask: “Find all hotels in Rome with rooftop pools”, or “Show family-friendly resorts in Mexico that also have waterparks”. But the results are only as good as the tags and the relationships that power it. If a hotel isn’t tagged, it will not show up in the search results. What’s more, if relationships are incomplete (say a hotel has a “waterpark” tag but is missing the “family friendly” tag) the traveler’s experience could feel patchy.


Above, the Charlie’s POI is not tagged with the “Live Music” tag


Until recently, tags were:


- Created and maintained via business rules, often leading to outdated or inconsistent coverage.
- Rarely updated when places changed, for example if a restaurant went vegan, a traveler couldn’t discover it until the owner or staff updated it.
- Difficult to govern at scale with overlapping concepts like beach resort, family beach resort and luxury beach hotel
- Slow at capturing new trends, which often appear in reviews long before a formal tag exists. This made it very difficult for travelers to find them.


Clearly, our existing tagging process wasn’t keeping pace with the richness and velocity of traveler data. That’s why refreshing how we tag the content became central to unlocking better discovery.


## From idea to proof of concept


As we built our other recent RAG (Retrieval Augmented Generation) applications like[review summaries](https://medium.com/tripadvisor/revolutionizing-travel-reviews-tripadvisors-ai-driven-summaries-d956e9e0f438) and[traveler FAQs](https://medium.com/tripadvisor/travel-faqs-made-smarter-ai-driven-answers-from-real-world-discussion-ec173da8b1a7) , we realized that other leading companies and research projects were showing promise with innovations in combining RAG and Knowledge Graphs. These approaches were often referred to as GraphRAG. In parallel, leading graph databases like Neo4j and Neptune began supporting vector indexes for semantic search. This was exciting to us because it demonstrated that knowledge graphs could now do more than answer structured, tag-based queries; they could now also help improve tag coverage and even evolve our existing taxonomy. For example, the graph could answer not only predefined queries, but also dynamic ones like “live music in New York,” powered by semantic search over reviews.


To test this, we launched a three-week prototype in Neo4j with a simple goal to:


- mine unstructured review text with semantic search for queries like “Live music in New York”
- combine these signals with structured graph traversal to validate or add the right tag-to-property connections
- determine if these new graph capabilities could scale in practice


Because these features were still in the early stages of adoption, we encountered a few obstacles. Documentation and best practices were scarce, vector index–based search in graphs lagged behind standalone vector databases, and features like prefix filtering weren’t yet available. To work around this, we built multiple vector indexes per geo/type combination.


Even so, the early results were encouraging. By combining structured traversal for existing tags with vector retrieval over reviews, we managed to identify several relevant and under-tagged POIs. Even better, when a certain topic or query wasn’t yet represented by a tag, we were able to propose it as a new one. Most importantly, we proved that we could develop a scalable approach to dynamically identify tag-to-property connections and new tags, and represent them in the graph for future structured retrieval.


We shared these findings with product leaders, showed early examples of how it could improve discovery, and secured investment to productionize the idea. Over the following six months, we scaled the proof of concept into an MVP. We tested how well this approach worked at global scale and measured the impact of improved tag coverage on real travelers.


## How Hybrid Graph-Semantic Relevance works


To properly scale the tagging of our content it was important for the search to optimize for recall, so that we could capture all relevant properties for a given tag. Since reviewers express essential ideas in many ways, we implemented rigorous validation to ensure that each tag assignment properly reflected a tag’s meaning and that the right POIs were captured. For example, one restaurant may only have a jazz performance on weekend evenings, while others host live music every night. We want to capture all restaurants that offer live performances.


To achieve this, we developed an extensive multi-step pipeline that applies cutting-edge machine learning techniques and models to ensure accurate and scalable tagging.


### Preparing and ingesting data


Every search and recommendation system starts with data. At Tripadvisor, we have an abundance of it: POIs, Geos, Tags, and reviews. The problem is that they are stored scattered across our data warehouse, and aren’t directly usable. To address this, we built a two-step data preparation and ingestion pipeline to load data into the Knowledge Graph with the data model below. Like any robust pipeline, it supports both full load and incremental loads.


Press enter or click to view image in full size


A high level representation of our data model


### Data preparation


Our ETL collects data from multiple sources and preprocesses it for recommendations. This includes filtering out inactive POIs and selecting only recent reviews, since outdated ones don’t reflect a POI’s current state. Since we want to enable semantic retrieval, we generate[embeddings](https://medium.com/tripadvisor/evolving-tripadvisor-search-building-a-semantic-search-engine-for-travel-recommendations-830f464318b7) for tag, destination and POI names using MPNet and review sentence embeddings using GTE (reviews chunked by sentence).


### Ingestion


We ingest data to Neo4j using Spark connectors to ensure parallelized performance. The following challenges required special handling:


**Avoiding deadlocks**


Graph databases like Neo4j can lock nodes when adding multiple edges with the same source or target, causing deadlocks in many-to-many relationships. This happens often between Tags and POIs. Instead of implementing a complex backoff system, we adopted a smart hybrid approach by partitioning data by tag, ingesting all tag-POI edges in parallel within each partition, and then processing partitions sequentially. Because tags are roughly 1000 times fewer than POIs, this method avoided race conditions efficiently.


**Enabling partial search within vector indices**


Our project required semantic search across a combination of tags, destination, and POI names along with review sentences, however Neo4j currently does not support searching within a subsection of a vector index. A naive solution would have been to create multiple global vector indices for each of the above, and filtering the review sentences post graph retrieval, however that would be costly and nondeterministic. Instead, we created a separate vector index for each destination to hold the review sentence embeddings. The retriever now searches only the index relevant to the destination, making queries efficient and scoped.


Press enter or click to view image in full size


A “30 thousand foot” view of our ingestion pipeline


### Graph-Semantic retrieval


To retrieve an extensive list of candidate properties, our first step was to introduce two types of searches over knowledge graph:


### Tag based retriever


This retriever searches across all tags in the database which are determined to be most similar to the query (for example, *live music* or *all-inclusive* ). The query is mapped into vector space, and tags above a similarity threshold are selected. Associated POIs are then fetched from the graph. Final ranking combines tag-query semantic similarity with the strength of the tag-POI relationship, using the existing relevance score.


### Semantic search retriever


This retriever scans reviews stored in the graph to find those semantically similar to the query, and then retrieves the associated POIs. Each POI is ranked based on both the similarity of its matched reviews to the query and the number of relevant reviews found for it.


These retrieval techniques combine to balance precision and coverage. The tag retriever always runs first, returning places with strong tag matches. If the number of tag results is insufficient, the semantic search retriever kicks in to surface additional POIs from review text. For example if the query is “Japanese restaurants that serve sushi”, the tag retriever returns all places tagged as Japanese restaurants, while the semantic search retriever returns those where reviews mention sushi. Together, they deliver a complete set of results, both officially tagged restaurants and those that were validated through customer reviews. These results are de-duplicated and ranked by a combination of popularity and relevance, ensuring the most meaningful POIs rise to the top.


Press enter or click to view image in full size


A high level view of our retrieval chain


## Refining relevance with models


After retrieval, we re-score each candidate POI using a cross-encoder model. In our evaluation, the Jina reranker (cross-encoder model) consistently outperformed other models at capturing fine-grained context and the interactions between queries and review sentences, though it comes with higher computational cost. We applied thresholds to the reranker’s output relevance scores, ensuring that only POIs with high-confidence match are retained, delivering the precision necessary for reliable tagging.


To further refine results, we introduced an LLM-based judge to validate whether each POI truly aligns with the tag’s intended meaning. Finally, we applied a multi-factor scoring method that combines POI relevance with popularity, ensuring highly relevant and widely favored places rise to the top.


For example, when tagging Chinese Hotpot restaurants in London, our initial Graph-Semantic Retrieval yields roughly 4000 locations. Over 90% turned out to be Chinese restaurants that don’t serve hotpot. The review below was identified in this stage due to its semantic similarity with our query ‘Chinese Hotpot.’ Although it’s considered by our travellers to be one of the best Chinese restaurants, it does not serve hotpot.


Press enter or click to view image in full size


A traveler review taken from Tripadvisor.com


The re-ranker filtered out most of these irrelevant results, but a few nuanced cases remained. For instance, this review was included because it serves Tofu hot pot which our model did not distinguish from Chinese Hotpot:


Press enter or click to view image in full size


A traveler review taken from Tripadvisor.com


To resolve these nuanced cases, the LLM-based judge validated the remaining candidates, leaving a refined list of authentic Chinese Hotpot restaurants that travelers can confidently discover.


Here is the LLM’s assessment of the restaurant discussed above:
` “The reviews do not mention hotpot specifically, and the restaurant is not known for hotpot, focusing instead on various other Chinese dishes.”`


As for Haidilao Huǒguō The O2:
` “Haidilao Hot Pot London O2 is specifically known for its hotpot cuisine, with multiple reviews highlighting the variety of hotpot bases, fresh ingredients, and the overall hotpot dining experience. The reviews consistently mention hotpot-related aspects, confirming its relevance to the query.”`


## Turning search into a tagging engine


This hybrid search not only powers our shelves but also continuously improves the tagging system and knowledge graph itself. When a traveler query (for example beach, swimming) maps cleanly to an existing tag, we return the tag along with relevant POIs. Any additional POIs that aren’t yet tagged are added as new tag-to-property connections. If a query doesn’t map cleanly to an existing tag, we can still fetch relevant POIs which are validated by review signals, and then scalably add the query as a new tag in the graph.


This approach addresses the tag coverage gaps (like missing venues associated with “live music”) and also improves query performance. Once queries become actual tags in the graph, subsequent lookups can rely on computationally-efficient structured graph traversal instead of intensive semantic retrieval, delivering faster results, lower operational cost, and more predictable performance.


A high level view of the tagging engine and how we use it to improve our tagging system and knowledge graph


In our latest run, we successfully restored 167,000 missing tag‑to‑property connections across 131 existing tags, strengthening discovery where coverage had long been incomplete. In addition, we introduced 133 new tags and established over 414,000 new tag‑to‑property connections, vastly expanding the breadth and accuracy of our tagging system.


To ensure accuracy over time, we re-run hybrid graph search every month to capture the latest reviews, to adjust existing assignments and to propose new ones. This closed-loop process is what enabled the interest-based shelves to scale so broadly.


For travelers, this means discovering more relevant content even faster, whether it’s the latest food trend, a hidden music venue, or the perfect boutique hotel.


## Challenges and learnings


As with any approach there are always some limitations, some of which we have yet to solve:


- Factual tags need safeguards. A number of tags in our system are objective, for example, hotel star rankings are awarded by a governing body, based on a very strict criteria. A hotel would not be pleased if we used this approach and accidentally misrepresented their star rating.
- Similarly, there will always be certain curated tags. For example, the *Traveler’s Choice* awards that Tripadvisor gives out on a yearly basis. These tags are human governed and will be excluded from automated assignments.
- Finally, we’d need to address what we call the cold start problem, or what happens when a brand new restaurant, hotel, or attraction opens for business. New POIs are added every day without reviews and will continue to require manual and partner-provided tag assignments.


## What’s next


We plan to scale the approach to dramatically improve traveler experience across Tripadvisor. Beyond POIs, we host a wide range of unstructured and multimodal content such as forums, Q&As, articles, images, and videos. By tagging this content and connecting it in our knowledge graph, we should be able to dynamically surface it based on each traveler’s interests.


Imagine discovering your favorite local sandwich shop in Chicago and, alongside it, seeing expert-written articles about the pulled pork sliders or the homemade milk shakes for the destination you are headed to next. This kind of personalization becomes possible simply by tagging and organizing all the different types of content on our platform.


We are exploring ways to improve existing tag assignments, adding precision where needed, and removing existing assignments when the latest reviews give us high confidence that they are no longer valid.


We believe that building a clearer, more coherent taxonomy and leveraging machine learning to understand neighborhoods of related tags will be invaluable in surfacing new and complementary interests. For example, if a traveler is browsing for beachwear in Huntington Beach, they might also want surf lessons at a nearby shop. Drawing on research in tag taxonomy and community detection on graphs, we can infer that clustered tag relationships help identify related interests even when they aren’t explicitly tagged.


On the technical front, we are continuing to refine each step of our approach. For semantic retrieval, we are exploring incremental/delta processing across months to avoid re-evaluating reviews we’ve already processed. This should make the pipeline faster and more cost-efficient.


## Conclusion


Moving from mostly manual tagging to a hybrid graph-semantic relevance platform represents a true paradigm shift. It brings together two of Tripadvisor’s greatest strengths: our rich base of traveler reviews and our modern graph. With appropriate human-in-the-loop (HiTL) processes, this approach will help to keep tags accurate, discover new concepts and make relevant content easier for travelers to find. The project itself also reflects something we value deeply: smart, ambitious teams taking calculated risks to solve problems that matter to travelers.


Our goal is to keep increasing the amount of personalized content we can serve, ultimately providing a more meaningful experience for travelers and trip planners alike. It’s the traveler’s job to share their experiences with the world, but it’s our responsibility to make those insights discoverable.


## Acknowledgements


A project of this scale is never the work of one group alone. While the post highlights the machine learning and graph innovations, it was made possible through close collaboration across Product, Engineering, and Data teams. We’d also like to recognize our external partners at Neo4j, whose support and expertise were invaluable in helping us push boundaries of what graph technology can do at scale.


Special thanks to Rahul Todkar, Peter Gaidarev, Gianluca Rossi, Scott Pingeton, and Chris Chalifour for their leadership in helping us prioritize this effort, and to our teammates Sonali Syngal, Chuchen Xiong, Rishabh Maheshwari, Joao Miguel Figueiredo Lopes De Simas, Haley Derrod, Long Yao, Abhishek Gurung and Konstantin Buzdin for their contributions throughout the journey. Together, their efforts turned an ambitious idea into a platform that meaningfully improves how travelers discover the world on Tripadvisor.
