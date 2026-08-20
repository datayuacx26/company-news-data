---
schema_version: "1.0.0"
document_id: "758ef3b8b14815280dcae05b7707a15136f544cfa85cc7f8d00337b2c77e79f7"
company_key: "wayfair-inc-class-a-common-stock"
company: "Wayfair Inc."
source_id: "wayfair-inc-class-a-common-stock-news-import-42cff1750757"
canonical_url: "https://www.aboutwayfair.com/careers/tech-blog/seeing-beyond-labels-how-wayfair-decoupled-taxonomy-to-power-visual-discovery"
published_at: "2026-06-09T15:25:26.961+00:00"
first_seen_at: "2026-07-22T19:30:59.923738+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:d7bd1fbb49e2a2a7cf53dce7dc2b777782c9b1086aa0bc9005d0b81fd9a3d873"
---

# Seeing Beyond Labels: How Wayfair Decoupled Taxonomy to Power Visual Discovery

Wayfair is the go-to destination where customers can find just the right piece for their home. But we know the journey to a perfect home often begins long before a customer types “Beige Sofa” into a search bar. It begins with a spark of inspiration, whether that is a photo, a feeling or a style that words simply cannot capture.


To meet our customers where they are, we have reimagined the Wayfair app experience with two visual search powered innovations: Discover and Image Search. The Discover tab (see Figure 1) delivers an AI-powered stream of inspiration, enabling users to explore beautiful spaces and instantly shop or find similar looks. Image Search extends that capability into the real world, enabling customers to snap a photo of a piece they love and find similar matches in our catalog.


**Figure 1.** *Example Discover tab interface featuring AI-generated inspirational images.*


But making these experiences seamless requires a massive technical engine under the hood. To accurately match a user’s photo or a curated AI image against a catalog of over 200 million images, we had to fundamentally rethink how computers “see” our products.


# The Industry Standard (and Why It Wasn’t Enough)


For years, the standard approach to visual search, both at Wayfair and across the industry, followed a rigid, two-step logic:


1. **Detect and Classify:** First, identify the object in the image and assign it a specific category from a fixed taxonomy ( *e.g.* , *Loveseat* ).
2. **Search:** Then, look for other items in that same category that look similar.


This approach works well for distinct objects, but it fails in style-driven domains where category boundaries are often ambiguous (see Figure 2). In a home goods catalog, visually similar products, like a small sofa and a large loveseat, are often forced into separate categories despite looking nearly identical. A standard system acts as a hard filter: if it classifies a user’s photo as a *Loveseat* , it restricts the search exclusively to that category. This means relevant results sitting in the *Sofa* category, which might be the perfect visual match, are completely excluded.


**Figure 2.** *Example of ambiguous product-category boundaries in a restricted class taxonomy.*


Furthermore, relying on fine-grained classification makes the system fragile and expensive to scale. Every time the business updates its taxonomy or adds a new niche category, the underlying models must be retrained with massive amounts of new annotated data. We needed a system that could understand visual similarity without being constrained by these rigid, shifting definitions.


# Our Innovation: Taxonomy-Decoupled Architecture


To overcome these limitations, we proposed a taxonomy-decoupled architecture as shown in Figure 3. This design separates the task of finding an object from the task of identifying its category, allowing for a more flexible and generalizable search process.


**Figure 3.** *An overview of our taxonomy-decoupled visual search architecture.*


## Class-Agnostic Localization


Instead of training a detector to recognize thousands of specific product classes, we developed a localizer based on the YOLOX architecture. This model focuses purely on localization by generating classification free region proposals. During the training phase, we utilized *superclasses* , which groups thousands of fine-grained product categories into a few hundred visually similar groups, to help the model learn the general structure of home goods objects. However, at inference time, we discarded these labels entirely.


By removing the dependence on a specific taxonomy, the system can identify objects even if they fall outside the distribution of standard categories. This ensures that the search process is not prematurely narrowed by an incorrect or overly specific classification. This localization step is crucial because it allows the system to focus on the exact pixels representing the product without the bias of an intermediate label.


## Unified Embeddings for Similarity


Once an object is localized, it is encoded into a shared embedding space to facilitate retrieval. We utilized an OpenCLIP model which was fine-tuned on a dataset comprising hundreds of millions product images, associated text descriptions and product attributes from our catalog. This large scale fine tuning enables the model to capture a broad range of visual features including style, texture and form in a single unified representation.


The model is trained using a contrastive learning objective that encourages visually similar items to reside close together in the vector space regardless of their metadata category. By learning these embeddings directly from visual data rather than relying on category labels, the system develops a more robust understanding of aesthetic similarity that aligns with human perception.


Recently, we have been developing the next generation of unified embedding models based on recent VLM architectures, with the goal of further improving retrieval quality, generalization and scalability.


## High Performance Retrieval


We serve these embeddings using Google Vertex AI Vector Search which is built on the ScaNN algorithm. This enables us to perform approximate nearest neighbor searches across the entire global catalog in seconds. Because the architecture is decoupled from taxonomy, we didn't have to partition our search index by category. Instead, we maintained a unified search space that allows the system to surface visually relevant items from across the entire catalog.


This global search capability is essential for capturing the cross category visual matches that traditional systems often miss. We apply metadata filters such as geographic availability only at the retrieval stage which ensures that the search results are actionable without being visually restrictive.


# Overcoming the Evaluation Bottleneck


A critical part of our research involved solving the evaluation bottleneck. Traditional metrics often rely on noisy catalog metadata or expensive human annotations which are difficult to scale. We introduced a Large Language Model(LLM)-as-a-Judge framework (shown in Figure 4) to assess the quality of our search results in a zero shot manner.


**Figure 4.** *LLM-as-a-Judge evaluation framework evaluating each query-result pair in three steps.*


This framework uses a state of the art LLM to evaluate query result pairs based on two dimensions. The first is category relevance which is measured on a 3-point scale to determine if the result is functionally the same type of item. The second is visual similarity which is measured on a 5-point scale to assess aesthetic and style alignment.


To ensure the reliability of these automated scores, we implemented a step to check rating consistency. In this module, the model is prompted to review its own scores and self correct any logical conflicts, such as giving a high similarity score to an irrelevant category. Our validation demonstrated that this automated judge achieves almost perfect agreement with human experts.


# Results and Business Impact


We deployed this taxonomy decoupled system at Wayfair to replace our legacy class dependent pipeline. To ensure the scientific rigor of our findings, we conducted an extensive benchmark comparing our system against both our legacy architecture and Google Lens.


## Qualitative Comparison with Google Lens


Figure 5 qualitatively demonstrates how our evaluation balances visual similarity with category-aligned shopping intent. Given a query image of a cream-colored, straightline upholstered sofa, our system retrieves visually consistent results from distinct granular product categories: *Loveseats* and *Sofas* . This cross-category retrieval is evaluated as correct because both specific categories belong to the same broad class (“ *Sofas* ”) and share the query’s critical straight-line configuration. In contrast, Google Lens returns an item from product category *Sectional* . Although related, the L-shaped configuration fundamentally contradicts the query’s visual structure and functional intent. This distinction clarifies that our framework does not indiscriminately reward crosscategory diversity, but rather prioritizes results that align with the user’s specific visual intent.


**Figure 5.** *Qualitative comparison of our system against Google Lens.*


## Superior Retrieval Performance


Our primary benchmark utilized a diverse set of images, including user queries and AI-generated inspirational scenes. The results highlight the clear advantages of removing rigid taxonomic constraints:


- **Retrieval Precision:** Our system outperforms the legacy class-dependent system by more than 10% and significantly outperforms Google Lens by more than 15% in *top k Retrieval Precision* .
- **Visual Similarity:** In capturing nuanced aesthetic details of visual similarity, our system outperforms the legacy system by 13% and Google Lens by 20%.
- **Overall Success Rate:** *Success Rat* e, defined as the percentage of queries with at least one relevant and visually similar result, improves by approximately 19% compared with the legacy system and by 18% compared with Google Lens under our updated architecture.


These metrics demonstrate that our system is better equipped to handle the subjective and open-ended nature of home goods discovery by prioritizing visual likeness over strict category labels.


## Real World Customer Engagement


The improvements observed in our offline benchmarks translated directly into measurable uplifts in live customer metrics. Following the full-scale production rollout, we analyzed how users interacted with our visual discovery tools:


- **Direct Search Engagement:** The rate at which customers viewed product detail pages via our direct search tool increased by 16%.
- **Contextual Discovery:** Within our recommendation carousels on product pages, the engagement rate rose by 4%.


The significant lift in discovery indicates that our system excels at helping users explore the catalog when they are in the inspirational phase of their journey.


# Looking Ahead


The success of this taxonomy-decoupled architecture and the LLM-as-a-Judge framework provides a new foundation for visual discovery. By removing the constraints of fixed classifications, we have created a system that better aligns with the strong user desire for visual and inspirational discovery. We are now working toward extending this system into a multimodal discovery tool to further enhance the ability of our customers to find the perfect items for their homes.


# Acknowledgment


The authors would like to thank Jeff Arena, Vinny DeGenova, Graham Ganssle, Brian Seaman, John Gill, Vaidya Chandrasekhar, Anne Dearing and the entire Catalog Science Foundations Machine Learning Science and Engineering team, as well as the Customer Technology team, for their collaborations and contributions to this work.
